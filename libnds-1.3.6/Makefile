ifeq ($(strip $(DEVKITPRO)),)
$(error "Please set DEVKITPRO in your environment. export DEVKITPRO=<path to>devkitPro)
endif
 
export TOPDIR	:=	$(CURDIR)

export LIBNDS_MAJOR	:= 1
export LIBNDS_MINOR	:= 3
export LIBNDS_PATCH	:= 6


VERSION	:=	$(LIBNDS_MAJOR).$(LIBNDS_MINOR).$(LIBNDS_PATCH)


.PHONY: release debug clean all docs

all: wrappers include/nds/libversion.h release debug 

wrappers: source/arm7/wrap_touch.c \
	  source/arm9/wrap_bios.c \
	  source/arm9/wrap_console.c \
	  source/arm9/wrap_input.c \
	  source/arm9/wrap_interrupts.c \
	  source/arm9/wrap_rumble.c \
	  source/arm9/wrap_system.c \
	  source/arm9/wrap_video.c \
	  source/arm9/wrap_videoGL.c

PYTHON := python
PYREXC := pyrexc

source/arm7/wrap_touch.c: include/nds/arm7/wrap_touch.pyx
	$(PYREXC) include/nds/arm7/wrap_touch.pyx -o source/arm7/wrap_touch.c

source/arm9/wrap_bios.c: include/nds/arm9/wrap_bios.pyx
	$(PYREXC) include/nds/arm9/wrap_bios.pyx -o source/arm9/wrap_bios.c

source/arm9/wrap_console.c: include/nds/arm9/wrap_console.pyx
	$(PYREXC) include/nds/arm9/wrap_console.pyx -o source/arm9/wrap_console.c

source/arm9/wrap_input.c: include/nds/arm9/wrap_input.pyx
	$(PYREXC) include/nds/arm9/wrap_input.pyx -o source/arm9/wrap_input.c

source/arm9/wrap_interrupts.c: include/nds/arm9/wrap_interrupts.pyx
	$(PYREXC) include/nds/arm9/wrap_interrupts.pyx -o source/arm9/wrap_interrupts.c

source/arm9/wrap_rumble.c: include/nds/arm9/wrap_rumble.pyx
	$(PYREXC) include/nds/arm9/wrap_rumble.pyx -o source/arm9/wrap_rumble.c

source/arm9/wrap_system.c: include/nds/arm9/wrap_system.pyx
	$(PYREXC) include/nds/arm9/wrap_system.pyx -o source/arm9/wrap_system.c

source/arm9/wrap_video.c: include/nds/arm9/wrap_video.pyx
	$(PYREXC) include/nds/arm9/wrap_video.pyx -o source/arm9/wrap_video.c

source/arm9/wrap_videoGL.c: include/nds/arm9/wrap_videoGL.pyx
	$(PYREXC) include/nds/arm9/wrap_videoGL.pyx -o source/arm9/wrap_videoGL.c


#-------------------------------------------------------------------------------
release: lib
#-------------------------------------------------------------------------------
	$(MAKE) -C arm9 BUILD=release || { exit 1;}
	$(MAKE) -C arm7 BUILD=release || { exit 1;}
 
#-------------------------------------------------------------------------------
debug: lib
#-------------------------------------------------------------------------------
	$(MAKE) -C arm9 BUILD=debug || { exit 1;}
	$(MAKE) -C arm7 BUILD=debug || { exit 1;}

#-------------------------------------------------------------------------------
lib:
#-------------------------------------------------------------------------------
	mkdir lib
 
#-------------------------------------------------------------------------------
clean:
#-------------------------------------------------------------------------------
	@$(MAKE) -C arm9 clean
	@$(MAKE) -C arm7 clean
	rm -fr source/arm9/wrap_*.c source/arm7/wrap_*.c

#-------------------------------------------------------------------------------
dist-src:
#-------------------------------------------------------------------------------
	@tar --exclude=*CVS* --exclude=.svn -cjf libnds-src-$(VERSION).tar.bz2 arm7/Makefile arm9/Makefile source include icon.bmp Makefile libnds_license.txt 

#-------------------------------------------------------------------------------
dist-bin: all
#-------------------------------------------------------------------------------
	@tar --exclude=*CVS* --exclude=.svn -cjf libnds-$(VERSION).tar.bz2 include lib icon.bmp libnds_license.txt

dist: dist-bin dist-src

#-------------------------------------------------------------------------------
install: dist-bin
#-------------------------------------------------------------------------------
	mkdir -p $(DEVKITPRO)/libnds
	bzip2 -cd libnds-$(VERSION).tar.bz2 | tar -x -C $(DEVKITPRO)/libnds

#---------------------------------------------------------------------------------
docs:
#---------------------------------------------------------------------------------
	doxygen libnds.dox
	cat warn.log

#---------------------------------------------------------------------------------
include/nds/libversion.h : Makefile
#---------------------------------------------------------------------------------
	@echo "#ifndef __LIBNDSVERSION_H__" > $@
	@echo "#define __LIBNDSVERSION_H__" >> $@
	@echo >> $@
	@echo "#define _LIBNDS_MAJOR_	$(LIBNDS_MAJOR)" >> $@
	@echo "#define _LIBNDS_MINOR_	$(LIBNDS_MINOR)" >> $@
	@echo "#define _LIBNDS_PATCH_	$(LIBNDS_PATCH)" >> $@
	@echo >> $@
	@echo '#define _LIBNDS_STRING "libNDS Release '$(LIBNDS_MAJOR).$(LIBNDS_MINOR).$(LIBNDS_PATCH)'"' >> $@
	@echo >> $@
	@echo "#endif // __LIBNDSVERSION_H__" >> $@


