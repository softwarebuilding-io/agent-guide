# SSH Key and Agent Setup Guide

This guide explains how to automate the process of starting the SSH agent and adding your SSH keys when using Git Bash on Windows. This is useful for seamless authentication with services like GitHub without the need to manually add your SSH keys each time you start a new session.

## Starting the SSH Agent in Git Bash

1. **Edit the `.bashrc` File**: Open Git Bash and edit the `.bashrc` file in your home directory. You can create this file if it doesn't exist.

    ```bash
    vi ~/.bashrc
    ```

2. **Add the SSH Agent Script**: Append the following script to your `.bashrc` file. This script checks if the SSH agent is running and starts it if necessary. It also adds your specified SSH keys to the agent.

    ```bash
    # Start SSH Agent
    env=~/.ssh/agent.env
    
    agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }
    
    agent_start () {
        (umask 077; ssh-agent >| "$env")
        . "$env" >| /dev/null ; }
    
    agent_load_env
    
    # Agent running?
    if ! ssh-add -l >| /dev/null; then
        agent_start
        ssh-add ~/.ssh/account1
        ssh-add ~/.ssh/account2
    fi
    
    unset env
    ```

   Replace `~/.ssh/account1` and `~/.ssh/account2` with the actual paths to your SSH keys.

3. **Save and Apply Changes**: Save your changes to `.bashrc`. If you're unfamiliar with `vi`, you can save changes by pressing `Esc`, typing `:wq`, and then pressing `Enter`. Apply the changes by sourcing the file or restarting Git Bash.

    ```bash
    source ~/.bashrc
    ```

## Resolving Startup File Warning

If you receive a warning about missing startup files like `.bash_profile`, `.bash_login`, or `.profile`, follow these steps to resolve it:

1. **Create `.bash_profile`**: In Git Bash, create a new `.bash_profile` in your home directory.

    ```bash
    vi ~/.bash_profile
    ```

2. **Source `.bashrc`**: Add the following lines to `.bash_profile` to ensure `.bashrc` is sourced during login.

    ```bash
    if [ -f ~/.bashrc ]; then
      source ~/.bashrc
    fi
    ```

3. **Save and Apply Changes**: Save your changes to `.bash_profile`. To save in `vi`, press `Esc`, type `:wq`, and press `Enter`. Apply the changes by sourcing `.bash_profile` or restarting Git Bash.

    ```bash
    source ~/.bash_profile
    ```

This setup ensures that your SSH keys are automatically added to the SSH agent, simplifying SSH authentication in your development workflow.
