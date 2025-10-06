f you want to run `smbmap` directly by typing `smbmap` in your terminal, you need to set it up as a command. You can create a symbolic link to the `smbmap.py` script, or add an alias to your shell configuration file. Here are the steps for both methods:

### Method 1: Creating a Symbolic Link

1. **Navigate to the Directory**: Open your terminal and navigate to the directory containing `smbmap.py`.
2. **Create the Symbolic Link**: Create a symbolic link in a directory that is in your PATH (e.g., `/usr/local/bin`). You might need `sudo` for this step:
    
    ```Shell
    sudo ln -s $(pwd)/smbmap.py /usr/local/bin/smbmap
    
    ```
    
3. **Make the Script Executable**: Ensure `smbmap.py` is executable:
    
    ```Shell
    chmod +x smbmap.py
    ```