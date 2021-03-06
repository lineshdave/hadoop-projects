### How to Re-run Linux Commands

#### Source: https://superuser.com/questions/7414/how-can-i-search-the-bash-history-and-rerun-a-command

Type Ctrl R at the command line and start typing the previous command. Once a result appears keep hitting Ctrl R to see other matches. When the command you want appears, simply press Enter

Note that while Ctrl R is the default, if you wanted the command (reverse-search-history) to be bound to Ctrl T you could configure that with the following:

bind '"\C-t": reverse-search-history'
There are a whole host of other readline bindable commands that are available to you as well. Take a look at the bash man page.

Bash has many facilities to search and access interactive command history. The most basic of which is the history builtin. Typing just:

$ history
Will print a list of commands along with a numeric index, like:

$ history
1 clear
2 ls -al
3 vim ~/somefile.txt
4 history
$
You can then execute any of these commands using their numeric index by prefacing the index with a single !, as Mitch pointed out:

$ !1
Will execute the clear command. The history builtin has many features itself, you can see more in the bash and history man pages.

You can also specify relative negative offsets when using the ! designator, so using our history list above, if we wanted to execute vim again, we could do:

$ !-2
Which is basically telling bash to execute the command you ran "two commands ago." To run the previous command in the history list, we can just use !! (which is just shorthand for !-1).

The ! designator doesn't limit you to numerically specifying which command to run. hayalci showed that you can instruct bash to execute a command based on either the text it begins with (using !) or text within the command itself (using !?). Again, using our example history list above, if we wanted to execute clear again, all we need to do is type:

$ !cl
and press Enter. And what about vim? That is as simple as:

$ !?some
The most important point from hayalci's response is the call to the shopt builtin:

$ shopt -s histverify
This will enable history verification so that commands that are matched by the !, !!, and !? designators are not blindly executed, but instead filled in on the command line so you can ensure they will do no evil before executing them. This is even more important when you are executing commands as the root user. This option can be set in your .bashrc startup file so that it is set whenever you log in.

As has already been pointed out, all of this information can be gleaned from the bash man page. For the !, !!, and !? designators, take a look at Section 9.3 History Expansion.
