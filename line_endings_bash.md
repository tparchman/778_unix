### Writing a bash script to replace Mac or Windows line endings with Unix line endings (a fix that is commonly needed for converting files from others)

On line endings:

**4.A** Write a bash script that you can use to convert Mac line endings (\r) to Unix line endings (/n). This script must be able to read in any number of files you feed it. Refer to unixIV_primer.md. You can simply use "tr" to do this as above, but instead of just executing this from the command line, you will have written a program to do this. You can do this by writing a simple  a shell script (see examples 3.29.1 and 2B above). Name this script mac_to_unix_line_endings.sh, and save it for future use (you will find this repeatedly useful).

First execute your 'tr' command from the prompt on the file: taurus_mtDNA.txt (available on the [githubpage](https://github.com/tparchman/778_unix)) to make sure that your 'tr' command works. You will see that this file has mac line endings to start with. Thus, if you look into it using 'less' you will see the curious looking `^M` character. In Unix speak, this is actually '\r', but simply displays as ^M in 'less'. The Unix format for line endings is '\n', and you will use this in Python or Perl scripts you write, so you might as well get comfortable with it today.

### Example of a slightly more useful bash script.

    #!/usr/bin/bash
    cat $1 | tr '\r' '\n' > u_$1

$1 here stores a command line argument, which in this case should be a text file with mac line endings. This will pipe the contents of that file to `tr` to replace `\r` with `\n` (thus replace mac with unix line endings), and will redirect to a file which will be named "u" plus the argument stored in $1.

    $ chmod u+x mac2unix.sh
    $ ./mac2unix.sh taurus_mtDNA.txt

This will produce a file "u_taurus_mtDNA.txt"
