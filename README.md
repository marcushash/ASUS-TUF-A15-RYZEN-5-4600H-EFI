# Hackintosh EFI – ASUS TUF A15 (Ryzen 5 4600H & GTX 1650)

💻 OpenCore EFI for running **macOS** on an **ASUS TUF A15** laptop with **Ryzen 5 4600H** and **GTX 1650**.  

> 🚧 **Work in Progress** – This guide will evolve as testing progresses.

---

## Project Status

### Screenshot 📸  
*(Add a screenshot of macOS running on your TUF A15!)*

| **macOS Version** | **Status** |
|------------------|-----------|
| macOS Ventura (`13.x`) | ✅ Stable |
| macOS Sonoma (`14.x`) | ✅ Stable |
| macOS Sequoia (`15.x`) | 🟡 In Testing |

---

## Hardware Compatibility
This EFI is designed for the following configuration:  

### System Specifications
| **Component** | **Details** |
|-------------|------------|
| **Model** | ASUS TUF A15 |
| **CPU** | AMD Ryzen 5 4600H |
| **GPU** | NVIDIA GTX 1650 *(disabled, unsupported in macOS)* |
| **Integrated Graphics** | AMD Radeon (patched with NootedRed) |
| **RAM** | 16GB DDR4 |
| **Storage Slot 1** | WD Green SN350 NVMe SSD (250GB) + Stock Samsung NVMe SSD (512GB) |
| **Wi-Fi & Bluetooth** | Intel AX210 *(requires OpenIntelWireless kext till Sonoma 14.4 or OCLP spoof and patch method in Sequoia)* |
| **Audio** | Realtek ALC256 (AppleALC) |
| **Ethernet** | RealtekRTL8111 |
| **SMBIOS** | MacBookPro16,3 |

---

### **ACPI Patches & SSDTs**
The following **SSDTs** are included for proper ACPI patching:  

| **SSDT** | **Purpose** | **Details** |
|---------|------------|------------|
| **SSDT-EC-USBX** | Emulates EC and fixes USB | Essential for AMD laptops |
| **SSDT-PLUG** | Enables CPU power management | Required for Ryzen CPUs |
| **SSDT-HPET** | Fixes IRQ conflicts | Improves system stability |
| **SSDT-XHC** | USB port mapping | Properly maps all USB ports |

These SSDTs are pre-configured in the `EFI/OC/ACPI` folder.

---

## Booting macOS ⛔

To get past the **prohibitory symbol** during boot, you must:
1. Place **OpenVariableRuntimeDxe.efi** **before** **OpenRuntime.efi** in the **config.plist**.
2. Set **LoadEarly** to **True** for **both** drivers.

⚠️ **Warning:** If using **ProperTree** with the **OC Clean Snapshot** function, it may place them in the wrong order and reset the **LoadEarly** value every time. Double-check the order and settings after using Clean Snapshot!

---

## Fixing Intel AX210 Wi-Fi & Bluetooth 🛜
For **Wi-Fi and Bluetooth** on the **Intel AX210**:

- **Ventura/Sonoma (up to 14.4)**: 
  - Wi-Fi: Use **OpenIntelWireless** with **itlwm** and **HeliPort**.
  - Bluetooth: Use **IntelBluetoothFirmware** and **BlueToolFixup** kexts.

- **Sequoia (15.x)**: 
  - Wi-Fi: Use **OCLP spoof and patch** method for native Wi-Fi support.
  - Bluetooth: Use **IntelBluetoothFirmware** with **BlueToolFixup**.
  
➡️ **[Download OpenIntelWireless](https://github.com/OpenIntelWireless/itlwm)**  
➡️ **[Download IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware)**  
➡️ **[Spoof & Patch Trick for Sequoia](https://github.com/OpenIntelWireless/itlwm/issues/1009#issuecomment-2370919270)**  
➡️ **[Fix Bluetooth in Sequoia](https://forum.amd-osx.com/threads/finally-intel-bluetooth-working-under-sequoia-15-0-24a335.5430/post-36917)**

---

## Fixing No Internal Display After Using NootedRed 🟠

If your laptop display goes **black** or doesn’t turn on after applying the **NootedRed** patch for AMD iGPU acceleration:  

1. **Try Alternate NootedRed Kexts:**  
   - Download the available **NootedRed** kexts from this forum post:  
   ➡️ [NootedRed Fix](https://forum.amd-osx.com/threads/nootedred-isnt-accelerating-graphics.5728/post-38822)  
   - Swap out the current **NootedRed.kext** with one of the versions from the post.  
   - Test one by one — **one should work** for your hardware.

2. **Increase VRAM to 2GB:**  
   - After installing macOS, use **Smokeless UMAF** to increase your iGPU VRAM to **2GB**.  
   ➡️ [Smokeless UMAF](https://github.com/SmokelessCPU/UMAF)  

3. **Update to Latest NootedRed:**  
   - Once your display works and macOS is installed, update **NootedRed** by running **CL Actions** on the GitHub repository:  
   ➡️ [NootedRed GitHub](https://github.com/NootInc/NootedRed/actions)  

This should resolve the **no internal display** issue and give you **full GPU acceleration**! 🚀

---

## Limitations ⚠️
- ❌ **No NVIDIA Support** – GTX 1650 is unsupported in macOS.
- ❌ **No Sidecar or Universal Control** – Limited by AMD/Intel architecture.
- ⚠️ **Wi-Fi Requires Patching** – Intel AX210 needs either OpenIntelWireless (up to Sonoma 14.4) or OCLP spoof and patch (Sequoia).

---

### **☕ Support Me on Ko-fi**
If this EFI helped you, consider **buying me a coffee** to support my work! 🚀  
[![Ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/J3J612PNM4)

---

## Credits 🙌
- [Acidanthera](https://github.com/acidanthera) for OpenCore, Lilu, AppleALC.  
- [Dortania’s OpenCore Guide](https://dortania.github.io/OpenCore-Install-Guide/) for documentation.  
- [OpenIntelWireless](https://github.com/OpenIntelWireless) for Intel Wi-Fi and Bluetooth support.  
- [NootedRed](https://github.com/NootInc/NootedRed) for AMD GPU patches.  
- [Smokeless UMAF](https://github.com/SmokelessCPU/UMAF) for iGPU VRAM tweaking.  
- Hackintosh communities for testing and feedback.

---

### 📜 Disclaimer  
⚠️ **This EFI is provided as-is.** I am not responsible for any issues that may arise from using this setup. Proceed at your own risk.
