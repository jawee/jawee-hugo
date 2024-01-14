---
title: "Dual-boot with Advantage 360 Pro"
date: 2024-01-14T08:31:45+02:00
draft: false
---

From [https://blog.christopherhoelter.com/kinesis-advantage-360-bluetooth-dualboot](https://blog.christopherhoelter.com/kinesis-advantage-360-bluetooth-dualboot)

1. Boot into windows and pair the keyboard on the target bluetooth profile.
2. Restart and boot into linux. On the keyboard, forget the pairing of the previous target profile (mod+right win). Now, pair the keyboard on linux.
3. Copy the paired bluetooth key from linux. This will be in a file at the path /var/lib/bluetooth/{controller-mac-address}/{keyboard-mac-address}/info. Look for a line like key={32 characters here} in the file, that's the key. It may there multiple times. Write it down or copy it somewhere accessible when you reboot into windows.
4. Reboot into windows and ensure the keyboard doesn't try and connect. Now, you need to edit the registry file for the bluetooth keyboard and replace the key with the key grabbed from the linux partition.
5. Download PSExec in order to open up the registry editor with the proper permissions. Once the tool is unzipped, in an admin command shell run .\PsExec64.exe -s -i regedit.exe.
6. Navigate to HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\BTHPORT\Parameters\Keys in the editor, and find the entry for the keyboard.
7. Replace the ltk key in that location with the one copied over from linux.
8. Reboot and the keyboard should now connect and be functional across both operating systems!
