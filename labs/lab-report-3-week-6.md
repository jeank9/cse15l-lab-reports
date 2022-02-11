# lab report 3 [week 6]

Due: 2/11/2022  
## **Streamlining `ssh` Configuration**
(Instructions from [here](https://ucsd-cse15l-w22.github.io/week/week5/#group-choice-1-streamline-ssh-configuration).)

Logging into `ieng6` takes a lot of typing:
```
$ ssh cs15lwi22zzz@ieng6.ucsd.edu
```
Can we make this shorter?

## Create/edit `~\.ssh\config`

The file `~\.ssh\config` tells SSH the username to use when logging into specific servers. You can also give servers nicknames!

To create the file `~\.ssh\config` (on Windows; on Linux it's `~/.ssh/config`):
1. `cd` into the `~\.ssh` folder. 
    - `ls` can be useful to see if `config` file already exists.
2. `code config`
    - this will either open the `config` file for editing if it exists, or create it if it doesn't.

These steps are shown below:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab3/openingconfig.png?raw=true)


3. Add these lines:
    ```
    Host [alias]
        HostName ieng6.ucsd.edu
        User cs15lwi22zzz (your username)
        IdentityFile ~/.ssh/id_rsa
    ```

The `[alias]` in the first line `Host [alias]` is the *alias* — a nickname for the server that can be used while `ssh`ing into it.
-  You can change this to whatever you'd like. 
    - Note: the `[]` is just to indicate that the alias is modifiable.
- I set this alias to **cs15l**, as this login is only used for this class.


Remember what we used to log in before:
```
$ ssh cs15lwi22zzz@ieng6.ucsd.edu
```
- The HostName is everything after the @, and the User is everything before it.

The last line is optional; it can be used if you set up a private/public `ssh` key.
- The `~/.ssh/id_rsa` refers to the location of the IdentityFile, or the *private key* that helps you log in.

Here is my `config` file:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab3/configfile.png?raw=true)

## The new way to `ssh`
A new, faster way to `ssh` is in town! Now you can just use whatever alias you chose:

```
$ ssh [alias]
```
Here's it running:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab3/sshin.png?raw=true)

## The new way to `scp`

You can also `scp` using the new alias:
```
$ scp [file] [alias]:~
```
This working; I `ls` into the server's root directory to show that the file has been successfully copied: ![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab3/scpfile.png?raw=true)
