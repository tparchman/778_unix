## Writing a bash script to replace Mac (or Windows) line endings with Unix line endings 
<p>&nbsp;</p>

**On line endings:**

As Unix commands, and many scripting languages, often entail processing one line at a time, line ending format  matters. `Unix` uses `\n`, Mac uses `\r`, and Microsoft uses `\r\n`. For working in Unix , files ideally would have `\n` line endings. Indeed, you will find that improper line endings will repeatedly cause you headaches with Unix, Python and Perl scripting. 

Use `less` to take a look at `grouse_barcodes.csv`, which can be downloaded from the [778 Unix github](https://github.com/tparchman/778_unix) directory.  This file should contain three columns of information separated by commas, but you'll notice that all of the information appears on a single line, and the highlighted symbol `^M`. In Unix speak, this is actually `\r`, but simply displays as `^M` in `less`. There are two ways that you can easily change the line endings of this file.

You could open the file in `Text Wrangler` (or a similar editor) to examine. At the bottom of the window, you will see toolbar reading `Legacy Mac OS (CR)`, which identifies the line ending format for the file. You can click that menu, change the line endings to `Unix (LF)`, and save the file, you should be good to go. Other text editors have similar capability. Yet, this is cumbersome. 

**Writing a bash script to alter line endings**

You could easily write a bash script to convert mac line endings (`\r`; or Windows `\r\n`) to Unix (`\n`).  You can simply use `tr`  to do this (`sed` could also work), but instead of just executing this from the command line, you will have written a shell script that can be used to do this repeatedly. 

First execute `tr` (see `man tr`) from the prompt on the file: `grouse_barcodes.csv` to ensure that your use of `tr`  works. 

From the prompt, on the fly, the below command is piping the contents of `grouse_barcodes.csv` into `tr` which is then replacing `\r` with `\n`.

    $ cat grouse_barcodes.csv | tr '\r' '\n'

Putting these commands into a useful Bash script is quite easy. The only additional issue to figure out is how to have an argument from the command line, the name of the file to process in this case, processed by the script. Doing this can be quite easy, in that the first argument following your script on the command line (`grouse_barcodes.csv` below) can be automatically assigned to `$1`. Once you have written and saved your script, when executed as below, it should change line endings as intended.

    $ bash mac2unix.sh grouse_barcodes.csv
<p>&nbsp;</p>

### Potential solution code:


    #!/usr/bin/bash
    cat $1 | tr '\r' '\n' > u_$1

$1 here stores a command line argument, which in this case should be a text file. This will pipe the contents of that file to `tr` to replace `\r` with `\n`, redirection to a file which will be named "u" plus the argument stored in $1.

    $ chmod u+x mac2unix.sh
    $ ./mac2unix.sh grouse_barcodes.csv

Running this script should then produce a file "u_grouse_barcodes.csv".
