TOOLPATH = ../z_tools/
INCPATH = ../z_tools/haribote/

MAKE = $(TOOLPATH)make -r
NASK = $(TOOLPATH)nask
CC1 = $(TOOLPATH)gocc1 -I$(INCPATH) -Os -Wall -quiet
GAS2NASK = $(TOOLPATH)gas2nask -a
OBJ2BIM = $(TOOLPATH)obj2bim
BIM2HRB = $(TOOLPATH)bim2hrb
MAKEFONT = $(TOOLPATH)makefont
BIN2OBJ = $(TOOLPATH)bin2obj
RULEFILE = $(TOOLPATH)haribote/haribote.rul
EDIMG = $(TOOLPATH)edimg
HARITOL = $(TOOLPATH)haritol
FDIMG0AT = $(TOOLPATH)fdimg0at.tek
FDIMAGE0 = $(TOOLPATH)qemu/fdimage0.bin

# デフォルト動作

default :
	$(MAKE) img

# ファイル生成規則

ipl10.bin : ipl10.nas Makefile
	$(NASK) ipl10.nas ipl10.bin ipl.lst

asmhead.bin : asmhead.nas Makefile
	$(NASK) asmhead.nas asmhead.bin asmhead.lst

bootpack.gas : bootpack.c Makefile
	$(CC1) -o bootpack.gas bootpack.c

bootpack.nas : bootpack.gas Makefile
	$(GAS2NASK) bootpack.gas bootpack.nas

bootpack.obj : bootpack.nas Makefile
	$(NASK) bootpack.nas bootpack.obj bootpack.lst

naskfunc.obj : naskfunc.nas Makefile
	$(NASK) naskfunc.nas naskfunc.obj naskfunc.lst

hankaku.bin : hankaku.txt Makefile
	$(MAKEFONT) hankaku.txt hankaku.bin

hankaku.obj : hankaku.bin Makefile
	$(BIN2OBJ) hankaku.bin hankaku.obj _hankaku

bootpack.bim : bootpack.obj naskfunc.obj hankaku.obj Makefile
	$(OBJ2BIM) @$(RULEFILE) out:bootpack.bim stack:3136k map:bootpack.map \
		bootpack.obj naskfunc.obj hankaku.obj
# 3MB+64KB=3136KB

bootpack.hrb : bootpack.bim Makefile
	$(BIM2HRB) bootpack.bim bootpack.hrb 0

haribote.sys : asmhead.bin bootpack.hrb Makefile
	$(HARITOL) concat haribote.sys asmhead.bin bootpack.hrb


haribote.img : ipl10.bin haribote.sys Makefile
	$(EDIMG) imgin:$(FDIMG0AT) \
		wbinimg src:ipl10.bin len:512 from:0 to:0 \
		copy from:haribote.sys to:@: \
		imgout:haribote.img

# コマンド

img :
	$(MAKE) haribote.img

run :
	$(MAKE) haribote.img
	$(HARITOL) concat $(FDIMAGE0) haribote.img
	$(MAKE) -C ../z_tools/qemu

#install :
#	$(MAKE) helloos.img
#	$(HARITOL) concat $(FDIMAGE0) haribote.img

clean :
	rm -f *.img *.bin *.lst *.sys *.gas *.bim *.hrb *.map *.obj \
		bootpack.nas
