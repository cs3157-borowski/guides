# SSHing into bsb

Are you tired of entering your password to log into bsb? If so, you’ll want to set up SSH keys to authenticate without a password. This is highly recommended.

First, if you haven’t already, generate an Ed25519 key pair on your local machine and add it to your ssh-agent following this [excellent guide by GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent). And while you’re at it, [add your key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account). This is necessary for authenticating with GitHub since it no longer supports authenticating over HTTPS.

Now that you have a public key, you can pass it to bsb to authenticate logins from your local machine. Assuming that you named your Ed25519 key pair `id_ed25519` (the default), run the following command:

```
[local-computer]$ ssh-copy-id <UNI>@bsb.cs.columbia.edu
```

At this point, you’ll be able to SSH into bsb without being prompted for your password.

If you’re interested in GitHub SSH authentication, you’ll want to forward your host’s SSH credentials to bsb. This enables you to authenticate with GitHub without entering your password or copying your private key.

First, let’s create our SSH `config` file if we haven’t already:

```
[local-computer]$ mkdir -p ~/.ssh
[local-computer]$ touch ~/.ssh/config
```

Here are two configuration options that you can put in `~/.ssh/config` using your favorite text editor:

**Specific Rule (Recommended)**

```
Host bsb
    HostName bsb.cs.columbia.edu
    User <UNI>
    ForwardAgent yes
    AddKeysToAgent yes
```

**Blanket Rule (Not recommended)**

```
Host *
    ForwardAgent yes
    AddKeysToAgent yes
```

This says that for any host you SSH into successfully, forward your credentials. Note that if you decide to do this, you should be careful to only SSH into trusted machines. We avoid having to specify the target machine’s information by applying this blanket rule.

After adding one of the two above configurations, you’ll then be able to SSH into bb with the following shorter command:

```
[local-computer]$ ssh bsb
```
