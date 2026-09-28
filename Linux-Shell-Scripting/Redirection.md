# I/O Redirection

### Displaying a line of text (echo)
```echo``` is a command that outputs the strings it is being passed as arguments

```console
santoshsm@santoshsmalagi:~$ echo "Talk is Cheap, Show Me the Code"
Talk is Cheap, Show Me the Code
```
```echo``` command is commonly used in scripts to display a line of text/string on the STDOUT. It can be combined with a redirection operator to write to a file. For example:

```console
santoshsm@santoshsmalagi:~$ echo "Talk is Cheap, Show Me the Code" > CodingPhilosophy.txt
santoshsm@santoshsmalagi:~$ cat CodingPhilosophy.txt
Talk is Cheap, Show Me the Code
```

### STDOUT, STDIN and STDERR

* **Redirecting STDOUT**
* **Redirecting STDERR**
* **Redirecting STDOUT and STDERR**
* **Disposing unwanted output**
* **Redirecting STDIN**

### Viewing file contents (less and more)
The ```less``` program allows easy viewing of long text documents in a page-by-page manner, common keyboard commands used by less are:

| Command | Action |
| --------|--------|
| ```PAGE UP or b``` | Scroll back one page |
| ```PAGE DOWN or Spacebar``` | Scroll forward one page |
| ```Up Arrow``` | Scroll up one line |
| ```Down Arrow``` | Scroll down one line. |
| ```G``` | Move to the end of the text file. |
| ```1G or g```  | Move to the beginning of the text file. |
| ```/characters``` | Search forward to the next occurrence of characters. |
| ```n``` | Search for the next occurrence of the previous search. |
| ```h``` | Display help screen. |
| ```q or Q``` | Quit less. |

<br>

> *On most systems, the less and more commands run the same program. The more command is a legacy program with limited features, but it’s still quite popular in documentation and on some minimalist operating systems due to its smaller size. For example, the more program could only page forward, the less program allows paging both forward and backward and has many other features as well. So the less is an improved version of more.*

The usefullness of the ```less``` program can be understood if we process a large text file. For example - on the Ubuntu system there is a dictionary of words located at ```/usr/share/dict/words```. The less command can be used to browse through it's contents.

```console
santoshsm@santoshsmalagi:~$ less /usr/share/dict/words
```

### Output the first and last parts of a file (head and tail)
To only view the last and the first few lines of a text file we can make use of the ```head``` and the ```tail``` commands respectively. By default the ```head``` command prints the
first 10 lines of a file, and the ```tail``` command prints the last 10 lines, For example:

```console
santoshsm@santoshsmalagi:~$ head /usr/share/dict/words
A
A's
AMD
AMD's
AOL
AOL's
Aachen
Aachen's
Aaliyah
Aaliyah's
```
The number of lines of text can be controlled by using the ```-n``` option. For example to print the first 5 lines only:
```console
santoshsm@santoshsmalagi:~$ head -n 5 /usr/share/dict/words
```

```tail``` command also has a ```-f``` option which allows us to monitor files in real time, particularly useful for viewing the output of log files, for example:

```console
santoshsm@santoshsmalagi:~$ tail -f /var/logs/messages
```

### Concatenate Files (cat)
The ```cat``` command reads one or more files and copies them to standard output:
```console
santoshsm@santoshsmalagi:~$ 
```
### Sort lines of text (sort)
```console
santoshsm@santoshsmalagi:~$ 
```
### Report or omit repeated lines (uniq)
```console
santoshsm@santoshsmalagi:~$ 
```
### Print newline, word, and byte counts for each file (wc)
```console
santoshsm@santoshsmalagi:~$ 
```

### Pipelines
### Read from STDIN and output to STDOUT and files (tee)
```console
santoshsm@santoshsmalagi:~$ 
```
