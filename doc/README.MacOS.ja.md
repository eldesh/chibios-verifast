# ChibiOS/RTのVeriFastによる検証 (MacOS版)

[ChibiOS/RT](http://www.chibios.org/)上で動作するアプリケーションを[VeriFast](https://people.cs.kuleuven.be/~bart.jacobs/verifast/)で検証します。

## 準備

関連パッケージをインストールしてください。

```
$ brew tap PX4/homebrew-px4
$ brew update
$ brew install wget git gcc-arm-none-eabi cmake picocom
```

[stlink](https://github.com/texane/stlink)をダウンロードしてビルドしてください。

```
$ git clone https://github.com/texane/stlink.git
$ (cd stlink && make)
$ (cd stlink/build/Release && sudo make install)
```

[VeriFastの最新版をダウンロード](https://github.com/verifast/verifast#binaries)し、展開してPATHを通してください。

```
$ wget http://82076e0e62875f063ae8-929808a701855dfb71539d0a4342d4be.r54.cf5.rackcdn.com/verifast-nightly-osx.tar.gz
$ tar xf verifast-nightly-osx.tar.gz
$ mv verifast-*/ verifast
$ export PATH=`pwd`/verifast/bin:$PATH
```

本ソースコードをダウンロードしてください。

```
$ git clone https://github.com/fpiot/chibios-verifast.git
```

## 検証

VeriFastでソースコードを検証するには、以下のようにしてVeriFast IDEを起動してください。

```
$ cd chibios-verifast/verifast_demo/STM32/RT-STM32F091RC-NUCLEO
$ make vfide
```

## ビルド

ソースコードをビルドしてください。

```
$ cd chibios-verifast/verifast_demo/STM32/RT-STM32F091RC-NUCLEO
$ make
```

## 実機動作

ボードとMacをUSBケーブルで接続した後、st-utilを起動して待機中にしてください。

```
$ st-util
```

別のコンソールを開いて、gdbserver経由でファームウェアを書き込みます。

```
$ cd chibios-verifast/verifast_demo/STM32/RT-STM32F091RC-NUCLEO
$ make gdbwrite
```

gdbのプロンプトが出るので、実行継続してください。

```
(gdb) c
```

さらに別のコンソールを開いて、シリアルコンソールを開いた後、ボードの`USER`スイッチを押下してください。以下のようなログが表示されます。

```
$ picocom -b 38400 /dev/tty.usbmodem1423
picocom v1.7
--snip--
Terminal ready

*** ChibiOS/RT test suite
***
*** Kernel:       3.1.5
*** Compiled:     Jan 15 2017 - 20:38:01
*** Compiler:     GCC 4.8.4 20140725 (release) [ARM/embedded-4_8-branch revision 213147]
*** Architecture: ARMv6-M
*** Core Variant: Cortex-M0
*** Port Info:    Preemption through NMI
*** Platform:     STM32F091xC Entry Level Access Line devices
*** Test Board:   STMicroelectronics NUCLEO-F091RC
--snip--
```
