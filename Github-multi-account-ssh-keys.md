# Using Github with multiple accounts and SSH keys

## Inital “main” account with SSH key
 
Create an SSH keypair on your computer (ssh-keygen).

```
ssh-keygen -t rsa -b 4096 -C "comment"
```

Note: Different versions of ssh-keygen can output keys in different formats. If you examine the public key file, if it starts with:

> ssh-rsa XXXXXXXXXX

You have an **RSA** formatted public key. If it starts with:

> ---- BEGIN SSH2 PUBLIC KEY ----
>Comment: "rsa-key-20221026"

You have an **SSH2** formatted key.

## Converting public keys

You might need to convert between the formats for certain systems/applications.

To convert a public key from **RSA to SSH2**.

```
ssh-keygen -f ~/.ssh/id_rsa.pub -e -m RFC4716 > ~/.ssh/id_rsa_ssh2.pub
```

To convert a public key from **SSH2 to RSA**.

```
ssh-keygen -i -f ~/.ssh/id_rsa_ssh2.pub > ~/.ssh/id_rsa.pub
```

Set the file permissions

```
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
```

Add a config file

Create a `~/.ssh/config` file with the following contents

```
# github.com - main account
Host github.com
  Hostname github.com
  User git
  IdentityFile ~/.ssh/id_rsa
```

Log into your main Github account and paste the public key contents

`github.com => Settings => SSH and GPG keys => New SSH key`

Your main Github account will now work with your main SSH key.

You will be able to clone and commit/push using the default SSH URLs

```
git clone git@github.com:<account-name>/<repo-name>.git
```

## Adding a second SSH key and Github account.


Create a second SSH keypair on your computer (ssh-keygen)

This time, we need to use a different filename to store these new keypair files.

```
ssh-keygen -t rsa -b 4096 -C "comment" -f ~/.ssh/second_key
```

This will result in the files:

```
~/.ssh/second_key
~/.ssh/second_key.pub
```

Again, make sure your public key is in the correct format (see above for format conversion)


Set the file permissions

```
chmod 600 ~/.ssh/second_key
chmod 644 ~/.ssh/second_key.pub
```

## Update the SSH configuration file

Append this after the main entry (note the Host difference !!)

```
# github.com - second account'
Host github.com-second   # Note the -second after the Host
  Hostname github.com
  User git
  IdentityFile ~/.ssh/second_key
```

Log into your secondary Github account and paste the public key contents

`github.com => Settings => SSH and GPG keys => New SSH key`

Your secondary Github account will now work with your main SSH key, with the below caveat.

You will be able to clone and commit/push using modified SSH URLs (note the `-second` after the hostname)

```
git clone git@github.com-second:<account-name>/<repo-name>.git
```

NOTE: The Github GUI buttons for copy/paste of a repo are hardcoded to:

```
git clone git@github.com:<blah blah blah>
```

But we need to use a modified hostname to match the ‘Host’ customisation in the SSH configuration file:

```
git clone git@github.com-second:
```

So you will either have to always type the clone URL, or if you copy paste from the secondary account URL from the GUI, you will need to edit that hostname.

You can repeat this process to add as many accounts/keys as you like.

The `-second` can be anything you like, you just need to set it in the config file and ensure you modify the clone URL to match, i.e.

```
# github.com - thing account
Host github.com-thing   # Note the -thing after the Host
  Hostname github.com
  User git
  IdentityFile ~/.ssh/thing_key
```
```
git clone git@github.com-thing:<account-name>/<repo-name>.git
```

```
# github.com - otherthing account
Host github.com-otherthing   # Note the -otherthing after the Host
  Hostname github.com
  User git
  IdentityFile ~/.ssh/otherthing_key
```

```
git clone git@github.com-otherthing:<account-name>/<repo-name>.git
```

You will only have to modify the clone URL for non main Github accounts. The main/default account will work with the GUI button copy/paste URL.
