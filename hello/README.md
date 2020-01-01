# Hello Linux Kernel Module

## Makefile

    [11:40:09]yuanyu@kplayer:~/___/hi-ldd/hello(master)$ cat Makefile
    obj-m += hello.o

    all:
            make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

    clean:
            make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean

## Code

    [11:40:28]yuanyu@kplayer:~/___/hi-ldd/hello(master)$ cat hello.c
    #include <linux/init.h>
    #include <linux/module.h>
    #include <linux/kernel.h>

    MODULE_LICENSE("GPL");
    MODULE_AUTHOR("YY");
    MODULE_DESCRIPTION("Hello Linux module.");
    MODULE_VERSION("0.01");

    static int __init hello_init(void) {
        printk(KERN_INFO "Good morning.\n");
        return 0;
    }

    static void __exit hello_exit(void) {
        printk(KERN_INFO "Good afternoon.\n");
    }

    module_init(hello_init);
    module_exit(hello_exit);

## Build the module

    [11:40:33]yuanyu@kplayer:~/___/hi-ldd/hello(master)$ make
    make -C /lib/modules/5.0.0-1021-gcp/build M=/home/yuanyu/___/hi-ldd/hello modules
    make[1]: Entering directory '/usr/src/linux-headers-5.0.0-1021-gcp'
      CC [M]  /home/yuanyu/___/hi-ldd/hello/hello.o
      Building modules, stage 2.
      MODPOST 1 modules
      CC      /home/yuanyu/___/hi-ldd/hello/hello.mod.o
      LD [M]  /home/yuanyu/___/hi-ldd/hello/hello.ko
    make[1]: Leaving directory '/usr/src/linux-headers-5.0.0-1021-gcp'

## Use the module

    [11:40:36]yuanyu@kplayer:~/___/hi-ldd/hello(master)$ sudo insmod hello.ko
    [11:40:51]yuanyu@kplayer:~/___/hi-ldd/hello(master)$ sudo rmmod hello

## dmesg

    ...
    [Wed Jan  1 11:40:50 2020] Good morning.
    [Wed Jan  1 11:40:57 2020] Good afternoon.

## Clean Up

    [11:40:58]yuanyu@kplayer:~/___/hi-ldd/hello(master)$ make clean
    make -C /lib/modules/5.0.0-1021-gcp/build M=/home/yuanyu/___/hi-ldd/hello clean
    make[1]: Entering directory '/usr/src/linux-headers-5.0.0-1021-gcp'
      CLEAN   /home/yuanyu/___/hi-ldd/hello/.tmp_versions
      CLEAN   /home/yuanyu/___/hi-ldd/hello/Module.symvers
    make[1]: Leaving directory '/usr/src/linux-headers-5.0.0-1021-gcp'
