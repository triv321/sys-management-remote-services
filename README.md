# System Management and Remote Server Operations

## Project Overview

*this is linked to what we did in the last repo pushed - cli_tools(w3d2)>dealing with more commands (main repo - clitool-filecleaner)*

The exercises documented here cover the essential skills for managing any Linux-based server: inspecting and controlling running processes, securing files with a robust permission model, and using the industry-standard SSH protocol to securely connect to and manage a cloud server hosted on Amazon Web Services (AWS).

## Core Skills Mastered

* **Process Management (`ps`, `htop`, `kill`):** Learned to view, monitor, and terminate running programs on a Linux system. This is a fundamental skill for debugging and resource management.
* **File Permissions (`chmod`):** Mastered the use of `chmod` to control file access, specifically the importance of the execute (`x`) permission for running scripts.
* **Remote Access (`ssh`, `scp`):** Gained hands-on experience with the complete lifecycle of a cloud server, from provisioning to remote access and file transfer.

## Real-World Problem Solving

During these exercises, several real-world challenges were encountered and overcome. Documenting these-

### Challenge 1: `chmod` Permissions Not Applying
* **The Problem:** When initially attempting the permissions exercise, the `chmod` command failed to change the permissions of the script file. The file remained fully executable (`-rwxrwxrwx`) regardless of the command used.
* **Root Cause Analysis:** After investigation, it was discovered that this issue stems from a filesystem conflict. The project files were located on the Windows `C:` drive (accessed via `/mnt/c` in WSL), which uses the NTFS filesystem. The Linux `chmod` command cannot reliably enforce POSIX permissions on this non-native filesystem.
* **The Solution:** The resolution was to move the entire project into the native Linux home directory (`~`), which uses the ext4 filesystem. Once inside this environment, `chmod` behaved exactly as expected, providing a crucial lesson on the importance of the development environment's underlying filesystem.

### Challenge 2: SSH "Unprotected Private Key File" Error
* **The Problem:** The initial attempt to connect to the newly created EC2 instance via `ssh` failed with a "Permissions are too open" error, and the client refused to use the private key.
* **Root Cause Analysis:** This is a deliberate and critical security feature of SSH. The client will not use a private key file that other users on the system have permission to read. My `.pem` key file had overly permissive settings from its default download location.
* **The Solution:** The issue was resolved by using the `chmod 400` command on the `.pem` file. This restricts the file's permissions to be "read-only" and accessible *only* by the file's owner, satisfying the SSH client's security requirements and allowing the connection to succeed.

## The Remote Server Workflow

This was the capstone exercise for the segment: a complete, end-to-end workflow for provisioning, accessing, and managing a real virtual server in the cloud.

1.  **Provisioning with AWS EC2**: A free-tier `t3.micro` virtual server was launched using the AWS EC2 console. This involved selecting an operating system (Amazon Linux), configuring network rules, and creating a secure key pair.

2.  **Secure Key Management**: A `.pem` private key file was generated, moved to the local `~/.ssh/` directory, and secured with `chmod 400`.

3.  **Connecting with `ssh` (Secure Shell)**: The `ssh` command was used to create a secure, encrypted terminal session on the remote EC2 instance, providing direct command-line access.

4.  **Transferring Files with `scp` (Secure Copy)**: The `scp` command was used to securely upload a local script to the home directory of the remote server, simulating a code deployment.

5.  **Termination**: Finally, the EC2 instance was terminated through the AWS console to ensure proper resource cleanup and cost management.

## Key Takeaways

* **Environment Matters:** The filesystem you work in has real consequences for how standard tools behave. For Linux development on Windows, the Linux home (`~`) is the most reliable location.
* **Security by Default:** Modern tools like SSH have security principles built-in. Understanding these principles (like private key permissions) is not optional.
* **The Cloud is Tangible:** The exercise demystified the cloud, showing that a "cloud server" is simply a Linux computer that can be controlled from anywhere using the same fundamental commands.

# License
MIT