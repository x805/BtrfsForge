# BtrfsForge

**Live BTRFS Migration Tool for EndeavourOS / Arch Linux**

BtrfsForge is a powerful live migration tool designed specifically for BTRFS filesystems. It enables seamless transfer of a complete BTRFS installation between machines or disks using BTRFS's native send/receive functionality.

## Why BTRFS is Special

BTRFS (B-tree File System) is fundamentally different from traditional filesystems like EXT4, NTFS, or FAT:

- **Subvolumes**: BTRFS supports multiple independent subvolumes within a single partition, each with its own mount options and snapshot capabilities
- **Snapshots**: Atomic, copy-on-write snapshots can be created instantly without duplicating data
- **Incremental Send/Receive**: Only changed blocks between snapshots need to be transferred
- **Checksums**: Data integrity verification built into the filesystem
- **Compression**: Transparent compression at the filesystem level

## Why Traditional Imaging Tools Fail

Tools like **Clonezilla** and **Rescuezilla** are designed for block-level imaging and fail with BTRFS for several critical reasons:

### 1. Subvolume Complexity

Traditional imaging tools copy entire partitions block-by-block. BTRFS installations typically have multiple subvolumes (e.g., `@`, `@home`, `@var`, `@cache`). These tools don't understand this structure and would need to either:
- Image the entire partition (wasting space and time)
- Attempt to copy each subvolume separately (which they can't do natively)

### 2. Target Size Independence

For BTRFS, the target partition can be **smaller** than the source! This is impossible with traditional imaging tools:
EXT4 Imaging:
Source (100GB) → Target must be ≥ 100GB (strict)

BTRFS Send/Receive:
Source (100GB, 50GB used) → Target can be 60GB (flexible!)

BTRFS only cares about the **total capacity of the disk**, not the partition size. The data is streamed at the subvolume level, not the block level.

### 3. Inefficient Transfer

Traditional imaging tools transfer **every block** on the disk, including empty space. For a 1TB disk with 100GB of data, Clonezilla would transfer 1TB of data. BtrfsForge transfers only the actual data (100GB).

### 4. No Snapshot Support

Clonezilla cannot preserve BTRFS snapshot structures or efficiently update existing installations. BtrfsForge uses BTRFS's native send/receive for efficient, incremental transfers.

## How BtrfsForge Works

BtrfsForge uses a three-step process:

### Step 1: Prepare Target (Run on TARGET machine)
- Formats the target partition as BTRFS
- Starts SSH server with temporary password
- Provides connection details for Step 2

### Step 2: Send from Source (Run on SOURCE machine)
- Detects BTRFS subvolumes on source
- Creates read-only snapshots
- Streams data via SSH using `btrfs send/receive`
- Only actual data is transferred

### Step 3: Finalize Target (Run on TARGET machine)
- Converts received snapshots to writable subvolumes
- Updates fstab with new UUID
- Installs GRUB bootloader
- Configures boot entries

## Key Features

- **No GUI required** - Uses dialog for TUI interface
- **Live USB compatible** - Runs from any EndeavourOS/Arch live environment
- **Network transfer** - Uses SSH for secure data transfer
- **Subvolume-aware** - Handles multiple subvolumes correctly
- **Efficient** - Only transfers actual data, not empty space
- **Target size flexible** - Target can be smaller than source (as long as it fits the data)

## Requirements

- Two machines (source and target) with BTRFS partitions
- Both booted from EndeavourOS/Arch live USB
- Network connectivity between machines
- sudo/root access

## Installation

```bash
# Download the script
curl -O https://raw.githubusercontent.com/x805/btrfsforge/main/btrfsforge.sh
chmod +x btrfsforge.sh

# Run as root
sudo ./btrfsforge.sh
