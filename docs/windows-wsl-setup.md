### Step 1: Create the Documentation File

In your forked repository, create a new file at `/docs/windows-wsl-setup.md` and
paste this exact content:

````markdown
# Windows Development Setup (WSL)

Gemini CLI development relies heavily on Linux-native features, including
sandboxing and specific path resolutions. For developers on Windows, attempting
to build or run the project natively (win32) can result in agent crashes,
freezing, and native dependency build errors (like `node-gyp`).

To ensure a stable and performant development environment on Windows, we
strongly recommend using the **Windows Subsystem for Linux (WSL)**.

## 1. Install WSL and Ubuntu

If you do not have WSL installed, open a PowerShell terminal as Administrator
and run:

```bash
wsl --install -d Ubuntu
```
````

Restart your computer if prompted, then open the "Ubuntu" app from your Start
menu to complete the initial user setup.

## 2. Clone in the Linux Home Directory

**⚠️ Critical Performance Warning:** Do **not** clone the Gemini CLI repository
into your mounted Windows C: drive (e.g., `/mnt/c/Users/Name/...`). Doing so
will cause severe performance degradation and file permission errors during the
build process.

Instead, always clone the repository directly into your Linux home directory
(`~`):

```bash
cd ~
git clone [https://github.com/google-gemini/gemini-cli.git](https://github.com/google-gemini/gemini-cli.git)
cd gemini-cli

```

## 3. Install Node.js via NVM

The Gemini CLI requires a specific Node.js version (`~20.19.0`). We recommend
using Node Version Manager (`nvm`) inside WSL to handle this:

1. Install `nvm` by following the
   [official instructions](https://www.google.com/search?q=https://github.com/nvm-sh/nvm%23installing-and-updating).
2. Install and use the required Node.js version:

```bash
nvm install 20.19.0
nvm use 20.19.0

```

## 4. Install Dependencies

You may need the standard Linux build tools to compile native dependencies:

```bash
sudo apt-get update
sudo apt-get install build-essential

```

Once installed, you can build the project as usual:

```bash
npm install
npm run build

```

## 5. IDE Integration (VS Code)

To write code from Windows but execute it inside your Linux environment:

1. Install the
   [WSL Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
   in VS Code.
2. Open your Ubuntu terminal, navigate to the cloned `gemini-cli` folder, and
   type:

```bash
code .

```

This will open VS Code connected directly to your WSL environment, ensuring all
terminal commands and debuggers run natively in Linux.

````

### Step 2: Update the Sidebar
According to their documentation process, you must link this new file. Open `/docs/sidebar.json` and add an entry for your new guide under the "Development setup" or "Getting Started" section:
```json
{
  "title": "Windows Setup (WSL)",
  "path": "/docs/windows-wsl-setup.md"
}

````

### Step 3: Execute the Git Workflow

Their `CONTRIBUTING.md` demands you run preflight checks and use conventional
commits. Run these exact commands in your terminal:

1. Create your branch: `git checkout -b docs/wsl-setup`
2. Run their required preflight check to ensure the linter is happy:
   `npm run preflight`
3. Commit using their required format (and linking the issue):
   `git add docs/windows-wsl-setup.md docs/sidebar.json`
   `git commit -m "docs: add Windows WSL setup guide"`
4. Push to your fork: `git push origin docs/wsl-setup`
