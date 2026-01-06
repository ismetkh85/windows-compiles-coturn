# Windows compiles coturn

This repository demonstrates **how to build coturn on Windows** using:

- **Visual Studio 2022**
- **CMake**
- **vcpkg**
- **GitHub Actions** (automatic Windows builds)

The goal is to provide a **clear, reproducible and automated** way to build `coturn` on Windows and obtain a ready-to-use `turnserver.exe`.

---

## Why this repository exists

Coturn is primarily developed for Linux, and building it on Windows can be confusing.

This project shows:

- how to build coturn **locally on Windows**
- how to manage dependencies using **vcpkg**
- how to configure **CMake + Visual Studio 2022**
- how to set up **GitHub Actions** to automatically build coturn on Windows

GitHub Actions allow anyone to:
- verify that coturn builds successfully on Windows
- download a prebuilt Windows binary (`turnserver.exe`)
- avoid manual setup and configuration

---

## What are GitHub Actions?

**GitHub Actions** are automated workflows that run on GitHub servers.

In this project, GitHub Actions:
1. Start a fresh Windows environment
2. Install `vcpkg`
3. Install required libraries (OpenSSL, libevent)
4. Run CMake with Visual Studio 2022
5. Build coturn
6. Upload the resulting `turnserver.exe` as a downloadable artifact

In simple terms:
> GitHub Actions act as a clean Windows machine that builds coturn for you automatically.

---

## Local build on Windows (step by step)

### Requirements

- Windows 10 / 11 (64-bit)
- Visual Studio 2022 (with **Desktop development with C++**)
- CMake
- Git
- Internet connection

---

## Step 1. Install vcpkg

Clone the vcpkg repository:

```bat
git clone https://github.com/microsoft/vcpkg.git vcpkg
```

---

## Step 2. Integrate vcpkg with Visual Studio

```bat
.\vcpkg\integrate install
```

This allows CMake and Visual Studio to automatically find libraries installed via vcpkg.

---

## Step 3. Install coturn dependencies

Coturn requires OpenSSL and libevent.

```bat
.\vcpkg\vcpkg install openssl:x64-windows libevent:x64-windows
```

---

## Step 4. Configure the project using CMake

Create a build directory:

```bat
mkdir build
cd build
```

Run CMake with Visual Studio 2022 and vcpkg toolchain:

```bat
"C:\Program Files\CMake\bin\cmake.exe" ^
-G "Visual Studio 17 2022" ^
-A x64 ^
-DCMAKE_TOOLCHAIN_FILE="C:\path\to\vcpkg\scripts\buildsystems\vcpkg.cmake" ^
-DVCPKG_TARGET_TRIPLET=x64-windows ^
..
```

> Replace `C:\path\to\vcpkg` with the actual path to your vcpkg directory.

---

## Step 5. Build coturn using Visual Studio

1. Open the generated `.sln` file from the `build` directory
2. Select **Release** configuration
3. Click **Build → Rebuild Solution**

After a successful build, you will get:

```
turnserver.exe
```

---

## Automatic build using GitHub Actions

This repository includes a GitHub Actions workflow that automatically builds coturn on Windows.

### How it works

- The workflow runs on `windows-latest`
- Uses Visual Studio 2022
- Installs dependencies via vcpkg
- Builds coturn using CMake and MSBuild
- Uploads `turnserver.exe` as an artifact

### How to use it

1. Go to the **Actions** tab in this repository
2. Select **Windows compiles coturn**
3. Wait for the workflow to finish
4. Download the `turnserver.exe` artifact

---

## References

- Coturn repository: https://github.com/coturn/coturn
- vcpkg: https://github.com/microsoft/vcpkg
- CMake: https://cmake.org/
- Related article:
  https://xue-fei.blog.csdn.net/article/details/155945401

---

## License

This repository follows the same license as the original coturn project.
