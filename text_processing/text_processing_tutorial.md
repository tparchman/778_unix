# I. Basic process monitoring and control

## 1. Process monitoring with `top`, killing jobs
`top` will display information on processes running on the machine you are logged into. Try it, read the output carefully. Doesn't matter what directory you call it from.

    $ top

`ps` will show your active process ids. Try it.

    $ ps

`ps aux` will show all active processes. Here `a`, `u`, and `x` are separate command line arguments, see `man ps` for details.

    $ ps aux

`ps aux | grep [search expression]` will pipe processes listed as above into `grep`, and can be used to locate PIDs for specific applications. For example, the below command should retrieve information on processes running associated with TextWrangler.

    $ ps aux | grep TextWrangler

If you have mutliple processes running, and want to kill one, use `kill` followed by the process ID, which you can locate with `top` or `ps`, e.g.:

    $ kill 90312

If you have a job running in the shell that isnt doing what you want, you can easily kill from the terminal with `ctrl c`. You can also temporarily kill it with `ctrl z`, and then restart it in the background with `bg` typed at the prompt with no additional arguments necessary.

If you are calling a command that is going to take some time, and you dont want it to occupy the shell you are working in, you can send it to the background with `&`. Once a job is running in the background, the job will continue once you close the terminal session or exit your connection to a remote server

    $ cat *fastq > allgenomefiles.fastq &

## 2. Practice running a program, running it in the background, and stopping it (in other words, job control)

`jot` is a little program that can generate strings of numbers, among other things (have a look at the `jot` `man` page). Try the following command which will print 100 random numbers:

    $ jot -r 100

You will notice that this command will print 100 numbers to the screen. 

Now, increase the number of random numbers until you get to a number of replicates (think millions or more) that takes your computer an appreciable amount of time to complete. If you made the number large enough, you’ve probably noticed that you can’t do anything else with your terminal window while it’s busy working on your command. Use ^c to stop it printing to that file, and then execute the commands again with an “&” at the end:

    $ jot -r 1000000000 > test.txt &

The ampersand (&) will cause the job to run in the background. Thus, you will have the normal prompt back in your terminal window, and closing it will not affect the job.  

You can use `top` or `ps` to identify the process number of the `jot` job in oder to kill it. If you use `top`, you can find the job in the list. The `ps` command will return a list of job ids running by the user:

    $ ps

After you identify the job id (e.g., 77654), you can stop it:

    $ kill 77564

# I. Unix text processing 

## Topics to cover

- compression, decompression
- redirection (>,>>) and pipes (|)
- quick tricks: `cut`, `sort`, `uniq`
- pattern matching and extraction: `grep`
- files needed: sample_passerina.fastq.gz from [unix_text_processing github page](https://github.com/tparchman/778_unix/tree/master/text_processing)

## 1. Compression and decompression using gzip and gunzip

Compression and de-compression are regular activities associated with large text data files, so get comfortable with it. `gzip` is a command for compressing and decompressing files. Download sample_passerina.fastq.gz from [unix_text_processing github page](https://github.com/tparchman/778_unix/tree/master/text_processing), and put it in a directory you can easily work in for the below examples.

The compressed .gz file can be easily decompressed.

    $ gunzip sample_passerina.fastq.gz

The below command will create the compressed file "sample_passerina.fastq".

    $ gzip sample_passerina.fastq

All .txt files in a directory can be compressed (or decompressed) using a wildcard, `*` with this command. Note the below command would compress, one by one, all files ending in .txt. 

    $ gzip *.txt .

`*` will make your life easier, and you will learn to use it in many contexts. Here is another simple example for now, which would copy all of the files starting with BS_1287 and ending in fastq.gz to a specified directory

    $ cp BS_1287*fastq.gz data_for_BS1287/

## 2. stdout, redirection (`>`), and pipes (`|`)

**stdout** (standard out) is normally printed to screen when Unix commands are executed. The `cat` command below will print the entire contents of passerina.fastq to screen.

    $ cat passerina.fastq

That doesn't seem very useful in most cases. Instead, we can redirect the output of any Unix command to a file simply by using redirection. Here are some simple examples.

Redirection of `ls -lh` below simply sends all that information on files in the directory to a text file.

    $ ls -lh > directory_contents.txt

The below command will concatenate the data from all files in a directory ending in data.txt into one file.

    $ cat *data.txt > all_data_in_directory.txt

The use of `>>` below will write (append) the contents of newdata.txt to the end of all_data_in_directory.

    $ cat newdata.txt >> all_data_in_directory.txt




## 3. Extracting fields and sorting (`cut`, `sort`, `uniq`)

Use examples from Haddock and Dunn Chapter 16

## 4. Regular expressions and text processing with `grep`

`grep` is a regular expression workhorse, and is among the most widely used Unix commands in data science. You can explore the examples below using sample_passerina.fastq, available under the [unix text processing github page](https://github.com/tparchman/778_unix/tree/master/text_processing). This is an increbily versatile command, so we better learn more. In it simplest invocation, `grep` with output every line in a file that matches the specified pattern.

Since fastq files have a standard four line format (ID starting with @, DNA sequence, quality id starting with +, and quality score), we know that every sequence has a line starting with @ associated with it. 

We could write all of the ID lines to a separate file:

    $ grep "^@" ample_passerina.fastq > idlines.txt

We can count the number of sequences in the file:

    $ grep "^@" -c sample_passerina.fastq

We can print the line with a match, plus any number of lines following it:

    $ grep "^@" -A 1 sample_passerina.fastq

SDN_AM_43432 is the ID of a specific bird represented in this data set. How many DNA sequences do we have for this bird?

    $ grep "SDN_AM_43432" -c sample_passerina.fastq

Have a look at the `man` page for `grep`, `tr`, and `wc`, and try some of thse additional commands that make use of pipes and redirection

In addition, you will want to learn what the `tr` command does.

    $ grep ^@ sample_passerina.fastq | wc -l   

    $ grep ^@ sample_passerina.fastq | grep “NVP_CY_48147” > NVP_CY.txt

    $ grep ^[ATCG] sample_passerina.fastq | wc –l

    $ grep ^[ATCG] sample_passerina.fastq | tr ‘T’ ‘U’ | less

    $ grep ^[ATCG] sample_passerina.fastq | tr ‘T’ ‘U’ | head –n 20 > first20seqs_transliterated.txt


# III. Permissions, package installation

## 1. Permissions

As Unix systems are  multi-user, the control of permissions on directories and files is critical for security, privacy, and collaboration. Typing `ls -l` (or `ll` depending on how you modified your bash_profile) will show you the permissions associated with files in your current directory.

    $ ll -h

    -rw-rw-r--.  1 parchman parchman  51G Jul 25  2017 J1b.clean.fastq
    -rw-rw-r--.  1 parchman parchman  51G Jul 22  2017 J1.clean.fastq
    -rw-rw-r--.  1 parchman parchman  44G Jul 22  2017 J2a.clean.fastq
    -rw-rw-r--.  1 parchman parchman  44G Jul 22  2017 J2b.clean.fastq
  

 A glance at this output illustrates how permissions are displayed. 3 permissions are defined for each owner level (`user`, 'owner'; `g`, 'user group'; `o` 'other'), only the owner of a file or directory can change the permissions therein. The first position from the output above specifies file type, the following 3 have permissions for 'user', then three for 'group', then 3 for 'other'. Permission is indicated by the letters below, a lack of permission with a `-`.
- Read (`r`): Permission to open and read a file, for a directory allows the listing of content.

- Write (`w`) Permission to modify the contents of a file, or for a directory, to add, remove and rename files.

- Execute (`x`): Permission to run an executable program.


Here are some examples of symbolic notation:

- `-rwxr--r--`: 'User' has read/write/execute permission, 'group' and 'other' have only read permissions.
- `drw-rw-r--`: A directory where 'user' and 'group' have read and write permissions, while 'other' has only read.
- `-rwxr-xr-x`: A file, 'user' has read/write/execute, 'group' has read/execute, and 'other' has read/execute.

The `chmod` command is used to alter permissions. The command can be controlled by either numeric or symbolic codes, the latter are illustrated below. You can find useful guides to the numeric system [here](https://gist.github.com/juanarbol/c44e736be70279c1fd5d68aa24f9d8be).
<p>&nbsp;</p>

| Operator | Action/level |
|----------|--------|
|  +        | user/owner     |
|  -       | Remove  |
|  =       | sets permission|
|  r        | read    |
|  w       | write  |
|  x       | execute|  

<p>&nbsp;</p>

  
| Abbreviation | User level |
|----------|--------|
| u      | User     |
| g       | Group  |
| o       | Other|
| a       | All|
<p>&nbsp;</p>

We are going to try to avoid messing with permissions  in this course, but if you go on to use remote or cluster computing systems with other groups, understanding permission in more detail is essential. There are, however, a few simple things we will do, and below are some examples. 

Convert the .sh shell script to an executable for all users.

    $ chmod a+x rsync_mirror_laptop.sh

Convert the .sh shell script to write for all users.

    $ chmod a+w rsync_mirror_laptop.sh

This script can then be executed from the currently directory:

    $ ./rsync_mirror_laptop.sh

## 1. Permissions
