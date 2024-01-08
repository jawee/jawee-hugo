---
title: "Raylib on ubuntu"
date: 2023-12-10T08:31:45+02:00
draft: false
---

## Build from source and get pkg-config working.

```bash
git clone https://github.com/raysan5/raylib.git
cd raylib
cmake -B build
cd build
make
sudo make install
```

Then

```bash
cd ../src
sudo make uninstall
make PLATFORM=PLATFORM_DESKTOP RAYLIB_LIBTYPE=SHARED
sudo make install RAYLIB_LIBTYPE=SHARED
```



Could possibly do something like this instead to make it easier. Haven't tried.
```bash
git clone https://github.com/raysan5/raylib.git raylib
cd raylib
mkdir build && cd build
cmake -DBUILD_SHARED_LIBS=ON ..
make
sudo make install
```
from [https://github-wiki-see.page/m/raysan5/raylib/wiki/Working-on-GNU-Linux](https://github-wiki-see.page/m/raysan5/raylib/wiki/Working-on-GNU-Linux)
