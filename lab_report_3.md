# Lab Report 3 - Researching Commands (Week 5)

In this [lab](https://ucsd-cse15l-s23.github.io/week/week5/#week5-lab-report), we were tasked with researching a command and finding 4 interesting command-line options or alternate ways to use them. In this lab report, I will be researching the `find` command. For each of the options, I will be providing 2 examples of using it on files and directories from `./technical`.

## 1) Type

Put simply, the `-type` option is used to search for all files of a certain type. 
According to the [Linux manual](https://man7.org/linux/man-pages/man1/find.1.html), which can also be accessed using the `man` command in the terminal, the `-type` option finds a file of type `c`, where `c` is replaced with:

```
-type c
      File is of type c:

      b      block (buffered) special

      c      character (unbuffered) special

      d      directory

      p      named pipe (FIFO)

      f      regular file

      l      symbolic link; this is never true if the -L option
             or the -follow option is in effect, unless the
             symbolic link is broken.  If you want to search for
             symbolic links when -L is in effect, use -xtype.

      s      socket

      D      door (Solaris)
```

In the `technical` directory, I used this option on the `911report` directory. First, I wanted to find all `.txt` files in this directory. This is what was returned:

```
[cs15lsp23bn@ieng6-201]:technical:146$ find ./911report -type f -name "*.txt"
./911report/chapter-1.txt
./911report/chapter-10.txt
./911report/chapter-11.txt
./911report/chapter-12.txt
./911report/chapter-13.1.txt
./911report/chapter-13.2.txt
./911report/chapter-13.3.txt
./911report/chapter-13.4.txt
./911report/chapter-13.5.txt
./911report/chapter-2.txt
./911report/chapter-3.txt
./911report/chapter-5.txt
./911report/chapter-6.txt
./911report/chapter-7.txt
./911report/chapter-8.txt
./911report/chapter-9.txt
./911report/preface.txt
```
We can also use the `-type` option to find directories. For example, I wanted to find all of the directories that were inside the `technical` directory. This is what was returned:

```
[cs15lsp23bn@ieng6-201]:technical:148$ find . -type d
.
./911report
./biomed
./government
./government/About_LSC
./government/Alcohol_Problems
./government/Env_Prot_Agen
./government/Gen_Account_Office
./government/Media
./government/Post_Rate_Comm
./plos
```
Not only did it show me the directories directly contained inside  `technical`, but it also showed me the directories inside those directories! This is useful to find a directory with less effort than doing the `ls`  and `cd` commands several times.

Sources used:
* ChatGPT: I used ChatGPT to get a simple explanation of this command option.
* [Linux Manual Page](https://man7.org/linux/man-pages/man1/find.1.html): I used the Linux manual to learn about what specific files could be found using the `-type` option.


## 2) Size

The next command option I stumbled upon in my research was the `-size` option. Put simply, the `-size` option is used to search for files of a specific size in a specific directory. For example, you can find files that are larger than 1 megabyte by using the option `-size +1M`. The + or - symbol are used to signify greater than or less than a given value. There are also more suffixes that can be used. According to the [Linux Manual Page](https://man7.org/linux/man-pages/man1/find.1.html), you can use the following suffixes:

```
    `b'    for 512-byte blocks (this is the default if no
           suffix is used)

    `c'    for bytes

    `w'    for two-byte words

    `k'    for kibibytes (KiB, units of 1024 bytes)

    `M'    for mebibytes (MiB, units of 1024 * 1024 = 1048576
           bytes)

    `G'    for gibibytes (GiB, units of 1024 * 1024 * 1024 =
           1073741824 bytes)

```

In the `technical` directory, I used this command option on the `biomed` directory. First, I wanted to find all files in this directory that were less than 20 kibibytes (1024 bytes). This is what was returned:

```
[cs15lsp23bn@ieng6-201]:technical:156$ find ./biomed -size -20k
./biomed/1468-6708-3-3.txt
./biomed/1468-6708-3-7.txt
./biomed/1471-2091-2-16.txt
./biomed/1471-2105-3-24.txt
          .
          .
          .
./biomed/cc713.txt
./biomed/cc973.txt
./biomed/cvm-2-6-286.txt
./biomed/gb-2003-4-3-r18.txt
./biomed/rr73.txt
```
**Note**: The vertical ellipses were not actually printed, and were only added for the purposes of saving space on the page.

The `-size` option can also be used to find directories of a specific size! For example, I wanted to check which directories inside of `technical` were greater than 1 kibibyte. This is what was returned:

```
[cs15lsp23bn@ieng6-201]:technical:159$ find . -type d -size +1k
.
./911report
./biomed
./government
./government/About_LSC
./government/Alcohol_Problems
./government/Env_Prot_Agen
./government/Gen_Account_Office
./government/Media
./government/Post_Rate_Comm
./plos
```

It's worth mentioning that I used more than one command option here. To limit the files to only directories, I used the `-type d` option that I just covered before this! Multiple options can be used on the same line to get more specific information.

Sources used:
* ChatGPT: I used ChatGPT to get a simple explanation of this command option.
* [Linux Manual Page](https://man7.org/linux/man-pages/man1/find.1.html): I used the Linux manual to learn about what units the `-size` command is able to take to find a file of a specific size.


## 3) User

The third command option I decided to research is the `-user` option. This command is used to search for files owned by a specific user. This option is relatively simple, as the [Linux Manual Page](https://man7.org/linux/man-pages/man1/find.1.html) does not provide an extensive description of the option. This is the description provided:

```
-user uname
    File is owned by user uname (numeric user ID allowed).
```

In the `technical` directory, I used this command option on the `government` directory. I wanted to check if I was the owner of these files, so I followed the option with my CSE 15L account name. This is what was returned:

```
[cs15lsp23bn@ieng6-201]:technical:160$ find ./government -user cs15lsp23bn@ieng6-201
find: 'cs15lsp23bn@ieng6-201' is not the name of a known user
```

Wait... I don't know my username! Thankfully this is not a big issue, since there is another command to identify who owns the file. This command is `ls -l <filename>`. I used this command in the `technical` directory, and this is what was returned:

```
[cs15lsp23bn@ieng6-201]:technical:162$ ls -l 
total 96
drwxr-s--- 2 cs15lsp23bn ieng6_cs15lsp23  4096 May  2 12:55 911report
drwxr-s--- 2 cs15lsp23bn ieng6_cs15lsp23 65536 May  2 12:55 biomed
drwxr-s--- 8 cs15lsp23bn ieng6_cs15lsp23  4096 May  2 12:55 government
drwxr-s--- 2 cs15lsp23bn ieng6_cs15lsp23 20480 May  2 12:55 plos
```
According to user Lee Stanton on [Alphr](https://www.alphr.com/check-who-owns-a-file-linux/), the third column is the user that owns the file that was searched for, in my case the directories inside of `technical`.

With my username now figured out, I tried it again and this is what was returned:

```
[cs15lsp23bn@ieng6-201]:technical:163$ find ./government -user cs15lsp23bn          
./government
./government/About_LSC
./government/About_LSC/CONFIG_STANDARDS.txt
./government/About_LSC/Comments_on_semiannual.txt
./government/About_LSC/LegalServCorp_v_VelazquezDissent.txt
            .
            .
            .
./government/Post_Rate_Comm/Mitchell_spyros-first-class.txt
./government/Post_Rate_Comm/Redacted_Study.txt
./government/Post_Rate_Comm/ReportToCongress2002WEB.txt
./government/Post_Rate_Comm/WolakSpeech_usps.txt
```
**Note**: The vertical ellipses were not actually printed, and were only added for the purposes of saving space on the page.

The `-user` command option can also be used on directories. For the purposes of this example, let's say I wanted to find out who owned the directories in the `technical` directly. This is what would be returned in the terminal after I enter the command option:

```
[cs15lsp23bn@ieng6-201]:technical:165$ find . -type d -user cs15lsp23bn
.
./911report
./biomed
./government
./government/About_LSC
./government/Alcohol_Problems
./government/Env_Prot_Agen
./government/Gen_Account_Office
./government/Media
./government/Post_Rate_Comm
./plos
```

Again, notice that I restricted the type of files to be only directories. If I had not put that, it would return every single file in `technical`! This command option does not have any particularly interesting functionality in the `technical` directly, since all the files are owned by my CSE 15L account. However, `-user` can have some more important uses if you were to be working in a shared directory, or a directory that is not yours!

Sources used:
* ChatGPT: I used ChatGPT to get a simple explanation of this command option.
* [Linux Manual Page](https://man7.org/linux/man-pages/man1/find.1.html): I used the Linux manual to get an official definition of the `-user` command option.
* [Alphr](https://www.alphr.com/check-who-owns-a-file-linux/): I used Alphr to learn how to find the owner of a file or directory using the `ls -l` command.


## 4) Print

The last command option I decided to research is the `-print` option. The `-print` option is used to print the names of the files found. This option is also relatively simple for most uses, but there are some uses that I will not be covering for the purposes of this assignment. This is what the [Linux Manual Page](https://man7.org/linux/man-pages/man1/find.1.html) has documented about the function of this option:

```
-print 
    True; print the full file name on the standard output,
    followed by a newline.
```

In the `technical` directory, I used this command on the `plos` directory. I wanted to find out what would happen if I just used the `-print` option on it's own. This is what was returned:

```
[cs15lsp23bn@ieng6-201]:technical:166$ find ./plos -print
./plos
./plos/journal.pbio.0020001.txt
./plos/journal.pbio.0020010.txt
./plos/journal.pbio.0020012.txt
./plos/journal.pbio.0020013.txt
            .
            .
            .
./plos/pmed.0020274.txt
./plos/pmed.0020275.txt
./plos/pmed.0020278.txt
./plos/pmed.0020281.txt
```
**Note**: The vertical ellipses were not actually printed, and were only added for the purposes of saving space on the page.

Initially, it seemed like this command option... wasn't very interesting. Surely there is a more interesting and specific use for `-print`, right?

Well, according to ChatGPT, there is! In one instance, you can choose to print files with specific names. This is what it would look like on the command line:

```
find . -type f -name "*.txt" -print
```

In another instance, you can combine `-print` with other `find` command options using the pipe symbol, `|`. The example that ChatGPT gave was a command that finds all `.txt` files in a directory and pipes their name to the `xargs` command, which runs the `cat` command on each file found. This is what the command line would like:

```
find . -type f -name "*.txt" -print | xargs cat
```

I wanted to try using the pipe example given by ChatGPT on the `plos` directory, and this is what it looked like:

```
                                                  .
                                                  .
                                                  .
        Dementia remains an incurable condition and its increasing prevalence is a deeply
        worrying aspect of the “graying” of the population. An important question for researchers
        is to establish whether dementia incidence, prevalence, and national history vary from one
        location to another. Incidence studies are particularly valuable for less biased comparison
        of disease occurrence, as well as being essential for policy makers. Many biases can,
        however, be introduced in such studies. Dropout and mortality are particular reasons for
        concern.
        Most of the numerous studies of dementia incidence have been restricted to single sites.
        Authors have frequently attempted to assess whether rates in a given study are similar to
        those obtained elsewhere. However, variations between studies in the methodology employed
        make such comparisons unreliable. Where within-country variations in incidence have been
        noted, as has happened in the US, they have often been ascribed to methodological
        differences, but one cannot be certain whether this is the case.
        Risk factors for other chronic disorders common in old age (notably cardiovascular
        disease and cancers) do vary in their prevalence between and within countries. In the UK,
        for example, the incidence of stroke is...
                                                  .
                                                  .
                                                  .
        A whistleblower's success depends upon competent and articulate media. The debate to
        improve the status quo—be it in pharmaceutical marketing or managed-care decision
        making—cannot proceed or flourish without it.
        Ralph Waldo Emerson, American essayist and philosopher (1803–1882), commented about
        success (I have adapted his comments for all of us who gathered in Washington in mid-May
        2005): “To leave the world a bit better, whether by a healthy child, a garden patch or a
        redeemed social condition; to know even one life breathed easier because you have lived;
        this is to have succeeded [as a whistleblower].”
      
```
**Note**: The vertical ellipses were not actually printed, and were only added for the purposes of saving space on the page.

This command line argument that used the `-print` option combined *all* of the `.txt.` files in the `plos` directory! The terminal was completely flooded with words, so I trimmed it down for viewing purposes. This is definitely a more interesting use case for `-print`, in the sense that you can combine the contents of files easier. Someone might use this if they wanted to combine the contents of a file and store those results in a new file using other command line arguments.

Sources used:
* ChatGPT: I used ChatGPT to get a simple explanation of this command option. I also used it to find more interesting use cases for `-print` that were not explicitly listed in the manual page.
* [Linux Manual Page](https://man7.org/linux/man-pages/man1/find.1.html): I used the Linux manual to get an official definition of the `-print` command option.

---

Thank you for reading! :)
