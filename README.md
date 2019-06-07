# NRSC5

Эта программа получает цифровые радиостанции NRSC-5 с помощью RTL-SDR. Он предлагает интерфейс командной строки, а также API, на котором могут быть построены другие приложения. Прежде чем использовать его, вам сначала нужно скомпилировать программу, используя инструкции ниже.

Вы можете скачать предварительно скомпилированный двоичный файл windows здесь: https://github.com/RF-Fox/--NRSC5/releases

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

Замените " 32 " на "64", Если вы хотите 64-разрядную сборку. После завершения сборки скопируйте *.dll и nrsc5.exe из папки build-win32/bin (или "build-win64/bin) на ваш компьютер с Windows.

### Кросс-компиляция для Windows из macOS

    $ brew install mingw-w64
    $ support/win-cross-compile 32

Замените " 32 " на "64", Если вы хотите 64-разрядную сборку. После завершения сборки скопируйте *.dll и nrsc5.exe из папки build-win32/bin (или build-win64/bin) на ваш компьютер с Windows.

## Использование

### Опция командной строки:

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

### Образцы:

Настройка на 107.1 МГц и воспроизведение аудиопрограммы 0:

     $ nrsc5 107.1 0

Настройка на 107.1 МГц и воспроизведение аудиопрограммы 0. Вручную установите коэффициент усиления в 49,0 дБ и сохраните необработанные образцы IQ в файл:

     $ nrsc5 -g 49.0 -w samples1071 107.1 0

Чтение необработанных образцов IQ из файла и воспроизведение аудиопрограммы 0:

     $ nrsc5 -r samples1071 0

Настройтесь на 90,5 МГц и конвертируйте аудиопрограмму 0 в формат WAV:

     $ nrsc5 -o - 90.5 0 | mplayer -

### Драйверы RTL-SDR для Windows

Если вы получаете ошибки при попытке доступа к устройству RTL-SDR, вам может потребоваться использовать [Zadig] (http://zadig.akeo.ie/)чтобы изменить драйвер USB. После того как вы загрузите и запустите Задиг, выберите устройство RTL-SDR, убедитесь, что драйвер настроен на WinUSB, а затем нажмите "Replace Driver". Если ваше устройство не указано, включите "Options" - > "List All Devices".
