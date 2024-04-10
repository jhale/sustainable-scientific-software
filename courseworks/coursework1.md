## Homework 1

## Instructions

The deadline for submission is Friday 26th April 2024 end of day. Please
submit an email with a .zip or .tar.gz file containing your work.

You are free to use Google, Stackoverflow etc. to scope out answers - you will
usually need to combine multiple parts to get a solution.

Points of assessment:

1. Concise high quality comments in scripts.
2. Consistent use of white space (indending, tabs etc.) 

## Exercises

1. Write a bash shell script that selects the n longest lines from a text file
   passed as an argument. The output of the file should be the n longest
   lines sorted from longest to shortest with the length prepended, e.g.

       ./longest_lines.sh 4 file1.txt

   Place your script in a directory `01/` with a file `file1.txt` for testing.

2. During the course we briefly touched upon the concept of standard output
   `stdout` which is the stream to which a program writes its normal output
   data. The standard error `stderr` is a special output stream which a program
   can use to write messages that should not be in stdout, such as error
   messages. This is useful because we typically do not want error (or status
   messages) passed through `stdout` to downstream programs.

   In a new directory `02/` modify a copy the script `longest_lines.sh` to echo
   an informative error message to stderr and signal an error using `exit` if
   the number of arguments passed is not equal to two.

   Additionally, check that the file passed exists and also echo an informative
   error message and `exit`.

3. Write a one-liner script `user_environment.sh` that prints the name of all
   environment variables `env` that contain the username of the current user
   `whoami`. You may find the command `cut` useful to get the variable name
   alone.

   Place your script in a directory `03/`.

4. Write a script `file_type_counter.sh` that takes a directory as input and
   prints the count of each unique file type (based on the extension e.g.
   `.txt`) within that directory and all subdirectories. You may find `find`,
   `cut` and `rev` useful, alongside some other commands we saw during the
   course.
