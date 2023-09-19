A shell script is a text file that contains a sequence of commands for a UNIX-based operating system. It is called a shell script because it combines a sequence of commands, that would otherwise have to be typed into the keyboard one at a time, into a single script. The shell is the operating system's command-line interface (CLI) and interpreter for the set of commands that are used to communicate with the system.

Opening the Shell
Before building a simple shell, we have to understand how it works. Before we type any commands into our shell to execute, when you open a terminal and the shell program opens up, it first checks for your PS1 variable in your environment variables and uses the value from PS1 to format your shell prompt. Once your shell prompt is formatted, you can then type in your command: $ ls -l .

Aliases and Special Characters
Once you’ve typed in your command and hit enter, a few things happen. First your shell program checks for any aliases associated with the commands you’ve entered. If an alias is found, it expands that alias before it executes. Once all aliases are checked, your shell program looks for special characters such as: ", ', \, *, &, # and executes the logic associated with each special character.

PATH
Once all aliases and special characters have expanded, the shell looks through the first word of a command to check if it’s a built-in function before checking for a program in the PATH. After it checks for built-ins, the shell checks your PATH variable and uses each directory in the PATH variable to check if the command exists, in this case: isls is located in each directory?. (FYI, ls is a built-in, but we’ll proceed in this case as if it wasn’t). Check all of your environment variables by typing in $ env into your shell program and look for your PATH variable.


Your shell program looks through each directory separated by a : to see if the ls command exists in each of these directories. When recreating the shell, we used a function called stat to check if the ls command existed by appending /ls to each PATH directory and entering the new PATH directory into the stat function to see if the executable file exists in each directory. If it didn’t exist, we recreated an error message similar to the normal output of the shell.
Creating Tokens
If the first built-in command or command exists, we execute different logic. First we would capture the entire command and create a double pointer array, with each command being stored as a char pointer, followed by a NULL terminator at the end of our double pointer array. We do this by first finding the number of commands in the prompt and add one for the NULL terminator. Next we use the strtok function, man strtok if you need more details and use a " " as a delimiter. We iterate in a loop until each token we’ve assigned from the strtok function is NULL. Each iteration we’ll have to malloc enough space for each command, meaning we’ll have to know the exact length of each command before mallocing. Then we use the strncpy function to copy each token to the newly malloced space, or in our case, our own _strncpy function we built. Below is an example illustration of a command, it’s data structure and the source code.
Finding the PATH
If we pass in just ls without the PATH where the command lives, our simple shell won’t work. This is why we have to have another condition that finds all the directories in the PATH variable, appends /ls to each PATH variable, then uses stat to check if the file exists. If it does exist, then use execve to execute the program.

For example, let’s take the PATH variable used above:

PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

We have to create either a linked-list, or a double pointer array like the example above to store each path where we’ll have to append /ls and loop through the data structure to check if the file exists in each directory.

Example structure:

char **all_directories = [*directory, *directory, *directory, *directory, *directory, *directory, NULL];

char *all_directories = ["/usr/local/sbin/ls", "/usr/local/bin/ls", "/usr/sbin/ls", "/usr/sbin/ls", "/usr/bin/ls", "/sbin/ls", "/bin/ls", NULL];

Source code can be found here.

You’ll have to loop through your data structure, check if the file exists using stat, and if it does, execute the program using execve. But how does the program continue to run and output a new prompt that waits for a new command?

Creating New Processes
Before reading on, man fork and read this. For extra credit, type in : pstree -pn into your terminal. This shows all the processes running on your computer via a nice illustration in your terminal. For your program to persist, you’ll need two things. You’ll need a while loop that runs forever or until a certain condition is met like reaching an end of file and you’ll need to be able to create a new child process from the parent process associated with your executable file (./hsh in our example) during each iteration of your while loop.
Finding the PATH
If we pass in just ls without the PATH where the command lives, our simple shell won’t work. This is why we have to have another condition that finds all the directories in the PATH variable, appends /ls to each PATH variable, then uses stat to check if the file exists. If it does exist, then use execve to execute the program.

For example, let’s take the PATH variable used above:

PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

We have to create either a linked-list, or a double pointer array like the example above to store each path where we’ll have to append /ls and loop through the data structure to check if the file exists in each directory.

Example structure:

char **all_directories = [*directory, *directory, *directory, *directory, *directory, *directory, NULL];

char *all_directories = ["/usr/local/sbin/ls", "/usr/local/bin/ls", "/usr/sbin/ls", "/usr/sbin/ls", "/usr/bin/ls", "/sbin/ls", "/bin/ls", NULL];

Source code can be found here.

You’ll have to loop through your data structure, check if the file exists using stat, and if it does, execute the program using execve. But how does the program continue to run and output a new prompt that waits for a new command?

Creating New Processes
Before reading on, man fork and read this. For extra credit, type in : pstree -pn into your terminal. This shows all the processes running on your computer via a nice illustration in your terminal. For your program to persist, you’ll need two things. You’ll need a while loop that runs forever or until a certain condition is met like reaching an end of file and you’ll need to be able to create a new child process from the parent process associated with your executable file (./hsh in our example) during each iteration of your while loop.
