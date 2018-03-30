# SSH SHORT REFERENCE

## Table of content

1. About SSH
2. Quick guide on creating ssh keys
    1. Linux/OS X
    2. Windows
3. More details on this topic
4. Troubleshooting common problems
5. Future reading

## About SSH Keys

Secure Shell (better known as SSH) is a cryptographic network protocol which allows users to securely perform a number of network services over an unsecured network. SSH keys provide a more secure way of logging into a server with SSH than using a password alone. While a password can eventually be cracked with a brute force attack, SSH keys are nearly impossible to decipher by brute force alone.

Generating a key pair provides you with two long string of characters: a public and a private key. You can place the public key on any server, and then unlock it by connecting to it with a client that already has the private key. When the two match up, the system unlocks without the need for a password. You can increase security even more by protecting the private key with a passphrase.

## Quick Guide on how to generate ssh keys

### Linux/OS X

**First Step - Create pair of public and private keys**

> `ssh-keygen -t rsa`

Once you have entered the Gen Key command, you will get a few more questions:

> `Enter file in which to save the key (/home/username/.ssh/id_rsa):`

You can press enter here, saving the file to the user home:

*Warning, you will overwrite old id_rsa key inside .ssh folder*
Also you can set up passphrase on top of key:

> `Enter passphrase (empty for no passphrase):`

Feel free to leave it blank. It's up to you whether you want to use a passphrase. Entering a passphrase does have its benefits: the security of a key, no matter how encrypted, still depends on the fact that it is not visible to anyone else. Should a passphrase-protected private key fall into an unauthorized users possession, they will be unable to log in to its associated accounts until they figure out the passphrase, buying the hacked user some extra time. The only downside, of course, to having a passphrase, is then having to type it in each time you use the key pair.

The entire key generation process looks like this:

![process](http://storage8.static.itmages.com/i/18/0330/h_1522401973_9356294_a1f48810b9.png)

Using man command you can check additinal settings that you can use with ssh-generate

> `man ssh-keygen`

Now in your /home/username/.ssh/ folder you will have atleast two files:

> `id_rsa`

> `id_rsa.pub`

*Never share your private key*

File id_rsa.pub you can share, just send it to admin or copy it to promts  that need your private key. You always can copy it from console, just use `cat` comand:

![cat](http://storage6.static.itmages.com/i/18/0330/h_1522402442_4778257_a2c33c28c4.png)

### Windows

*You can try this [guide](https://www.howtogeek.com/336775/how-to-enable-and-use-windows-10s-built-in-ssh-commands/), MS somehow added some Linux coreutils inside their OS, so you can use guide for Linux,  but I will also cover more convention way to use SSH on Windows via PuTTY*

**Download and Install PuTTy and PuTTYgen**

To get started, we'll need to download and install both PuTTY, the utility used to connect to remote servers through SSH (secure shell), and PuTTYgen, a utility used to create SSH keys. Dowloand [here](https://www.chiark.greenend.org.uk/~sgtatham/putty/).

After installation open "PuTTYgen". It will launch the key generation program, which should look something like this:
![PuTTYgen](https://assets.digitalocean.com/articles/putty_do_keys/putty_gen.png)

To create a new key, select the parameters at the bottom or just leave it on default values and hit 'Generate' button. After few dialogs it will output the public key to a text box on the screen.

![public-key-putty](https://assets.digitalocean.com/articles/putty_do_keys/generated_key.png)

*You can share this key*

You can use this information by copying and pasting it from the box, but we'll save it for later using the interface provided. Click on both the Save public key button and the Save private key button and select a secure location to keep them:

![saving](https://assets.digitalocean.com/articles/putty_do_keys/save_keys.png)

**Setting Up an SSH Session with SSH Keys in PuTTY**

After you shared your public key you can connect to the server.
Start by opening up the main PuTTY program. You can do this by double clicking on the PuTTY program, or by tapping the Windows key and typing "PuTTY".

Inside, you'll be taken to the main session screen. The first step is to enter the IP address of your server into the session page. 

By default, SSH happens on port 22, and the "SSH" connection type should be selected. These are the values we want.

Next, we'll need to select the "Data" configuration inside the "Connection" heading in the left-hand navigation menu:

![data](https://assets.digitalocean.com/articles/putty_do_keys/data_category.png)

Here, we will enter our server's username. For the initial setup, this should be the "root" user or your 'your-username' that was set by admin. This is the account that has been configured with your SSH public key. Enter 'your-username' into the "Auto-login username" prompt:

![promt](https://assets.digitalocean.com/articles/putty_do_keys/enter_user.png)

Next, we'll need to click on the "SSH" category in the navigation menu:

![step2](https://assets.digitalocean.com/articles/putty_do_keys/ssh_category.png)

Within this category, click on the "Auth" sub-category.

There is a field on this screen asking for the "Private key file for authentication". Click on the "Browse" button:

![select](https://assets.digitalocean.com/articles/putty_do_keys/browse_keys.png)

Search for the private key file that you saved. This is the key that ends in ".ppk". Find it and select "Open" in the file window:

![pk](https://assets.digitalocean.com/articles/putty_do_keys/open_key.png)

Now, in the navigation menu, we need to return to the "Session" screen that we started at.

This time, we need to create a name for the session that we will be saving. This can be anything, so select something that will help you remember what this is for. When you are finished, click on the "Save" button.
You now have saved all of the configuration data needed to connect to your new server. Just hit "Open" button after dialog on the first time that you connect with the remote host, you will be asked to verify the identity of the remote server. This is expected the first time you connect to a new server, so you can select "Yes" to continue.

## More deatils on this topic

*Server validation*

To prevent a user from logining into a wrong machine accidentally, SSH client maintains the records of machine key signatures in `~/.ssh/known_hosts`. When a SSH client connects to a machine for the first time, a new record (the IP and key signature of the machine), will be appended to the `known_hosts` file. Later when the SSH client connects to the machine again, the key signature provided by the machine this time will be compared with the recorded signature. If they don’t match, the connection attempt will be terminated by SSH client.

In some situations, e.g. reinstallation of operating system or IP being occupied by another machine, the records in `~/.ssh/known_hosts` can become invalid. The invalid records must be deleted from `known_hosts` file so that the SSH client will not forbid users’ connection attempt.

*Why bother with it?*

Password is the most common way of user authentication. While using password is very intuitive, it may not be sufficient in some situations when, for example, password is not secure enough (if brute-force attack is a concern), or when entering password is tedious or even impossible (running automated scripts on a cluster of machines).

Public key authentication can provide higher security and more convenience. In a SSH server, the public keys that can be used to login are recorded in `~/.ssh/authorized_keys`. With a private key that matches one of the public key, a SSH client can be granted access to the server.

Public key is the default authentication method of Linux instances in cloud ([Amazon AWS](https://aws.amazon.com/) or [OpenStack](https://www.openstack.org/)). In a Web console of cloud platform, a cloud instance can be configured to using a public key specified by user, which will be inserted into `~/.ssh/authorized_keys` file on boot. After launched, the instance can grant access to any SSH client that uses the corresponding private key. In OpenSSH client, this can be done by using the `-i` parameter, for example, `ssh -i identity.pem ubuntu@cloud-node`.

If the `-i` parameter is not given to specify the file that contains the identity (private key), then OpenSSH client will try to read the identity from the default file `~/.ssh/id_XXX`, whose public key counterpart is `~/.ssh/id_XXX.pub` (where XXX can be either dsa, ecdsa or rsa, representing the type of key). Keys can be generated by `ssh-keygen` utility. As it may become clear by now, appending the public key in your `~/.ssh/id_XXX.pub` to `~/.ssh/authorized_keys` on another machine can grant you access to the machine without entering password.
s

## Future reading

A lot of materials above was taken from next resources:

[1](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2)
[2](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)
[3](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-putty-on-digitalocean-droplets-windows-users)
[4](http://www.tatetian.io/2015/06/15/ssh-essentials-in-three-steps/)
[5](https://www.ssh.com/ssh/key/)
[6](https://www.digitalocean.com/community/tutorials/how-to-troubleshoot-ssh-authentication-issues-on-your-droplet)