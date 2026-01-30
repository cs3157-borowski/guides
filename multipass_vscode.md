# Setting up Multipass with VS Code

This guide will walk you through the steps to set up coding in Visual Studio code (VS Code) using Multipass virtual machines (VMs).
This setup allows you to leverage Linux features without the use of Vim or Nano. This setup can also work in your favorite code editor.

## Prerequisites

Before you begin, ensure you have the following installed on your system:

- [Multipass](https://multipass.run/)
- [Visual Studio Code](https://code.visualstudio.com/)


## Step 1: Create a Multipass VM

Open your terminal and create a new Multipass VM:

```bash
multipass launch lts --name primary --cpus 4 --memory 4G --disk 12G
```

## Step 2: Prepare Your SSH Key

On your MacBook, check if you already have an SSH key pair. Open your terminal and run:

```bash
ls ~/.ssh/id_ed25519.pub
```

If it exists: copy the content to your clipboard: (or manually copy it if `pbcopy` is not working)

```bash
cat ~/.ssh/id_ed25519.pub | pbcopy
```

If it doesn't, generate one first:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Follow the prompts (pressing Enter for defaults is fine), then copy the public key as shown above.

*Note: unless you're working for the NSA, you can leave the passphrase empty for convenience. Usually what ends up happening is you forget your passphrase or write it down somewhere, making the security worthless.*


## Step 3: Add the Key to Your Multipass Instance

You need to tell the Ubuntu instance to trust your MacBook's key. Replace primary with your instance name if it's different.

```bash
# Get the IP address of your instance
# Note: this IP address may change if you restart the instance
multipass list

# Add your key to the instance
multipass exec primary -- bash -c "echo '$(pbpaste)' >> ~/.ssh/authorized_keys"
```

Note, if that doesn't work, you can manually open a shell in the instance and add the key:

```bash
multipass shell primary
# Then inside the instance:
echo 'your-copied-public-key' >> ~/.ssh/authorized_keys
exit
```


## Step 4: Configure VS Code

Install Extension: Ensure you have the Remote - SSH extension installed in VS Code.

Edit Config: Press `Cmd + Shift + P`, type Remote-SSH: Open SSH Configuration File..., and select ~/.ssh/config.

Add Entry: Add the following block (using the IPv4 address from multipass list):

```bash
Host multipass-local-vm
    HostName 192.168.64.x  # Replace with your VM's IP
    User ubuntu
    IdentityFile ~/.ssh/id_ed25519
```


## Step 5: Connect

Click the Remote Window icon (green/blue icon in the bottom-left corner of VS Code).

Select Connect to Host... and choose multipass-local-vm.

A new window will open. Select Linux as the platform if prompted.


## Step 6: Open Your Project

Once connected, you can open a folder on the VM by going to File > Open Folder... and navigating to your desired directory.

### Done! Now you can code in VS Code using your Multipass VM


To fix these errors, you need to properly nest the code block inside the first list item.

In Markdown, if a code block is not indented, it breaks the list. This causes the linter to think the list has ended, so when it sees `2.`, it flags it as an error because a "new" list should start with `1.`.

Here is the corrected Markdown:


## Opening your Project in the Future

In the future, to open your project in VS Code using the Multipass VM, follow these steps:

1.  Start your Multipass VM if it's not already running: `multipass start primary`

2.  Open VS Code.
3.  Click the Remote Window icon (green/blue icon in the bottom-left corner of VS Code).
4.  Select Connect to Host... and choose multipass-local-vm.


## Troubleshooting

- If you encounter permission issues, ensure that the `~/.ssh/authorized_keys` file on the VM has the correct permissions:

```bash
  multipass exec primary -- chmod 600 ~/.ssh/authorized_keys
```

- If the connection fails, double-check the IP address in your SSH config file and ensure the Multipass VM is running.
- If you need to restart the Multipass VM, use:

```bash
  multipass restart primary
```

## Next Steps

- Add an SSH key from your Multipass VM to GitHub, using [this guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)


### Acknowledgements

This guide was written by Amit Aharoni, January 2026
