---
title: "Mount SHR Raid 1 Drive Ubuntu"
date: 2020-08-31T16:03:56+02:00
draft: false
---

This assumes it's a raid array of 1 device

Following this guide:
https://wiredverse.com/2016/06/recoving-a-synology-shr-raid-1-array-with-only-one-disk/


```bash
sudo mdadm --examine /dev/sdg5
```
Check the offset and insert in command below
```bash
sudo losetup --find --show --read-only --offset $((2048*512)) /dev/sdg5
```

```bash
sudo pvdisplay
```

```bash
sudo vgdisplay
```

```bash
sudo lvdisplay
```

end with (check LV Path from lvdisplay)

```bash
mount /dev/vg1001/lv /mnt -o ro
```

Footnote: Should probably append a folder after /mnt
