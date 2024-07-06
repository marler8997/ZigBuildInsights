# Test Cases

From pure platform environments
- Windows: Sandbox
- Linux: A pure minimal userspace
         custom minimal userspace
- macOS: is there something equivalent to Window Sandbox

* Build a native "CPU-specific" version
    on Windows/Mac/Linux
* Build a native "generic CPU" version
    on Windows/Mac/Linux
* CrossCompile to each target from every other target
* Use wine to run the windows version on linux/macos
* WSL to run the linux version on windows?
  WSLg?
* Can you run linux apps on macos?
* Can you run macos apps on linux? No?

Macos ARM and Macos x86



Window Sandbox: Zig
--------------------------------------------------------------------------------
Run these 3 commands from any directory, you'll end up with a "zig" folder and "VVVVV" folder with the built game:
```batch
@REM Download Zig
curl --create-dirs -o zig/zig.zip https://ziglang.org/download/0.13.0/zig-windows-x86_64-0.13.0.zip -J -L && tar -xf zig/zig.zip --strip-components=1 --directory=zig && del zig\zig.zip

@REM Download the VVVVVV (zig build) repository
curl --create-dirs -o VVVVVV/VVVVVV.zip https://github.com/allyourcodebase/VVVVVV/archive/9e57d70522593107a2cc80df646bec4f8b033731.tar.gz -J -L && tar -xf VVVVVV/VVVVVV.zip --strip-components=1 --directory=VVVVVV && del VVVVVV\VVVVVV.zip

cmd /C "cd VVVVVV && ..\zig\zig build run"
```

Windows Sandbox: Sans Zig
--------------------------------------------------------------------------------
* The VVVVVV repository uses git submodules, so first install git from https://git-scm.com/downloads

From any directory run these commands:

```batch
git clone --recurse-submodules https://github.com/TerryCavanagh/VVVVVV

@REM download prebuilt SDL development library
curl --create-dirs -o sdl/devel/sdl.zip https://github.com/libsdl-org/SDL/releases/download/release-2.30.5/SDL2-devel-2.30.5-VC.zip -J -L && tar -xf sdl/devel/sdl.zip --strip-components=1 --directory=sdl/devel && del sdl\devel\sdl.zip

@REM download prebuilt SDL runtime dll
curl --create-dirs -o sdl/runtime-x86/sdl.zip https://github.com/libsdl-org/SDL/releases/download/release-2.30.5/SDL2-2.30.5-win32-x86.zip -J -L && tar -xf sdl/runtime-x86/sdl.zip --directory=sdl/runtime-x86 && del sdl\runtime-x86\sdl.zip
```

* Install Visual Studio Build tools
    - Download and run: https://aka.ms/vs/17/release/vs_BuildTools.exe
    - When asked, make sure to select the "Desktop development with C++" in the Workloads tab.
* Open a cmd.exe command prompt, then run this to make it a "Visual Studio Developer Command Prompt":

```
"c:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\Common7\Tools\VsDevCmd.bat"
```

Now run these commands inside the visual sutdio command prompt:
```
cmake -S VVVVVV\desktop_version -B VVVVVV\out -A Win32 -G "Visual Studio 17 2022" -DSDL2_INCLUDE_DIRS="%cd%\sdl\devel\include" -DSDL2_LIBRARIES="%cd%\sdl\devel\lib\x86\SDL2.lib;%cd%\sdl\devel\lib\x86\SDL2main.lib"
msbuild VVVVVV\out\VVVVVV.vcxproj
copy sdl\runtime-x86\SDL2.dll VVVVVV\out\Debug
curl -o VVVVVV/out/Debug/data.zip https://thelettervsixtim.es/makeandplay/data.zip -J -L
.\VVVVVV\out\Debug\VVVVVV
```
