# Constructing and Using File Systems and Swap

A file systme is a logical container that is used to store files and directories that is created in a separate partition or logical volume. 

There are several different types of file systems that can be categorized in theree basic groups: disk-based, network-based and memory-based. 

* Disk-based are typically create on hard drives using SCIS, iSCSI, SATA, SAS, USB, Fibre Channel...
* Network-based are disk-based file systems shared over the network for remote access
* Memory based file systems are virtual

| File System | Description | 
| --- | --- | 
| ext2,ext3, ext4 | Different generations of extened file systems | 
| xfs | 64-bit file system. Supports metadata journaling, online defrag, expansion, quote journaling |
| btrfs | B-tree file system is designed for handling more files thant ext4 |
| iso9660 | This is used for CD/DVD-based optical file systems | 
| BIOS Boot | Small partition required for GPT on a BIOS | 
| EFI System Partition | Small partition for GPT on UEFI system | 
| NFS | Network File System |
| AutoFS | Auto File System. NFS file system set to mount and unmount automatically on a remote system | 
| CIFS | Common Internet File System (e.g. Samba)


#### XFS

The structure is divided into two sets. One holds the file system's metadata information and is tiny. The other stored the actual data. The metadata includes the  superblock which keeps the file system structural information. The metadata also contains thde inode table and holds file attributes such as type, permissions, ownership etc. 

ext3 onwards support a journaling mechanism that keeps track of their structural changes in a journal. This helps with crash recovey. 

```
# show file system utilization
df
# calculate disk usage of directories and file systems
du
# displays block device attributes
blkid
# list all mounted file systems in tree form
findmnt
# determine UUID of file system 
xfs_admin -u /dev/vda1
grep boot /etc/fstab
# determine the label set 
xfs_admin -l /dev/vda1
```

Label a file system

```bash
# xfs_admin -l /dev/sda1
# umount /boot
# xfs_admin -L bootfs /dev/sda1
# mount /boot
```


#### Mounting a File System 

* Mounting a partition makes its storage available starting at the specified directory

```
mkfs.xfs /dev/sdb1
fdisk -l /dev/sdb1
mount /dev/sdb1 /data
# update /etc/fstab file 
```