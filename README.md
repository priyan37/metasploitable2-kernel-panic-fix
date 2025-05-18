# Kernel Panic Fix (noapic) for Metasploitable2 in VirtualBox

A common issue when running Metasploitable2 on VirtualBox is the dreaded:

```

kernel panic – not syncing: IO-APIC + timer doesn't work!
Boot with apic=debug and send a report.
Then try booting with the ‘noapic’ option.

```

---

## ❗ Problem

When starting the Metasploitable2 VM in VirtualBox, you may encounter:

- `APIC: spurious interrupt`
- `Kernel panic - not syncing`
- System freezes or drops to recovery shell

This happens due to **I/O APIC incompatibility** with older kernels used in Metasploitable2.

---

## ⚙️ Phase 1: VirtualBox Settings Fix

Disable the problematic I/O APIC feature from VirtualBox:

1. **Shut down the VM**
2. Open **VirtualBox > Settings > System > Motherboard**
3. **Uncheck** ✅ “Enable I/O APIC”
4. (Optional) Check:
   - ✅ “Enable VT-x/AMD-V”
   - ❌ “Enable EFI”
5. **Start the VM**  
   - If it boots successfully, you're done!
   - If not, continue to Phase 2.

---

## 🐧 Phase 2: Temporary GRUB Fix (Boot-Time Patch)

If the error persists, apply a temporary fix via GRUB:

1. **Boot the VM**
2. When GRUB menu appears, press `e` to edit the first entry
3. Find the line starting with:
```

kernel /boot/vmlinuz-...

```
4. Append `noapic` to the end of that line
```

kernel /boot/vmlinuz-... ro quiet splash noapic

````
5. Press `Ctrl + X` or `F10` to boot

✅ If it boots fine, go to Phase 3 to make it permanent.

---

## 💾 Phase 3: Permanent GRUB Fix

Make the `noapic` setting permanent to avoid future issues:

1. Open terminal inside the VM:
```bash
sudo nano /boot/grub/menu.lst
````

2. Find the boot entry (`kernel /boot/vmlinuz-...`) and append:

   ```
   noapic
   ```
3. Save (`Ctrl + O`) and exit (`Ctrl + X`)
4. (Optional but safe):

   ```bash
   sudo update-grub
   ```
5. Reboot:

   ```bash
   sudo reboot
   ```

---

## ✅ Result

Metasploitable2 should now boot **cleanly and consistently**, with no APIC kernel panic errors.

---

## 🧠 Notes

* This fix targets legacy Ubuntu 8.04 kernels inside Metasploitable2
* Tested on VirtualBox 6.x and 7.x
* Works across most host operating systems

---
