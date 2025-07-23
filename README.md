ติดตั้งเลย์เอาต์แป้นพิมพ์ไทยแบบมนูญชัยและแบบกำหนดเองอื่น ๆ บนระบบ Linux (XKB)

Install the Manoonchai and other custom Thai keyboard layouts on Linux (XKB) [(English below)](#english)

## เตรียมไฟล์เลย์เอาต์
ดาวน์โหลดไฟล์เลย์เอาต์
```
wget https://github.com/hiohlan/kiimo/raw/main/output/Manoonchai/Manoonchai_xkb --output-document=Manoonchai_xkb
```
ⓘ หากคุณใช้ไฟล์ของตนเองอยู่แล้ว ไม่จำเป็นต้องดาวน์โหลดไฟล์ด้านบน

### หากคุณใช้เลย์เอาต์อื่น (นอกเหนือจาก Manoonchai_xkb)  
กรุณาเปลี่ยนชื่อ `Manoonchai_xkb` ที่ปรากฏในคำสั่งต่อๆ ไป ให้ตรงกับชื่อไฟล์เลย์เอาต์ที่คุณใช้งาน เช่น:
  - `Manoonchai-WittNV_xkb`
  - `Manoonchai_jis_xkb`
  - หรือชื่ออื่น ๆ ตามไฟล์ที่คุณต้องการติดตั้ง

## ทำการติดตั้ง
มีแบบอย่าง 3 แบบให้เลือกใช้ตามความเหมาะสมของระบบของคุณ:
## ➊ แบบ `setxkbmap`, สำหรับเซสชัน X ปัจจุบันเท่านั้น
เหมาะสำหรับผู้ที่ไม่เข้าถึงสิทธิ Super User หรือไม่อยากไปยุ่งอะไรกับไฟล์ระบบ
>⚠️ หมายเหตุ: ตอนนี้ผู้เขียนใช้วิธีนี้กับ GNOME Shell (Wayland) ไม่ได้;  
ให้ใช้วิธีถัดไปแทน หากท่านมีวิธีแก้จักขอบคุณยิ่ง

1. ติดตั้งลงในโฟลเดอร์ผู้ใช้  
    ```
    mkdir -p $HOME/.xkb/symbols;
    sed '1s/^\xEF\xBB\xBF//' Manoonchai_xkb > $HOME/.xkb/symbols/Manoonchai_xkb;
    ```

2. เรียกใช้เลย์เอาต์  
  US-QWERTY คู่กับมนูญชัย โดยสามารถสลับเลย์เอาต์ได้ด้วยการกดปุ่ม CAPS LOCK
    ```
    setxkbmap -layout 'us,Manoonchai_xkb' -option 'grp:caps_toggle' -print | xkbcomp -I"$HOME/.xkb" - $DISPLAY
    ```
    หากคอมของท่านไม่ล้างไฟล์หลังออกจากระบบ คำสั่งเดียวกันนี้สามารถใช้ได้ในเซสชันถัดไปทันทีโดยไม่ต้องติดตั้งใหม่ และสามารถเพิ่มใน `~/.xprofile` หรือ `~/.xinitrc` (ขึ้นกับระบบของท่าน) เพื่อให้เรียกใช้เลย์เอาต์เองเมื่อเข้าสู่ระบบได้ทันที

## ➋ แบบลงเป็น Variant ส่วนหนึ่งของแป้นพิมพ์ภาษาไทยที่มีอยู่เดิม
เหมาะสำหรับผู้ที่มีสิทธิ์ Super User และต้องการรักษาแป้นพิมพ์ภาษาไทยอันเดิมไว้ เพื่อใช้งานร่วมกับแป้นพิมพ์ใหม่
1. ดาวน์โหลดสคริปต์ `installmanoonchai.sh` ไว้ที่เดียวกับที่คุณเตรียมไฟล์เลย์เอาต์  
    ```
    wget https://github.com/hiohlan/mnc/raw/main/installmanoonchai.sh --output-document=installmanoonchai.sh
    ```
2. แปลงให้เป็นไฟล์รันได้  
    ```
    chmod +x ./installmanoonchai.sh
    ```
3. ติดตั้งแป้นพิมพ์  
    ```
    sudo ./installmanoonchai.sh Manoonchai_xkb
    ```
4. เพื่อความมั่นใจ ควร logout จาก X, หรือ restart ด้วยก็ดี
5. ไปที่ Keyboard setting, มองหา/เพิ่ม `Thai (Manoonchai v1.0)`.  
    (ถ้าคุณใช้เลย์เอาต์ที่กำหนดเองหรือเปลี่ยนชื่อไฟล์ ให้มองหาชื่อ variant ที่คุณกำหนดไว้ในไฟล์ xkb ของคุณ ในบรรทัด `name[Group1]= "..."`)  
    (ขั้นตอนนี้อาจแตกต่างกันตาม desktop environment ที่ท่านใช้)

*(หากอัปเดต Kernel, แล้วแป้นพิมพ์หาย, ให้ซ้ำขั้นตอนทั้งหมดใหม่)*

## ➌ แบบไทยจงเจริญ
1. เปิดไฟล์ `Manoonchai_xkb` ที่โหลดมา
2. คัดลอกทุกอย่างที่ขึ้นต้นด้วย `key <XXXX>` รวมถึง `include "level3(ralt_switch)"`
3. เปิดไฟล์ `/usr/share/X11/xkb/symbols/th`
4. ใน `xkb_symbols "basic" {...}` ลบทุกอย่างที่ขึ้นต้นด้วย `key <XXXX>`
5. นำของจากข้อ 2 วางลงไปใน `xkb_symbols "basic" {...}` นั้น
6. บันทึกไฟล์แล้วออก
7. logout จาก X, หรือ restart. หรือถ้าใช้ Debian-based *(อาทิ Ubuntu, Linux Mint)* ล้าง xkb cache ด้วย, โดย `sudo dpkg-reconfigure xkb-data`
8. แป้นพิมพ์ของท่านกลายร่างเป็นแป้นพิมพ์มนูญชัย

*(หากอัปเดต Kernel, แล้วแป้นพิมพ์หาย, ให้ซ้ำขั้นตอนทั้งหมดใหม่)
*
# English
## Prepare the Layout File
Download layout file:
```
wget https://github.com/hiohlan/kiimo/raw/main/output/Manoonchai/Manoonchai_xkb --output-document=Manoonchai_xkb
```
ⓘ If you're using your own layout file, skip the command above.

### If you're using a different layout (not Manoonchai_xkb)  
Replace all instances of `Manoonchai_xkb` in any commands with the actual name of your layout file, such as:
  - `Manoonchai-WittNV_xkb`
  - `Manoonchai_jis_xkb`
  - Or other names according to the file you want to install.

## Installation
There are 3 methods depending on your system and needs:
## ➊ Using `setxkbmap` (for the current X session only)
Best if you don’t have superuser privileges or don’t want to touch system files
>⚠️ Note: This method currently does not work for the author when using GNOME Shell on Wayland.  
Please consider the next method instead.  
If you know a workaround — your contribution would be most appreciated!  
1. Install to your user directory:  
    ```
    mkdir -p $HOME/.xkb/symbols;
    sed '1s/^\xEF\xBB\xBF//' Manoonchai_xkb > $HOME/.xkb/symbols/Manoonchai_xkb;
    ```

2. Apply the layout  
  US-QWERTY combined with Manoonchai, toggleable via the CAPS LOCK key:
    ```
    setxkbmap -layout 'us,Manoonchai_xkb' -option 'grp:caps_toggle' -print | xkbcomp -I"$HOME/.xkb" - $DISPLAY
    ```
    If your system doesn't clean files between sessions, the same command can be reused in future sessions without reinstalling. You can also add it to `~/.xprofile` or `~/.xinitrc` (depending on your system) to automatically apply the layout at login.

## ➋ Installing as a Variant of the Existing Thai Keyboard
Best for users with superuser privileges who want to keep the existing Thai layout while using the new one.
1. Download the `installmanoonchai.sh` script to the same directory as your layout file:  
    ```
    wget https://github.com/hiohlan/mnc/raw/main/installmanoonchai.sh --output-document=installmanoonchai.sh
    ```
2. Make it executable:  
    ```
    chmod +x ./installmanoonchai.sh
    ```
3. Install the keyboard layout:  
    ```
    sudo ./installmanoonchai.sh Manoonchai_xkb
    ```
4. For best results, log out of your X session or restart your system.
5. Go to your keyboard settings and look for/add `Thai (Manoonchai v1.0)`  
    (If you use a custom layout or renamed it, look for the variant name you defined in your xkb file, `name[Group1]= "..."`).  
    (The exact steps may vary depending on your desktop environment.)

*(If your keyboard layout disappears after a kernel update, repeat the entire process.)*

## ➌ ไทยจงเจริญ Style Installation
1. Open the `Manoonchai_xkb` file you downloaded
2. Copy everything starting with `key <XXXX>` including the line `include "level3(ralt_switch)"`
3. Open the file  `/usr/share/X11/xkb/symbols/th`
4. In the section `xkb_symbols "basic" { ... }`, remove all lines starting with `key <XXXX>`
5. Paste the copied content from step 2 into the `xkb_symbols "basic" { ... }`
6. Save and close the file
7. Log out of your X session or restart your system. If you're on a Debian-based system *(e.g., Ubuntu, Linux Mint)*, also clear the xkb cache with: `sudo dpkg-reconfigure xkb-data`
8. Your keyboard is now transformed into the Manoonchai layout

*(If your keyboard layout disappears after a kernel update, repeat the entire process.)*
