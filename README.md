# Kiwi Browser

![automatic build of apk](https://github.com/kiwibrowser/src/workflows/automatic%20build%20of%20apk/badge.svg)

## ภาพรวม

[Kiwi Browser](https://kiwibrowser.com/) คือเว็บเบราว์เซอร์สำหรับ Android ที่เป็นโอเพนซอร์สเต็มรูปแบบ

Kiwi สร้างขึ้นบนพื้นฐานของ Chromium คุณสามารถสลับไปใช้ Kiwi ได้อย่างง่ายดายโดยไม่ต้องเรียนรู้อินเทอร์เฟซใหม่หรือทำลายพฤติกรรมการท่องเว็บที่คุ้นเคย

ฟีเจอร์บางอย่างที่ Kiwi Browser รองรับ ได้แก่:

 - โหมดกลางคืน (การทำงานต่างจาก Chromium)
 - รองรับส่วนขยายของ Chrome
 - แถบที่อยู่ด้านล่าง
นอกจากนี้ยังมีการปรับปรุงประสิทธิภาพ (เช่น การ rasterize แบบบางส่วนของ tiles เป็นต้น)

เบราว์เซอร์นี้ได้รับอนุญาตภายใต้ไลเซนส์เดียวกับ Chromium ซึ่งหมายความว่าคุณสามารถสร้างอนุพันธ์ของเบราว์เซอร์ได้

โปรดให้เครดิตโค้ดให้เหมาะสมกับที่มาจาก repository นี้ (อย่าเปลี่ยนเป็นชื่อของคุณเพียงอย่างเดียว)

## สารบัญ

- [ไทม์ไลน์](#timeline)
- [การมีส่วนร่วม (Contributing)](#contributing)
- [การแก้ไข (Modifying)](#modifying)
- [การสร้าง (Building)](#building)
  - [การรับซอร์สดโค้ดและสภาพแวดล้อม](#getting-the-source-code-and-environment)
  - [การติดตั้ง Dependencies](#setting-up-dependencies)
  - [การเตรียมคีย์สำหรับลงนาม](#preparing-a-signing-key)
  - [การกำหนดประเภทการสร้างและแพลตฟอร์ม](#configuring-the-build-type-and-platform)
  - [การเตรียมการสำหรับการสร้างครั้งแรก](#preparing-the-first-build)
  - [การคอมไพล์ Kiwi Browser](#compiling-kiwi-browser)
  - [การสืบสวนการล่มของแอป](#investigating-crashes)
  - [การดีบักจากระยะไกล](#remote-debugging)
  - [การปรับขนาดไบนารีให้เล็กลง](#optimizing-binary-size)
- [โร้ดแมป (Roadmap)](#roadmap)
- [ความช่วยเหลือเพิ่มเติม](#additional-help)

## ไทม์ไลน์

- 15 เมษายน 2018 - การปล่อย Kiwi Browser ครั้งแรก

- 15 เมษายน 2019 - Kiwi Browser ได้รับการรองรับสำหรับ Chrome Extensions

- 17 เมษายน 2020 - Kiwi Browser เปิดเป็นโอเพนซอร์สเต็มรูปแบบ

โค้ดนี้เป็นปัจจุบันและตรงกับการสร้างบน Play Store

การสร้างใหม่จะทำจากเวอร์ชันเปิดของโค้ดโดยตรงไปยัง [Play Store](https://play.google.com/store/apps/details?id=com.kiwibrowser.browser)

มีชั่วโมงการทำงานและไฟล์จำนวนมากที่ถูกเปลี่ยนแปลงใน repository นี้

## การมีส่วนร่วม

ยินดีต้อนรับและสนับสนุนการมีส่วนร่วมทุกชนิด

ถ้าคุณต้องการให้โค้ดของคุณถูกรวมเข้าใน Kiwi ให้เปิด merge request มา ผม (หรือสมาชิกชุมชน) จะช่วยทบทวนโค้ดกับคุณและผลักขึ้น Play Store ให้

## การแก้ไข

ถ้าคุณสร้างเบราว์เซอร์ของคุณเองหรือม็อด ให้แน่ใจว่าได้เปลี่ยนชื่อเบราว์เซอร์และไอคอนในไฟล์ `chrome/android/java/res_chromium/values/channel_constants.xml` และสตริงแปลภาษา (ค้นหาและแทนที่ตามความจำเป็น)
เมื่อเปลี่ยนไอคอนของแอป ให้เพิ่มไฟล์ไอคอนใหม่ในโฟลเดอร์ `chrome/android/java/res/mipmap` (mdpi, hdpi เป็นต้น) และอัปเดต AndroidManifest.xml ด้วย

## การสร้าง

เครื่องอ้างอิงสำหรับการสร้างใช้ Ubuntu 19.04 (ทดสอบบน Ubuntu 18.04 และ 19.10 แล้วเช่นกัน)

ข้อกำหนดขั้นต่ำของระบบคือ 2 vCPU และ 7.5 GB หน่วยความจำ

คุณสามารถใช้ virtual machine, AWS VM หรือ Google Cloud VM

### การรับซอร์สดโค้ดและสภาพแวดล้อม

ในการสร้าง Kiwi Browser คุณสามารถโคลน repository โดยตรง เพราะเราได้รวม dependencies มาแล้ว:

ใน ~ (ไดเรกทอรีบ้านของคุณ) ให้รัน:

```
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
```

และแก้ไขไฟล์ ~/.bashrc โดยเพิ่มตอนท้ายด้วย

```bash
export PATH=$HOME/depot_tools:$PATH
```

ตรวจสอบการเปลี่ยนแปลงด้วยการรัน:

```bash
source ~/.bashrc
```

นี่จะทำให้คุณเข้าถึงยูทิลิตี้หนึ่งตัวคือ gclient (ย่อมาจาก "Google client")

สร้างไดเรกทอรีชื่อ ~/chromium/ และใน ~/chromium/ ให้รัน:

```bash
git clone https://github.com/kiwibrowser/dependencies.git .cipd
cp ~/chromium/.cipd/.gclient ~/chromium/
cp ~/chromium/.cipd/.gclient_entries ~/chromium/
git clone https://github.com/kiwibrowser/src.git
```

ในขั้นตอนนี้ ใน ~/chromium/ คุณจะมีโฟลเดอร์ .cipd และโฟลเดอร์ src ซึ่งเป็นซอร์สโค้ดของ Kiwi Browser

### การติดตั้ง Dependencies

เพื่อให้สามารถสร้าง Kiwi Browser ได้ คุณต้องมี python และ OpenJDK (OpenJDK เพื่อสร้าง Java bindings สำหรับ Android):

```bash
sudo apt-get update
sudo apt-get install python openjdk-8-jdk-headless libncurses5
```

เราต้องการใช้ Java 1.8 เพื่อหลีกเลี่ยงข้อผิดพลาดคอมไพล์ (เช่น lint และ errorprone):

```bash
sudo update-java-alternatives --set java-1.8.0-openjdk-amd64
```

จากนั้นรันคำสั่งต่อไปนี้ใน ~/chromium/src:

```bash
bash install-build-deps.sh --no-chromeos-fonts
build/linux/sysroot_scripts/install-sysroot.py --arch=i386
build/linux/sysroot_scripts/install-sysroot.py --arch=amd64
```

คำสั่งเหล่านี้จะติดตั้งแพ็กเกจระบบที่จำเป็นทั้งหมดผ่าน apt-get และรวบรวมระบบไฟล์สำหรับการสร้างขั้นต่ำ

### การเตรียมคีย์สำหรับลงนาม

ไฟล์ APK บน Android ต้องถูกลงนามโดยนักพัฒนาเพื่อที่จะเผยแพร่

เพื่อสร้างคีย์:

```bash
keytool -genkey -v -keystore ~/chromium/keystore.jks -alias production -keyalg RSA -keysize 2048 -validity 10000 -storepass HERE_YOUR_ANDROID_KEYSTORE_PASSWORD -keypass HERE_YOUR_ANDROID_KEYSTORE[...]
```

### การกำหนดประเภทการสร้างและแพลตฟอร์ม

รัน:

```bash
mkdir -p ~/chromium/src/out/android_arm
```

สร้างไฟล์ชื่อ args.gn ใน ~/chromium/src/out/android_arm/ โดยมีเนื้อหาแบบนี้:

```bash
target_os = "android"
target_cpu = "arm" # <---- can be arm, arm64, x86 or x64
is_debug = false
is_java_debug = false

android_channel = "stable"
is_official_build = true
is_component_build = false
is_chrome_branded = false
is_clang = true
symbol_level = 1
use_unofficial_version_number = false
android_default_version_code = "158"
android_keystore_name = "production"
android_keystore_password = "HERE_YOUR_ANDROID_KEYSTORE_PASSWORD"
android_keystore_path = "../../../keystore.jks"
android_default_version_name = "Quadea"
fieldtrial_testing_like_official_build = true
icu_use_data_file = false
enable_iterator_debugging = false

google_api_key = "KIWIBROWSER"
google_default_client_id = "42.apps.kiwibrowser.com"
google_default_client_secret = "KIWIBROWSER_NOT_SO_SECRET"
use_official_google_api_keys = true

ffmpeg_branding = "Chrome"
proprietary_codecs = true
enable_hevc_demuxing = true
enable_nacl = false
enable_wifi_display = false
enable_widevine = false
enable_google_now = true
enable_ac3_eac3_audio_demuxing = true
enable_iterator_debugging = false
enable_mse_mpeg2ts_stream_parser = true
enable_remoting = false
rtc_use_h264 = false
rtc_use_lto = false
use_openh264 = false

v8_use_external_startup_data = true
update_android_aar_prebuilts = true

use_thin_lto = true

enable_extensions = true
enable_plugins = true
```

คุณสามารถแทนรหัสผ่าน Android keystore และเส้นทาง keystore ด้วยข้อมูลของ keystore ของคุณเอง (หรือสร้างคีย์ใหม่ได้)

### การเตรียมการสำหรับการสร้างครั้งแรก

เพื่อเตรียมการตั้งค่าเริ่มต้น ให้รันจาก ~/chromium/src:

```
gclient runhooks
```

จากนั้นสร้างไฟล์บิลด์ใน ~/chromium/src:

```
gn gen out/android_arm
```

ทางเลือกคือใช้: gn args out/android_arm

### การคอมไพล์ Kiwi Browser

เพื่อคอมไพล์ ให้ใช้คำสั่ง:

```
ninja -C out/android_arm chrome_public_apk
```

คุณจะได้ไฟล์ APK ผลลัพธ์ที่ ~/chromium/src/out/android_arm/apks/ChromePublic.apk

จากนั้นคุณสามารถรัน APK บนอุปกรณ์ของคุณได้

### การสืบสวนการล่มของแอป

คุณต้องมีสัญลักษณ์ของเวอร์ชันที่ล่ม สัญลักษณ์สามารถสร้างได้โดยใช้:

```
components/crash/content/tools/generate_breakpad_symbols.py --build-dir=out/lnx64 --symbols-dir=/tmp/my_symbols/ --binary=out/android_arm/lib.unstripped/libchrome.so --clear --verbose
```

ถ้าคุณมีข้อมูลการล่มจาก logcat:

```
out/lnx64/microdump_stackwalk -s /tmp/dump.dmp /tmp/my_symbols/
```

ถ้าคุณมีข้อมูลการล่มในไฟล์ tombstone:

```
./third_party/android_ndk/ndk-stack -sym out/android_x86/lib.unstripped -dump tombstone
```

### ดีบักจากระยะไกล

คุณสามารถใช้ Google Chrome เพื่อดีบักโดยใช้ devtools console

ในกรณีที่ devtools console ใช้งานไม่ได้ (เกิดข้อผิดพลาด 404) วิธีแก้คือใช้ chrome://inspect (Inspect fallback)
หรือเปลี่ยนค่า SHA1 ใน build/util/LASTCHANGE

```
LASTCHANGE=8920e690dd011895672947112477d10d5c8afb09-refs/branch-heads/3497@{#948}
```

และยืนยันการเปลี่ยนแปลงด้วย:

```
rm out/android_arm/gen/components/version_info/version_info_values.h out/android_x86/gen/components/version_info/version_info_values.h out/android_arm/gen/build/util/webkit_version.h out/android_[...]
```

### การปรับขนาดไบนารีให้เล็กลง

ถ้าต้องการปรับขนาด APK สุดท้าย คุณสามารถดูขนาดของแต่ละคอมโพเนนต์ด้วยคำสั่ง:

```
./tools/binary_size/supersize archive chrome.size --apk-file out/android_arm/apks/ChromePublic.apk -v
./tools/binary_size/supersize html_report chrome.size --report-dir size-report -v
```

## ไบนารีที่คอมไพล์ไว้ล่วงหน้า

<a href="https://play.google.com/store/apps/details?id=com.kiwibrowser.browser"> <img src="https://camo.githubusercontent.com/59c5c810fc8363f8488c3a36fc78f89990d13e99/68747470733a2f2f706c61792e67[...]"></a>

## โมเดลธุรกิจ

เบราว์เซอร์ได้รับรายได้จากเครื่องมือค้นหาสำหรับแต่ละครั้งของการค้นหาที่ทำผ่าน Kiwi Browser

ขึ้นอยู่กับการ���ลือกเครื่องมือค้นหา คำขออาจผ่านเซิร์ฟเวอร์ของ Kiwibrowser / Kiwisearchservices
ซึ่งใช้สำหรับการออกใบแจ้งหนี้ให้พันธมิตรการค้นหาและให้ผลลัพธ์การค้นหาทางเลือก (เช่น bangs หรือ "shortcuts")

ในบางประเทศ เบราว์เซอร์จะแสดงไทล์หรือข่าวที่มีสปอนเซอร์บนหน้าหลัก

ข้อมูลผู้ใช้ (การท่องเว็บ การนำทาง รหัสผ่าน บัญชี) จะไม่ถูกเก็บรวบรวมเพราะเราไม่มีความสนใจที่จะ���ู้ว่าคุณทำอะไรในเบราว์เซอร์ เป้าหมายหลักของเราคือชักจูงให้คุณใช้เครื่องมือค้นหาที่เป็นพันธมิตรของเราเท่านั้น

## โร้ดแมป

* ในปี 2020 เป้าหมายของโปรเจกต์คือการทำการแก้ไขการบำรุงรักษาและปรับปรุงด้านความปลอดภัย

ถ้ามีปัญหาหรือบั๊กที่คุณต้องการให้รวมเข้าใน Kiwi โปรดเปิด issue โดยอ้างถึงบั๊กหรือ commit ที่เกี่ยวข้องของ Chromium ให้ชัดเจน เพราะมีการเปลี่ยนแปลงจำนวนมากใน repository นี้

* ในปี 2021 Kiwi Browser จะย้ายไปยังสาขาใหม่ที่เรียกว่า Kiwi Browser Next โดยมีระบบรีเบส Chromium ที่ค่อนข้างอัตโนมัติ

## ความช่วยเหลือเพิ่มเติม

คุณสามารถขอความช่วยเหลือเพิ่มเติมได้ที่ Discord ของเรา:

<a href="https://discord.gg/XyMppQq"> <img src="https://discordapp.com/assets/e4923594e694a21542a489471ecffa50.svg" height="50"></a>

ขอให้สนุกกับการใช้ Kiwi!

Arnaud.
