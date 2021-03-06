## -----------------------------------------------------------------------
##
##   Copyright 2001-2008 H. Peter Anvin - All Rights Reserved
##   Copyright 2010 Intel Corporation; author: H. Peter Anvin
##
##   This program is free software; you can redistribute it and/or modify
##   it under the terms of the GNU General Public License as published by
##   the Free Software Foundation, Inc., 51 Franklin St, Fifth Floor,
##   Boston MA 02110-1301, USA; either version 2 of the License, or
##   (at your option) any later version; incorporated herein by reference.
##
## -----------------------------------------------------------------------

SYSLINUX_DIR = /home/jon/Work/syslinux
topdir = $(SYSLINUX_DIR)
MAKEDIR = $(topdir)/mk
objdir = $(topdir)/bios
ARCH = i386
DATE = 20190321
FWCLASS = BIOS
FIRMWARE = BIOS
SRC = ./

VPATH = $(SRC)
include $(MAKEDIR)/elf.mk
-include $(topdir)/version.mk

LIBS	   = $(objdir)/com32/libupload/libcom32upload.a
LNXLIBS	   = $(objdir)/com32/libutil/libutil_lnx.a

CFLAGS += -I$(com32) -I$(topdir)

MODULES	  = acpi_fix.c32
TESTFILES =

SRCS = $(sort $(wildcard $(SRC)/*.c))
OBJS = $(subst $(SRC)/,,$(patsubst %.c,%.o,$(SRCS)))

# The DATE is set on the make command line when building binaries for
# official release.  Otherwise, substitute a hex string that is pretty much
# guaranteed to be unique to be unique from build to build.
ifndef HEXDATE
HEXDATE := $(shell $(PERL) $(topdir)/now.pl $(SRCS) $(sort $(wildcard *.h)))
endif
ifndef DATE
DATE    := $(shell sh $(topdir)/gen-id.sh $(VERSION) $(HEXDATE))
endif

CFLAGS    += -DDATE='"$(DATE)"'

all: $(MODULES) $(TESTFILES)

acpi_fix.elf : $(OBJS) $(LIBS) $(C_LIBS)
	$(LD) $(LDFLAGS) -o $@ $^

tidy dist:
	rm -f *.o *.lo *.a *.lst .*.d *.tmp

clean: tidy
	rm -f *.lnx 

spotless: clean
	rm -f *.lss *.c32 *.com *.elf
	rm -f *~ \#*

install:

-include .*.d
