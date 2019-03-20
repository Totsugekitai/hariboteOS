OBJS_BOOTPACK = bootpack.obj naskfunc.obj hankaku.obj graphic.obj dsctbl.obj \
				int.obj fifo.obj keyboard.obj mouse.obj memory.obj sheet.obj \
				timer.obj

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
	$(NASK) ipl10.nas ipl10.bin ipl10.lst

asmhead.bin : asmhead.nas Makefile
	$(NASK) asmhead.nas asmhead.bin asmhead.lst

hankaku.bin : hankaku.txt Makefile
	$(MAKEFONT) hankaku.txt hankaku.bin

hankaku.obj : hankaku.bin Makefile
	$(BIN2OBJ) hankaku.bin hankaku.obj _hankaku

bootpack.bim : $(OBJS_BOOTPACK) Makefile
	$(OBJ2BIM) @$(RULEFILE) out:bootpack.bim stack:3136k map:bootpack.map \
		$(OBJS_BOOTPACK)
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

# 一般規則

%.gas : %.c bootpack.h Makefile
	$(CC1) -o $*.gas $*.c

%.nas : %.gas Makefile
	$(GAS2NASK) $*.gas $*.nas

%.obj : %.nas Makefile
	$(NASK) $*.nas $*.obj $*.lst

# コマンド

img :
	$(MAKE) haribote.img

run :
	$(MAKE) img
	$(HARITOL) concat $(FDIMAGE0) haribote.img
	$(MAKE) -C ../z_tools/qemu

#install :
#	$(MAKE) helloos.img
#	$(HARITOL) concat $(FDIMAGE0) haribote.img

clean :
	rm -f *.img *.bin *.lst *.sys *.gas *.bim *.hrb *.map *.obj \
		bootpack.nas graphic.nas dsctbl.nas
