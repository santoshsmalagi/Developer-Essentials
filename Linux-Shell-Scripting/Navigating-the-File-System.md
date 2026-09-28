# Navigating the File System

### Print Working Directory (pwd) 
The ```pwd``` command prints the full path, or location on the filesystem, to the current working directory

```console
santoshsm@santoshsmalagi:~$ pwd
/home/santoshsm
```
Upon login to the system, by default the current working directory is set to the user's home directory.

### Change directory (cd)

```console
santoshsm@santoshsmalagi:~$ cd Documents
santoshsm@santoshsmalagi:~$ pwd
/home/santoshsm/Documents
```

```Shell
cd ~             # change to the home directory
cd /             # change to the root directory
cd ..            # move up one directory level
cd .             # will leave you in same directory
cd               # go to the username's home directory
cd -             # go to the previous directory
cd <subdir>      # change to the subdir within the current working directory
cd ~username     # change to username's home directory
```

### List the contents of the directory (ls)
```console
santoshsm@santoshsmalagi:~$ ls
Desktop   Documents  Downloads  Music  Pictures  Public  Templates  Vidoes
```

### Absolute and relative paths
