# BtrfsForge Release Notes

## Version 1.0.0 - Initial Release

### Features
- Complete BTRFS live migration tool
- Three-step workflow for system transfer
- SSH-based secure data transfer
- TUI interface using dialog
- Subvolume detection and handling
- Automatic GRUB installation
- fstab UUID correction
- Support for both UEFI and BIOS systems

### Requirements
- Linux kernel 4.14+ with BTRFS support
- BTRFS tools (btrfs-progs)
- SSH server/client
- dialog (auto-installed if missing)
- sudo/root privileges
- Network connectivity between machines

### Known Issues
- Large transfers may be interrupted by network instability
- Very slow networks may cause SSH timeouts
- Some custom subvolume layouts might need manual adjustment
- GRUB detection may fail in non-standard installations
