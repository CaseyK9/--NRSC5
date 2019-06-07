# NRSC5

Эта программа получает цифровые радиостанции NRSC-5 с помощью RTL-SDR. Он предлагает интерфейс командной строки, а также API, на котором могут быть построены другие приложения. Прежде чем использовать его, вам сначала нужно скомпилировать программу, используя инструкции ниже.

## Построение на Ubuntu, Debian или Raspbian

    $ sudo apt install git build-essential cmake autoconf libtool libao-dev libfftw3-dev librtlsdr-dev
    $ git clone https://github.com/theori-io/nrsc5.git
    $ cd nrsc5
    $ mkdir build
    $ cd build
    $ cmake [options] ..
    $ make
    $ sudo make install
    $ sudo ldconfig

Доступные параметры сборки:

    -DUSE_COLOR=ON       Colorize log output. [default=OFF]
    -DUSE_NEON=ON        Use NEON instructions. [ARM, default=OFF]
    -DUSE_SSE=ON         Use SSSE3 instructions. [x86, default=OFF]
    -DUSE_FAAD2=ON       AAC decoding with FAAD2. [default=ON]

Вы можете протестировать программу с помощью включенного образца capture:

     $ xz -d < ../support/sample.xz | src/nrsc5 -r - 0

## Здание в Fedora

Следуйте инструкциям Ubuntu выше, но замените первую команду следующей:

    $ sudo dnf install make patch cmake autoconf libtool libao-devel fftw-devel rtl-sdr-devel libusb-devel

## Построение на macOS с использованием [Доморощенный](https://brew.sh)

     $ brew install --HEAD https://raw.githubusercontent.com/theori-io/nrsc5/master/nrsc5.rb

## Здание для Windows

Чтобы создать программу для Windows, вы можете использовать [MSYS2](http://www.msys2.org) в Windows или используйте кросс-компилятор на машине Ubuntu, Debian или macOS. В обоих случаях предоставляются сценарии.

### Построение на Windows с помощью MSYS2

Установить [MSYS2](http://www.msys2.org). Откройте терминал с помощью ярлыка "MSYS2 MinGW 32-bit". (Или используйте 64-разрядный ярлык, если вы предпочитаете 64-разрядную сборку.)

    $ pacman -Syu

Если это первый запуск pacman, вам будет сказано закрыть окно терминала. После этого снова откройте, используя тот же ярлык, что и раньше.

    $ pacman -Su
    $ pacman -S git
    $ git clone https://github.com/theori-io/nrsc5.git
    $ nrsc5/support/msys2-build

Вы можете протестировать установку с помощью включенного файла примера:

    $ cd ~/nrsc5/support
    $ xz -d sample.xz
    $ nrsc5.exe -r sample 0

Если пример файла не работает, убедитесь, что вы выполнили все инструкции.

После того, как все построено, вы можете запустить nrsc5 независимо от MSYS2. Скопируйте следующие файлы из MSYS2/mingw32 directory (e.g. C:\\msys64\\mingw32\\bin):

* libnrsc5.dll
* nrsc5.exe

### Кросс-компиляция для Windows из Ubuntu / Debian

    $ sudo apt install mingw-w64
    $ support/win-cross-compile 32

Замените " 32 " на "64", Если вы хотите 64-разрядную сборку. После завершения сборки скопируйте .dll и nrsc5.exe из папки build-win32/bin (или "build-win64/bin) на ваш компьютер с Windows.

### Кросс-компиляция для Windows из macOS

    $ brew install mingw-w64
    $ support/win-cross-compile 32

Replace `32` with `64` if you want a 64-bit build. Once the build is complete, copy `*.dll` and `nrsc5.exe` from the `build-win32/bin` (or `build-win64/bin`) folder to your Windows machine.

## Usage

### Command-line options:

       frequency                       center frequency in MHz or Hz
                                         (do not provide frequency when reading from file)
       program                         audio program to decode
                                         (0, 1, 2, or 3)
       -g gain                         gain
                                         (example: 49.6)
                                         (automatic gain selection if not specified)
       -d device-index                 rtl-sdr device
       -p ppm-error                    rtl-sdr ppm error
       -r iq-input                     read IQ samples from input file
       -w iq-output                    write IQ samples to output file
       -o audio-output                 write audio to output WAV file
       -q                              disable log output
       -l log-level                    set log level
                                         (1 = DEBUG, 2 = INFO, 3 = WARN)
       -v                              print the version number and exit
       --dump-aas-files dir-name       dump AAS files
                                         (WARNING: insecure)
       --dump-hdc file-name            dump HDC packets

### Examples:

Tune to 107.1 MHz and play audio program 0:

     $ nrsc5 107.1 0

Tune to 107.1 MHz and play audio program 0. Manually set gain to 49.0 dB and save raw IQ samples to a file:

     $ nrsc5 -g 49.0 -w samples1071 107.1 0

Read raw IQ samples from a file and play back audio program 0:

     $ nrsc5 -r samples1071 0

Tune to 90.5 MHz and convert audio program 0 to WAV format for playback in an external media player:

     $ nrsc5 -o - 90.5 0 | mplayer -

### RTL-SDR drivers on Windows

If you get errors trying to access your RTL-SDR device, then you may need to use [Zadig](http://zadig.akeo.ie/) to change the USB driver. Once you download and run Zadig, select your RTL-SDR device, ensure the driver is set to WinUSB, and then click "Replace Driver". If your device is not listed, enable "Options" -> "List All Devices".
