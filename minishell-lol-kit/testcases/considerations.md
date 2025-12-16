things to consider when doing minishell testsreading valgrind:

- remember to use bash as a reference

- THERE WILL BE FDS LEFT OPEN. 0, 1 and 2 is fine being left open because the shell needs them. can be left open for all i care lol.

- ultimately, i don't think custom fd redirects should be mandatory (e.g. 3> OUTFILE) because:
	- the evaluation sheet doesn't suggest anything apart from it
	- us figuring out this stuff was way too much, but we learned that:
		- in terms of fd management, we need to 'reserve' fds that were explicitly specified in the readline.
		- afterwards when executing the ast tree, whenever we open(), we immediately dup() to a non-reserved fd (remember to close the previous one!).
		- i saw someone do piping for their custom redirects, that's cool but i wish i could think about how they did it
		- if you want a test case:
			- main.c compiled to a.out : have a write(4, "test\n", 5); // writes "test\n" to fd 4.
			- a.out 4> outfile : it should have the "test\n" text in outfile
	- if people don't do this, that's fine, i hope they have a way to stop testers from crashing their shell though through this (throw as a syntax error!)

- readline valgrind errors, and brandon's suppression file
	- we use readline() but there's loads of leaks
	- i just use the valgrind file to suppress the errors so that i don't think about it
-------------------------
# cd (the stupid rabbithole)

generally the thing goes like this:

NEW_OLDPWD = $PWD (current value)
change_directory(TARGET)
IF success:
    OLDPWD = NEW_OLDPWD
    PWD = new_path

cool thing is that OLDPWD and PWD will independently be updated only if they exist (cd will not create the env if they don't exist), and they can still work if both don't exist