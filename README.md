<p align="center">
    <a href="https://github.com/paulocoutinhox/pdfium-lib" target="_blank" rel="noopener noreferrer">
        <img width="120" src="extras/images/logo.png" alt="PDFium Library Logo">
    </a>
</p>

<h1 align="center">PDFium Library</h1>

<p align="center">
  <a href="https://github.com/paulocoutinhox/pdfium-lib/actions/workflows/ios.yml"><img src="https://github.com/paulocoutinhox/pdfium-lib/actions/workflows/ios.yml/badge.svg" alt="PDFium - iOS"></a>
  <a href="https://github.com/paulocoutinhox/pdfium-lib/actions/workflows/macos.yml"><img src="https://github.com/paulocoutinhox/pdfium-lib/actions/workflows/macos.yml/badge.svg" alt="PDFium - macOS"></a>
  <a href="https://github.com/paulocoutinhox/pdfium-lib/actions/workflows/android.yml"><img src="https://github.com/paulocoutinhox/pdfium-lib/actions/workflows/android.yml/badge.svg" alt="PDFium - Android"></a>
  <a href="https://github.com/paulocoutinhox/pdfium-lib/actions/workflows/wasm.yml"><img src="https://github.com/paulocoutinhox/pdfium-lib/actions/workflows/wasm.yml/badge.svg" alt="PDFium - WASM"></a>
</p>

<p align="center">
Project to compile PDFium library to multiple platforms.
</p>

## How to compile for Checker Software use cases

We are only going to use the wasm binaries for our purposes. You can either utilize a Ubuntu **20.04/22.04/24.04** instance or run a docker container instance. _Note that the minor ver .04 is important here as compiling it on a different minor .10 will not work._ Alternatively use the docker container to setup the compiling environment beforehand.

NOTE: STEPS 1-3 IS ONLY REQUIRED TO BE RAN ONCE. OTHER STEPS MUST BE RAN EVERY TIME FOR A BUILD.

1. Get the source, make sure that it is one commit ahead of upstream with `addFunction`, `removeFunction`, and `-sALLOW_TABLE_GROWTH` part of the wasm compilation exports.

```
git clone https://github.com/checkersoftware/pdfium-lib.git
cd pdfium-lib
```

<details>
<summary>Docker setup for if you are not using a standalone Ubuntu distro.</summary>
<br>

---

1. Build the image with command (this is for macbooks with M series processors):

```
docker build --platform linux/amd64 -t pdfium-wasm -f docker/wasm/Dockerfile docker/wasm
```

2. Now you can open an interactive shell for the container with:

```
docker run --platform linux/amd64 -v ${PWD}:/app -it pdfium-wasm
```

Run the rest of the commands in this interactive shell.

---

<br>

</details>
<br>

2. Install PIP requirements:

```
python3 -m pip install -r requirements.txt
```

3. Get Google Depot Tools:

```
python3 make.py build-depot-tools
export PATH=$PATH:$PWD/build/depot-tools
```

From here forth only these steps need to be ran for compilation.

4. Get Emscripten SDK:
   `python3 make.py build-emsdk`

5. Execute EMSDK environment file "emsdk_env" according to your system. For us this should be `/emsdk/emsdk_env.sh`

6. Get PDFium:
   `python3 make.py build-pdfium-wasm`

7. Patch:
   `python3 make.py patch-wasm`

8. PDFium Linux dependencies
   `./build/wasm32/pdfium/build/install-build-deps.sh`

9. Compile:
   `python3 make.py build-wasm`

10. Install libraries:
    `python3 make.py install-wasm`

11. Test:
    `python3 make.py test-wasm`

12. Generate javascript libraries:
    `python3 make.py generate-wasm`

At this point you should be able to find `pdfium.js` and `pdfium.wasm` inside `./build/wasm32/wasm/release/node`, those replace the current `pdfium.js` and `pdfium.wasm` files in our projects.

<br>
<br>

<br>

## Platforms

This project currently compiles to these platforms:

- [x] iOS device (arm64)
- [x] iOS simulator (x86_64, arm64)
- [x] Android (armv7, armv8, x86, x86_64)
- [x] macOS (x86_64, arm64)
- [x] WASM (Web Assembly)

Platforms in roadmap:

- Linux
- Windows

Obs: PDFium project is from Google and i only patch it to compile to all platforms above. Check all oficial details and PDFium license here:

https://pdfium.googlesource.com/

## Web demo

Since this project generate WASM version, i published a demo that you can test PDFium direct on web browser here:

https://pdfviewer.github.io

Or with a public PDF as parameter:

https://pdfviewer.github.io/?title=Demo%20PDF%20with%201MB&url=https://raw.githubusercontent.com/mozilla/pdf.js-sample-files/master/tracemonkey.pdf

## Requirements

1. Ninja Build
2. Python 3
3. PIP

Obs: Generally Python 3 already come with PIP installed. Check it with command `python3 -m pip --version`.

## Prebuilt binary

Access releases page to download prebuilt binaries:

https://github.com/paulocoutinhox/pdfium-lib/releases

## How to include files and extend pdfium

Check tutorial here: [How to include files](docs/HOW_TO_INCLUDE_FILES.md)

## Buy me a coffee

Support the continuous development of this project.

<a href='https://ko-fi.com/paulocoutinho' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://az743702.vo.msecnd.net/cdn/kofi1.png?v=2' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>

## My other projects

- XPLPC - Cross Platform Lite Procedure Call: [https://github.com/xplpc/xplpc](https://github.com/xplpc/xplpc)
- Nativium - C++ Multiplatform Modular Toolkit Template: [https://github.com/nativium/nativium](https://github.com/nativium/nativium)

## License

This license informations is about this personal project, not the Google PDFium Library.

[MIT](http://opensource.org/licenses/MIT)

Copyright (c) 2018-2024, Paulo Coutinho
