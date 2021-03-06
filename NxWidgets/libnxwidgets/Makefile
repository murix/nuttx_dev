#################################################################################
# NxWidgets/libnxwidgets/Makefile
#
#   Copyright (C) 2012-2014 Gregory Nutt. All rights reserved.
#   Author: Gregory Nutt <gnutt@nuttx.org>
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#
# 1. Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
# 2. Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in
#    the documentation and/or other materials provided with the
#    distribution.
# 3. Neither the name NuttX, NxWidgets, nor the names of its contributors
#    me be used to endorse or promote products derived from this software
#    without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
# FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
# COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
# INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
# BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS
# OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED
# AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
# LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN
# ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.
#
#################################################################################

-include $(TOPDIR)/Make.defs

# Control build verbosity

ifeq ($(V),1)
export Q :=
else
export Q := @
endif

NXWIDGETDIR := ${shell pwd | sed -e 's/ /\\ /g'}

ASRCS =
CSRCS =
# Infrastructure
CXXSRCS  = cbitmap.cxx cbgwindow.cxx ccallback.cxx cgraphicsport.cxx
CXXSRCS += clistdata.cxx clistdataitem.cxx cnxfont.cxx
CXXSRCS += cnxserver.cxx cnxstring.cxx cnxtimer.cxx cnxwidget.cxx cnxwindow.cxx
CXXSRCS += cnxtkwindow.cxx cnxtoolbar.cxx crect.cxx crlepalettebitmap.cxx
CXXSRCS += cscaledbitmap.cxx cstringiterator.cxx ctext.cxx cwidgetcontrol.cxx  cwidgeteventhandlerlist.cxx
CXXSRCS += cwindoweventhandlerlist.cxx singletons.cxx
# Widget APIs
CXXSRCS += cbutton.cxx cbuttonarray.cxx ccheckbox.cxx ccyclebutton.cxx
CXXSRCS += cglyphbutton.cxx cglyphsliderhorizontal.cxx cglyphsliderhorizontalgrip.cxx
CXXSRCS += cimage.cxx ckeypad.cxx clabel.cxx clatchbutton.cxx
CXXSRCS += clatchbuttonarray.cxx clistbox.cxx clistboxdataitem.cxx cmultilinetextbox.cxx
CXXSRCS += cnumericedit.cxx
CXXSRCS += cprogressbar.cxx cradiobutton.cxx cradiobuttongroup.cxx cscrollbarhorizontal.cxx
CXXSRCS += cscrollbarpanel.cxx cscrollbarvertical.cxx cscrollinglistbox.cxx
CXXSRCS += cscrollingpanel.cxx cscrollingtextbox.cxx csliderhorizontal.cxx
CXXSRCS += csliderhorizontalgrip.cxx cslidervertical.cxx csliderverticalgrip.cxx
CXXSRCS += cstickybutton.cxx cstickybuttonarray.cxx cstickyimage.cxx ctabpanel.cxx
CXXSRCS += ctextbox.cxx
# Images
CXXSRCS += glyph_nxlogo160x160.cxx glyph_nxlogo320x320.cxx
CXXSRCS += glyph_arrowdown.cxx glyph_checkboxon.cxx glyph_screendepthup.cxx
CXXSRCS += glyph_arrowleft.cxx glyph_control.cxx glyph_screenflipdown.cxx
CXXSRCS += glyph_arrowright.cxx glyph_cycle.cxx glyph_screenflipup.cxx
CXXSRCS += glyph_arrowup.cxx glyph_radiobuttonoff.cxx glyph_shift.cxx
CXXSRCS += glyph_backspace.cxx glyph_radiobuttonmu.cxx glyph_windowclose.cxx
CXXSRCS += glyph_capslock.cxx glyph_radiobuttonon.cxx glyph_windowdepthdown.cxx
CXXSRCS += glyph_checkboxmu.cxx glyph_return.cxx glyph_windowdepthup.cxx
CXXSRCS += glyph_checkboxoff.cxx glyph_screendepthdown.cxx
SRCS = $(ASRCS) $(CSRCS) $(CXXSRCS)

AOBJS = $(ASRCS:.S=$(OBJEXT))
COBJS = $(CSRCS:.c=$(OBJEXT))
CXXOBJS = $(CXXSRCS:.cxx=$(OBJEXT))
OBJS = $(AOBJS) $(COBJS) $(CXXOBJS)

BIN = libnxwidgets$(LIBEXT)

ifeq ($(WINTOOL),y)
INCDIROPT = -w
endif

CFLAGS += ${shell $(INCDIR) $(INCDIROPT) "$(CC)" $(NXWIDGETDIR)$(DELIM)include}
CXXFLAGS += ${shell $(INCDIR) $(INCDIROPT) "$(CC)" $(NXWIDGETDIR)$(DELIM)include}
DEPPATH = --dep-path src
VPATH = src

all: check_nuttx $(BIN)
.PHONY = check_nuttx depend clean distclean export

$(AOBJS): %$(OBJEXT): %.S
	$(call ASSEMBLE, $<, $@)

$(COBJS): %$(OBJEXT): %.c
	$(call COMPILE, $<, $@)

$(CXXOBJS): %$(OBJEXT): %.cxx
	$(call COMPILEXX, $<, $@)

check_nuttx:
	$(Q) ( \
		if [ -z "$(TOPDIR)" ]; then \
			echo "The path to the nuttx directory must be provided on the command line."; \
			echo "Usage: make -C $(NXWIDGETDIR) TOPDIR=\"<nuttx directory>\""; \
			exit 1; \
		fi; \
		if [ ! -d "$(TOPDIR)" ]; then \
			echo "The nuttx directory (TOPDIR) does not exist: $(TOPDIR)"; \
			exit 1; \
		fi; \
		if [ ! -f "$(TOPDIR)$(DELIM).config" ]; then \
			echo "The nuttx directory (TOPDIR) has not been configured"; \
			echo "Please configure NuttX and try again"; \
			exit 1; \
		fi; \
	)

$(BIN): $(OBJS)
	$(call ARCHIVE, $@, $(OBJS))

.depend: Makefile $(SRCS)
	$(Q) $(MKDEP) $(DEPPATH) $(CC) -- $(CFLAGS) -- $(SRCS) >Make.dep
	$(Q) touch $@

depend: .depend

clean:
	$(call DELFILE, $(BIN))
	$(call CLEAN)

distclean: clean
	$(call DELFILE, Make.dep)
	$(call DELFILE, .depend)

export: $(BIN)
	zip -r  nxwidgets-export.zip include $(BIN) COPYING

-include Make.dep
