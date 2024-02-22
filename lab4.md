# Lab Report 4 - Vim

## Step 4 - Logging into ieng6

![Image](/lab4_images/lab4_1.png)

Keys pressed: `ssh j4suh@ieng6.ucsd.edu<ENTER>`. This command is meant to log me into ieng6 using the ssh key that we made in a previous lab, and enter ran the command.

## Step 5 - Cloning the repository

![Image](/lab4_images/lab4_2.png)

Keys pressed: `git clone git@github.com:jsuh0020/lab7<ENTER>`. This command clones the lab7 repository that I forked onto my github account using SSH. Pressing enter ran the command.

## Step 6 - Run the failing tests

![Image](/lab4_images/lab4_3.png)

Keys pressed: `cd l<TAB><ENTER>bash t<TAB><ENTER>`. The `cd` portion of these commands changes my directory into the `lab7` directory, which I accessed by tab completing `l` into `lab7`. Then, I ran the command using enter. Now that I'm in the `lab7` directory, I tab completed `bash t` into `bash test.sh` and pressed enter to run the command. This ran the `test.sh` script, which tests the `ListExamples.java` file. Here, the `merge` method seems to be failing.

## Step 7 - Edit and fix the test

![Image](/lab4_images/lab4_4.png)

Keys Pressed: `vim L<TAB>.j<TAB><Enter>`. This command runs vim on the `ListExamples.java` file. I tab completed `L` into `ListExamples`, but since there are multiple files starting with `ListExamples`, I had to tab complete `.j` into `.java` to get the exact file. I pressed enter to run the command.

Notice that this screenshot was before I pressed enter, since running the command would take me to the file contents.

![Image](/lab4_images/lab4_5.png)

Keys Pressed: `G6ker2:wq<ENTER>`. `G` takes me to the bottom of the file. `6k` moves me up 6 lines, to where the line we needed to fix was. `e` goes to the end of the word `index1`, and `r2` replaces the 1 with a 2. `:wq` saves and exits the file, which had to be run by pressing enter.

Again notice that this screenshot was before I pressed enter, since running the command would have exited vim.

## Step 8 - Re-run the tests

![Image](/lab4_images/lab4_6.png)

Keys Pressed: `<UP><UP><ENTER>`. The `bash test.sh` command was ran 2 commands ago, so pressing the up arrow twice takes me to the command, and pressing enter runs it. Since we fixed the file with vim, all tests now pass.

## Step 9 - Commit/push changes

![Image](/lab4_images/lab4_7.png)

Keys Pressed `git add L<TAB>.j<TAB><ENTER>git commit - m "updated L<TAB>.j<TAB>"<ENTER>git push origin main<ENTER>`. `git add` adds the file with changes to the staging area, and `git commit` commits the changes to the repository, with the `-m` flag allowing the user to add a message. Both of these commands were tab completed similarly to step 7 and were both ran by pressing enter. `git push origin main` pushes the changes to our repository, and enter runs this command.
