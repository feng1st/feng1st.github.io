---
title: Using Git to Sync Your Obsidian Vault on Android Devices
date: 2024-10-20
type: blog
---

Syncing your Obsidian vault across devices, including Android, can be a challenge without using a paid service like Obsidian Sync. Fortunately, you can use Git to sync your vault efficiently. This guide outlines how to set up Git on your Android device using Termux and Termux Widget, and includes a custom script to make the process interactive and flexible.

<!--more-->

## Step 1: Install Termux and Termux Widget

First, you'll need to install Termux and Termux Widget on your Android device. These are essential tools that allow you to run command-line programs, including Git. You can download the APKs from the [official Termux GitHub page](https://github.com/termux/termux-app) or use F-Droid, an open-source Android app store.

Once installed, you'll need to give Termux access to your storage:

```bash
termux-setup-storage
```

This command allows Termux to interact with your files, including your Obsidian vault.

## Step 2: Set Up Git and GitHub for Repository Manipulation

Now that Termux is set up, you can install Git and configure GitHub for syncing your Obsidian vault. Open Termux and run the following commands:

1. Update Termux’s package list and install Git:

    ```bash
    pkg update && pkg upgrade
    pkg install git
    ```

2. Optionally, install the GitHub CLI tool:

    ```bash
    pkg install gh
    ```

3. Authenticate with your GitHub account:

    ```bash
    gh auth login
    ```

4. Configure Git by setting your name and email:

    ```bash
    git config --global user.name "Your Name"
    git config --global user.email "youremail@example.com"
    ```

At this point, you're ready to clone your repository. To sync your Obsidian vault stored on GitHub, you can use the standard Git commands:

```bash
git pull
git add -A
git commit -m "Backup message"
git push
```

These commands let you pull updates from GitHub, add new or changed files, commit your changes, and push everything back to the cloud.

## Step 3: Create an Interactive Git Sync Script

To make syncing easier, we’ll create a custom bash script that allows you to interactively manage your Git workflow. Here’s the script, assuming your Obsidian vault is stored in the folder named **Obsidian** on your Android device:

```bash
#!/bin/bash

# Define the working directory
WORKING_DIR="storage/shared/Obsidian"

# Navigate to the working directory
cd "$WORKING_DIR" || { echo "Directory not found!"; exit 1; }

# Start the main loop
while true; do
    # Clear the screen
    clear
    
    # Run git status
    git status
    
    # Prompt the user with options
    echo
    echo "Pu[l]l, [D]iff, [A]dd, [C]ommit, [P]ush, or [Q]uit"
    read -n1 -p "Please select an option: " option
    echo
    
    # Handle the user's input
    case $option in
        [lL])
            echo "Running git pull -r --ff-only --autostash..."
            git pull -r --ff-only --autostash
            echo "Press any key to continue..."
            read -n1 -r
            ;;
        [dD])
            echo "Running git diff HEAD..."
            git diff HEAD
            echo "Press any key to continue..."
            read -n1 -r
            ;;
        [aA])
            echo "Running git add -A..."
            git add -A
            ;;
        [cC])
            COMMIT_MESSAGE="android vault backup: $(date +"%Y-%m-%d %H:%M:%S")"
            echo "Running git commit -a -m \"$COMMIT_MESSAGE\"..."
            git commit -a -m "$COMMIT_MESSAGE"
            echo "Press any key to continue..."
            read -n1 -r
            ;;
        [pP])
            echo "Running git push..."
            git push
            echo "Press any key to continue..."
            read -n1 -r
            ;;
        [qQ])
            echo "Exiting..."
            break
            ;;
        *)
            echo "Invalid option. Press any key to continue..."
            read -n1 -r
            ;;
    esac
done
```

### How the Script Works

This script provides an interactive menu where you can:

- Pull the latest changes (`L`)
- View the differences in your files (`D`)
- Add all changes to the staging area (`A`)
- Commit with a timestamped message (`C`)
- Push your changes to the repository (`P`)
- Quit the script (`Q`)

The script makes the process straightforward and user-friendly, even if you’re new to Git.

### Creating the Script

1. In Termux, create a directory for your shortcuts:

    ```bash
    mkdir .shortcuts
    ```

2. Use the nano text editor to create the script:

    ```bash
    nano .shortcuts/git-obsidian
    ```

3. Paste the script into the nano editor and save it.

4. Make the script executable:

    ```bash
    chmod +x .shortcuts/git-obsidian
    ```

## Step 4: Add a Termux Widget to Your Home Screen

To easily run the script, add a Termux Widget to your home screen:

1. Long press on your home screen and add a "Termux:Widget".
2. Configure the widget to run the `git-obsidian` script you created.

Now, you can quickly sync your vault by tapping the widget. This will allow you to interactively manage your vault syncing process whenever necessary.

---

\[^1\]: [\[Guide\] Using Git to sync your Obsidian vault on Android devices](https://forum.obsidian.md/t/guide-using-git-to-sync-your-obsidian-vault-on-android-devices)