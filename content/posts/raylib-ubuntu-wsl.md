---
title: "Raylib on ubuntu"
date: 2023-12-10T08:31:45+02:00
draft: false
---

Build from source and get pkg-config working.

```bash
git clone https://github.com/raysan5/raylib.git
cd raylib
cmake -B build
cd build
make
sudo make install
```

```bash
cd ../src
sudo make uninstall
make PLATFORM=PLATFORM_DESKTOP RAYLIB_LIBTYPE=SHARED
sudo make install RAYLIB_LIBTYPE=SHARED
```
