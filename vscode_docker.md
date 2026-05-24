# Setting up Docker with VS Code

This guide will walk you through editing your code in Visual Studio Code (VS Code) while running and compiling it inside your Docker container.

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

### Acknowledgements

This guide was written by Amit Aharoni, May 2026
