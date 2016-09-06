We intercepted this image, but it must have gotten corrupted during the transmission. Can you try and fix it? corrupt.png

First, we will try some command to check this file.
$ file corrupt.png
corrupt.png: data

So, we know that this file is not a png file or it is corrupted. To fix it, let see its hexdump:
$ xxd corrupt.png > hexdump
$ less hexdump
00000000: 9050 4e47 0e1a 0a1b 0000 000d 4948 4452  .PNG........IHDR
00000010: 0000 01f4 0000 0198 0806 0000 00b4 e010  ................

Compare this with PNG file header ( or significant ) at http://forensicswiki.org/wiki/Portable_Network_Graphics_(PNG)
It must have this form
89 50 4E 47 0D 0A 1A 0A

So, just use vim to correct this:
$ vim hexdump
00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR
00000010: 0000 01f4 0000 0198 0806 0000 00b4 e010  ................
$ xxd -r hexdump > correct.png
$ file correct.png 
correct.png: PNG image data, 500 x 408, 8-bit/color RGBA, non-interlaced

Yep, finally, we have the flag: IceCTF{t1s_but_4_5cr4tch}
