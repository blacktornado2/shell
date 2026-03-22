# macOS File System

## Overview

macOS uses a hierarchical directory structure similar to Unix/Linux systems.

## Root Directory (`/`)

The **root directory** is the top-level directory that contains all files and folders on your system. Everything on your Mac starts here.

```
/
├── Applications/        # Third-party applications
├── Library/             # System libraries and preferences
├── System/              # Core macOS system files
├── Users/               # User home directories
├── Volumes/             # Mounted drives
├── bin/                 # Essential system binaries
├── sbin/                # System administration binaries
├── etc/                 # System configuration files
├── tmp/                 # Temporary files
├── var/                 # Variable data (logs, caches)
├── usr/                 # User programs and libraries
├── cores/               # Core dump files (crash data)
├── dev/                 # Device files (hardware interfaces)
├── home/                # Alternative user home directory
├── opt/                 # Optional software packages
└── private/             # Private system directory (symlink to /)
```

## Key Directory Explanations

### `/Applications`
- **Purpose**: Contains all installed applications and software accessible to **all users**
- **Example**: Finder, Safari, VS Code, Chrome, etc.
- **User Perspective**: This is the primary location where your downloaded apps live
- **Access**: Open Finder → Applications folder
- **Accessibility**: All users on the Mac can see and run these applications
- **Installation**: Typically requires admin/sudo privileges to install apps here
- **Shared vs Individual**: Use this for applications you want to share with other users

### `~/Applications`
- **Purpose**: User-specific applications accessible **only to that particular user**
- **Location**: Your personal user directory, not visible to other users
- **Full path**: `/Users/username/Applications/`
- **User Perspective**: Your private application directory
- **Installation**: Does not require admin privileges; you can install here freely
- **Accessibility**: Only you can see and run these applications; other users cannot access them
- **When to use**: Install applications here if you want to keep them personal, or for testing purposes
- **Visibility**: Not shown in the system-wide Applications folder in Finder

### Key Differences: `/Applications` vs `~/Applications`

| Aspect | `/Applications` | `~/Applications` |
|--------|-----------------|-----------------|
| **Accessibility** | All users | Individual user only |
| **Admin Required** | Yes (to install) | No |
| **Visibility** | Visible to all users | Private to one user |
| **Use Case** | Shared/common apps | Personal/test apps |
| **Storage** | System/root partition | User home partition |
| **Backup** | Part of system backup | Part of user data backup |

### `/Users`
- **Purpose**: Contains home directories for all user accounts
- **Structure**:
  ```
  /Users/
  ├── username1/        # Your user home directory
  ├── username2/        # Other users' home directories
  ├── ankit/            # ankit's home directory
  └── ...
  ```

### `/Users/username` (User Home Directory)
The most important directory for regular users. This is denoted as `~` (tilde) in the terminal.

**Common subdirectories in the home folder:**

| Directory | Purpose |
|-----------|---------|
| `Desktop/` | Files on your desktop |
| `Documents/` | Your documents |
| `Downloads/` | Downloaded files |
| `Pictures/` | Photos and images |
| `Music/` | Audio files |
| `Movies/` | Video files |
| `Public/` | Shared with other users |
| `.ssh/` | SSH keys and configs (hidden) |
| `.config/` | Application configuration files (hidden) |
| `.cache/` | Cached data (hidden) |
| `Library/` | Application data, preferences, caches |

### `/Library`
Contains application data, preferences, and system information. There are two main Library folders:

1. **User Library** (`~/Library/`)
   - Personal to each user
   - Stores preferences, caches, and application data
   - Usually hidden in Finder
   
2. **System Library** (`/Library/`)
   - Shared across all users
   - System extensions and shared application data

**Notable subdirectories:**
```
~/Library/
├── Applications/       # User-installed applications
├── Preferences/        # Application preferences (.plist files)
├── Caches/            # Cached data to speed up apps
├── Logs/              # Application and system logs
└── Application Support/ # App-specific data and files
```

### `/System`
- **Purpose**: Core macOS operating system files
- **Usage**: Critical system files needed for macOS to function
- **Warning**: Don't modify files here without expert knowledge

### `/Volumes`
- **Purpose**: Mount points for external drives and storage
- **Example**: USB drives, external hard drives, network drives appear here
- **Note**: Your main drive (`Macintosh HD`) is also mounted here

### `/var`
- **Purpose**: Variable data that changes frequently
- **Common subdirectories**:
  - `/var/log/` — System and application logs
  - `/var/tmp/` — Temporary files
  - `/var/mail/` — Mail data

### `/tmp` and `/var/tmp`
- **Purpose**: Temporary files created by applications
- **Auto-cleanup**: macOS automatically cleans old temporary files
- **Note**: Files here may be deleted at any time

### `/usr`
- **Purpose**: User programs and libraries
- **Key subdirectories**:
  - `/usr/bin/` — Common command-line programs
  - `/usr/local/bin/` — Locally installed programs (homebrew installs here)
  - `/usr/lib/` — Libraries for programs

### `/bin` and `/sbin`
- **Purpose**: Essential system commands and binaries
- `/bin/` — Standard user commands (ls, cp, rm, etc.)
- `/sbin/` — System administration commands

### `/etc`
- **Purpose**: System configuration files
- **Example**: Network settings, system preferences
- **Note**: Usually requires admin privileges to modify

### `/cores`
- **Purpose**: Stores core dump files (crash memory dumps)
- **When created**: Generated when applications crash unexpectedly
- **Content**: Memory state and debugging information at the time of crash
- **Use**: Developers use these files to debug application crashes
- **Typically empty**: Most users won't have files here unless apps are crashing frequently

### `/dev`
- **Purpose**: Device files that represent hardware and pseudo-devices
- **Common examples**:
  - `/dev/disk0`, `/dev/disk1` — Hard drives and SSDs
  - `/dev/null` — Discards all data written to it
  - `/dev/random` — Provides random data
  - `/dev/stdout`, `/dev/stdin` — Standard input/output streams
- **Usage**: Advanced users and developers use these in terminal commands
- **Warning**: Don't modify these files unless you know what you're doing

### `/home`
- **Purpose**: Alternative home directory location (rarely used on macOS)
- **Note**: On macOS, user home directories are in `/Users/`, not `/home/`
- **Comparison**: The `/home/` directory is more common on Linux systems
- **Usage**: Some Unix/Linux software may try to access this directory

### `/opt`
- **Purpose**: Contains optional software and third-party applications
- **Installation**: Some installers place software here instead of `/Applications/`
- **Structure**:
  ```
  /opt/
  ├── local/             # Locally installed optional software
  ├── [software-name]/   # Individual software packages
  └── homebrew/          # Some Homebrew packages may be installed here
  ```
- **Permissions**: Usually requires admin access to install software here
- **vs `/Applications/`**: Graphical applications typically go in `/Applications/`, while command-line tools often go in `/opt/`

### `/private`
- **Purpose**: Private system directory (actually a symlink to the root `/`)
- **Technical detail**: `/tmp`, `/var`, and other directories are actually linked to `/private/tmp`, `/private/var`, etc.
- **Behind the scenes**: macOS uses `/private/` internally for system organization
- **End user note**: You typically don't need to interact with this directory directly
- **Accessing**: `/tmp` and `/private/tmp` refer to the same location
- **System Integrity Protection**: Contents are protected by macOS security features

## Hidden Files and Folders

On macOS, files and folders starting with a dot (`.`) are hidden by default.

**Common hidden items:**
- `.gitignore` — Git configuration
- `.ssh/` — SSH keys for secure connections
- `.config/` — Application configurations
- `.bash_history` — Command history
- `.DS_Store` — macOS folder metadata (created automatically)

**To view hidden files in Finder:**
- Press `Cmd + Shift + .` (period)

**To view hidden files in Terminal:**
```bash
ls -la    # Shows all files including hidden ones
```

## File Path Conventions

### Absolute Paths
Start from the root directory and show the complete path:
```bash
/Users/username/Documents/file.txt
/Applications/Safari.app
```

### Relative Paths
Start from the current location:
```bash
Documents/file.txt      # From home directory
./Documents/file.txt    # Explicit relative path
../Documents/file.txt   # Go up one level, then into Documents
```

### Home Directory Shortcut
The tilde (`~`) represents your home directory:
```bash
~/Documents/file.txt    # Same as /Users/username/Documents/file.txt
```

## File Extensions

macOS uses file extensions to identify file types (like Windows):
- `.txt` — Text files
- `.md` — Markdown files
- `.jpg`, `.png` — Image files
- `.pdf` — PDF documents
- `.app` — Application bundles
- `.dmg` — Disk image (installation files)
- `.zip` — Compressed archive

## Permissions

Each file and folder has permission settings:
- **User (u)** — File owner
- **Group (g)** — Group of users
- **Others (o)** — Everyone else

**Three permission types:**
- **Read (r)** — View file contents
- **Write (w)** — Modify or delete
- **Execute (x)** — Run as a program

**Example:** `drwxr-xr-x` means:
- `d` = directory
- `rwx` = owner has read, write, execute
- `r-x` = group has read and execute
- `r-x` = others have read and execute

## Quick Navigation Tips

### In Finder
- `Cmd + Shift + G` — Go to folder (enter path directly)
- `Cmd + Shift + D` — Go to Desktop
- `Cmd + Shift + O` — Go to Documents
- `Cmd + Shift + H` — Go to Home folder

### In Terminal
```bash
cd ~                    # Go to home directory
cd /Path/To/Folder     # Go to specific location
pwd                     # Print working directory (show current path)
ls                      # List files in current directory
ls -la                  # List all files including hidden
open .                  # Open current directory in Finder
```

## Package Files (.app, .bundle)

On macOS, **applications are actually folders** presented as single files in Finder. These are called "packages."

- You can right-click an `.app` file and select "Show Package Contents" to explore inside
- The `Contents/MacOS/` folder typically contains the executable binary
- The `Contents/Resources/` folder contains images, sounds, and other assets

## .DS_Store Files

You'll notice `.DS_Store` files appearing in folders. These are created automatically by macOS and store:
- Folder layout preferences
- Icon positions
- View settings
- Thumbnails

These are safe to ignore and are commonly excluded from version control (Git).

## Common Use Cases

### Finding Application Support Files
```
~/Library/Application Support/[App Name]/
```

### Finding Application Preferences
```
~/Library/Preferences/com.[Company].[App].plist
```

### Finding Logs
```
~/Library/Logs/
/var/log/
```

### Accessing Recently Deleted Files
Finder → Trash (empty or recover items)

## Security Considerations

- **System Integrity Protection (SIP)**: Prevents unauthorized modifications to protected system files
- **Gatekeeper**: Controls software installation from untrusted sources
- **Admin Password Required**: Modifying `/System`, `/Library`, or installing software often requires admin privileges
- **Privacy**: Some directories (like Photos or Messages) are protected even from admin users

## Tips for New macOS Users

1. **Don't modify system folders** like `/System` or `/etc` unless you know what you're doing
2. **Use `~/Library/`** for storing application preferences and data
3. **Keep `/Applications` clean** by regularly removing unused apps
4. **Use iCloud Drive or Dropbox** for syncing files across devices
5. **Organize your home directory** (Desktop, Documents, etc.) to stay organized
6. **Enable Time Machine** for regular backups
7. **Learn the Terminal** for more powerful file system operations

## Related Resources

- **Finder**: Your primary file manager (Cmd + Space, then type "Finder")
- **Terminal**: For command-line file operations
- **Activity Monitor**: To see what applications are using disk space
- **Disk Utility**: To manage drives and storage
- **Get Info**: Right-click files → "Get Info" to see permissions and details

---

## Summary

The macOS file system is organized hierarchically with the home directory (`~/`) being the most important for regular users. Understanding these key directories will help you navigate, organize, and manage your files effectively on macOS!
