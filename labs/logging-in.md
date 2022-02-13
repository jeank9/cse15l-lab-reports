# Setting up `ssh` for new ieng6 logins
For other classes with other logins (I had to do this for CSE 12)

- Use the keys you generated for CSE 15L! (no need to keygen)
    - id_rsa
        - already on local
    - id_rsa.pub
        - just need to copy this to new server

## Setting up the keys

1. log into new account
	- `$ ssh cs12wi22amg@ieng6.ucsd.edu`
2. create a `.ssh` directory
    - `$ ls -a`
        - check if the .ssh directory already exists
    - `$ mkdir .ssh`
3. log out (CTRL+d)
4. `scp` the public key `id_rsa.pub` into the `.ssh` directory in the new server
    - `$ cd ~/.ssh` if not in .ssh directory
    - `$ scp id_rsa.pub cs12wi22amg@ieng6.ucsd.edu:~/.ssh/authorized_keys`

Now you should be able to `ssh` in without a password!

## Updating the config file
To make `ssh`ing in even easier...
1. open the config file
    - `$ code ~/.ssh/config`
2. add the new server stuff to that file
    ```
    Host cs15l
        HostName ieng6.ucsd.edu
        User cs15lwi22zzz
        IdentityFile ~/.ssh/id_rsa
    
    Host newServer!
        HostName ieng6.ucsd.edu
        User cs12wi22zzz 
        IdentityFile ~/.ssh/id_rsa
    ```
Note: showing error
```
Connection closed by some ip address
```