Linux Logical Volume Management (LVM) and AWS EBS
In Linux, Logical Volume Management (LVM) is a system for managing disk drives and storage devices. LVM abstracts the underlying physical storage and presents it to the user as logical volumes, which can be resized, moved, or expanded dynamically. AWS Elastic Block Store (EBS) volumes are often used with Linux LVM for managing storage in cloud environments.
________________________________________
Key Concepts in LVM:
•	Physical Volume (PV): A physical storage device (e.g., hard disk, partition, or EBS volume) that is used by LVM.
•	Volume Group (VG): A pool of physical volumes that are combined together to create logical volumes.
•	Logical Volume (LV): A virtual partition created within a volume group that can be formatted and mounted like a physical partition.
________________________________________
Useful Linux Disk Management Commands
1.	lsblk
Lists information about block devices in your system (such as hard drives, partitions, and LVM volumes).
Example:
Displays all available block devices and their attributes.
2.	df -h
Shows disk space usage in human-readable format (GB, MB, etc.).
Example:
Displays information about disk space usage.
3.	pvs
Displays information about the physical volumes (PVs) in the system.
Example:
Shows a list of all physical volumes available.
4.	vgs
Displays information about the volume groups (VGs) in the system.
Example:
Lists all volume groups and their attributes.
5.	lvs
Displays information about logical volumes (LVs).
Example:
Shows details about logical volumes in the system.
________________________________________
Commands for Creating and Managing LVM
1.	pvcreate /dev/xvdf /dev/xvdg /dev/xvdh
Initializes physical devices (e.g., /dev/xvdf, /dev/xvdg, /dev/xvdh) to be used as physical volumes.
2.	vgcreate tws_vg /dev/xvdf /dev/xvdg
Creates a volume group (tws_vg) using the specified physical volumes (/dev/xvdf, /dev/xvdg).
3.	lvcreate -L 10G -n tws_lv tws_vg
Creates a logical volume (tws_lv) of size 10GB in the tws_vg volume group.
4.	pvdisplay
Displays detailed information about physical volumes.
5.	vgdisplay
Displays detailed information about volume groups.
6.	lvdisplay
Displays detailed information about logical volumes.
________________________________________
Formatting and Mounting Logical Volumes
1.	Creating a mount point and formatting a logical volume:
o	mkdir /mnt/tws_lv_mount
Creates a mount point for the logical volume.
o	mkfs.ext4 /dev/tws_vg/tws_lv
Formats the logical volume (/dev/tws_vg/tws_lv) with the EXT4 file system.
o	mount /dev/tws_vg/tws_lv /mnt/tws_lv_mount
Mounts the logical volume to the mount point.
2.	Unmounting the logical volume:
o	umount /mnt/tws_lv_mount
Unmounts the logical volume from the mount point.
________________________________________
Managing Physical Volumes and Filesystems
1.	Mounting a new disk:
o	mkfs -t ext4 /dev/xvdh
Formats the physical volume (/dev/xvdh) with the EXT4 filesystem.
o	mount /dev/xvdh /mnt/tws_disk_mount
Mounts the new disk to a mount point (/mnt/tws_disk_mount).
o	df -h
Checks the disk usage of mounted filesystems.
________________________________________
Expanding Logical Volumes
1.	lvextend -L +5G -n /dev/tws_vg/tws_lv
Increases the size of the logical volume (tws_lv) by 5GB within the volume group (tws_vg).
2.	resize2fs /dev/mapper/tws_vg-tws_lv
Resizes the filesystem to use the newly allocated space after extending the logical volume.
________________________________________
Summary of Key LVM Workflow
1.	Initialize physical volumes (PV) using pvcreate.
2.	Create a volume group (VG) using vgcreate by combining multiple PVs.
3.	Create a logical volume (LV) with lvcreate from the VG.
4.	Format the LV with mkfs and mount it to a directory.
5.	Expand the LV when more space is required, using lvextend and resize2fs.
This LVM system provides flexibility to grow and shrink storage volumes as required, without needing to repartition or reformat physical disks.
