# Project1
Implementation of a simple shell 

adadadadadadadadadadadadadadadadadadadadadadadadadadadadadadadadadadadadadadadad

# Design Choices
### struct Arguments

We used a struct named **Arguments** with four arrays to store the command line. 

1. char *firstarg[] stores the the command line commands before '>'
2. char *secondarg[] stores the the command line commands after '>'
3. char *thirdarg[] stores the second job that pipeline needs to do
4. char *fourtharg[] stores the third command in a pipe

Then there are three int variables that act as flags:

1. int outputre: if 1 then output redirection, if 2 then append to file
2. int background: stores 1 if its a background process
3. int pipe: if this variable is > 0, then executes pipe

### int main(void)

- We create a pointer to the struct Arguments called first
- Next we implement an infinite while loop to print out the prompt "sshell", \
keeps prompting the user for commands until they enter "exit". 
- Otherwise, when the user enters a command, we parse it and set the \
appropriate variables in the "Arguments" struct.
- **char *token** on line 71 checks if the command being parsed is valid, and if\
so proceeds to execute the corresponding command using execvp() in a while \
loop that will continue until we reach the end of the command line input.
- Background processes are handled by forking, and we place a waitpid() call in \
the parent process to suspend execution of the parent until the child returns.
- Will print completed process after returning, and also prints error message \
if error occurs. 

### int changedirec(char *path[])
- Called from main when the command is **cd**.
- If a path is provided from the user, will implement the *chdir** command.
- Else prints error message, then returns.

### int outputredirect(struct Arguments s)
- Takes in the Argument struct as command line arguments.
- Returns error if there is no secondary argument provided.
- Will either call **O_TRUNC** or **O_APPEND** depending on usage mode, then \
calls dup2() on the file descriptor and closes the file.

### int pipeline(struct Arguments s) and int pipeline2(struct Arguments s)
- We created two pipeline functions in order to facilitate multiple piping calls
- Calls fork, creates the parent and child processes.
- pipeline() handles only one piping call, while pipeline2() handles more than 1.
