[testing c(hange)d(irectory)]
=============
cd : by default uses the $HOME env. goes to $HOME.
cd - : not tested. goes to $OLDPWD
cd ~ : not tested, goes to $HOME
options are not required.
only absolute and relative paths are tested, CDPATH is not required.

if $HOME is not set, it should throw an error or else you're (and the evaluation will be) cooked.

when in doubt trust the bash documentation:

> If the directory change is successful, cd sets the value of the PWD environment variable to the new directory name, and sets the OLDPWD environment variable to the value of the current working directory before the change. 

# basic tests and simple error tests
cd
cd .
	> this should update PWD and OLDPWD btw
cd /
	> this takes you to root folder
cd ..
cd ../
	> these 2 are treated the same
cd /some/legit/directory
cd /some/fake/directory
	>error.
cd ./real/directory
	> yeah, bash treats it normal
cd ./a/file
	> not a directory error!
unset HOME && cd
	> error should pop up.
export HOME="nonsense" && cd
	> another error

# uh oh
(observe the output in both bash and minishell)

## OLDPWD test // watch how the value propogates from PWD to OLDPWD
cd $HOME
unset PWD OLDPWD && export PWD OLDPWD
cd .. && (export | grep PWD)
unset PWD && export PWD && (export | grep PWD)
cd .. && (export | grep PWD)
cd .. && (export | grep PWD)
...

## NO PWD test // there may be a weird edge case observed on OLDPWD
there may be undefined behaviour in OLDPWD on the first cd .., ignore it.

cd $HOME
(export | grep PWD)
unset PWD && (export | grep PWD) && pwd
cd .. && (export | grep PWD) && pwd
cd .. && (export | grep PWD) && pwd
...

## NO PWD ENVS
you should still be able to use cd, the envs will jsut not be made and updated. 

cd $HOME
unset PWD OLDPWD && (export | grep PWD)
pwd && cd .. && (export | grep PWD)
pwd && cd .. && (export | grep PWD)