# TR1X Developer Environment Guide for Windows 10
## GitHub
1. Create a [GitHub](https://www.github.com) account.
1. Go to the [TR1X repository](https://github.com/LostArtefacts/TR1X).
    * This is where the primary codebase (repository) lives and new releases are published.
    * The issue tracker is also located on this repo.

1. Click "Fork" in the top right of the page.
    * A fork is a copy of the repo under your username.
    * The master branch of the forked repo synchronizes to the primary repo's master branch.
    * On your repo's GitHub page, click "Fetch upstream" and then "Fetch and merge" to synchronize your repo's master branch to the primary repo when there are new commits.

## Creating a Development Environment
### Installing WSL
1. To build the code on Windows, WSL is recommended.
    * The Windows Subsystem for Linux lets developers run a GNU/Linux environment -- including most command-line tools, utilities, and applications -- directly on Windows unmodified, without the overhead of a traditional virtual machine or dualboot setup. [More info.](https://docs.microsoft.com/en-us/windows/wsl/about)
1. Follow the [Install WSL](https://docs.microsoft.com/en-us/windows/wsl/install) guide from Microsoft.
    * Windows 10 version 2004 and higher (Build 19041 and higher) or Windows 11 are required to use WSL.
1. After a reboot, the default Linux distribution finishes installing. Currently that is Ubuntu 20.04.
1. Enter a username and password to use for Ubuntu in the terminal window.
    * WSL stores its files in a different location than normal Windows files.
    * To open the location in File Explorer for easier access, type `cd ~` and then `explorer.exe .` in the Ubuntu terminal.
    * This opens File Explorer in the Ubunutu user's home folder.
    * To pin this location to the File Explorer navigation pane, drag the folder icon from the top of the address bar to the navigation pane's quick access near other folders like `Documents` or `Downloads`.
1. The Ubuntu environment can be navigated using the app's terminal.

### Installing Windows Terminal (Optional)
1. Another good Ubuntu terminal option is [Windows Terminal](https://www.microsoft.com/store/productId/9N0DX20HK701).
1. Windows Terminal opens PowerShell by default, but clicking the down arrow near the top gives the option to open Command Prompt or Ubuntu.
1. The default profile can be changed in the settings to open Ubuntu.
1. Windows Terminal doesn't open the Ubuntu home folder by default, so type `cd ~` to go to the Ubuntu home folder.

### Installing Docker Desktop for Windows
1. Download and install [Docker Desktop for Windows](https://docs.docker.com/desktop/windows/install/).
1. Make sure to keep "Install required Windows components for WSL 2" selected during the installation.
1. At the end of the installation, a Windows log out is required.
1. Open the Docker app, and click the gear button to open settings.
1. Click `Resources` on the left.
1. Click `WSL Integration` in the dropdown under `Resources`.
1. Make sure `Enable integration with my default WSL distro` is checked.
1. Under `Enable integration with additional distros`, turn on `Ubuntu`.
1. Click `Apply & Restart` at the bottom.

### Cloning Your TR1X Forked Repo
1. Go to your forked repo's GitHub page.
1. Click the green `Code` button, under `Clone`, copy the `HTTPS` link to the repo.
1. From the home directory in the terminal, type `git clone LINK` where LINK is the pasted link you copied.
    * Your repo should now clone be cloned into a `TR1X` folder.

### Installing Visual Studio Code (Optional)
1. Download and install [Visual Studio Code](https://code.visualstudio.com/), a source code editor from Microsoft.
1. VS Code can be used for [Developing in WSL](https://code.visualstudio.com/docs/remote/wsl).
1. You can log into your GitHub account in VS Code.
1. Click `Extensions` on the left side of the window.
1. Download the `Remote Development` extension from Microsoft.
1. Click `File` at the top of the window and click `Open Folder`.
1. Navigate to your cloned `TR1X` folder and click `Select Folder`.
    * Pinning the Ubuntu folder to your navigation pane as detailed in [Installing WSL](#installing-wsl) makes finding the repo's folder much easier. Open the `TR1X` folder from the Ubuntu home folder. 
    * If you did not pin the Ubuntu folder, navigate to `\\wsl$\Ubuntu\home\USERNAME` in the address bar where USERNAME is the Ubuntu name you chose.
    * When you open the repo, there may be a security popup. Click that you trust the author of the repo.
1. (Optional) If you want to use Git in the VS Code GUI, download and install [Git](https://git-scm.com/). VS Code should have a popup in the bottom right of the screen if it's not installed.
1. If you want to use the VS Code built in terminal, click `File`, `Preferences`, and then `Settings`.
1. Search for `terminal.integrated.defaultProfile.windows` and change the default terminal profile to `Ubuntu (WSL)`.

### Building TR1X 
1. In an Ubuntu terminal, type:
    * `sudo apt-get update` and enter your Ubuntu password.
    * `sudo apt-get upgrade` and press `y` to confirm.
1. Install `make` by typing:
    * `sudo apt-get install make`
1. To be able to format the code to match the repo's coding style, install `clang-format` by typing:
    * `sudo apt-get install clang-format`
1. Install `clang-format-12` by typing:
    * `sudo apt-get install clang-format-12`
1. To set the default command `clang-format` to use version 12 instead of version 10 to match the repo, type:
    * `sudo update-alternatives --install /usr/bin/clang-format clang-format /usr/bin/clang-format-12 100`
1. Finally, run `make debug` to build the project.
    * The first build has to download and create the Docker environment, but subsequent builds are quicker.
1. Running `make lint` formats the code to match the coding style of the project.
1. Copy all `.dll` and `.exe` files from `build/` to your Tomb Raider directory.
1. Copy the contents of `bin/` to your game directory.
1. For subsequent builds, always run `make clean` before building another debug build to clean the environment to avoid glitches.
1. The game directory also needs the levels, music, and FMVs to run the game.
1. See the [Contributing](https://github.com/LostArtefacts/TR1X/blob/master/CONTRIBUTING.md) guide in the TR1X repo for more information.

## Using Git to Track Code Changes
1. Git is used to safely develop new code without losing or overwriting old code. Git also eases simultaneous development by multiple developers.
1. Type `git config --global user.email "you@example.com"` using your GitHub account email address.
1. Type `git config --global user.name "Your Name"` using your GitHub username.
    * These steps tell Git who is making the commits to log that with your commit messages.
    * This also allows you to push to your GitHub repo.
    * For advanced users, you can leave out `--global` and configure your Git credentials on a per repo basis if you use multiple accounts.
1. Typing `git status` in the terminal in your repo's folder shows if there are any code modifications that have not been committed.
    * Your repo should be clean on the master branch to start.
1. To make a new branch of code from the current master branch, type `git checkout -b branch-name` where branch name can be something like `alligator-fix`.
1. On a branch of code, make any code changes related to that feature, bug fix, etc.
1. When a significant amount of progress has been reached, type `git status` to see any files that have been changed but not yet committed.
1. Type `git add filename` for each file that has changed that you want to commit.
1. When all files have been staged to be committed, type `git commit`.
1. Write a short commit message to describe the changes you made such as `Fix the alligator AI when Lara is still in the water`.
1. With those changes committed, type `git push` to push the branch and the changes to your forked repo's GitHub page.
    * The first time pushing a new branch requires `git push --set-upstream origin branch-name` to create the new branch on GitHub.
1. When your changes are ready to be reviewed, go to the main repo's [Pull requests](https://github.com/LostArtefacts/TR1X) and click `New pull request`.
1. Keep the `base:master` and select your branch in `compare:branch-name` then click `Create pull request`.
1. There's much more to Git including merging, pulling, diffs, keeping your master branch synchronized, etc. Some documentation and tutorials are available on the [Git website](https://git-scm.com/docs/gittutorial).