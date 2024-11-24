# DOS 4.01 compiling kit

First, get started by extracting the hard drive with the code (from [here](https://github.com/neozeed/dos400)):

```
$ unxz -k code.raw.xz
```

You can create this yourself by formatting a raw disk using DOS, then copying the contents of the Git repo to `\SRC`.


Now, spawn a QEMU instance with the DOS 5 bootdisk and the hard drive:

```
$ qemu-system-i386 -fda Dos5.0.img -hda code.raw -boot a -accel kvm
```

Once booted, run:

```
A:\>C:
C:\>cd SRC
C:\SRC>setenv
C:\SRC>nmake
```

This will take a few minutes.

Once compilation is complete, run:

```
C:\SRC>md C:\BIN
C:\SRC>cpy C:\BIN 
```

Now close QEMU, and create a diskette:

```
$ qemu-img create -f raw output.img 1440K
```

Now boot up QEMU again with the DOS 4 diskette and our diskette:

```
$ qemu-system-i386 -fda Dos4.01.img -fdb output.img -hda code.raw -boot a -accel kvm
```

Now, in DOS 4 run:

```
A>format B:
<ENTER>
<ENTER>
N
A>sys C:\BIN B:
A>copy C:\BIN\COMMAND.COM B:
```

Now close QEMU, and you should have a DOS 4.01 diskette at `output.img` compiled from scratch!
