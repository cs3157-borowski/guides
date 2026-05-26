# Setting up Docker with VS Code

This guide will walk you through editing your code in Visual Studio Code (VS Code) while running and compiling it inside your Docker container. 
Note: This will **NOT** work on the `BSB` server due to the intensive computing resources VS Code requires.

## Prerequisites

Before you begin, ensure you have:

- Docker set up and running using the [ap-env guide](https://github.com/CUAdvProg/ap-env)
- [Visual Studio Code](https://code.visualstudio.com/) installed
- Two terminal windows open:
  - One **inside your Docker container** (from running `./run_docker.sh`)
  - One on your **local machine**

## Step 1: Install the Dev Containers Extension

Open VS Code and install the [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension by Microsoft.

## Step 2: Make Sure Your Container Is Running

In your local terminal, navigate to your `ap-env` folder and start the container if it isn't already:

```bash
./run_docker.sh
```

You should now have a shell inside the container. Keep this window open.

## Step 3: Attach VS Code to the Container

1. Open VS Code.
2. Press `Cmd+Shift+P` (Mac) or `Ctrl+Shift+P` (Windows) to open the Command Palette.
3. Type **Dev Containers: Attach to Running Container...** and select it.
4. Choose the `ap-env` container from the list.

A new VS Code window will open connected to the container.

## Step 4: Open Your Project

In the new VS Code window, go to **File > Open Folder...** and navigate to `/ap`. This is your working directory inside the container, and it mirrors the `ap-env` folder on your local machine — so any files you edit in VS Code will instantly be available inside the container.

### Done! You can now edit code in VS Code and compile/run it in your Docker terminal.

## Opening Your Project in the Future

1. Start the container: run `./run_docker.sh` from your `ap-env` folder.
2. Open VS Code.
3. Press `Cmd+Shift+P` / `Ctrl+Shift+P`, select **Dev Containers: Attach to Running Container...**, and choose the `ap-env` container.

## Troubleshooting

- **Container not appearing in the list:** Make sure the container is running (`docker ps` in your local terminal should show it). If it's not, run `./run_docker.sh` from your `ap-env` folder.
- **Can't find the `/ap` folder:** The `/ap` directory is only populated when the container is running with the volume mount from `run_docker.sh`. Make sure you started the container with that script, not `docker run` directly.
- **Dev Containers extension not working / repeated errors:** See the [SSH fallback section](#alternative-ssh-into-the-container-if-dev-containers-extension-fails) below.

---

## Alternative: SSH into the Container (if Dev Containers Extension Fails)

If the Dev Containers extension is misbehaving — crashing, failing to attach, throwing cryptic errors, or simply not finding your container — you can connect to the container over SSH instead. VS Code has a separate **Remote - SSH** extension that is often more reliable and doesn't require Docker-specific tooling.

This section covers two approaches:
- **Option A:** Use VS Code's Remote - SSH extension (gives you a full VS Code editing experience inside the container, just like Dev Containers)
- **Option B:** Use a plain terminal SSH session (no VS Code, but useful for quick edits or debugging)

---

### Part 1: Expose the SSH Port When Starting the Container

By default, `run_docker.sh` does not map the container's SSH port to your local machine. You need to add a port mapping so your local machine can reach the container over SSH.

#### Check whether `run_docker.sh` already exposes a port

Open `run_docker.sh` in a text editor and look for a `-p` flag in the `docker run` command. If you see something like `-p 2222:22`, SSH is already mapped and you can skip to [Part 2](#part-2-start-the-ssh-server-inside-the-container).

#### Add the SSH port mapping

Find the `docker run` line in `run_docker.sh`. It will look something like:

```bash
docker run -it --rm -v "$(pwd):/ap" --name ap-env ap-env-image
```

Add `-p 2222:22` to it:

```bash
docker run -it --rm -v "$(pwd):/ap" -p 2222:22 --name ap-env ap-env-image
```

This maps port **2222 on your local machine** to port **22 inside the container**. Port 2222 is used to avoid conflicts with any SSH server that may already be running on your local machine.

> **If `run_docker.sh` uses a different structure** (e.g. `docker compose`), add the following under your service's `ports:` key in `docker-compose.yml` instead:
> ```yaml
> ports:
>   - "2222:22"
> ```

After editing, stop any running container (`exit` in the container shell, or `docker stop ap-env` from your local terminal) and relaunch with `./run_docker.sh`.

---

### Part 2: Start the SSH Server Inside the Container

Once the container is running (via `./run_docker.sh`), you need to make sure the SSH server (`sshd`) is installed and running inside it.

In your **container shell**, run:

```bash
# Check if openssh-server is installed
which sshd
```

If you get a path back (e.g. `/usr/sbin/sshd`), it's already installed. If not, install it:

```bash
# Debian/Ubuntu-based containers
apt-get update && apt-get install -y openssh-server

# Alpine-based containers
apk add --no-cache openssh
```

Then start the SSH daemon:

```bash
mkdir -p /run/sshd
/usr/sbin/sshd
```

Verify it's running:

```bash
ps aux | grep sshd
```

You should see a `sshd` process listed.

> **Note:** The SSH server only runs for the lifetime of that container session. Each time you start the container fresh, you'll need to run `/usr/sbin/sshd` again. You can add this line to your container's startup script or shell profile (e.g. `~/.bashrc`) to automate it.

---

### Part 3: Set Up Authentication

You need a way to authenticate when SSH-ing into the container. The two options are **password** or **SSH key**. Keys are strongly recommended.

#### Option 3a: Password authentication (easier to set up, less secure)

In your container shell, set a password for the current user (usually `root`):

```bash
passwd
```

Enter and confirm a password. Then enable password authentication in the SSH config:

```bash
# Make sure this line exists and is not commented out in /etc/ssh/sshd_config
grep -q "^PermitRootLogin" /etc/ssh/sshd_config \
  && sed -i 's/^PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config \
  || echo "PermitRootLogin yes" >> /etc/ssh/sshd_config

grep -q "^PasswordAuthentication" /etc/ssh/sshd_config \
  && sed -i 's/^PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config \
  || echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config

# Restart sshd to apply changes
pkill sshd; /usr/sbin/sshd
```

#### Option 3b: SSH key authentication (recommended)

**On your local machine**, check if you already have an SSH key:

```bash
ls ~/.ssh/id_ed25519.pub
```

If the file doesn't exist, generate a new key pair:

```bash
ssh-keygen -t ed25519 -C "ap-env-docker"
```

Press Enter to accept the default location (`~/.ssh/id_ed25519`) and optionally set a passphrase.

**Copy your public key into the container.** In your **container shell**, run:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Then, in a **local terminal**, copy your public key:

```bash
cat ~/.ssh/id_ed25519.pub | docker exec -i ap-env bash -c "cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

Verify the key was added inside the container:

```bash
# Run this in your container shell
cat ~/.ssh/authorized_keys
```

You should see your public key listed.

---

### Part 4, Option A: Connect with VS Code Remote - SSH

This gives you a full VS Code window inside the container, just like the Dev Containers approach.

#### 1. Install the Remote - SSH extension

Open VS Code, go to the Extensions panel (`Cmd+Shift+X` / `Ctrl+Shift+X`), and search for **Remote - SSH** by Microsoft. Install it.

#### 2. Add the container to your SSH config

Open (or create) `~/.ssh/config` on your local machine and add:

```
Host ap-env-docker
    HostName localhost
    Port 2222
    User root
    IdentityFile ~/.ssh/id_ed25519
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null
```

> Replace `root` with whatever user is active inside your container if it's not root (run `whoami` inside the container to check).
>
> `StrictHostKeyChecking no` and `UserKnownHostsFile /dev/null` prevent SSH from complaining when the container's host key changes across restarts — which it will.

#### 3. Connect from VS Code

1. Open VS Code.
2. Press `Cmd+Shift+P` / `Ctrl+Shift+P` to open the Command Palette.
3. Type **Remote-SSH: Connect to Host...** and select it.
4. Choose `ap-env-docker` from the list (it reads from your `~/.ssh/config`).

A new VS Code window will open connected to the container over SSH. Then go to **File > Open Folder...** and navigate to `/ap`, just as you would with the Dev Containers approach.

#### 4. Test that it's working

Open the integrated terminal in VS Code (`Ctrl+\`` or **Terminal > New Terminal**). You should see a shell prompt inside the container. Run:

```bash
hostname
ls /ap
```

If you see the container hostname and your project files, you're in.

---

### Part 4, Option B: Connect via Terminal SSH (No VS Code)

If you just need a command-line session inside the container — for example to run or debug a program — you can SSH in directly from your local terminal without VS Code at all.

Make sure the container is running and `sshd` is started (see Parts 1–3), then:

```bash
ssh -p 2222 root@localhost
```

Or, if you set up `~/.ssh/config` as described above:

```bash
ssh ap-env-docker
```

You'll land in a shell inside the container. Navigate to your project:

```bash
cd /ap
```

You can use any terminal-based editor here (`vim`, `nano`, `emacs`) or simply run and test your code.

---

### Automating SSH Server Startup

Having to run `/usr/sbin/sshd` every time you start the container gets old quickly. Here are two ways to automate it.

#### Approach 1: Add it to your shell's RC file

In your container shell, add the following to `~/.bashrc` (or `~/.zshrc` if you use zsh inside the container):

```bash
# Start sshd if not already running
if ! pgrep -x sshd > /dev/null; then
    mkdir -p /run/sshd
    /usr/sbin/sshd
fi
```

This starts `sshd` automatically whenever you open a shell in the container. Since `run_docker.sh` opens an interactive shell, `sshd` will be running by the time you try to SSH in.

#### Approach 2: Modify run_docker.sh to start sshd explicitly

Edit `run_docker.sh` and add the sshd startup before or alongside the shell invocation. For example, if the script ends with:

```bash
docker exec -it ap-env bash
```

Change it to:

```bash
docker exec ap-env bash -c "mkdir -p /run/sshd && /usr/sbin/sshd 2>/dev/null; true"
docker exec -it ap-env bash
```

---

### Troubleshooting SSH

- **`Connection refused` on port 2222:** The SSH daemon isn't running inside the container. Open a shell via `docker exec -it ap-env bash` and run `mkdir -p /run/sshd && /usr/sbin/sshd`.
- **`Connection refused` immediately (port not found):** The container wasn't started with `-p 2222:22`. Stop the container and relaunch `./run_docker.sh` after adding the port mapping.
- **`Permission denied (publickey)`:** The public key isn't in `~/.ssh/authorized_keys` inside the container, or the permissions are wrong. Re-run the key copy step and check `chmod 600 ~/.ssh/authorized_keys` inside the container.
- **`Permission denied (password)`:** Password auth may be disabled in `sshd_config`. Run the `sed` commands from Option 3a to enable it, then restart `sshd`.
- **Host key changes warning / "REMOTE HOST IDENTIFICATION HAS CHANGED":** This is expected — the container's SSH host key is regenerated each time the container is rebuilt. The `StrictHostKeyChecking no` setting in `~/.ssh/config` suppresses this. If you didn't add that setting, run `ssh-keygen -R "[localhost]:2222"` on your local machine to clear the old key.
- **VS Code Remote-SSH keeps failing to install server:** VS Code tries to install a helper inside the container. Make sure the container has internet access and that `/tmp` is writable inside it. You can test with `curl https://example.com` inside the container.
- **`sshd: no hostkeys available`:** The SSH server needs host keys generated. Run inside the container: `ssh-keygen -A` to generate them, then restart `sshd`.

---

### Acknowledgements

This guide was written by Amit Aharoni, May 2026
