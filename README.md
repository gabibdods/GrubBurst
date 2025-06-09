# How to Uninstall GRUB from a Windows Boot Drive After Removing Linux

🖥️ If you’ve uninstalled Linux from your system and want to remove the GRUB bootloader from your Windows boot drive, this guide will walk you through the process safely.

> ⚠️ **Important:**  
> - These steps modify your system’s EFI partition. Proceed with caution.  
> - Always back up important data before making changes to your boot configuration!

---

## 📝 Steps to Remove GRUB

### 1️⃣ Open DiskPart

Open a Command Prompt window **as administrator**, and type:

```bash
diskpart
````

### 2️⃣ List Available Disks

```bash
list disk
```

Identify the disk that contains your Windows EFI partition (usually the system boot disk).

### 3️⃣ Select the Correct Disk

Replace `<x>` with the number of the disk:

```bash
select disk <x>
```

### 4️⃣ List the Volumes

```bash
list vol
```

Identify the **EFI System Partition** (usually has the label `SYSTEM`).

### 5️⃣ Select the EFI Volume

Replace `<y>` with the volume number:

```bash
select vol <y>
```

### 6️⃣ Assign a Drive Letter for Convenience

```bash
assign letter=Z
```

This assigns the EFI partition to the drive letter `Z:`.

### 7️⃣ Exit DiskPart

```bash
exit
```

---

### 8️⃣ Remove GRUB Boot Entries

Back in the Command Prompt (still as admin), type:

```bash
Z:
```

List directories:

```bash
dir
```

Navigate to the EFI directory:

```bash
cd EFI
```

List the contents of the EFI directory:

```bash
dir
```

You should see a folder called `ubuntu` (or a similar Linux distribution).
Remove the GRUB folder:

```bash
rmdir /S ubuntu
```

> 🔥 This permanently deletes the `ubuntu` (GRUB) boot files from the EFI partition.

---

### 9️⃣ Clean Up (Optional)

If you want to remove the assigned drive letter:

```bash
diskpart
select vol <y>
remove letter=Z
exit
```

---

## 💡 Additional Notes

✅ This process only removes GRUB’s boot entries.
✅ Your Windows boot manager will automatically take over at the next boot.
✅ If you want to double-check, you can reboot and enter your BIOS/UEFI boot menu to verify the GRUB entry is gone.

---

## 📚 Resources

* Microsoft Docs: [diskpart commands](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/diskpart)
* Windows Boot Manager troubleshooting: [bcdedit](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/bcdedit-command-line-options-techref-di)

---

## 🎉 Done!

You’ve successfully removed GRUB from your Windows boot drive! If you have questions or need further assistance, feel free to ask. 🎉

```
Let me know if you’d like me to prepare this as a downloadable `.md` file! 🚀
```