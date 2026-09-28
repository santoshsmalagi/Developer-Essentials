# Archiving and Compression in Linux

> *Archiving : process of gathering multiple files/directories, and packaging them into a bundle known as a tar file* <br>
  *Compression : process of removing redundancy from data, with an objective of storing it in the smallest possible space* <br>
  *Decompression : process of decompressing compressed files* <br>
  *Synchronozation : detect differences between two remote directories and only perform minimum amount of copying required to bring them in sync with each other* <br>
  *Encryption : encoding files in such a way that only those who are authorized can access it (e.g. GnuPG, bcrypt, ccrypt etc.)* <br>
  *Decryption : decoding encrypted files* <br> <br>
  **Compressing a Compressed file doesn't yield much returns** <br>
  Attempting to compress a file which has already been compressed (using some form of compression algorithm - e.g. .jpg, .mp3 etc.) will actually result in a larger file!! This is
  because all compression techniques involve some overhead which is added to the file to describe the compression. Trying to compress a file, that already contains no redundant
  information will not result in any savings to offset the additional overhead.

Linux provides the following utilities:
* Archiving
  * ```tar``` - *tape archive*, classic utility in Linux for packaging files and directories into archives
* Compression
  * ```gzip``` - compress or expand files
    * ```gunzip``` - expand compressed files
    * ```zcat``` - view the contents of a compressed file without actually decompressing it
  * ```bzip2``` - similar to gzip program, uses a different compression algorithm providing higher levels of compression but at slower speeds
    * ```bunzip2``` - uncompress files compressed with bzip2
    * ```bzip2recover``` - to recover damaged .bz2 files
    * ```bzcat``` - to cat the output of bzip2 compressed files
  * ```zip``` - compress and archive files
    * ```unzip``` - extract the contents of a zip file
  * Others - ```xz```, ```lzma```, ```lza```, ```lz4```, ```brotli```, ```zopfli```, ```zstd```,  ```zpac```, ```pax```, ```peazip```, ```7zip```, ```shar```, ```cpio```, ```ar```, ```kgb``` etc.

### Compressing and decompressing (gzip, bzip2, zip)

#### gzip (compress files)
The ```gzip``` program is used to compress one or more files, it replaces the original file with a compressed version of the original. The corresponding
```gunzip``` program is used to restore compressed files to their original, uncompressed form. The ```gunzip``` program, which uncompresses gzip files, assumes that filenames
end in the extension .gz, so it’s not necessary to specify it, as long as the specified name is not in conflict with an existing uncompressed file.

```console
gzip file.txt                           # compress a file using gzip. Adds the extension .gz to the filename
gzip file1.txt file2.txt file3.txt      # compress multiple files using gzip
gzip -c file.txt > file.txt.gz          # compress the file, but preserve the original file as well
gzip -9 file.txt                        # specify a compression level
gzip -d file.txt.gz                     # specifying the -d option causes gzip to act like gunzip
gunzip file.txt.gz                      # uncompress a gzipped file
gunzip file.txt                         # uncompress a gzipped file, may not be required to add the .gz extension
gunzip -c file.txt.gz                   # only view the contents but dont decompress the file
zcat file.txt.gz | less                 # zcat program can also be used to view contents of a compressed 
                                          file without actually decompressing it                      
ls -l /etc | gzip > etc-list.gz         # gzip can also accept STDIN or write to STDOUT
```

Common ```gzip``` options:

| Option  | Description                                                                                                                                                                                                                           |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -c      | Write output to standard output and keep original files. May also be specified with --stdout and --to-stdout.                                                                                                                         |
| -d      | Decompress. This causes gzip to act like gunzip. May also be specified with --decompress or --uncompress.                                                                                                                             |
| -f      | Force compression even if a compressed version of the original file already exists. May also be specified with --force.                                                                                                               |
| -h      | Display usage information. May also be specified with --help.                                                                                                                                                                         |
| -l      | List compression statistics for each file compressed. May also be specified with --list.                                                                                                                                              |
| -r      | If one or more arguments on the command line are directories, recursively compress files contained within them. May also be specified with --recursive.                                                                               |
| -t      | Test the integrity of a compressed file. May also be specified with --test.                                                                                                                                                           |
| -v      | Display verbose messages while compressing. May also be specified with --verbose.                                                                                                                                                     |
| -number | Set amount of compression. number is an integer in the range of 1 (fastest, least compression) to 9 (slowest, most compression). The values 1 and 9 may also be expressed as --fast and --best, respectively. The default value is 6. |


#### bzip2 (higher compression at cost of speed)
```bzip2``` program utilizes the Burrows–Wheeler algorithm for compression, providing higher levels of compression but at additional compression-decompression times. Files compressed using ```bzip2``` are denoted with *.bz2* or *.bz* extensions.

```console
bzip2 file.txt                           # compress a file using bzip2 utility. Adds the extension .bz2 to the filename.
bzip2 file1.txt file2.txt file3.txt      # compress multiple files
bzip2 -c file.txt > file.txt.bz          # compress the file, but preserve the original file as well
bzip2 -v -9 file.txt                     # specify a compression level
bzip2 -tv file.bz2                       # test for the integrity
bunzip2 file.txt.bz2                     # uncompress a bz2 file
bzcat file.txt.bz2                       # view the contents of bzip2 file without decompressing it
```

#### zip (package and compress)
The ```zip``` program is both a compression tool and an archiver. Linux users mostly use the ```zip``` program as a means to exchange data with Windows based systems, as *zip* files can be natively read on Windows systems. ```zip``` permits the use of several compression algorithms though DEFLATE is the most common.

```console
zip options zipfile file1 file2....
```
Unlike ```tar``` + ```gzip```, ```zip``` compresses each file individually, and leaves the "directory" information about the constituent files un-compressed. This makes it easy to work with individual files in the archive (insert, delete, decompress, etc.) but also means that it usually won't get as good of compression overall. One more interesting difference (vs tar) is that, if an existing archive is specified then new files are added and matching files are replaced, but the archive itself is preserved.

```console
zip -r data.zip data                     # if -r option is not used then the contents under data directory 
                                           are not added recursively to the zip file. 
                                           numeric value indicates the amount of compression achieved.
zip -r -Z bzip2 data.zip data            # to specify a compression technique
zip -9 -r data.zip data                  # to specify a level of compression
zip -e  data.zip data                    # password protect a zip file
ls -l /etc/ | zip ls-etc.zip -           # compress output from other programs
unzip data.zip                           # extract the contents of a zip file
unzip -l data.zip data/share/DFWStats/   # -l option causes the files to be only listed and not extracted 
unzip data.zip data/share/DFWStats       # selectively unzip data
unzip -p ls-etc.zip | less               # output to STDOUT when -p flag is used
```

### Archiving (tar)
Archiving is the process of bundling multiple files/directorues into a single large file. In Linux based systems ```tar``` is the standard utility for archiving files. 

```console
tar [mode] [options] [archive-file] [pathname]
```

| Mode |                       Description                       |
|:----:|:-------------------------------------------------------:|
| -c   | creates an archive from a list of files and directories |
| -x   | extracts an archive                                     |
| -f   | creates an archive with given filename                  |
| -t   | displays or lists files in an archive                   |
| -u   | archives and adds to an existing archive                |
| -v   | displays verbose information                            |
| -A   | concatenates the archive file                           |
| -z   | compresses the tar file using gzip                      |
| -j   | compress the tar file using bzip2                       |
| -W   | verifies an archive file                                |
| -r   | updates or adds files in an already existing tar file   |

```console
tar cf data.tar data                     # create an archive containing the entire data directory hierarchy
tar tvf data.tar                         # list the contents of the tar file, verbose option enabled
tar xvf data.tar /home/malagi/data       # extract the archive
tar cvzf archive.tar file1 file2 file3   # create a compressed archive using gzip
tar xvzf archive.tar.gz                  # extract files from a compressed gzip archive
tar cvjf etc.tar.bz2 /etc/               # compress a compressed archive using bzip2
tar xvjf archive.tar.bz2                 # extract files from a compressed gzip archive
```

* *Unless you are operating as the superuser, files and directories extracted from archives take on the ownership of the user performing the restoration, rather than the original owner. Another interesting behavior of tar is the way it handles pathnames in archives.* 
* *The default for pathnames is relative, rather than absolute.*
* *You can give tar options in the standard unix manner, where you need the dash to differentiate between options and parameters or the file names at the end of the command line. Or you can put all the options together in the first argument, in which case the dash is optional. All of these are valid:*
   * ```tar -x -v -z -f archive.tar.gz```
   * ```tar -xvzf archive.tar.gz```
   * ```tar xvzf archive.tar.gz```
   * ```tar --extract --verbose --gzip --file archive.tar.gz```

### File extentions for archives and compressed files
```tar``` itself doesn't feature native data compression, so tar archives are compressed using one of the compression utilities like ```gzip```, or ```bzip2```. To identify the type of compression utility used, the resulting tar files are either named with a single extension, e.g. .tgz, .tbz, .txz, .tzst, or with double file extension, e.g. .tar.gz, .tar.br, .tar.bz2, .tar.xz, .tar.zst etc.

**Compression utilities*** ```gzip``` **and** ```bzip2``` **when used standalone (without tar) can only operate on files and not directories.**  

* *.tar* - no compression, only a package of files/sub-directories
* *.gz* - ```gunzip``` compressed file
  * *.tgz, .tar.gz* - ```tar``` + ```gunzip``` based on the DEFLATE algorithm (a combination of LZ77 and Huffman coding)
* *.bz2* - ```bzip2``` compressed file
  * *.tbz, .tar.bz2* - ```tar``` + ```bzip2``` based on the Burrows–Wheeler algorithm
* *.xz* - ```xz``` compressed file
  * *.txz, .tar.xz* - ```tar``` + ```xz``` based on the LZMA2 algorithm
* *.zip* - *zip* - permits several algorithms though DEFLATE is the most common
* *.br* - ```br``` compressed file
  * *.tbr, .tar.br* - ```tar``` + ```br``` based on the LZ77 algorithm, Huffman coding and 2nd order context modeling
* *.zst* - ```zst``` compressed file
  * *.tzst, .tar.zst* - ```tar``` + ```zstd``` based on the LZ77 algorithm, Huffman coding and FSE
  
>  *For most practical applications the combination of* ```tar``` + ```gzip``` *compression provides the most optimal space (file size) and time (compression-decompression time) performance.*

### Some Useful Links
[GZIP vs BZIP2 vs LZMA vs ZIP vs Compress](https://bashitout.com/2009/08/30/Linux-Compression-Comparison-GZIP-vs-BZIP2-vs-LZMA-vs-ZIP-vs-Compress.html) <br>
[Quick Benchmark: Gzip vs Bzip2 vs LZMA vs XZ vs LZ4 vs LZO](https://catchchallenger.first-world.info/wiki/Quick_Benchmark:_Gzip_vs_Bzip2_vs_LZMA_vs_XZ_vs_LZ4_vs_LZO) <br>
[A Quick Benchmark: Gzip vs. Bzip2 vs. LZMA](https://tukaani.org/lzma/benchmarks.html) <br>
[Compression Benchmarks](https://bbengfort.github.io/observations/2017/06/07/compression-benchmarks.html) <br>
[brotli-Google](https://github.com/google/brotli) <br>
[Zstandard-Facebook](https://facebook.github.io/zstd) <br>
