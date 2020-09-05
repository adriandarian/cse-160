CSE30 Lab #1

========
The Goal
========

This lab will begin to get you familiar with the C++ programming language
and the tools available to you on the Linux systems in the lab. For this 
exercise, you should do your work on the lab systems directly, to minimize
any headaches in the beginning. After you get comfortable with the system and 
the language would be a better time for exploring your other options. 

====================
What You'll Be Doing
====================

The goal of this lab is to create a simple program that will read input 
from the user, temporarily store that input and then display it. Additionally, 
you will learn how to create automated build instructions called Makefiles, 
how to use the compiler to compile C++ source code into an executable that you
can run, and learn how to use revision control. 

===============
Getting Started
===============

Log into the machines using your UCMId and password. Then, once you're logged
in you will be using a terminal and SSH to connect to the Engineering
department's server 

Example: ssh jewart@englab0001.ucmerced.edu 

Once you're logged in, the first thing you'll do is set up a repository for 
your work. This will do several things for you: first it sets up a location 
for youto store all your work, secondly, and more valuably, it will provide 
you with what we call "Revision Control". This means you can incrementally
create new versions of your code, and backtrack to old versions if you've
broken something terribly. We will start with very simple functionality, 
adding files, checking out files, and committing new versions to the
repository. 


Creating the repository 
-----------------------

To initialize a new repository waiting for you to add files to it, use the
following command:
    
   	svnadmin create $HOME/repository


   	[NOTE: $HOME is a variable that is set to your 'home directory' (this is 
the directory that belongs to you on the server), so $HOME/foo.txt refers to 
the file 'foo.txt' that resides in your home directory (on our server it's
/home/username) -- Whenever I type $HOME, you can also type out the full path 
to your home directory (in my case this would be /home/jewart)]


Now, make a directory for your CSE30 assignments, something like:

	mkdir $HOME/CSE30 
	
will work, you are of course, free to choose your own directory names and 
locations -- whatever makes sense to you, just change my example commands
to match your directory name accordingly.

Once you've done that, you're going to add your new folder to the code 
repository with this command:

   	svn import -m "Creating CSE30" $HOME/CSE30 file://$HOME/repository/CSE30

This will import your newly created CSE30 directory into your repository. 

Now we have one more command to run before we can get started writing code, so 
we can tell the system that the CSE30 directory should be put under version
control:

	cd $HOME/CSE30 
	svn co file://$HOME/repository/CSE30 .
	
So, now we have a folder that's under version control, and we can create our 
files and folders. We'll visit how to add those files and folders to the 
repository as we go along. 



Creating Our First Source File
------------------------------

This document covers using a very common and powerful text editor called 'vi', 
which has a bit of a learning curve but you can do some really neat things with 
it once you get the hang of it. I've put up a 'vi cheat sheet' on Sakai if you
want or need it. You can use any editor you like but I'll walk you through 
using vi here... 

Make sure you're in your newly created CSE30 directory, and create a new 
directory for this lab, I'll call it 'lab1':

	cd $HOME/CSE30
	mkdir lab1

Now, let's add it to version control: 

	svn add lab1

This will mark it as being ready to be put in the repository. Now, put yourself
in the directory you just created and create a new source file with vi.
	
	cd lab1
	vi main.cpp
	
This will start vi and edit the file "main.cpp" (it will be created when you 
save if the file doesn't exist). To enter editing mode, press the 'i' key, 
this will cause vi to say "-- INSERT --" at the bottom of the screen. It will 
say this as long as you're in insert mode (you can insert new text). In vi 
there are two main modes, "insert mode" and "command mode". To exit insert 
mode, press the ESC key and the "-- INSERT --" text will disappear at the 
bottom of the screen. 

Once you are in insert mode, you can type text as you normally would. 

[NOTES:]

* To save your file, exit insert mode with ESC and type :w

* When you want to quit vi, be in command mode and type :q (you can combine 
them as :wq to 'write and quit' [clever, eh?]) 

* When you try to quit, it will warn you if you've made changes to your file
that haven't been saved yet, if you want to quit with reckless abandon, simply
append a ! to the command and vi will stop warning you and assume you know
what you're doing.


Adding Code 
-----------

Now, editing your main.cpp file with vi, in insert mode, we'll move on to
writing a basic C++ program, and a Makefile. You'll want to type into your 
main.cpp the contents of the EXAMPLE 1 below. Once you've done that, go ahead
and write/quit from vi. 

EXAMPLE 1: main.cpp

#include <iostream>

void main(void) {

	cout << "Hello world!!" << endl;

}


To compile your source code, we will use GCC (the GNU C Compiler), which 
is a free, open-source C compiler that's available for loads of different 
platforms. To compile your program, you need to tell gcc which file to 
compile and what to name the executable program it creates. You can do this 
by using the following:

	g++ <sourcefile> -o <executable>
	
So, for this example:

	g++ main.cpp -o lab1
	
This will produce a file called 'lab1' in the same directory as your .cpp 
file that is executable, meaning you can run it. Assuming that it compiled 
properly (which it should have if you typed the above example verbatim), then 
in order to execute your program, simply type:

	./lab1 
	
This will cause your program to run (In UNIX, "." means the current directory, 
so ./lab1 refers to the file lab1 in the current directory)

Once that's done, and it compiles properly, we can add it to our repository so 
that it's saved there as well as locally. To do this, we type (from the $HOME/CSE30/lab1 directory):

	svn add main.cpp 
	
Then, once it's been added, we can proceed to "commit" it to the repository,
meaning that we create a new version in the repository using the data we have
locally. We can do that with the command:

 	svn commit -m "Enter some meaningful message here"

What this will do is to commit to the repository a new "revision", meaning that 
it remembers the old data but creates a new version of the code in the 
repository. Why do we want to do this? Because this allows us to incrementally 
update the repository with "good code" meaning that the newest data in the 
repository is known to work properly at all times, and we can basically "go 
back" to any version we want at any time later on. Effectively, this creates 
a history of all the changes you've made to your code, so you don't have to 
keep making copies of your source files before you try a new approach or make 
some major changes, it will do that for you! We'll discuss some of the more 
advanced features of version control as the semester goes on, but for now we'll
just use it to keep track of our code. 

[NOTE: You do *NOT* need to add a file more than once to the repository, it 
will tell you that you can't do it, so don't bother, after you make any changes
that you want to commit to the repository permanently, run the "svn commit"
command ]

When you commit, make sure you put in a meaningful message after the -m option, 
that way when you look at the log of changes later on you'll know what you were
doing. This can be very helpful at 2AM, I promise ;) 

To get (or go back to) the most recent version in the repository, simply move 
your current main.cpp file out of the way (rename it to something else) and 
type:

	svn update

This will update the files in the current directory with the data that's in 
the repository. Look at your directory contents with 'ls', you will see your 
old main.cpp file renamed whatever you called it, and you'll see a main.cpp 
file has come back. If you examine your "new" main.cpp file, you'll see it 
matches whatever you most recently committed to your repository.


The Last Leg: Accepting Input
-----------------------------

Once you've done all this, we're going to change the program to accept some 
input. Here's an example program that accepts some input and then outputs 
what it took in:

EXAMPLE 2: in_out.cpp 

#include <stdio>
#include <string>

using namespace std; 

int main(void) {

	string name; 
	
	cout << "Please enter your name:"; 
	cin >> name; 
	
	cout << "Hello, " << name << "!!" << endl;

	return 0;
}

Create a new file called something other than main.cpp, place the example 
contents in it, compile it like you did with the main.cpp file, and then add 
the new .cpp file to your repository, and commit the changes with another 
message. 

Once you do that, you should now have two .cpp files, remove the executables 
from that directory and then compress your lab1 directory with tar:

	cd $HOME/CSE30
	tar zcvf lab1-`whoami`.tgz ./lab1

[NOTE: those `s are not 's they are the BACK-TICK located on the same key as 
the ~ is, what that does is execute the whoami program that will simply report 
back your username and put it in there, so if I were to run that, it would 
automatically become "tar zcvf lab1-jewart.tgz ./lab1" ]

Then, you can move the new .tgz file to your local desktop directory
($HOME/UCMEng_lab00/Desktop) and upload it to Sakai for the first lab. 

Congratulations, you've survived the first lab! 

"It is pitch black, you are likely to be eaten by a grue."