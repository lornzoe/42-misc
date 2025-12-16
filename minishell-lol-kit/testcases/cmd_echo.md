[testing echo]
================
echo : should just output what is put as argument, then newline
echo -n : same thing as above, but no newline
echo -e : NOT TESTED. but enables escape characters
## the default test cases:

echo
echo -n
echo msg
echo "msg spaced"
echo 'another msg'
echo -n msg
echo -n "msg spaced"
echo -n 'another msg'

## error checking
echo heywhathappensifiputthishere -n
	> -n option should only work as argv[1]
echo -nnnnnnnnnnnnnnnnnnnnnnnnn heh
	> did you know this counts as -n ? special feature of options
echo -nm lol
	> another -n test lol
echo -n- testy
	>another -n test. -n- should not be considered an option
echo -en test
	> if somehow e is coded in (not required) it should pass in. otherwise treated like -en is a text arg
echo -ne test
	> same thing.
echo -n -nn -nnn -nnnn -nnnnnnnnn -nnnnnnnm
	> yet nother -n test
echo hi "hi; | &&" hihi
	> this is more on quotation marks testing
echo "\n"
	> honestly, this does nothing in bash
echo '\n'
	> this too.
echo -n $NONEXISTENT
	> should print nothing (if nonexistent )
echo -n a | cat -e 
	> testing the usage of -n but honestly seeing it 'mix' with the readline prompt should be sufficient enough.

# bonus territory?	
echo *
	> ಠ_ಠ ?
echo **
	> ಠ__ಠ ?
echo *******************************
	> ಠ_________________________________ಠ ?
touch 42-lyangatest1 42-lyangatest2 42-lyangatest3 && echo 42************************ && rm 42-lyangatest1 42-lyangatest2 42-lyangatest3
	> really, no matter how many *s are there it seems like they are really the equivalent of 1 *... what's more important is that they only echo anything with 42 in the beginning of their name
