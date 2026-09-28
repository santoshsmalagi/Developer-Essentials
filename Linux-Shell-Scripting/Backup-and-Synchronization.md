# Synchronizing Files and Directories (rsync)

```rsync``` can synchronize both remote and local directories using the *rsync remote update* protocol. ```rsync``` detects the differences between the source and the destination and 
performs only a minimum amount of copying required to bring the two paths in sync with each other, if there are no differencces then nothing is copied. 

```rsync``` offers several advantages such as:

* efficiently copy and update files, copying only what is necessary using the *remote update protocol*. First time, it copies the whole content of a file or a directory 
 from source to destination but from next time, it copies only the changed blocks and bytes to the destination
* much faster than ```scp```
* supports copying links, devices, owners, groups and permissions
* consumes less bandwidth as it uses compression and decompression of data

```console
rsync options source destination
```
*source* or *destination* are one of the following:
* a local file or a directory
* remote file or directory in the form of ```user@host:path```
* remote rsync server specified using a url of form ```rsync://[user@]host[:port]/path```

Common ```rsync``` options:

```
    -v : verbose
    -r : copies data recursively (but don’t preserve timestamps and permission while transferring data
    -a : archive mode, archive mode allows copying files recursively and it also preserves symbolic links, file permissions,   
         user & group ownerships and timestamps
    -z : compress file data
    -h : human-readable, output numbers in a human-readable format
```

**Either the source or the destination must be a local file, remote to remote copying not supported.**

```console
rsync PrintUtils.C BAK/src/   # backup a single file
rsync -avzh src BAK/src       # copy/sync a directory

# copy multiple directories to a storage device, --delete deletes files on destination path which no longer exist at the source
rsync -av --delete /home /usr/local /media/USB/BAK/  

# rsync remote directory with local directory
rsync -av src malagi@remote-desktop:/home/PckgMgmt/src

# rsync local directory with remote
rsync malagi@remote-desktop:/home/PckgMgmt/share CoreUtils/share

# rsync using SSH
rsync -avz --rsh=ssh malagi@remote-desktop:/home/PckgMgmt/data /tmp

# rsync with a rsync server
rsync -av -delete rsync://rsync.github.com/santoshsmalagi/Linux-and-Shell-Scripting Linux-and-Shell-Scripting
```
