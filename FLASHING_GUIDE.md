# Sofle Hybrid Keyboard Flashing Guide

Complete step-by-step guide for flashing new firmware to your Sofle Hybrid keyboard.

---

## 📋 Overview

This guide will walk you through:
1. Manually triggering a firmware build on GitHub
2. Downloading the compiled firmware
3. Flashing both left and right keyboard halves
4. Testing your keyboard

**Estimated time:** 20-30 minutes (including build time)

---

## ⚠️ CRITICAL SAFETY RULES

**READ THIS BEFORE STARTING:**
- ✅ Only ONE keyboard side should be powered at a time
- ✅ Power can come from EITHER the USB cable OR the power switch
- ✅ When flashing one side, the OTHER side MUST be completely OFF
- ✅ Turn off power switch AND unplug USB on the side you're NOT flashing

**Why?** Having both sides powered during flashing can cause connection issues and firmware conflicts.

---

## STEP 1: Trigger the Build Manually

1. Open your web browser and go to your GitHub repository
2. Click the **"Actions"** tab at the top of the page
3. In the left sidebar, click **"Build ZMK Firmware"**
4. Click the **"Run workflow"** dropdown button (top right area, above the workflow list)
5. In the dropdown:
   - **Branch:** Select `suho` (or whichever branch has your changes)
   - Click the green **"Run workflow"** button

**What happens next:** GitHub Actions will compile your firmware. This process takes approximately 5-10 minutes.

---

## STEP 2: Monitor the Build

1. Stay on the Actions page (or refresh it)
2. You'll see your workflow run appear at the top of the list
3. **Status indicators:**
   - 🟡 Yellow dot = Build in progress
   - ✅ Green checkmark = Build succeeded
   - ❌ Red X = Build failed

4. **Wait for the green checkmark** before proceeding
5. If you see a red X, click on the run to see error details

---

## STEP 3: Download the Firmware

1. Click on your completed workflow run (the one with the green checkmark)
2. Scroll down to the bottom of the page
3. Find the **"Artifacts"** section
4. Click on **"firmware"** to download a ZIP file
5. **Extract/unzip the downloaded file** to a folder on your computer

### Files You Should Have:

After extracting, you should see these 3 files:
- `sofle_left-nice_nano_v2-zmk.uf2` ← LEFT keyboard firmware
- `sofle_right-nice_nano_v2-zmk.uf2` ← RIGHT keyboard firmware
- `settings_reset-nice_nano_v2-zmk.uf2` ← Optional reset utility

---

## STEP 4: Understanding the RST Button

Before flashing, you need to know how to enter bootloader mode.

### Where are the RST and GND Pins?

The RST (Reset) and GND (Ground) pins are on the Nice!Nano microcontroller:

1. Look at your keyboard - find the Nice!Nano board (the small controller with USB-C and OLED)
2. On the Nice!Nano board, locate the pin headers (the rows of metal pins along the edges)
3. **RST pin:** Labeled "RST" or "RESET" (usually on the edge of the board)
4. **GND pin:** Labeled "GND" (Ground) - there are multiple GND pins, use any one

### How to Enter Bootloader Mode (Connect GND and RST)

**⚠️ IMPORTANT: This method is for keyboards WITHOUT a physical reset button.**

**The Technique:**
1. **Connect USB cable** to the keyboard first (keyboard must be powered)
2. **Locate RST and GND pins** on the Nice!Nano board
3. **Use a conductive tool** (tweezers, wire, paperclip, or screwdriver)
4. **Touch both RST and GND pins simultaneously** for a brief moment (~0.5 seconds)
5. **Remove the tool** - don't hold too long

**Alternative Method (if RST button exists):**
- Press and release the RST button **twice quickly**
- Like double-clicking a mouse: "click-click"
- **Timing:** Both presses within ~0.5 seconds

### How to Know It Worked:

When successful, you'll see:
1. ✅ **A USB drive appears** on your computer (within 2-3 seconds)
   - Windows: New drive letter appears (D:, E:, etc.)
   - Mac: Drive appears on desktop or in Finder
   - Name: Usually "NICENANO" or "NRF52BOOT"
2. ✅ **OLED screen goes blank** or changes
3. ✅ **Keyboard stops responding** to keypresses

**If nothing happens:** Try again with slightly different timing (faster or slower).

---

## STEP 5: Flash the RIGHT Side

### Preparation:
1. ✅ Ensure **LEFT side is completely OFF** (power switch off, no USB)
2. ✅ Have the file `sofle_right-nice_nano_v2-zmk.uf2` ready

### Flashing Steps:

1. **Connect RIGHT keyboard** to your computer with USB-C cable
2. **Locate the RST button** on the Nice!Nano
3. **Double-press the RST button** (quick tap-tap)
4. **Wait for USB drive to appear** (2-3 seconds)
   - Drive name: "NICENANO" or "NRF52BOOT"
5. **Drag and drop** `sofle_right-nice_nano_v2-zmk.uf2` onto the USB drive
   - You can also copy-paste the file
6. **Wait** - The file will copy, then:
   - Keyboard automatically reboots
   - USB drive disappears
   - **Your OS may show an error - THIS IS NORMAL, ignore it**
7. **Unplug USB cable** from right keyboard
8. **Keep right side OFF** (power switch off)
9. ✅ **RIGHT SIDE IS NOW FLASHED**

---

## STEP 6: Flash the LEFT Side

### Preparation:
1. ✅ Ensure **RIGHT side is completely OFF** (power switch off, no USB)
2. ✅ Have the file `sofle_left-nice_nano_v2-zmk.uf2` ready

### Flashing Steps:

1. **Connect LEFT keyboard** to your computer with USB-C cable
2. **Locate the RST button** on the Nice!Nano
3. **Double-press the RST button** (quick tap-tap)
4. **Wait for USB drive to appear** (2-3 seconds)
5. **Drag and drop** `sofle_left-nice_nano_v2-zmk.uf2` onto the USB drive
6. **Wait** - The file will copy, then:
   - Keyboard automatically reboots
   - USB drive disappears
   - **Your OS may show an error - THIS IS NORMAL, ignore it**
7. **Keep LEFT keyboard plugged in** via USB
8. ✅ **LEFT SIDE IS NOW FLASHED**

---

## STEP 7: Power On and Connect Both Sides

1. **LEFT keyboard should still be connected** via USB-C
2. **Turn ON the RIGHT keyboard** using its power switch
3. **Wait 5-10 seconds** for Bluetooth pairing
4. **Check OLED screens** on both sides:
   - Look for Bluetooth/WiFi icon
   - A checkmark ✓ next to it means successfully connected
5. If no checkmark appears after 15 seconds, see Troubleshooting below

---

## STEP 8: Test Your Keyboard

1. **Open a text editor** (Notepad, TextEdit, VS Code, etc.)
2. **Test the new modifier key layout:**

### NEW Bottom Row Layout (Thumb Clusters):

**LEFT SIDE (from left to right):**
- MUTE → **CTRL** → ALT → **GUI** → LOWER → ENTER

**RIGHT SIDE (from left to right):**
- SPACE → RAISE → **GUI** → ALT → **CTRL**

### What Changed:
- **Left side:** CTRL and GUI positions swapped
- **Right side:** CTRL and GUI positions swapped

3. **Test combinations:**
   - Left CTRL+C (should be copy)
   - Left GUI+C (on Mac should NOT be copy)
   - Try all modifier combinations

4. **Test other keys:**
   - All letter keys
   - Number row
   - Function keys (hold LOWER layer)
   - Navigation keys (hold RAISE layer)

5. **Test the 5-way switch** on the right side
6. **Test the rotary encoder** on the left side

---

## 🎉 Success!

If everything works, you're done! Your keyboard now has:
- ✅ CTRL and GUI keys swapped on both sides
- ✅ Fresh firmware flashed
- ✅ Both sides connected wirelessly

---

## ⚠️ Troubleshooting

### Issue: Sides Won't Connect to Each Other

**Solution 1: Settings Reset**
1. Turn off both keyboards
2. Flash `settings_reset-nice_nano_v2-zmk.uf2` to the RIGHT side:
   - Connect right side via USB
   - Double-press RST button
   - Drag settings_reset file onto USB drive
   - Wait for reboot
   - Unplug USB
3. Flash `settings_reset-nice_nano_v2-zmk.uf2` to the LEFT side:
   - Connect left side via USB
   - Double-press RST button
   - Drag settings_reset file onto USB drive
   - Wait for reboot
   - Keep USB plugged in
4. Re-flash the actual firmware (repeat Steps 5-6)
5. Try connecting again (Step 7)

**Solution 2: Power Cycle**
1. Turn off both keyboards
2. Unplug all USB cables
3. Wait 30 seconds
4. Connect LEFT side via USB
5. Wait 10 seconds
6. Turn ON right side
7. Wait 15 seconds for connection

### Issue: Bootloader Mode Won't Activate

**Try these:**
- Press RST button faster (quicker tap-tap)
- Press RST button slower (with small pause between)
- Press RST button THREE times instead of two
- Use a different tool (pen tip, tweezers, etc.)
- Try a different USB cable
- Try a different USB port on your computer

### Issue: USB Drive Doesn't Appear

**Try these:**
- Different USB cable (some cables are charge-only)
- Different USB port
- Restart your computer
- Try on a different computer
- Check that the power switch is ON (or USB provides power)

### Issue: Keys Don't Work as Expected

**Check:**
- Did you flash the correct file to each side?
  - RIGHT side needs the "right" file
  - LEFT side needs the "left" file
- Did you flash the latest firmware from the correct workflow run?
- Try re-flashing both sides

### Issue: OLED Shows Nothing

**This is normal if:**
- You just flashed settings_reset (screen stays blank after reset)
- Solution: Flash the actual firmware files

**If firmware is flashed but screen is blank:**
- Check power switch is ON
- Try power cycling the keyboard
- OLED may be disabled in config (check `CONFIG_ZMK_DISPLAY=y` in config)

### Issue: Error Message When Copying Firmware File

**Don't worry!** This is NORMAL behavior:
- Windows/Mac often shows "Device was ejected improperly"
- The keyboard reboots automatically during flashing
- This causes the USB drive to disappear suddenly
- **Your firmware IS flashed correctly - ignore the error**

---

## 📝 Tips for Future Flashing

1. **Keep firmware files organized** - Label them with dates or version numbers
2. **Flash both sides** whenever you update firmware (don't skip one side)
3. **Settings reset** - Use this if you have persistent connection issues
4. **Bookmark your GitHub Actions page** for easy access
5. **Practice double-pressing RST** - You'll get better with practice!

---

## 🔗 Additional Resources

- **ZMK Documentation:** https://zmk.dev/docs
- **ZMK Studio:** https://zmk.studio/
- **Visual Keymap Editor:** https://nickcoutsos.github.io/keymap-editor
- **Your Repository Actions:** Click "Actions" tab on GitHub

---

## ✅ Checklist

Use this checklist when flashing:

- [ ] Trigger workflow on GitHub Actions
- [ ] Wait for green checkmark
- [ ] Download firmware artifact
- [ ] Extract ZIP file (3 .uf2 files)
- [ ] Turn off BOTH keyboards
- [ ] Flash RIGHT side with right .uf2 file
- [ ] Unplug RIGHT side, keep it OFF
- [ ] Flash LEFT side with left .uf2 file
- [ ] Keep LEFT side plugged in
- [ ] Turn ON RIGHT side
- [ ] Wait for Bluetooth connection
- [ ] Test all keys
- [ ] Test new modifier layout
- [ ] Celebrate! 🎉

---

**Questions?** Check the troubleshooting section or refer to the main README.md file.

**Good luck with your flashing!** 🚀
