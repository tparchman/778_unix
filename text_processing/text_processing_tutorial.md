# II. Basic process monitoring and control

`top` will display information on processes running on the machine you are logged into. Try it, read the output carefully. Doesnt matter what directory you call it from.

    $ top

`ps` will show your active process ids. Try it.

    $ ps

If you have mutliple processes running, and want to kill one, use `kill` followed by the process ID, which you can locate with `top` or `ps`

Lets use a simple Unix tool, `jot`
    $ kill 9031

If you have a job running in the shell that isnt doing what you want, you can easily kill from the terminal with "ctrl c". You can also temporarily kill it with "ctrl z", and then restart it in the background with `bg` typed at the prompt with no additional arguments necessary.

If you are calling a command that is going to take some time, and you dont want it to occupy the shell you are working in, you can send it to the background with `&`. Once a job is running in the background, the job will continue once you close the terminal session or exit your connection to a remote server

    $ cat *fastq > allgenomefiles.fastq &

# JOT

# I. Unix text processing 

## Topics to cover

- compression, decompression
- redirection (>,>>) and pipes (|)
- pattern matching and extraction (`grep`)
## Compression and decompression using gzip and gunzip

Compression and de-compression are regular activities associated with large text data files, so get comfortable with it. `gzip` is a command for compressing and decompressing files. Download sample_passerina.fastq.gz from [unix_text_processing github page](https://github.com/tparchman/778_unix/tree/master/text_processing), and put it in a directory you can easily work in for the below examples.

The compressed .gz file can be easily decompressed.

    $ gunzip sample_passerina.fastq.gz

The below command will create the compressed file "sample_passerina.fastq".

    $ gzip sample_passerina.fastq

All .txt files in a directory can be compressed (or decompressed) using a wildcard, `*` with this command. Note the below command would compress, one by one, all files ending in .txt. `*` will make your life easier.

    $ gzip *.txt .

`*` will make your life easier, and you will learn to use it in many contexts. Here is another simple example for now, which would copy all of the files starting with BS_1287 and ending in fastq.gz to a specified directory

    $ cp BS_1287*fastq.gz data_for_BS1287/

## stdout, redirection (`>`), and pipes (`|`)

**stdout** (standard out) is normally printed to screen when Unix commands are executed. The `cat` command below will print the entire contents of passerina.fastq to screen.

    $ cat passerina.fastq

That doesn't seem very useful in most cases. Instead, we can redirect the output of any Unix command to a file simply by using redirection. Here are some simple examples.

The below command will concatenate the data from all files in a directory ending in data.txt into one file.

    $ cat *data.txt > all_data_in_directory.txt

The use of `>>` below will write the contents of newdata.txt to the end of all_data_in_directory.

    $ cat newdata.txt >> all_data_in_directory.txt

Redirection of `ls -lh` below simply sends all that information on files in the directory to a text file.

    $ ls -lh > directory_contents.txt

The use of `grep` below will send all lines containing a match to "BS_1287" to a new file.

    $ grep "BS_1287" data_all_inds.txt > data_for_ind_BS1287.txt


## `grep`: powerful regular expression engine
`grep`is among the most widely used Unix commands in data science.


## regular expressions and text extraction with `grep`

You had a subtle introduction to `grep` last week. You can explore the examples below using sample_passerina.fastq, available under week1 on the [course github page](https://github.com/tparchman/BIOL792). This is an increbily versatile command, so we better learn more. In it simplest invocation, `grep` with output every line in a file that matches the specified pattern.

Since fastq files have a standard four line format (ID starting with @, DNA sequence, quality id starting with +, and quality score), we know that every sequence has a line starting with @ associated with it. 

We could write all of teh ID lines to a separate file:

$ grep "^@" -c sample_passerina.fastq > idlines.txt

We can cound the number of sequences:

$ grep "^@" -c sample_passerina.fastq

We can print the line with a match, plus any number of lines following it:

grep "^@" -A 1 sample_passerina.fastq

SDN_AM_43432 is the ID of a specific bird represented in this data set. How many DNA sequences do we have for this bird?

$ grep "SDN_AM_43432" -c sample_passerina.fastq


