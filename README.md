Preparation for LFCS Certification Exam
===

Part 1 - Common Commands To Manipulate Stream Lines
----

###Processing Text Streams in Linux
The difference between `>` (redirection operator) and `|` (pipeline operator) is that while the first connects a command with a file, the latter connects the output of a command with another command
<table>
 <tr>
 	<th>Operator</th>
   <th>Command line</th>
   <th>Description</th>
 </tr>
 <tr>
 	<td>`cat`</td>
   <td>`cat mytext.txt`</td>
   <td>Show in standard output the content of the file `mytext.txt`.</td>
 <tr>
 	<td>`>`</td>
   <td>`cat > mytext.txt`</td>
   <td>Redirect standard input to `mytext.txt` file, so everything you write in the console is going to be in the text file.</td>
 </tr>
 <tr>
 	<td>`>>`</td>
   <td>`cat mytext.txt >> mytext2.txt`</td>
   <td>Concatenate the content of the file `mytext.txt` with the content of the file `mytext2.txt`.</td>
 </tr>
 <tr>
 	<td>`<`</td>
   <td>`cat < mytext.txt`</td>
   <td>Similar to `>`. Redirect standard input and passed as an argument to `cat` command.</td>
 </tr>
 <tr>
 	<td>`|`</td>
   <td>`man sudo | grep login`</td>
   <td>Connect the output of a command with another command</td>
 </tr>
</table>

###`sed` Command
It is a stream editor useful to perform basic text transformations on an input stream (a file or input from a pipeline).
<table>
 <tr>
   <th>Command line</th>
   <th>Description</th>
 </tr>
 <tr>
   <td>`sed ‘s/term/replacement/flag’ file`</td>
   <td>Basic syntax</td>
 </tr>
 <tr>
   <td>`sed ‘s/y/Y/g’ ahappychild.txt > ahappychild2.txt`</td>
   <td>Replace all the ocurrences of _y_ for _Y_ in the file `ahappychild.txt` and added to the file `ahappychild2.txt`.</td>
 </tr>
 <tr>
   <td>`sed 's/and/\&/g;s/^I/You/g' ahappychild.txt`</td>
   <td>Special characters  **/**, **\\**, **&**, need to escape it using backward slash (**\\**). Also use **;** to do a second replace command. **^** (caret sign) is the beginning of the line.</td>
 </tr>
 <tr>
   <td>`sed -n '/^Jun  8/ p' /var/log/messages | sed -n 1,5p`</td>
   <td>With the **-n** option we tell `sed` to print (indicated by **p**) only the part of the file (or the pipe) that matches the pattern (**Jun 8** at the beginning of line in the first case and lines **1** through **5** inclusive in the second case).</td>
 </tr>
 <tr>
   <td>`sed '/^#\|^$/d' apache2.conf`</td>
   <td>`sed` one-liner deletes (**d**) blank lines or those starting with **#** (the **|** character indicates a boolean OR between the two regular expressions).</td>
 </tr>
</table>
   
###`sort` and `uniq` Command
The `uniq` command allows us to report or remove duplicate lines in a file. We must note that `uniq` does not detect repeated lines unless they are adjacent. Thus, `uniq` is commonly used along with a preceding `sort` (which is used to sort lines of text files). By default, `sort` takes the first field (separated by spaces) as key field. To specify a different key field, we need to use the `-k` option.
<table>
 <tr>
   <th>Command line</th>
   <th>Description</th>
 </tr>
 <tr>
   <td>`du -sch /var/* | sort –h`</td>
   <td>The `du –sch /path/to/directory/*` command returns the disk space usage per subdirectories and files within the specified directory in human-readable format (`-h`).</td>
 </tr>
 <tr>
   <td>`cat /var/log/mail.log | uniq -c -w 6`</td>
   <td>You can count the number of events in a log by date by telling `uniq` to perform the comparison using the first **6** characters (`-w 6`) of each line (where the date is specified), and prefixing each output line by the number of occurrences (`-c`) with the following command.</td>
 </tr>
 <tr>
   <td>`cat sortuniq.txt | cut -d: -f1 | sort | uniq`</td>
   <td>Cut the first field (fields are delimited by a colon), sort by name, and remove duplicate lines.</td>
 </tr>
</table>

###`grep` Command
`grep` searches text files or (command output) for the occurrence of a specified regular expression and outputs any line containing a match to standard output.
<table>
 <tr>
   <th>Command line</th>
   <th>Description</th>
 </tr>
 <tr>
   <td>`grep -i alucard /etc/passwd`</td>
   <td>Display the information from **/etc/passwd** for user alucard, ignoring case.</td>
 </tr>
 <tr>
   <td>`ls -l /etc | grep rc[0-9]`</td>
   <td>Show all the contents of **/etc** whose name begins with **rc** followed by any single number.</td>
 </tr>
</table>
###`tr` Command
The `tr` command can be used to translate (change) or delete characters from stdin, and write the result to stdout.
<table>
 <tr>
   <th>Command line</th>
   <th>Description</th>
 </tr>
 <tr>
   <td>`cat sortuniq.txt | tr [:lower:] [:upper:]`</td>
   <td>Change all lowercase to uppercase in **sortuniq.txt** file.</td>
 </tr>
 <tr>
   <td>`ls -l | tr -s ' '`</td>
   <td>Squeeze the delimiter in the output of `ls –l` to only one space.</td>
 </tr>
</table>
###`cut` Command
The `cut` command extracts portions of input lines (from stdin or files) and displays the result on standard output, based on number of bytes (`-b` option), characters (`-c`), or fields (`-f`). In this last case (based on fields), the default field separator is a tab, but a different delimiter can be specified by using the `-d` option.
<table>
 <tr>
   <th>Command line</th>
   <th>Description</th>
 </tr>
 <tr>
   <td>`cat /etc/passwd | cut -d: -f1,7`</td>
   <td>Extract the user accounts and the default shells assigned to them from **/etc/passwd** (the `–d` option allows us to specify the field delimiter, and the `–f` switch indicates which field(s) will be extracted.</td>
 </tr>
 <tr>
   <td>`last | grep alucard | tr -s ‘ ‘ `
   `| cut -d’ ‘ -f1,3 | sort -k2 | uniq`</td>
   <td>Summing up, we will create a text stream consisting of the first and third non-blank files of the output of the last command. We will use `grep` as a first filter to check for sessions of user **alucard**, then squeeze delimiters to only one space (`tr -s ‘ ‘`). Next, we’ll extract the first and third fields with `cut`, and finally `sort` by the second field (IP addresses in this case) showing unique.</td>
 </tr>
</table>


Part 2 - How to Install and Use `vi/m` as a full Text Editor
----



