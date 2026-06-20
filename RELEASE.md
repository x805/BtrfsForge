
## RELEASE.md

```markdown
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

### Bug Fixes
- Fixed SSH key handling to prevent duplicate generation
- Added proper cleanup trap for temporary files and mounts
- Improved subvolume detection with error handling
- Added pre-transfer space verification
- Enhanced error messages for troubleshooting
- Fixed variable quoting issues

### Changes from Original
- Renamed from btrfs-migrate to BtrfsForge
- Removed version-specific references from script
- Improved error handling throughout
- Added SSH connection testing before key exchange
- Better support for non-standard subvolume naming
- Enhanced fstab UUID replacement
- Added comprehensive documentation

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

### Installation
```bash
curl -O https://raw.githubusercontent.com/yourusername/btrfsforge/main/btrfsforge.sh
chmod +x btrfsforge.sh
sudo ./btrfsforge.sh
