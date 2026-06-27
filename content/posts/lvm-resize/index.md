---
title: 'How To Resize LVM Volumes Dynamically Using Cloud-Init (Step-By-Step)'
date: 2026-03-26
summary: "Configuring cloud-init and UEFI to dynamically change the size of VM disks on first-boot."
categories: ['blog']
tags: ["Proxmox", "Tutorial", "Linux"]
---
Logical Volume Manager (LVM) is the preferred storage framework for Linux VMs. It's easy to resize LVM volumes, migrate data between devices, and combine multiple physical disks into one easy-to-mange volume. These features are especially useful in a virtualized environment, as they enable you to do these tasks without physical access to the machine.

When creating VM templates, it's common to create a fixed-size volume for the root partition and **use cloud-init to grow the partition size dynamically when cloning the template** (unless you want to _create multiple templates_ with each one having 10G, 11G, 12G... storage and so on). However, you can't simply use the built-in growpart module to resize logical volumes. It [only works on partitions](https://forum.proxmox.com/threads/cloud-init-lvm-resize-not-working.68947/).

But that doesn't mean you should avoid using LVM in your templates. After a few trials and tribulations, I've found a workaround that allows cloud-init to automatically resize logical volumes, as well as a few pitfalls you should avoid when using cloud-init with Debian machines. Here's what I've learned.

![Animated GIF depicting a fight between Debian and LVM as the fight between the Boss and Snake from Metal Gear Solid 3.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ur32gebhokmal55curod.gif)

## The Two Steps to Grow LVM Volumes with Cloud-Init

![The partition configuration of the Linux VM. The third partition has 30 GB of storage.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/caspg3ugkp3vo05s7kcc.png)

Here's the scenario. There are 3 partitions on my VM template, with `/dev/sda3` containing the root logical volume (LV) named `ubuntu--vg-ubuntu--lv`. The VM's disk had 30 GB of storage, but I've resized the drive to be 70 GB. The root LV is still 30 GB, so I need cloud-init to automatically resize the volume to fill the remaining space.

### Grow the Partition
First, we need to **grow the partition** using the [growpart](https://docs.cloud-init.io/en/latest/reference/modules.html#growpart) module. While we can't use growpart directly on the root LV, we can use it to grow the partition that contains the underlying physical volume (PV) of the root volume.

Create a cloud-init configuration in `/etc/cloud/cloud.cfg.d/90-LVM.cfg`, and add the following:
```yaml
growpart:
  devices: [/dev/sda3]
```

To test the config, set cloud-init to run on the next boot and reboot the computer.
```bash
sudo cloud-init clean -r
```

Here's how the partitions look after growpart.

![The partitions of the VM, now with the third partition having 68 GB.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k94jaijq276ag6b3ssps.png)

Growpart expanded `/dev/sda3` to 68 GB using the newly added free space in the storage disk.

![PVS command showing that the PV of the root volume was expanded.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8sybz46otn4igfvansb8.png)

Because the `sda3` partition is mapped to the root PV, `debian-vg`, it also expanded the underlying PV of the volume. We can now expand the LV to fill the space in the physical volume.

### Grow the Root Volume
Second, we need to **grow the logical volume**. There is no built-in cloud-init module to manage LVM volumes, but we can use the [runcmd](https://docs.cloud-init.io/en/latest/reference/modules.html#runcmd) module to execute the shell commands to resize the logical volume.

One caveat is that we have to make sure runcmd only triggers after the growpart module. Otherwise, we are expanding the LV before the PV is expanded. We can check the modules execution order by looking at the default cloud-init config at `/etc/cloud/cloud.cfg` and making sure that `runcmd` is executed after `growpart`.

![Snippet of the boot stages and their modules.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/avsjrtpee3wbjcsevtzu.png)

Modules in the "config" stages will always run after the "init" stage, so make sure that runcmd is under `cloud_config_module`.  

> Fun fact: runcmd actually [defers the execution until the scripts_user module in the "final" boot stage](https://docs.cloud-init.io/en/latest/reference/modules.html#runcmd), so make sure that `scripts_user` is under `cloud_final_module` as well.


![Excerpt from the official documentation describing how the runcmd module runs its script in the final boot stage.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/at6xotmjok3hfp03nsj7.png)

Now, we can add a runcmd command to resize the root LV into `/etc/cloud/cloud.cfg.d/90-LVM.cfg`. In my case, the root path (`/`) of my VM is mounted to a logical volume named `ubuntu-lv`, which is mapped to a PV named `ubuntu-vg`, so my runcmd looks like this:
```yaml
# append to /etc/cloud/cloud.cfg.d/90-LVM.cfg
runcmd:
  - [lvresize, -l, +100%FREE, -r, /dev/ubuntu-vg/ubuntu-lv]
```

This command resizes the volume using all of the remaining space (`+100%FREE`) while also resizing the file system in the volume (`-r`).

Test the config again by rebooting the machine and rerunning cloud-init:
```bash
sudo cloud-init clean -r
```

Upon reboot, cloud-init should resize the root LV to use the remaining free space in the storage disk.


![Partitions of the VM, now the root volume is using all of the available space.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6y23l5tts0ig7x8n459p.png)

### Troubleshooting
If cloud-init isn't resizing the volumes as expected, your first troubleshooting step should be checking the logs at `/var/log/cloud-init.log`.

For example, I had some confusion regarding the possible values for the runcmd module when I started working on this project. Look at this:

![Snippet of the runcmd module with its schema.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/q98uut02cwybbdpy5frv.png)

Does this mean the module accepts 1) an array containing arrays containing strings, or 2) an array containing arrays of strings, *or* just a string? To get a practical understanding of the schema, I first tried using a simple string for the module.

![A snippet of the runcmd module with only a string as its input.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/82aawnh4l0z51oerc5ou.png)

After a reboot, cloud-init ran the configuration, but the LV is still the same size. Take a look at the logs:

![Snippet of the cloud-init logs. One of the errors shows that the runcmd module did not expect a string as its input.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0mui4hbywmcehq4z4426.png)

Note the `TypeError: Input to shellify was type 'str'. expected list or tuple` line. This line tells us that the module expects the inputs to look like this in practice:
```yaml
# this is correct
Array [
  - StringArray ["a", "b"]
  - String "abc"
  - Null null
]

# not this
Array [] || String "abc"
```

Niche issue? Probably. But if you need to write a more advanced runcmd config, reading `/var/log/cloud-init.log` can help clarify some of the ambiguity.

## Debian-specific Issues ("No free sectors")
![Comedic depiction of Debian and LVM as the Boss and Snake from the game Metal Gear Solid 3. Debian is injuring LVM.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kb3v5d5oxhf5qywtmi5c.png)

The steps above should work for most cases. However, when using cloud-init and growpart to resize volumes in Debian VMs, you might run into the following error:

![Snippet of the growpart error. The root volume is at sda5, and a line in this error shows that the module failed to resize partition sda3.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hrxslp55j689xyyozoeh.png)

Note the `/dev/sda3: No free sectors available` line. Our root LV is located on `sda5`, and growpart failed to resize that partition because it failed to resize `sda3`. But... that disk *doesn't exist*. Why is growpart trying to resize a non-existent disk?

![Snippet of the growpart documentation.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/06m010hrxvzno7y3ydqq.png)

In the documentation, growpart will only resize the last partition on the disk. In practice, this means the *last* partition. As in, the partition has to be last numerically, and you cannot skip numbers. If we want growpart to resize our root LV, it has to be on partition `sda3` rather than `sda5`. In other words, 1,2,3 and not 1,2,5.

But why does our VM create the partitions in this way? It's because [the MBR partitioning scheme requires any LVM storage to be on sda5](wiki.debian.org/Partition#MBR_format_partitions). A GPT partition scheme won't have this issue, so we should change the partitioning scheme of our VM to use GPT instead.

But how do you make Debian use GPT over MBR? By using UEFI. The Debian installer automatically decides the partitioning scheme [based on whether you're using BIOS or UEFI](https://unix.stackexchange.com/questions/518377/where-does-the-debian-installer-choose-mbr-vs-gpt). So if we want the partitions to be in order without skipping numbers, we have to complete the installation process with UEFI enabled.

Convoluted? Yes, but the fix is simple: **use UEFI during installation**. In Proxmox, you can enable UEFI by changing the `BIOS` option to `OVMF (UEFI)`:

![Snippet of the BIOS setting in Proxmox. The BIOS is set to OVMF (UEFI).](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8pjxgf2n5yfa77za4k5y.png)

Then add an EFI disk:
![Snippet of the hardware options in Proxmox. The "Add" tab is opened and shows all of the possible device options.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gvwrv5kcz5wezlntajxj.png)

Then go through the installer as normal and choose `Guided - use entire disk and set up LVM` during installation. After completing the installation, your partitions should now be in order:
![Partitions of the VMs, the root volume is now at sda3.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7k68sf1a3cv78l24dlvk.png)

These are the partitions of my Debian VM after using UEFI during installation. Note how the root LV is now at `/dev/sda3`.

I could now use growpart and runcmd to resize the root LV. This is my configuration for the Debian VM:

![The cloud-init config for the Debian VM. The config creates a default user called "debian" and resizes the root volume using growpart and runcmd.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eq56htysicuwjix2tnwz.png)

Here's the result after rerunning cloud-init:
![Partitions of the Debian VM, now the root volume is using all of the available space on the storage disk.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5eyqi9enx0vc6dh13uj4.png)

Cloud-init resized the root LV without the `No free sectors` issue.

### Troubleshooting "status: disabled" and Unrecognized Cloud-init Drive Issues
You might run into other issues with cloud-init while setting up a Debian VM. Here's a short guide on how I troubleshoot and resolve these issues.

First, I got this error:
![Snippet of the "cloud-init status -l" command. The status shows that cloud-init is disabled by its generator.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/r5vcnnil4844kulmyjzj.png)

Cloud-init wasn't running, and running a status check tells us that it was disabled by the cloud-init-generator, but it doesn't tell us why.

Maybe the source code of cloud-init-generator can tell us more about the cause of the issue. You can find cloud-init-generator and its logs by running this command:
```bash
dpkg-query -L cloud-init | less
```

The command shows all the files that were installed with the cloud-init package.  Look for `cloud-init-generator`:
![Filtered result of the dpkg-query command. It shows that cloud-init-generator is located in the /usr/lib directory.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lr5mc509caeinr3t7n0j.png)

It's in `/usr/lib/systemd/system-generators/cloud-init-generator`. Here's the first few lines of the file:
![Snippet of the system-generators file. One of the lines contains the location of the log file for this script.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l709mtuo511k2mncyoi6.png)

Note the `LOG_F` variable. That's the location of the log file where we can learn more about why Cloud-init was disabled.

![Snippet of the log file. A line describes that the script ran but didn't find any data source.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6l2peltpd8m2go7sc4aq.png)

Cloud-init used the `ds-identify` component to identify data sources, and it couldn't find any valid configuration sources. However, I've attached a cloud-init drive to the VM via the Proxmox GUI, so what's going on?

### Data Sources Formatting
Let's check the logs related to ds-identify at `/run/cloud-init/ds-identify.log` (also recommended by [the documentation](https://docs.cloud-init.io/en/latest/howto/debugging.html#cloud-init-did-not-run)).

![A line from the log file describing how the field datasource_list wasn't found.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/v1mjpso0tept57o0ic1g.png)

From the `WARN: no datasource_list found` message, it seems like one of the problems is that the configuration expects you to use `datasource_list` in your configuration.

Here's how I changed the data sources configuration:
![Text snippet showing the wrong and right way to write the data sources section. The data sources list is on a single line with a list as the value.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gycw79bmx19gjxv0n93g.png)

And here's the output of `/run/cloud-init/ds-identify.log` after applying this change:
![A line from the ds-identify log file. The script can now read the data sources list.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fvgjw7gq7xt25ia5ldlv.png)

Cloud-init now detects the sources list, but it still doesn't recognize the cloud-init device. I was puzzled by this issue for a while, until I found something a few days later.

### Cloud-init Drive Interface
As of March 2026, according to [this post](https://forum.proxmox.com/threads/cloud-init-drive-missing.174803/) and [this post](https://forum.proxmox.com/threads/cicustom-cloud-init-stopped-working.164297/#post-759392), there is a compatibility issue between IDE devices and OVMF. In practice, cloud-init drives that use IDE aren't recognized by the VM if you're using OVMF (UEFI). 

The fix is simple: **use SCSI for your cloud-init drive**. When you're creating the cloud-init drive, choose the SCSI option in the Proxmox GUI:
![Proxmox GUI for the CloudInit Drive setting. The drive is set to SCSI.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vs87qara09011588plpv.png)

For example, here are the hardware options for my VM:
![The hardware options in Proxmox. The cloud-init drive is set to IDE.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/05fhp3qx17sjd9q5s1ir.png)

Here's the list of block devices recognized by the VM. Note how, despite having two `cdrom` drives and one of them being the cloud-init drive, only the CD/DVD drive shows up in the list.

![Block devices list of the VM. Cloud-init drive is not shown.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/q762jrmw1zhxuyxmf6wb.png)

Now, I changed the cloud-init drive to use SCSI:
![The hardware options in Proxmox. The cloud-init drive is set to SCSI.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wpd78g4ftxznkrm9v6ph.png)

Here's the updated list of block devices. Note how the VM now recognizes the cloud-init drive at `/dev/sr0`. The VM should now recognize the cloud-init drive and execute your configuration on startup.
![Block devices list of the VM. Cloud-init drive is on the list.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0s0a5ng8lfx0jnktl800.png)

## Closing
Questions? Thoughts? Feel free to leave a comment on [dev.to](https://dev.to/patimapoochai/how-to-resize-lvm-volumes-dynamically-in-cloud-init-vms-step-by-step-1oml).

Need someone skilled in RHEL, Kubernetes, and AWS? I'm open to work! View my [portfolio](https://patimapoochai.github.io/) and reach out via [LinkedIn](www.linkedin.com/in/patima-poochai808) or [Mastodon](https://infosec.exchange/@patlikestechnology).
