# CSE 15L Lab 1

## cd Command
1. cd with no arguments
   The directory I was in was /home/lecture1. Running the cd command with no arguments appears to just have brought me back to the home directory, at /home. cd means to "change directory", so having no arguments would just bring you to the home directory. There is no output, because you are just changing the directory.
2. cd with path to directory as argument
   The directory I was in was /home. Running "cd /home/lecture1/messages/ brought me to the messages directory, which has lecture1 as its parent directory. Using cd with a path to a directory as an argument will take you to that directory, and again there is no output because you are just changing your location in the file structure.
3. cd with path to file as argument
   I was in the messages directory, and tried to cd into "zh-cn.txt", and got an output of "bash: cd: zh-cn.txt: Not a directory". This makes sense because cd means to change directory, not to change to a file, so attempting to change to something that is not a directory will produce a similar output. This output is an error because the expected use of cd is to change to a directory and this was not a proper use of cd.

## ls Command
1. ls with no arguments
2. ls with path to directory as argument
3. ls with path to file as argument

## cat Command
1. cat with no arguments
2. cat with path to directory as argument
3. cat with path to file as argument
