# CSSE4011: Tute 2 - First Program

## 1.0 Motivation

The following tutorial will focus on developing your own very first Zephyr application. This invloves, setting up an appropriate directory structure and necessary configuration files.

## 1.1 Hardware Required

>Segger JLink EDU Mini

>Particle Argon

>2xMicro USB Cables

**Connection guide:**
1. Connect the Particle Argon using the micro-usb cable to your computer
2. Connect JLink ribbon cable to the JTAG header on the Particle Argon, and the opposing end to the J-Link EDU-mini. The lip of the header should be facing away from the edu mini.
3. Connect the JLink edu mini to your computer, and make sure that USB passthrough for this device is enabled in your vitual machine settings. 

## 2.0 Directory Overview

A typical Zephyr application is organised in the following format [1].

```
<home>/apps
├── CMakeLists.txt
├── prj.conf
└── src
    └── main.c
```

In this course, you will be required to modify and re-use code to implement new features/applications. To allow for this, code that is not specific to an application is placed in a 'myoslib' directory. Where code within 'myoslib' could be drivers for a particular module, for example, an ultrasonic sensor.  The structure below is an example representation.

```
.
├── csse4011_repo/apps/
│   ├── prac1/
│   │   ├── CMakeLists.txt
│   │   ├── prj.conf
│   │   └── src/
│   │       └── main.c 
│   └── prac2/
│       ├── CMakeLists.txt
│       ├── prj.conf
│       └── src/
│           └── main.c
|
└── csse4011_repo/myoslib/
    ├── hal/
    │   ├── status_leds/
    │   │   ├── hal_status_leds.c 
    │   │   └── hal_status_leds.h 
    │   └── ultrasonic/
    │       ├── hal_ultrasonic.c
    │       └── hal_ultrasonic.h
    └── cli/
        ├── cli_kernel_time_cmd.c
        └── cli_status_leds_cmd.c
```

## **3.0 Making A Zephyr Application**

3.1 Within the 'csse4011_repo', make the following modifications to setup a sample directory
```
    cd ~/csse4011/csse4011_repo/
    mkdir -p apps/sample
```

3.2 For the purposes of this tutorial, we will use the existing blinky sample provided in the Zephyr source code.

Start by navigating to the Zephyr source we attained in Tute 1.

```
    cd ~/csse4011/zephyrproject/zephyr/samples/basic/blinky
```
3.3 Copy blinky files into our sample application directory, then navigate to sample directory.
```
    cp -R * ~/csse4011/csse4011_repo/apps/sample/

    cd ~/csse4011/csse4011_repo/apps/sample/
```
3.4 Verify that the following exist within your sample dir
```
.
└── csse4011/csse4011_repo/apps/
    └── sample/
        ├── CMakeLists.txt
        ├── prj.conf
        ├── README.rst
        ├── sample.yaml
        └── src/
            └── main.c
```
3.5 Add ZEPHYR_BASE to path (Lets you build from outside the Zephyr Source Dir). Append the following line to the end of 'bashrc'.
```
    vim ~/.bashrc                               or use any text editor
    
    #Add Zephyr Base to PATH
    export ZEPHYR_BASE=~/csse4011/zephyrproject/zephyr   
```
save and exit.

3.6 Read and execute bashrc (Loads ZEPHYR_BASE for current shell instance)
```
    source ~/.bashrc

    echo $ZEPHYR_BASE
```
Should now display the path to zephyr base. 

3.7 Build the sample application within our directory
```
    cd ~/csse4011/csse4011_repo/apps/sample/

    west build -p auto -b particle_argon
```

3.8 Flash and Verify that Blinky works. 
```
    west flash -r jlink
```

The option '-r' lets you specify a runner, in this case we are using JLink to flash the Particle Argon. Make sure that the cables are connected properly.

If flashing fails, refer to the end of tute 1 and follow instructions for installing runners / additional runners. 


## Links

[1]https://docs.zephyrproject.org/latest/application/index.html