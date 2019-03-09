TOOLPATH = ../z_tools/
INCPATH = ../z_tools/haribote/

MAKE = $(TOOLPATH)make -r
EDIMG = $(TOOLPATH)edimg
HARITOL = $(TOOLPATH)haritol

NASK = $(TOOLPATH)nask
FDIMG0AT = $(TOOLPATH)fdimg0at.tek
FDIMAGE0 = $(TOOLPATH)qemu/fdimage0.bin

# デフォルト動作

default :
	$(MAKE) img

# ファイル生成規則

ipl.bin : ipl.nas Makefile
	$(NASK) ipl.nas ipl.bin ipl.lst

helloos.img : ipl.bin Makefile
	$(EDIMG) imgin:$(FDIMG0AT) wbinimg src:ipl.bin len:512 from:0 to:0 imgout:helloos.img

# コマンド

asm :
	$(MAKE) ipl.bin

img :
	$(MAKE) helloos.img

run :
	$(MAKE) helloos.img
	$(HARITOL) concat $(FDIMAGE0) helloos.img
	$(MAKE) -C ../z_tools/qemu

install :
	$(MAKE) helloos.img
	$(HARITOL) concat $(FDIMAGE0) helloos.img

clean :
	rm -f *.img *.bin *.lst
