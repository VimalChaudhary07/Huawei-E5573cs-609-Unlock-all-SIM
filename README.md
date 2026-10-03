# Huawei E5573Cs-609 (Airtel 4G HotSpot) — Unlock All SIM

Complete file kit to unlock the **Huawei E5573Cs-609** MiFi (Airtel India locked) so it accepts **Jio / Vi / BSNL / any SIM**.

> 🎬 **Watch this video first — it is the walkthrough for this exact kit:**
> **[Huawei mtn mifi E5573cs-609 unlocking tutorial complete guide](https://www.youtube.com/watch?v=By-oze8cFYQ)** by Guystave Mabila

These devices do **not** accept NCK codes generated from the IMEI, and once the
"remaining attempts" counter hits 0 the code route is dead permanently. The only
reliable method is the **boot-shot (test-point) flash** described below.

---

## What's in this repo

| Folder | What it contains |
|---|---|
| `Step 1 (Installing Drivers)` | Huawei HiLink driver + Mobile Partner, and `Error Code 10.reg` — a registry patch (**required on Windows 8/10**, it makes Windows skip the USB BOS-descriptor query that stalls the Balong flasher) |
| `Step 2 (Using Balong Tool)` | `Balong.exe` flasher + two usbloaders: **`E5573_usblsafe.bin` (use this one — keeps your IMEI, S/N and reset button)** and `E5573CSusbloader.bin` (standard, but erases NV → IMEI/SN/reset lost) |
| `Step 3 (Downgrading Firmware)` | Firmware `21.318.99.03.00` package |
| `Step 4 (Upgrading Firmware)` | Final firmware `21.318.03.00.00` package |
| `Step 5 (Installing Web UI)` | Three dashboards — **Airtel**, **Huawei Stock**, **Idea** (Idea one supports USSD dialing) — each with a preview image |
| `imei repair` | DC-Unlocker client, used to restore IMEI / S/N after flashing |

## The procedure (summary)

1. **Install drivers** — run the installers in Step 1. Then double-click
   `Error Code 10.reg` and accept (this is the Windows 8/10 fix — skip it and
   Balong will hang at flashing).
2. **Open the dongle** down to the board, battery in, device **off**, SIM removed.
3. **Boot-shot:** touch a thin copper wire across the two test points and
   **keep it there while plugging in USB**. When the PC detects the device,
   remove only the wire. (Test-point photo is in the video.)
4. **Balong tool:** Detect → `...` → select **`E5573_usblsafe.bin`** → Load →
   wait for 100%. Do not disconnect.
5. Immediately flash **Step 3** (`Downgrade.exe`) → wait for success.
6. Immediately flash a **Web UI from Step 5** (Airtel / Huawei Stock / Idea).
7. Immediately flash **Step 4** (`Upgrade.exe`). Device is now **unlocked**.
8. Default Wi-Fi password after unlock: **`12345678`**.

### Restoring your original IMEI / S/N (after flash)

Check in DC-Unlocker (`imei repair` folder). If IMEI shows `000000000000000`:

1. **Insert any working SIM** (required — IMEI write is refused without one),
   power on, connect USB, detect in DC-Unlocker.
2. In the command box type (your original 15-digit IMEI, from the sticker):
   ```
   AT^CIMEI="XXXXXXXXXXXXXXX"
   ```
3. To restore the 16-character S/N, convert it to hex bytes (HxD editor or any
   ASCII→hex tool) and write it to NV item 6:
   ```
   AT^NVWR=6,16,XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX
   ```
4. Restart the device and re-detect to confirm.

## Important notes

- **Never unplug the device between the usbloader load and the last firmware.**
- If both LEDs blink green after flashing, run `Upgrade.exe` again and restart.
- Check band support: unlocking removes the SIM lock, but a SIM still needs a
  band the hardware supports (E5573 = LTE B1/B3/B5/B8/B40/B41 region dependent).
- APN if data doesn't auto-connect: Jio `jionet`, Vi `internet`, BSNL `bsnlnet`.

## Credits

- Method & files: [Hovatek guide — How to unlock Huawei E5573Cs-322/609/931 MiFi](https://www.hovatek.com/forum/thread-16772.html)
- Video walkthrough: [Guystave Mabila](https://www.youtube.com/watch?v=By-oze8cFYQ)
- Firmware © Huawei / carriers — hosted here solely for repair of devices you
  own. No warranty; flashing carries a real brick risk if done wrong.
