# xigen

xigen is a generator for libximc library. It generates the library, bindings, tests and so on using `protocol.xi` file.

## Building

### For Linux

**Preparing the system**

0. Install build essentials, containing C++ compiler

```shell
 sudo apt install build-essential
```

1. Install cmake

```shell
sudo apt install cmake
```

2. Install bison

```shell
sudo apt install bison
```

3. Install flex

```shell
sudo apt install flex
```

**Building**

Then from the top folder of the release prepare cmake environment by

```shell
cmake .
```

And finally build:

```shell
cmake --build .
```

xigen executable will be generated in `src` directory.
