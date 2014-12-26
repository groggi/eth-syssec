# Part 7: Linux and Windows OS Security

## Linux Security Model
- Software can run in *Kernel Space* or *User Space*
- Software in Kernel Space has full access to the whole computer it's running on
- Process running in User Space has the same permissions as the user running the application
	- User ID 0 is reserved for the superuser, Kernel gives this user full access
- Simple model: one special superuser that can do anything and all other users have limited access
- Attacker interested in getting root (superuser) access
- Kernel runs in processor's Ring 0, user space application in Ring 5
- Process separation and memory protection done by using virtual memory

### File System Security
- Linux treats everything as a file including memory, device drivers, named pipes,etc.
- User Accounts (UID) represents someone capable of using files, can represent a human or process
- Group Account (GID) list of user accounts, may belong to other groups
- UID/GID are not files
- Each file has owner and group (may have several)
- Each process has owner and group (may have several)
- Kernel verifies permission before executing system calls
	- If UID=0, everything is allowed (root user)
	- Otherwise user or group is used to check if call can be executed
- Process runs normally with the identity of the user that executed it
- Owner can change object's permissions

### File/Directory/Application Permissions
- File permissions: `r`: read, `w`: write, `x`: execute for *owner*, *group* and *world* (everyone).
- Directory permissions: `r`: list contents, `w`: create/delete files in directory, `x` use anything in or change into directory
- `setuid`: if set, process runs as the owner of the file, not depending who runs it
- `setgid`: if set, process runs as a member of the group of the file, not depending who runs it
- `setuid` has no effect for directories, but `setgid` causes any file created inside the directory to inherit the directory's group
- Different UIDs
	- Real UID (RUID) is the user ID running the program
	- Effective UID (EUID) is the user with whose privileges the program runs

## Windows

- Vista+ even administrator accounts do not run applications with full administrator rights
- User Account Control (UAC)
	- Helps user to make no mistakes
	- The moment an application needs further rights to complete a task, UAC prompts user to grant permission through a dialog box
	- Such tasks include ones that may compromise integrity or security of the system
	- Works slightly different for normal users and administrator accounts
- Windows Integrity Levels
	- Isolates different objects on a trust-based scale
	- Controlled by Windows Integrity Control (WIC)
	- For example malware runs not in the privilege level of the logged-in user, but rather in the same integrity level as the object that spawned it.
	- Levels:
		- Untrusted: for anonymous logons, seldom seen
		- Low: Internet features (browser, temporary files folder of the browser)
		- Medium: for normal user accounts and most files, default mode
		- System: for kernel and system services
		- High: for administrator account
		- Installer: installers need high level to allow uninstalls
- Windows Service Hardening and Process Isolation:
	- Vista+ reduced amount of services running
	- Services run on minimal privilege level
- File and Registry Virtualisation
- Address Space Layout Randomizer
	- Randomize memory address of Windows data structures at boot time
- Boot Sequence, always same boot order and components
	1. Boot Manager
		- If full disk encryption is used, loads libraries and keys to unencrypt 
	2. OS Loader
		- Opens the disk, discovers system disk/partition, etc.
		- Does some basic checks of hashing and signature algorithms
		- Loads operating system's signature catalog
		- (optional) boot drivers' signatures are checked
	3. OS Kernel
		- Responsible for verification of system drivers and ones loaded at runtime
