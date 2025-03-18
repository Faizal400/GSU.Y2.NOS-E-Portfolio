# Week 8 - Commonly Used OS

> As the titles suggests this weeks notes mainly goes over commonly used operating systems


## Intro to OSs
- **What is an Operating System (OS)?**
  - > An OS is a software that manages computer hardware and software resources and provides services for computer programs. It acts as an intermediary between applications and hardware.
- **Functions of an OS:**
  - **Process Management**: Handles the scheduling and execution of processes.
- **Memory Management**: Allocates and deallocates memory to programs.
- **File System Management**: Manages data storage and retrieval.
- **Device Management**: Controls hardware devices like printers, keyboards, and displays.
- **User Interface**: Provides a way for users to interact with the system, either through a graphical user interface (GUI) or a command-line interface (CLI).

## Introduction to Windows
- Developed by Microsoft.
- First released in 1985 (Windows 1.0).
- Dominant desktop operating system globally.
- Focus on user-friendliness and broad hardware compatibility.

### Evolution of Windows Versions
- **Timeline**: Windows 95, Windows XP, Windows 7, Windows 8, Windows 10, Windows 11.
- **Key features and changes in each major version:**
  - **Windows 95**: Introduced the Start Menu.
- **Windows XP**: Known for its stability and user-friendly interface.
- **Windows 7**: Refined user experience and improved performance.
- **Windows 8**: Introduced the Metro UI (Modern UI) with tiles.
- **Windows 10**: Brought back the Start Menu and focused on cross-device compatibility. Windows 10'S is a mode, but is often advertised as a separate OS. It can also only be enabled by OEMs.
- **Windows 11**: Features a redesigned user interface with Fluent Design.

### Windows Architecture
- **Kernel Mode**: Direct access to hardware, critical system operations. Code running in kernel mode has unrestricted access to the system's hardware.
- **User Mode**: Applications and user processes, restricted access. Processes in user mode have limited access to system resources, which enhances stability and security.
- **Win32 API**: Application Programming Interface for software development. It provides a set of functions, procedures, and protocols that developers can use to write applications that run on Windows.

### Windows Interface
- **Taskbar**: Navigation, application switching. It allows users to easily switch between running applications.
- **Start Menu**: Application launcher, settings.
- **Control Panel/Settings**: System configuration. It allows users to configure various system settings.

### Windows File System
- **NTFS (New Technology File System)**: Default, advanced features, security. Supports file compression, encryption, and access control lists (ACLs).
- **FAT32 (File Allocation Table 32)**: Older, compatibility, limited features. Limited to a maximum file size of 4GB.
- **exFAT (Extended File Allocation Table)**: For flash drives, large file support. Designed for flash drives and supports larger files than FAT32.

### Windows Process Management
- **Task Manager**: Monitoring and managing processes, performance. Provides detailed information about running processes and system performance.
- **Services**: Background processes, system components. These are long-running processes that perform system-level tasks.

### Windows Security Features
- **Windows Defender**: Antivirus and anti-malware. Provides real-time protection against viruses and malware.
- **User Account Control (UAC)**: Permission prompts, security. Requires user confirmation for actions that could affect system security.
- **Windows Firewall**: Network security. Controls network traffic to prevent unauthorized access.

### Windows Server OS
- Features of Windows Server editions: Active Directory, Hyper-V, IIS.
  - **Active Directory**: Manages users, computers, and resources in a network.
  - **Hyper-V**: Microsoft's virtualization platform.
  - **IIS (Internet Information Services)**: Web server software.
- Designed for server environments, data centers.

## Pros & Cons of Windows
- **Pros:**
  - Wide software compatibility.
  - User-friendly.
  - Large user base.
- **Cons:**
  - Susceptible to malware.
  - Resource-intensive.
  - Licensing costs.

## Introduction to macOS
- Developed by Apple Inc.
- Unix-based operating system.
- Known for its user-friendly interface and integration with Apple hardware.
- Focus on creativity, productivity, and a seamless user experience.

### macOS OS History
- Mac OS X Cheetah, Puma, Jaguar, Panther, Tiger, Leopard, Snow Leopard, Lion, Mountain Lion, Mavericks, Yosemite, El Capitan, Sierra, High Sierra, Mojave, Catalina, Big Sur.
- Each version introduces new features and improvements to the user experience.

### macOS Interface
- **Finder**: File management, navigation. It's the default file manager on macOS.
- **Dock**: Application launcher, quick access. It's a customizable bar for launching and switching applications.
- **Spotlight**: System-wide search. It allows users to quickly find files, applications, and information.
- **Menu Bar**: Application-specific menus. It provides access to application commands and settings.

### macOS File System
- **HFS+ (Hierarchical File System Plus)**: Older file system, used in earlier versions. It supports metadata and journaling.
- **APFS (Apple File System)**: Modern file system, optimized for SSDs, improved security, and performance. It features cloning, snapshots, and encryption.

### macOS Security Features
- **Gatekeeper**: Prevents installation of malicious software. It verifies that applications are from trusted sources.
- **FileVault**: Full-disk encryption. It encrypts the entire disk to protect data.
- **T2 / M series Security Chip**: Hardware-based security, secure boot, and encrypted storage. It enhances security with hardware-level encryption and secure boot processes.

### Apple Ecosystem
- **iCloud**: Cloud storage, synchronization. It allows users to store files and data in the cloud and sync them across devices.
- **Continuity**: Features like Handoff, Universal Clipboard, and AirDrop. These features enable seamless integration between Apple devices.

## Pros & Cons of macOS
- **Pros**:
  - User-friendly interface.
  - Strong security.
  - Seamless ecosystem.
  - Stable performance.
- **Cons**:
  - Limited hardware compatibility.
  - Higher price point.
  - Less gaming options compared to Windows.

## Introduction to Unix
- Originated at AT&T Bell Labs in the 1970s.
- Developed by Ken Thompson, Dennis Ritchie, and others.
- A multi-user, multi-tasking operating system.
- Influenced the development of many modern operating systems.

### Design Philosophy of Unix
- **Modularity**: Small, focused programs that can be combined.
- **Simplicity**: Clear and concise design principles.
- **Portability**: Designed to run on various hardware platforms.
- **"Everything is a file"**: Unified approach to data and devices.

### Unix File System
- **Hierarchical structure**: Root directory (/) and subdirectories.
- **File permissions**: Read, write, execute (rwx).
- **Ownership**: User and group ownership.
- **Inodes**: Data structures that store file metadata. An inode (index node) is a data structure in a Unix-style file system that stores metadata about a file, such as its ownership, permissions, and location.

### Unix Process Management
- **Background processes**: Running without user interaction.
- **Foreground processes**: Running with user interaction.
- **Process IDs (PIDs)**: Unique identifiers for processes.
- **Commands**: `ps`, `kill`, `nice`.
- `ps`: Displays information about active processes.
- `kill`: Terminates a process.
- `nice`: Adjusts the priority of a process.

### Unix Security
- **Root privileges**: Superuser access for system administration.
- **User authentication**: Passwords, SSH keys.
- **File permissions**: Restricting access to files and directories.
- **Principle of least privilege**: Giving users only the necessary permissions.

### Unix Variants
- **BSD (Berkeley Software Distribution)**: FreeBSD, OpenBSD, NetBSD.
- **Solaris**: Developed by Sun Microsystems (now Oracle).
- **AIX (Advanced Interactive Executive)**: Developed by IBM.

### BSD
Berkeley Software Distribution.
- 4.xBSD is widely used in academic installations and has served as the basis of a number of commercial UNIX products.
- 4.4BSD was the final version of BSD to be released by Berkeley.
- There are several widely used, open-source versions of BSD.
- **FreeBSD**:
  - Popular for Internet-based servers and firewalls.
  - Used in a number of embedded systems.
- **NetBSD**:
  - Available for many platforms.
  - Often used in embedded systems.
- **OpenBSD**:
  - An open-source OS that places special emphasis on security.

### Solaris 11
- Oracle’s SVR4-based UNIX release.
- Provides all of the features of SVR4 plus a number of more advanced features such as:
  - A fully preemptable, multithreaded kernel.
  - Full support for SMP (Symmetric Multiprocessing).
  - An object-oriented interface to file systems.

## Pros & Cons of Unix
- **Pros**:
  - Stability.
  - Security.
  - Flexibility.
  - Powerful command-line tools.
  - Portability.
- **Cons**:
  - Steep learning curve.
  - Less user-friendly GUI compared to modern OSs.
  - Hardware compatibility can vary.

## Introduction to Linux OS
- Open-source, Unix-like operating system.
- Developed by Linus Torvalds in 1991.
- Based on the Linux kernel.
- Known for its flexibility, security, and community-driven development.

### Linux Kernel & Distributions
- **Linux kernel**: Core of the OS, manages hardware.
- **Distributions (Distros)**: Complete OS packages built around the kernel.
  - Examples: Ubuntu, Fedora, Debian, Arch Linux.

### Linux File System
![image](https://github.com/user-attachments/assets/ef924764-9b66-4018-b312-af46a1835bed)

### Linux User Interface
- **Command-Line Interface (CLI)**: Terminal for text-based interaction.
- **Graphical User Interface (GUI)**: Desktop environments like GNOME, KDE, XFCE.
- **Essential commands**:
  - `ls` (list files).
  - `cd` (change directory).
  -  `cp` (copy).
  - `mv` (move).
  - `rm` (remove).
  - `chmod` (change permissions).

### Linux Process Management
- **Commands**:
  - `ps` (process status).
  - `top` (real-time process monitoring).
  - `kill` (terminate process).
  - `nice` (change process priority).

### Linux Security Features
- **SELinux (Security-Enhanced Linux)**: Mandatory access control.
- **iptables/firewalld**: Firewall management.
- **sudo privileges**: Controlled root access.
- **User permissions and file ownership**.

### Linux for Servers
Why Linux dominates server environments: 
  - Stability
  - security
  - performance
  - open-source nature
  - cost-effectiveness.
Examples: Web servers (Apache, Nginx), database servers (MySQL, PostgreSQL).

### Real-World Uses of Linux
- **Embedded systems**: Routers, smart devices.
- **Internet of Things (IoT)**: Smart home devices, industrial automation.
- **Cloud computing**: AWS, Azure, Google Cloud.
- **Supercomputing**: High-performance computing clusters.

## Pros & Cons of Linux
- **Pros**:
  - Open-source.
  - Highly customizable.
  - Secure.
  - Stable.
  - Vast software repository.
  - Strong community support.
- **Cons**:
  - Steeper learning curve for beginners.
  - Hardware compatibility issues (less common now).
  - Some proprietary software limitations.

## Introduction to Android Operating System
- Linux-based mobile operating system developed by Google.
- Dominant mobile OS globally.
- Open-source, highly customizable.
- Designed for touchscreen mobile devices.
- A Linux-based system originally designed for mobile phones.
- The most popular mobile OS.
- Development was done by Android Inc., which was bought by Google in 2005.
- 1st commercial version (Android 1.0) was released in 2008.
- Most recent version is Android 16.0 (Baklava).
- Android has an active community of developers and enthusiasts who use the Android Open Source Project (AOSP) source code to develop and distribute their own modified versions of the operating system.
- The open-source nature of Android has been the key to its success.

### Android Architecture
- **Kernel**: Linux kernel at the base.
- **Hardware Abstraction Layer (HAL)**: Interface between hardware and software.
- **Android Runtime (ART)**: Executes Android applications.
- **Framework**: Provides APIs for app development.

### Android App Development
- **Programming languages**: Java, Kotlin.
- **Android Studio***: Official Integrated Development Environment (IDE).
- **Android Software Development Kit (SDK)**: Tools for developing Android apps.

### Android Security Features
- **Google Play Protect**: Malware scanning.
- **App sandboxing**: Isolates apps for security.
- **Encryption**: Data protection.
- **Permissions**: User control over app access.

## Pros & Cons of Android
- **Pros**:
  - Wide range of devices.
  - Open-source.
  - Highly customizable.
  - Large app ecosystem.
- **Cons**:
  - Fragmentation (different versions and devices).
  - Potential security vulnerabilities.
  - Bloatware from manufacturers.

## Android Runtime (ART)
- Most Android software is mapped into a bytecode format which is then transformed into native instructions on the device itself.
- Earlier releases of Android used a scheme known as Dalvik, however Dalvik has a number of limitations in terms of scaling up to larger memories and multicore architectures.
- More recent releases of Android rely on a scheme known as Android runtime (ART).
- ART is fully compatible with Dalvik’s existing bytecode format so application developers do not need to change their coding to be executable under ART.
- Each Android application runs in its own process, with its own instance of the Dalvik VM.

### Advantages and Disadvantages of ART
- **Advantages**
  - Reduces startup time of applications as native code is directly executed.
  - Improves battery life because processor usage for JIT is avoided.
  - Lesser RAM footprint is required for the application to run as there is no storage required for JIT cache.
  - There are a number of Garbage Collection optimizations and debug enhancements that went into ART.
- **Disadvantages**
  - Because the conversion from bytecode to native code is done at install time, application installation takes more time.
  - On the first fresh boot or first boot after factory reset, all applications installed on a device are compiled to native code using dex2opt, therefore the first boot can take significantly longer to reach Home Screen compared to Dalvik.
  - The native code thus generated is stored on internal storage that requires a significant amount of additional internal storage space.

## Feature Comparison: Windows vs macOS vs Unix vs Linux vs Android
![image](https://github.com/user-attachments/assets/29631c85-775d-4383-ab22-65273853792d)
