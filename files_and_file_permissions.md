## Files and File Permissions

#### /proc

`/proc` maintains the information about the current state of the Kernel. The contents are created in memory at boot time and a number of system utilities such as `top, ps etc.` are actually referencing these files. 

```
# see the output of the files from /proc
cat /proc/cpuinfo
cat /proc/meminfo
```

#### Paths

```
# see the type of file
file .bash_profile
# sybmolic link are shortcuts to another file or directory
ll /usr/sbin/vigrlrwxrwxrwx. 1 root root 4 Aug  8 12:39 /usr/sbin/vigr -> vipw
```

#### Create Files and Directories

```
# create a file using touch
touch newfile
# create a file using cat, add text and Cltr+d
cat > newfile
# create using vi
vi newfile
# create a directory 
mkdir newdir
```

#### Listing Files and Directories

The ll command list files and dictories

```
-rw-rw-r--. 1 guser guser 5 Aug 28 12:19 newfile
```

| Column | Description |
| ---    |  ---         |
| -rw-rw-r--. | The first character tells the file type and the next 9 show permissions |
| 1 |  Number of links |
| guser | Owner name | 
| guser | Owner group name | 
|  5 | File size in bytes |
| 5 Aug 28 12:19 | Date n' that |
| newfile | file name | 

#### Displaying Contents of Files

```
# display contents using cat
cat .bash_profile
# use more to and less to view long text fiels
more /etc/profile
less /etc/profile
# see the first 100 lines of a file
head -100 /etc/profile
# view a log file while it's being updated
tail -f /var/log/messages
```

#### Copying and moving files

```
# copy file1 to file2 in the same directory
cp file1 file2
# copy a directory using the -r flag
cp -r dir1 dir2
# move a file. The -i option requires user confirmation
mv -i file1 file2
# remove file
rm -i file
# remove directory
rm -r file
```

#### Control Attributes

```
# see the current attributes for a file
lsattr file1
# update attribues of a file
chattr +a file1
```

#### Finding Files

The find command is very powerful as it can recursively search the directory tree and perform actions on any files find if required. 

```
# find a file in the home directory
find . -name -filetofind -print
# find files large than 100MB in /proc directory
find /proc -size +100M
# search for all the files in the system with sticky bit
find / -type d -perm -1000
```

#### Linking Files/Directories

Each file has associated metada which is callede the file's *inode*. The inode is assigned a unique number that is used by the kernel for managing the file. 

A soft link or symlink allows one file to be associated with another.

Hard links associate files with a single inode number making all files undistinguishable.

```
# create a soft link for the newfile in the home directory
cd ~
ln -s newfile newfilelinked
# soft links can be seen in the / directory
ll /
--> lrwxrwxrwx.  1 root root    7 Aug  8 12:38 bin -> usr/bin
# create a hard link
ln newfile2 newfile3
```

The `ll` command shows the following output `lrwxrwxrwx`

| Characters | Description |
| ---    |  ---         |
| l | The first character tells the file type which in this case is a symbolic link |
| rwx | First three characters show the permissions for the owner  |
| rwx | Shows the permissions for the owner's group | 
| rwx | Shows permissions for public | 

#### Permissions

Permission classes are user, group and public. Permission types are read, write and execute. Permission modes are add, revoke and assign.

| Permission Class | Description |
| ---    |  ---         |
| User (u) | The owner of the file or directory |
| Group (g) | A set of users that have identical access on files and directories that they share |
| Others (o) | All others users - public | 

`chmod` can modify permissions in either symbolic or octal.

```
# add execute permission for the owner
chmod u+x newfile -v
# add write permission for group members and public
chmod go+w newfile
# remove write permissions for the public
chmod o-w newilfe
# assign rwx for all u,g,o using octal notation
chmod 777 newfile
```

Linux assigns default permissions to a file/directory at the time of its creation. These are calculated from the umask permission value substracted from a preset value called initial permissions. 

```
# see the umask value
umask
# change ownership of file to newuser
chown newuser newfile
# change group ownership
chgrp newgroup newfile -v
# assign ownership and owning group at the same time
chown newuser:newgroup newfile
```

#### Special Permissions

setuid is set on executable files at the owner level. With this bit set, the file is executed by other regular users with the same priviledges at that of the owner. setgid is the same concept but at the group level.

```
ll /usr/bin/su
--> -rwsr-xr-x. 1 root root 32208 Mar 14 10:37 /usr/bin/su
```

The sticky bit is set on public writable directories. This protects file and sub-directories being deleted by other regular users. 

```
ll -d /tmp
# The bolded t in
drwxrwxrwt. 8 root root 172 Aug 28 08:18 /tmp
# set the sticky bit
chmod 1755 /var -v
# unset
chmod 755 /var
```