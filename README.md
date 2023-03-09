# pico-starter-project
Minimal Raspberry Pi Pico project using C-SDK

## Getting started using VSCode and Devcontainer

### Requirements
1. [Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview)
1. VSCode extension [`ms-vscode-remote.remote-containers`](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
1. [Docker](https://docs.docker.com/get-docker/)

### Building the Pico examples

1. Clone this repository
```
$ git clone --depth=1 --recursive https://github.com/feabhas/pico-starter-project.git
$ cd pico-starter-project
```
* `--depth=1` is a shallow clone, only pulling down the latest commit (smaller, faster clone)
* `--recursive` ensures any nested submodules are also cloned

Open the project in VSCode
```
$ code .
```

VScode will detect the presence of the `.devcontainer` folder and prompt you to select this option if you want to **Reopen in Container**.

    The first time you run this, the Docker image must be pulled from DockerHub and the running container invoked. This will likely take a few minutes as packages are being pulled from the web. This is a one-off build; subsequently, running in a container will only take a couple of extra seconds.

Once the container is running, open a `New Terminal` in VSCode.

This starts a `bash` based terminal, e.g.
```
vscode ➜ /workspaces/pico-starter-project (master) $ 
```

### Building the example - Using VSCode CMake

VSCode will prompt you to select a kit for building with CMake:

* Select the `arm-none-eabi` option

Then on the bottom of the VSCode window, click on the `Build` option. This invokes CMake and builds the target executables.


### Building the example - CLI

Create the build files using CMake:
```
$ cmake -S . -B build -DCMAKE_BUILD_TYPE=Debug
```
This will finish with:
```
PICO_SDK_PATH is /workspaces/pico-sdk
PICO platform is rp2040.
Build type is Debug
Using regular optimized debug build (set PICO_DEOPTIMIZED_DEBUG=1 to de-optimize)
...
...
-- Configuring done
-- Generating done
-- Build files have been written to: /workspaces/pico-examples/build
```

the `-DCMAKE_BUILD_TYPE=Debug` can be ommited if you just want a release build.

Now compile all the examples:
```
$ cmake --build build/
```

### Target Build files

Once the build is complete, you will find `.uf2` files for each example under the `build` directory, e.g. :
```
$ ls build/
CMakeCache.txt  bs2_default.elf.map  cmake_install.cmake    elf2uf2    pico-sdk  test.bin  test.elf      test.hex
CMakeFiles      build.ninja          compile_commands.json  generated  pioasm    test.dis  test.elf.map  test.uf2
```

The `.uf2` file can be copied to the Pico to execute the example.


## Basic Debugging with GDB/OpenOCD (Raspberry Pi Debug Probe)

First, we need to start an OpenOCD session on the host machine.

As we are connecting with a particular adapter with some specific target,
we need to source both the SWD interface and the target configs,
e.g.:

  openocd -f interface/cmsis-dap.cfg -c "adapter speed 5000" -f target/rp2040.cfg -s tcl


### Host debugging

After OpenOCD startup, connect GDB with
   
  $ arm-none-eabi-gdb :3333 build/test.elf

  (gdb) target extended-remote :3333
  (gdb) mon reset halt
  (gdb) load
  (gdb) mon reset halt
  (gdb) continue

### DevContainer debugging

The Cortex-Debug extension is included in the VScode extensions. Running this as a debug session will connect to the target.

However, the interaction with the target appears *flakey* at the moment [ To Be Investigated Further]


## Installing OpenOCD on Host machine

If you haven’t already, install vcpkg.

### Linux/macOS

`. <(curl https://aka.ms/vcpkg-init.sh -L)`

### PowerShell

`iex (iwr -useb https://aka.ms/vcpkg-init.ps1)`

### CMD Shell

`curl -LO https://aka.ms/vcpkg-init.cmd && .\vcpkg-init.cmd`

Then:
`vcpkg activate`

See [Embedded Software Development in Visual Studio Code](https://devblogs.microsoft.com/cppblog/vscode-embedded-development/) to install `OpenOCD` using [vcpkg]()
