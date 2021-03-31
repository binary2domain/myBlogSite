+++
title = "Password-less login for Github with SSH"
description = ""
type = ["posts","post"]
tags = [
    "git",
    "ssh",
]
date = "2021-03-31"
categories = [
    "Git",
    "SSH",
]
series = ["Git 101"]
[ author ]
  name = "Binary Domain"
+++

Github repositories by default are secured with username and password. Github also allows access to repositories via SSH login protocol.

The SSH protocol, let's you can connect and authenticate to remote servers and services. 
With SSH keys, you can connect to GitHub without supplying your username and personal access token at each visit or any git operation such as clone, fetch or push.

This quick guide will help you set up a password-less github experience.

#### Checking for existing SSH keys in the system

1. Open a terminal

2. Navigate to the folder `~/.ssh` to check for any existing keys.

```bash
$ ls -al ~/.ssh
# Lists all files in your .ssh directory, if they exist
```
3. Check the directory listing to see if you already have a public SSH key. By default, the filenames of the public keys are one of the following:
    - id_rsa.pub
    - id_ecdsa.pub
    - id_ed25519.pub


If you don't have an existing public and private key pair, or don't wish to use any that are available to connect to GitHub, then [generate a new ssh key.](#generate-a-new-ssh-key)

If you see an existing public and private key pair listed (for example id_rsa.pub and id_rsa) that you would like to use to connect to GitHub, you can [add your ssh key](#add-your-ssh-key) to the ssh-agent.


#### Generate a new SSH key
Follow the steps below if you have no existing SSH keys or would like yo use a new SSH key for Github.

This step section shows how to generate SSH key in Linux. For windows and mac commands guides see [here](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).


1. Open a Terminal

2. Paste the text listed in the command below, substituting in your Github email address.

```bash
$ ssh-keygen -t ed25519 -C "<your github email address here>"
```

This command uses the Edwards-curve Digital Signature Algorithm (EdDSA), a public-key cryptography algorithm to generate public and private key pair.

```bash
> Generating public/private ed25519 key pair.
```

3. When prompted accept default file path location

```bash
> Enter a file in which to save the key (/home/you/.ssh/id_ed25519): [Press enter]
```

4. When prompted enter and confirm a secure passphrase.

```bash
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```


#### Add your SSH key

Adding your pre-existing or newly generated key to ssh-agent can be done (on Linux) with the following steps.

1. Start the `ssh-agent` in the background with the following command.

```bash
$ eval "$(ssh-agent -s)"

> Agent pid 69887

```

If a `pid` is returned then the `ssh-agent` was started successfully.

2. Add your SSH private key to the ssh-agent. If you created your key with a different name, or if you are adding an existing key that has a different name, replace id_ed25519 in the command below with the name of your private key file.

```bash
$ ssh-add -k ~/.ssh/id_ed25519
```

The `-k` option to `ssh-add` adds the credentials to the system keyring which remains active when the user is logged in into the PC and makes the git experience effortless.

Note: On macOS the command is `ssh-add -K ~/.ssh/id_ed25519` and on windows there is no system native keyring. So the `-k` has no effect. Instead to manage SSH keys in windows you can use Pageant which comes with the Putty suite.

3. [Adding SSH key to your Github Account](#adding-ssh-key-to-your-github-account)


#### Adding SSH key to your Github Account

To configure your GitHub account to use your new (or existing) SSH key, you'll also need to add it to your GitHub account.

1. Copy the SSH public key toy your clipboard. Open the public key file `~/.ssh/id_ed25519.pub` file in your favorite text editor and copy the contents of the file to your clipboard.

2. Login into Github.com and in the upper right corner under your profile picture menu click the `Settings`.

3. In the user settings sidebar, click SSH and GPG keys. 

4. Click New SSH key or Add SSH key (green button on the right).

5. In the "Title" field, add a descriptive label for the new key. For example, if you're using a personal PC, you might call this key with the `hostname` of the PC.

6. Paste your key into the "Key" field.

7. Click Add SSH Key.

8. If prompted, confirm your GitHub password.


That's it ! Now when a git repo is cloned/pushed there is no username and password prompt and the git experience is streamlined.