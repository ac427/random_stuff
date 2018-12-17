I have this intel wifi card and the drivers are not supported by Intel only on kernels 4.6 + . With the 4.6 kernel, my lenvo yoga is doing real yoga

```
00:1f.6 Ethernet controller: Intel Corporation Ethernet Connection (4) I219-LM (rev 21)
04:00.0 Network controller: Intel Corporation Wireless 8265 / 8275 (rev 88)

```

#### here is what I did to fix wifi. ( before you start, make sure you have the updated gcc and kernel header package installed) 

```
wget https://mirrors.edge.kernel.org/pub/linux/kernel/projects/backports/stable/v4.20-rc5/backports-4.20-rc5-1.tar.gz
## untar it 
[root@andover backports-4.20-rc5-1]#  make defconfig-wifi
cc -Wall -Wmissing-prototypes -Wstrict-prototypes -O2 -fomit-frame-pointer   -c -o conf.o conf.c
cc -Wall -Wmissing-prototypes -Wstrict-prototypes -O2 -fomit-frame-pointer   -c -o zconf.tab.o zconf.tab.c
cc   conf.o zconf.tab.o   -o conf
#
# configuration written to .config
#
[root@andover backports-4.20-rc5-1]# make
make[5]: `conf' is up to date.
#
# configuration written to .config
#
Building backport-include/backport/autoconf.h ... done.
  CC [M]  /home/ananta.chakravartula/Downloads/backports-4.20-rc5-1/compat/main.o
  CC [M]  /home/ananta.chakravartula/Downloads/backports-4.20-rc5-1/compat/backport-4.2.o
/home/ananta.chakravartula/Downloads/backports-4.20-rc5-1/compat/backport-4.2.c:14:28: error: static declaration of ‘scatterwalk_ffwd’ follows non-static declaration
 static struct scatterlist *scatterwalk_ffwd(struct scatterlist dst[2],
                            ^
In file included from /home/ananta.chakravartula/Downloads/backports-4.20-rc5-1/compat/backport-4.2.c:11:0:
include/crypto/scatterwalk.h:105:21: note: previous declaration of ‘scatterwalk_ffwd’ was here
 struct scatterlist *scatterwalk_ffwd(struct scatterlist dst[2],
                     ^
make[6]: *** [/home/ananta.chakravartula/Downloads/backports-4.20-rc5-1/compat/backport-4.2.o] Error 1
make[5]: *** [/home/ananta.chakravartula/Downloads/backports-4.20-rc5-1/compat] Error 2
make[4]: *** [_module_/home/ananta.chakravartula/Downloads/backports-4.20-rc5-1] Error 2
make[3]: *** [modules] Error 2
make[2]: *** [modules] Error 2
make[1]: *** [modules] Error 2
make: *** [default] Error 2
[root@andover backports-4.20-rc5-1]# 

```

If you get above error edit below file and remote static from line 14 

```
[root@andover backports-4.20-rc5-1]# cat -n  compat/backport-4.2.c  | grep "static struct scatterlist"
    14	static struct scatterlist *scatterwalk_ffwd(struct scatterlist dst[2],

[root@andover backports-4.20-rc5-1]# sed -i 's/static struct scatterlist/struct scatterlist/' compat/backport-4.2.c 

[root@andover backports-4.20-rc5-1]# make
[root@andover backports-4.20-rc5-1]# make install

```
