
# Linux and BASH: a Versatile and Powerful Software Platform:

## 1. Linux OS:

Linux is a free and open-source operating system that's widely used by researchers and engineers to develop and deploy software systems in a very wide range of fields, tailoring to highly varying requirements in terms of performance, security, reliability, and ease-of-use. 

### a. Linux Architecture Overview:

Consider this simplified view of the architecture of Linux operating systems which, as all others do, live on top of the hardware:

- User Space: also called "userland", is the memory area containing applications and libraries that are less privileged in terms of hardware access, from which we can list out 3 layers: 
    - User Applications: applicative programs, also including shells (e.g. `bash`, `zsh`)
    - System Components: high priority processes that provide essential services such as audio, graphics display, user-process management, and configuration
    - C Standard Library: a library of functions, data types definitions, and macros that are used by user applications and system components to request the execution of the various software operations, as a higher-level alternative to directly using system calls
- Kernel Space:
    - System Call Interface: often called SCI, an interface that provides user space components with safe, well-defined means of requesting the execution of kernel operations 
    - Kernel: the main component of the operating system, it acts as a mediator between the hardware and the rest of the system, interpreting system calls and executing the required operations such as process management, file management, device management, and network communication
    - Architecture-Specific Code: portions of the Linux kernel that are not independent of the underlying hardware architecture it runs on, often related to system boot-up and memory management

More about the Linux architecture: https://linux-kernel-labs.github.io/refs/heads/master/lectures/intro.html


### b. Linux Fundamentals:

To use Linux OS for development or deployment, it's important to have a minimal understanding of its different functionalities.

#### System Startup:

On startup, the operating system kernel is loaded into live memory (RAM) by a program called "boot loader" or "bootstrap loader". This program (at least partially) is often stored at the first sector of the main disk of the system in a special record of the disk called "Master Boot Record". Most Linux systems use the "Grand Unified Boot Loader" (GRUB).

This step is often preceded by the BIOS POST.

Once the kernel is loaded into memory, self-extracts from its compressed state, then loads `systemd`, the parent of all other processes, which handles mounting filesystems, starting other critical processes, and configuring the target state to put the system in (e.g. single-user shell, multi-user shell, multip-user GUI).

More about system bootup: https://man7.org/linux/man-pages/man7/bootup.7.html


#### Disks and Filesystems:

Storage disk drives connected to a computer are mounted and partitioned into one or more logical drives. These partitions are represented by device files which (located under `/dev`), with the names depending on the type of drive (IDE vs SCSI), the drive number, and the partition number. For example: `/dev/hda2` is the second partition of the first IDE drive.

Partitioning enables having multiple filesystems (so potentially multiple operating systems) on the same physical disk and separates files to lower the risk of corruption (among other benefits). 

Filesystems are a combination of data structures and logic necessary to manage the read and write operations of files on disk partitions. So each partition must be formatted with a filesystem that's compatible with the operating system, in order to be usable. Some utilities that are often used for this are `fdisk`, `parted`, and `dd`.

The filesystem hierarchy starts from `/`, also known as the "root directory". Full paths to all files and directories, thus, starts with `/`. 

Examples of filesystems used under Linux are `ext`, `BtrFS`, and`FAT`.

More about partitioning and filesystems: https://developer.ibm.com/tutorials/l-lpic1-104-1


#### User Management:

Users in Linux are identified by a username and are assigned to a group. The the superuser named `root` has complete control over the system. Other users can escalate their privileges in a controlled manner using the `sudo` program.

User and group information is stored across 4 important files:

- `/etc/group`: contains the list of groups (group name, password, group id, users in the group - if any)
- `/etc/gshadow`: contains the passwords of the groups if they are encrypted (so the value of the password in `/etc/group` would be a dummy: `x`)
- `/etc/passwd`: contains the list of users (user name, password, user id, group id, an optional comment, the user's home directory, and the user's default shell)
- `/etc/shadow`: contains the passwords of the users if they are encrypted (so an `x` would be the placeholder in `/etc/passwd`)

More about user management: https://wiki.archlinux.org/index.php/users_and_groups


#### Process Management:

Processes refer to programs (code and data) that are loaded into memory and scheduled for execution. They are characterized by an array of properties, mainly including:
- **Type**: a categorization into one of two types of processes:
    - **Foreground Processes**: interactive, visible processes that take user inputs and write outputs through a shell or a graphical interface
    - **Background Processes**: non-interactive, invisible processes that are run automatically by other processes or services 
- **Process ID**: usually referred to as `pid`, is a unique identifier of the process among all others running
- **Parent Process ID**: usually referred to as `ppid`, is the `pid` of the process that launched this child process, with `systemd` being the parent of all processes
- **Priority**: the true process priority for the Linux scheduler, calculated by the kernel
- **Niceness**: a user-configurable value that could be used to encourage increasing or decreasing the true priority of the process
- **Hardware Usage**: CPU, GPU, and memory usage statistics
- **Command**: the command used to launch the process, including arguments 
- **State**: processes start from a "New/Runnable" state, and go through other states during their lifetime up until being "Terminated". These o states are:
    - **Running**: labeled `R`, in execution
    - **Sleeping**: waiting to start or resume execution, and could be in one of two main sub-states:
        - **Interruptible sleep**: labeled `S`, waiting for an event to complete (e.g. user action, results, or signals from other related processes)
        - **Uninterruptible sleep**: labeled `D`, waiting for hardware conditions to be satisfied (e.g. reading from a disk) and cannot be interrupted
    - **Stopped**: labeled `S`, suspended and is awaiting a signal to be continued or killed
    - **Zombie**: labeled `Z`, processes whose execution was completed but are still tracked in the process table (so, still retaining a `pid`) so that their parent processes can read their exit status then reap (terminate) them

More about processes: https://tldp.org/LDP/tlk/kernel/processes.html 


#### File Management:

Linux inherited an important property from UNIX: ***Everything is a file.***

Files can be of many types, mainly including:
- **Regular files**: ordinary files such as text files, binary files, or image files, labeled `-`
- **Directories**: which are special files that store other types of files or other directories
- **Device files**: are mounted under the `devfs` virtual filesystem which and have three main variants:
    - **Character files**: for reading from devices character-by-character, labeled `c`, also including some commonly used pseudo-device files which don't correspond to a physical device, but rather expose functionality of the operating system (e.g. generating random bytes by reading from `/dev/random`)
    - **Block files**: for reading from devices in bulk, labeled `b`
- **Symbolic link files**: files without content that just refer to other files in a different location with a different name, labeled `l`, also including virtual files that provide access to information about system internals and running processes that's otherwise very hard to access (e.g. `procfs`, `sysfs`) or that enable implementing file sharing using networks (e.g. `NFS`)

We already mentioned some of these before, but here are some of the important root subdirectories to remember:
- `/bin`: programs that usually come bundled with the operating system distribution (programs that require superuser privileges are in `/sbin`)
- `/boot`: programs and configuration files required to bootup the system, including the bootloader
- `/dev`: the root of `devfs` virtual filesystem, containing device files
- `/etc`: configuration files, et cetera ...
- `/home`: home directories of the different users
- `/lib`: libraries for use by other programs (e.g. GNU Standard C library `glibc`, the C++ library `cpp`), service configuration, and loadable kernel modules, also known as LKM (e.g. network drivers, sound drivers)
- `/proc`: the root of `procfs` virtual filesystem, containing information about the kernel, running processes, etc ...
- `/root`: the home directory of the superuser `root`
- `/usr`: originally contained users' home directories, but now contain the vast majority of binaries and libraries that either come with the operating system or are installed by the users
- `/tmp`: a mount point for the commonly used temporary filesystem (`tempfs`), which, like any other `tempfs`, loses its contents upon system restarts


To manage access to files, a combination of permission types and permission groups are used.

When assigning permissions, they are targetted towards on or more permission groups:
- **user**: labeled `u`, a group containing one user called the "owner", who is by default the creator of the file
- **group**: labeled `g`, a group containing any user of the group assigned to the file, which is by default the group of the creator of the file 
- **others**: labeled `o`, a group containing any user that's not in the previous two groups
- **all**: labeled `a`, a group containing all users in the system

There are three basic types of permission types that could be assigned to files or directories, with different meanings for each:
- **read**: labeled `r`, refers to the ability to:
    - Read file contents
    - List directory contents (other files or subdirectories)
- **write**: labeled `w`, refers to the ability to:
    - Modify/delete file contents
    - Modify directory attributes or contents (i.e. adding, renaming, or deleting files)
- **execute**: labeled `x`, refers to the ability to:
    - Execute files as scripts
    - Use a directory as a working directory (i.e. `cd` into it and read, write, or execute files inside it)

Permissions are often managed by using octal numbers where each digit represents the sum of the values of the `r,w,x` bits in binary for each of the first three permission groups `u`, `g`, `o`:
- `r` = `100`
- `w` = `010`
- `x` = `001`

So for example `764` is:
- `u`: `7`(base-7) = `111`(base-2) = `100` + `010` + `001`, meaning `r`, `w`, and `x`
- `g`: `6`(base-7) = `110`(base-2) = `100` + `010`, meaning `r` and `w`
- `o`: `4`(base-7) = `100`(base-2), meaning `r` only


The default file and directory permissions can be managed using Umasks.


More details about files and permissions: https://www.linux.com/training-tutorials/linux-filesystem-explained https://www.linux.com/training-tutorials/understanding-linux-file-permissions


#### Services and Jobs:

Linux services are background processes are managed as "units" using `systemd`, the modern init daemon we already mentioned. They're usually configured using `.service` files that can be found under `/lib/systemd/system`. Each service is configured to be required for a given target state.

Service logs are organized and stored using "journals", which is a centralized logging mechanism that enables reviewing service activities and debugging errors and failures.

`systemd` services can `started` or `stopped` manually after system startup, but could be `enabled` so that they're started automatically on system startup, or `disabled` to require manual starts if preferable.

Services, in fact, often require multiple processes to perform their functions, so `systemd` uses control groups or CGroups (a Linux kernel feature for allocating and limiting resources) to bundle manage services as groups of processes from startup to termination.

In Linux, in addition to automating the launching of services on startups, scheduling the execution of a `systemd` service or any program (using a regular command) is possible through the use of Cronjobs or Timers.

More info about `systemd` service management, Timers, and Journal: https://wiki.archlinux.org/index.php/Systemd https://wiki.archlinux.org/index.php/Systemd/Timers https://wiki.archlinux.org/index.php/Systemd/Journal

And for Cronjobs: https://help.ubuntu.com/community/CronHowto


#### Software Package Management:

In Linux OS, libraries, shells, utilities, and the kernel itself are all bundled, distributed, and managed as packages. The two most used Linux package management systems are the Red Hat Package Manager (RPM) and the Debian Package Manager.

Package management systems, enable:
- Building packages from source code
- Distributing and versioning packages through repositories (official or self-hosted)
- Searching for packages in repositories
- Verifying the integrity of packages before and after installation
- Downloading and installing packages and their dependencies
- Updating installed packages
- Uninstalling and cleaning packages and their dependencies

To support these features, PMs maintain complex information and data, including:
- **Package Indexes**: databases of packages (and versions) available in a given repository
- **Distributed Package Files**: files that make the packages, usually distributed in a `zip` or `tar` compressed format
- **Dependency Packages**: lists of required "dependency" packages for a given package
- **Installed Package Files**: databases of the files that belong to an installed package
- **Checksums**: hashes (usually MD5) of distributed package files as well as installed package files

Debian packages are bundled into `.deb` files. Popular distributions like Ubuntu or Mint use Debian packages (because they're based on Debian). RPM packages are bundled into `.rmp` files and are used by distributions like Fedora, CentOS, and OpenSUSE.

For more about Linux package managers: https://linuxconfig.org/comparison-of-major-linux-package-management-systems


#### Networking and Communication:

Networks involve many hardware and software components. The most common stack of network protocols in use today is Transmission Control Protocol/Internet Protocol (TCP/IP) suite. Network communication between two devices requires a physical network interface. 

In protocols with IP support, devices (or hosts) in a network have a logical address assigned to them, referred to as an Internet Protocol (IP) address. A four-number address, where each number is an unsigned 8-bit integer. Home networks composed of the router, laptops, and mobile devices usually assign IPs like `192.168.1.X`.

Hosts on different networks are connected through devices called gateways, which perform an operation that's very important to modern networking called "Network Address Translation" (NAT). In a nutshell, NAT enables reusing the same IP address ranges in local networks (hence the familiarity of addresses like `192.168.1.X`) by hiding them under the umbrella of a public address (e.g. `197.2.114.157`) that's unique in the external network that it wants to communicate in (e.g. the internet).

The gateway device in a network with addresses like `192.168.1.X` usually has an IP address of `192.168.1.1`, with other notable addresses being the network's identifying IP address `192.168.1.0`, and the broadcast address `192.168.1.255` that enables sending data to all hosts on the network.

Because devices have multiple processes that require network communication, logical constructs called "ports" are appended to the IP address of a given host, to enable internal routing of network traffic to the right service. Port numbers are unsigned 16-bit integers.


Quite often, we require executing certain instructions remotely or transferring data through networks. Performing such actions means worrying about security.

One of the most commonly used ways of satisfying communication security requirements is "Public-key Cryptography".

This method is based on the use of a pair of asymmetric keys generated using cryptographic algorithms based on one-way functions (such as the factoring of two very large prime numbers, used in the RSA algorithm). These two keys are:
- **Public Key**: can be distributed freely to parties to communicate with
- **Private Key**: must be stored safely

Let's consider a scenario with a server and a client who wants to exchange information. With Public-key Cryptography, three main security concerns are addressed by having the client share its public key with the server. 

Loosely speaking, these concerns are:
- **Impersonation**: also referred to as "spoofing" if coming from the client-side, and as "misrepresentation" if coming from the server-side, is when one of the parties is impersonated by a malicious third-party and is solved by using authentication
- **Eavesdropping**: also called "sniffing", is when the information sent by the server is read by a malicious third-party and is solved by using encryption
- **Tampering**: is when the information sent by the server is altered by a malicious third-party and is solved by implementing tamper detection

Public-key cryptography is widely used to authenticate for the use of Secure Shells (SSH) which enables controlling a shell command line remotely, and Secure Copy (SCP) which enables transferring data from and to a remote computer. 


More networking basics: https://developer.ibm.com/tutorials/l-lpic1-109-1

More about public-key cryptography: https://docs.oracle.com/cd/E19957-01/816-6154-10/


#### Graphical Display:

Modern computer usage heavily relies on graphical interfaces. In Linux, the X Window System is used to implement GUIs. X enables configuring the resolution, refresh rate, mice, keyboards, rendering devices, and everything related to graphics and interactions with graphical interfaces.

The X system uses a client-server architecture. The X-server receives inputs from X-client programs, then serves graphical output to display devices on port 6000.

The X-client (e.g. Visual Studio Code) and the X-server are often on the same computer, but this client-server model enables an X-client application to be run remotely, sending keyboard and mouse signals through the network to an X-server that's on a different computer, then receiving the rendered visual output. 

More about X-Window: https://en.wikipedia.org/wiki/X_Window_System_protocols_and_architecture


## 1. Shell Interactions and Scripting:


