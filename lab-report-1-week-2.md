[home](index.md)

# lab report 1

(Instructions from [here](https://ucsd-cse15l-w22.github.io/week/week1/#lab-tasks).)

> [Installing VScode](##Installing-VScode)\
> [Remotely Connecting](##Remotely-Connecting)\
> [Trying Some Commands](##Trying-Some-Commands)\
> [Moving Files with `scp`](##Moving-Files-with-`scp`)\
> [Setting an SSH Key](##Setting-an-SSH-Key)\
> [Optimizing Remote Running](##Optimizing-Remote-Running)\
> ^^so sad these don't work

## Installing VScode
Use the website: [https://code.visualstudio.com/](https://code.visualstudio.com/)

How it should look:

![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab1/vscode.png?raw=true)

> I had VScode already installed, so I skipped this step!

## Remotely Connecting
On Windows: Install OpenSSH (or check that it's installed)
- OpenSSH Client, OpenSSH Server
    - Settings > Apps > Apps & Features --> Optional Features


Look up course-specific account here: [https://sdacs.ucsd.edu/~icc/index.php](https://sdacs.ucsd.edu/~icc/index.php)
- For CSE 15L, something like `cse15lwi22zzz@ieng6.ucsd.edu`
- **Reset password**

Open a terminal in VScode with:
- Terminal --> New Terminal
- Ctrl + `

Connect using `ssh` - *secure shell*
```
ssh cs15lwi22zzz@ieng6.ucsd.edu
```
> If first time connecting, enter `yes`

Enter password!
> You won't see yourself "typing" anything - the password stays hidden.\
> This is a great security measure and bamboozled me the first time. 

Should look like this:

![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab1/terminal.png?raw=true)

To exit:
- `exit` or CTRL+D

---
Fun notes:
- `ssh cs...@ieng6.ucsd.edu javac Fun.java`
    - connects, runs command, exits
    - *can only run one command by default*
- `ssh cs...@ieng6.ucsd.edu "javac Fun.java; java Fun"`
    - "quotation marks" allow you to run multiple commands in one line

## Trying Some Commands
Try out some fun terminal commands! 

Ideas include: `cd ~`, `cd`, `ls -lat`, `ls -a` ... 

An example I tried: `ls <directory>`
- `<directory>` is `/home/linux/ieng6/cs15wi22/cs15wi22aaa`
- the last part `cs15wi22aaa` is the username of another student (uh oh)

Here's what happened:

![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab1/trying.png?raw=true)

> I tried to access someone else's files. I couldn't. That's probably good.


## Moving Files with `scp`

`scp` - *secure copy*
- make a copy of a file from your computer to another
- **run only from client (not the server)*

Create `WhereAmI.java`:
```java
class WhereAmI {
    public static void main(String[] args) {
        System.out.println(System.getProperty("os.name"));
        System.out.println(System.getProperty("user.name"));
        System.out.println(System.getProperty("user.home"));
        System.out.println(System.getProperty("user.dir"));
  }
}
```
Run it using `javac` and `java`

Then, run  `scp WhereAmI.java cs15lwi22zzz@ieng6.ucsd.edu:~/`

Log in with `ssh` again, and use `ls` (list files).
- Should see `WhereAmI.java`

Now you can run `javac` and `java` on the ieng6 computer! :00

Should look like:

![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab1/sshscp.png?raw=true)

>note: I ran two commands at once using the semicolon `;` !

When running on the client, the output gives info about my laptop. When running on the server, the output gives info about the remote ieng6 server computer. `getProperty` probably gives info about a property of the computer it is being run on.

> It took me around two minutes to make a change to `WhereAmI.java`, save it, copy it to the server, run it there, and log out. This x100? Yikes. 200 minutes of just running it. A big problem seemed to be having to log in multiple times.


## Setting an SSH Key
`ssh` keys can make it easier to log into a remote server.

`ssh-keygen` creates a *public key* and *private key*. 
- the public key is copied somewhere on the server
- the private key is copied somewhere on the client

These keys can be used in place of a password!

To set up these keys:
1. run `ssh-keygen`
    - can choose to use a passphrase or not
        - not is fastest!
> Note: I got a "Could not open a connection to your auth agent" error here. I Googled it and ran ``eval `ssh-agent -s``, and it worked eventually. No idea if this is right.
2. run `ssh-add` according to [this](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement#user-key-generation)
- `add-ssh ~/.ssh/id_rsa`

This creates the private key (in `id_rsa`) and the public key (in `id_rsa.pub`), stored in your computer's `.ssh` directory.

3. copy public key (`id_rsa.pub`) to server directory.
    - log in using `ssh`
    - run `mkdir .ssh`. 
        - where to copy the public key
    - run `scp /Users/[username]/.ssh/id_rsa.pub cs15lwi22@ieng6.ucsd.edu:~/.ssh/authorized_keys`*

![Images](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab1/keys.png?raw=true)

> The timing experiment took 45 seconds. 1 minute and 15 seconds were saved per run. 

> Note: Try this process a few more times. Not 100% on the process.


## Optimizing Remote Running

The best process I came up with:
```java
scp WhereAmI.java cs15lwi22zzz@ieng6.ucsd.edu:~
...
ssh cs15lwi22zzz@ieng6.ucsd.edu "javac WhereAmI.java; java WhereAmI"
```
This makes use of:
- running commands in the same line as `ssh` 
    - which also logs you out after running
- running multiple commands in one go by using `;`
- running multiple commands in the same line as `ssh` by using `;` and `""`

![Images](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab1/optimize.png?raw=true)
> This entire process, when coupled with using the up arrow, now takes only around 4 seconds!

