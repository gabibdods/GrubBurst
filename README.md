# Uninstall GRUB from a Windows Boot Drive

### Description

- This guide provides a step-by-step method for safely removing the GRUB bootloader from a Windows EFI system after uninstalling a Linux distribution
- It ensures that the Windows Boot Manager resumes control without requiring a full OS reinstall or boot repair

---

## NOTICE

- These steps affect system boot configurations
- Proceed only if you are comfortable making changes to your EFI partition
- Always back up important data before modifying boot settings — your attention to detail is appreciated

---

## Problem Statement

- After uninstalling Linux, GRUB may persist in the boot sequence, causing confusion or boot failures
- This guide addresses how to manually remove GRUB files and reclaim sole boot control for Windows

---

## Project Goals

### Reclaim Windows Boot Manager

- Remove GRUB's EFI entries and ensure Windows Boot Manager is the sole active bootloader

### Safely Modify EFI Partition

- Assign and access the EFI system partition without harming other system-critical files or configurations

---

## Tools, Materials & Resources

### DiskPart

- Windows utility for managing disks, partitions, and volumes from the command line

### EFI Boot Partition

- Must be present and correctly identified. Generally labeled as `SYSTEM` with FAT32 format

---

## Design Decision

### Manual Partition Assignment

- Assigning a drive letter to the EFI partition makes it easier to navigate and manage GRUB files directly

### Conservative Cleanup

- The method only targets the GRUB directory (e.g., `ubuntu`) to avoid damaging other boot entries

### Reversibility

- Optional steps like removing the assigned drive letter ensure the system remains tidy and predictable after cleanup

---

## Features

### Safe Disk Navigation

- Uses DiskPart to enumerate and select correct volumes without ambiguity

### Targeted GRUB Removal

- Directly deletes the GRUB-related folder from the `EFI` directory without affecting Windows boot files

### Minimal Downtime

- Designed to be completed in minutes, allowing you to reboot and verify changes immediately

---

## Procedure

### Steps to Remove GRUB

1. Open a Command Prompt window as administrator, and type:
```plaintext
diskpart
```

2. List Available Disks:
```plaintext
list disk
```

3. Identify the disk that contains your Windows EFI partition (usually the system boot disk).

4. Select the Correct Disk:
```plaintext
select disk <disk_name>
```

5. List the Volumes:
```plaintext
list vol
```

6. Assign a Drive Letter for Convenience:
```plaintext
assign letter=X
```

7. Exit DiskPart:
```plaintext
exit
```

8. Back in the Command Prompt (still as admin), type:
```plaintext
Z:
```

9. List directories:
```plaintext
dir
```

10. Navigate to the EFI directory:
```plaintext
cd EFI
```

11. List the contents of the EFI directory:
```plaintext
dir
```

12. You should see a folder called ubuntu (or a similar Linux distribution). Remove the GRUB folder:
```plaintext
rmdir /S ubuntu
```

13. Remove the assigned drive letter:
```plaintext
diskpart
select vol <y>
remove letter=Z
exit
```

---

## Functional Overview

- Identify the EFI system partition using DiskPart
- Mount it with a drive letter (e.g., Z:)
- Delete the GRUB (e.g., ubuntu) directory from within the EFI folder
- Remove the assigned drive letter for cleanliness

---

## Challenges & Solutions

### EFI Partition Access

- Use DiskPart to mount the hidden EFI partition and make it accessible via a drive letter

### Removing Only GRUB Files

- Navigate precisely to the `EFI` directory and delete only the `ubuntu` (or similar) folder

---

## Lessons Learned

### Editing Boot Drives

- Modifying boot partitions requires caution, but Windows provides robust tools (e.g., DiskPart) for precision control

### Removing Bloatware

- Cleaning up boot configurations post-Linux removal is important to prevent future boot confusion

---

## Future Enhancements

- Include automated PowerShell or Batch script to simplify EFI mounting and GRUB removal
- Add checks to confirm if GRUB folder exists before deletion
- Extend support for systems with dual-boot managers (e.g., rEFInd, systemd-boot)
