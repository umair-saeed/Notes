# Linux cheet-sheet

* ### Command Line Interface (CLI): 
	Familiarity with using the command line interface is crucial for navigating the Linux environment, managing files and directories, and executing commands.

	Commands like cd, ls, mkdir, rm, cp, mv, cat, etc., are generally consistent across distributions.

  | Command                                   | Description                                   |
  |-------------------------------------------|-----------------------------------------------|
  | `pwd`                                     | Print the current working directory.          |
  | `ls`                                      | List directory contents.                      |
  | `cd directory_name`                      | Change directory to specified directory.      |
  | `cd ..`                                   | Move to the parent directory.                 |
  | `cd ~`                                    | Move to the home directory.                   |
  | `mkdir new_directory`                     | Create a new directory.                       |
  | `touch new_file.txt`                      | Create a new file.                            |
  | `cp file.txt destination_directory/`      | Copy a file to a destination directory.       |
  | `mv file.txt new_location/`               | Move or rename a file.                        |
  | `rm file.txt`                             | Remove/delete a file.                         |
  | `cat file.txt`                            | Output the contents of a file.                |
  | `less file.txt`                           | View file content with pagination.            |
  | `head file.txt`                           | Output the first part of a file.              |
  | `tail file.txt`                           | Output the last part of a file.               |
  | `man ls`                                  | Display the manual page for the `ls` command. |
  | `clear`                                   | Clear the terminal screen.                    |
  | `history`                                 | Display command history.                      |
  | * Interrupt (Ctrl + C)                    | Interrupt the currently running process.      |
  | * Exit (Ctrl + D)                         | Exit the current shell session.               |



* ### File System Structure: 

	| Directory                | Description                                                                                    |
	|--------------------------|------------------------------------------------------------------------------------------------|
	| `/` (Root Directory)     | The top-level directory in the Linux file system hierarchy.                                    |
	| `/bin` (Binaries)        | Essential user command binaries (programs) are stored here.                                     |
	| `/sbin` (System Binaries)| System binaries that are typically used by system administrators are stored here.                |
	| `/usr` (User)            | Contains user-related programs, libraries, documentation, and binaries.                         |
	| `/etc` (Editable Text Configuration) | Contains system-wide configuration files.                                               |
	| `/home` (Home Directories) | Each user on the system typically has a subdirectory within `/home` for personal files.    |
	| `/var` (Variable Data)   | Contains variable data files, such as logs, temporary files, and caches.                        |
	| `/tmp` (Temporary)       | Stores temporary files that are created by various programs.                                     |
	| `/dev` (Devices)         | Contains device files, which represent hardware devices connected to the system.                |
	| `/proc` (Process Information) | A virtual file system that provides information about running processes and system resources. |



* ### Package Management: 
	**Debian-based Systems (e.g., Ubuntu, Debian)**
	
	Package Management Tool: APT (Advanced Package Tool)
	Installation:
	- To install a package: `sudo apt install package_name`
	  - Example: `sudo apt install nginx`
	Update Package Lists:
	- `sudo apt update`
	Upgrade Packages:
	- `sudo apt upgrade`
	Remove Package:
	- `sudo apt remove package_name`
	Search for Packages:
	- `apt search search_term`

	**Red Hat-based Systems (e.g., CentOS, Fedora)**
		
	Package Management Tool: YUM (Yellowdog Updater Modified) / DNF (Dandified Yum)
	Installation:
	- To install a package: `sudo yum install package_name` or `sudo dnf install package_name`
	  - Example: `sudo yum install httpd` or `sudo dnf install httpd`
	Update Package Lists:
	- `sudo yum check-update` or `sudo dnf check-update`
	Upgrade Packages:
	- `sudo yum update` or `sudo dnf upgrade`
	Remove Package:
	- `sudo yum remove package_name` or `sudo dnf remove package_name`
	Search for Packages:
	- `yum search search_term` or `dnf search search_term`




* ### User and Permission Management: 
	Understanding Linux user and group management, as well as file permissions and ownership, is important for securing your system and managing access to resources.

* ### Networking: 
<p>Knowledge of basic networking concepts, such as IP addressing, DNS resolution, firewall configuration, and network troubleshooting, is helpful for working with networked applications and services.</p>


* ### Process Management: 
	<p>Understanding how to manage processes, monitor system resource usage, and troubleshoot performance issues is important for optimizing your development environment and ensuring smooth operation of your applications.</p>

* ### Text Processing: 
 
	Familiarity with text editors like nano, vim, or emacs, as well as text processing tools like grep, sed, and awk, is useful for editing configuration files, analyzing log files, and manipulating text data.
