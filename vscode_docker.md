# Setting up Docker with VS Code

This guide walks you through setting up VS Code to work with the course Docker environment.
With Docker, you edit files on your host machine using VS Code while the container provides a consistent Linux environment for compiling and running your code — no SSH tunneling or virtual machine setup needed.

## Prerequisites

Before you begin, ensure you have [Visual Studio Code](https://code.visualstudio.com/) installed. Docker installation is covered in Step 3.


## Step 1: Set Up SSH Keys for GitHub

To clone the course repository, your computer needs to communicate with GitHub over SSH.

If you have never set up GitHub SSH keys before, follow GitHub's official guide:

> [Generating a new SSH key and adding it to the SSH agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

That page walks you through creating a key and registering it with your GitHub account. Once done, you can `git clone` and `git pull` without entering a password.


## Step 2: Clone the Course Environment Repository

Pick a **permanent folder** on your computer where you will keep your course work. This folder is shared with the container, so do not move or rename it later.

```bash
# macOS / Linux / WSL (Ubuntu terminal)
mkdir -p ~/cs3157
cd ~/cs3157
git clone git@github.com:CUAdvProg/ap-env.git
cd ap-env
```

After cloning, the folder will contain `setup_docker.sh` and `run_docker.sh`.


## Step 3: Install Docker

### macOS

1. Download and install **Docker Desktop for Mac**:
   > [https://docs.docker.com/desktop/install/mac-install/](https://docs.docker.com/desktop/install/mac-install/)

   Choose the installer that matches your chip (Apple Silicon or Intel — check via Apple menu → About This Mac).

2. Open the `.dmg` file and drag Docker to your Applications folder.

3. Open Docker from Applications. Wait until the whale icon in the menu bar stops animating and shows "Docker is running."

4. You do not need a Docker account — skip or dismiss any sign-in prompt.

### Windows (WSL)

Docker on Windows requires WSL (Windows Subsystem for Linux).

**Step A — Install WSL with Ubuntu (once)**

1. Open PowerShell as Administrator (Start → PowerShell → right-click → "Run as administrator").
2. Run:
   ```powershell
   wsl --install
   ```
3. Restart your computer when prompted.
4. After reboot, an Ubuntu terminal opens automatically. If it does not, open "Ubuntu" from the Start Menu.
5. Create a Linux username and password when prompted.

**Step B — Install Docker Desktop for Windows**

1. Download and install from:
   > [https://docs.docker.com/desktop/install/windows-install/](https://docs.docker.com/desktop/install/windows-install/)

2. During installation, ensure "Use WSL 2 instead of Hyper-V" is checked.

3. Open Docker Desktop and wait until the whale icon says "Docker is running."

4. Go to **Settings → Resources → WSL Integration** and enable your Ubuntu distro.

**Step C — Clone inside WSL**

Open your Ubuntu terminal and follow Step 2 above.

> **Important:** Always work inside the WSL filesystem (paths like `/home/yourname/...`), not `/mnt/c/...`. The Windows C: drive path is slower and causes permission issues.

### Linux

1. Install Docker Engine:
   > [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)

2. Add your user to the `docker` group so you do not need `sudo` every time:
   ```bash
   sudo usermod -aG docker $USER
   ```
   Then log out and log back in.

3. Clone the repo as in Step 2.


## Step 4: Make the Scripts Executable

From inside the `ap-env` folder, run this once:

```bash
chmod +x setup_docker.sh run_docker.sh
```


## Step 5: First-Time Setup

```bash
./setup_docker.sh
```

This pulls the course Docker image and drops you into a shell at `/ap` inside the container. The first pull may take a few minutes.


## Step 6: Open the Folder in VS Code

Open the `ap-env` folder in VS Code: **File → Open Folder...** and select the `ap-env` directory you cloned.

All your course work should live inside this folder — it is what gets mounted into the container at `/ap`.


## Step 7: Edit Files and Run the Container

Open a terminal inside VS Code (**Terminal → New Terminal**) and start the container:

```bash
./run_docker.sh
```

Your prompt will change to:

```
(ap-env) student@<id>:/ap$
```

You are now inside the Linux environment with all course tools installed (`gcc`, `clang`, `make`, `gdb`, `valgrind`, and more).

Edit files in VS Code as normal — every save is instantly visible inside the container at `/ap`. To compile and run:

```bash
cd /ap
make
./my_program
valgrind ./my_program
```

To exit the container:

```bash
exit
```

Your files remain on your host machine.

### Done! Now you can code in VS Code with the Docker environment.


## Opening the Container in the Future

From the `ap-env` folder, simply run:

```bash
./run_docker.sh
```

If the container already exists from a previous session, the script resumes it. If not, it creates a fresh one.


## Optional: Attach VS Code to the Container (Dev Containers)

For a more integrated experience — IntelliSense, extensions, and the file explorer all running inside the container — install the **Dev Containers** extension:

1. Open the Extensions panel (`Cmd/Ctrl + Shift + X`) and search for **Dev Containers** (publisher: Microsoft).
2. Start the container with `./run_docker.sh` in a terminal.
3. Press `Cmd/Ctrl + Shift + P`, type **Dev Containers: Attach to Running Container...**, and select the `ap-env` container.

A new VS Code window opens attached to the container. The file explorer shows `/ap` and the integrated terminal is already inside the container.


## Troubleshooting

**"Permission denied" when running a script**

Run the `chmod` command from Step 4:
```bash
chmod +x setup_docker.sh run_docker.sh
```

**"Docker command not found"**

Docker is not installed or not in your PATH. Revisit Step 3 for your OS.

**"Cannot connect to Docker daemon"**

Docker is installed but not running. Open Docker Desktop (macOS or Windows) and wait until it says "Docker is running", then try again.

**"Permission denied" using Docker on Linux/WSL**

Your user is not in the `docker` group yet:
```bash
sudo usermod -aG docker $USER
```
Then log out and log back in. On WSL, close and reopen the Ubuntu terminal.

**Files don't appear inside the container**

Make sure you ran `./run_docker.sh` from inside the `ap-env` folder, not a different directory.

**The IP address changes / connection drops**

Unlike Multipass, Docker uses bind mounts — there is no IP address to track. If something feels broken, run `./run_docker.sh` again.


## Fairness and Grading Policy

All students are required to compile and test code inside this Docker environment. Grading uses the same Docker image (`ghcr.io/cuadvprog/ap-env:latest`) with the same resource limits (4 CPUs, 4 GB RAM). If your program works outside the container but fails inside, we grade based on its behavior inside.


### Acknowledgements

This guide was written by Amit Aharoni, May 2026
