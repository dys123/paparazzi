# Hey Emacs, this is a -*- makefile -*-

DATADIR = $(PAPARAZZI_HOME)/conf/maps_data
Q=@

UNAME = $(shell uname -s)
ifeq ("$(UNAME)","Darwin")
	MKTEMP = gmktemp
else
	MKTEMP = mktemp
endif

all: $(PAPARAZZI_HOME)/conf/maps.xml

clean:
	$(Q)rm -f $(DATADIR)/maps.google.com

$(DATADIR):
	mkdir $(DATADIR)

$(DATADIR)/maps.google.com: $(DATADIR) FORCE
	@echo "-----------------------------------------------"
	@echo "DOWNLOAD: google maps version code";
	$(Q)wget -q -O $(@) http://maps.google.com/ || \
		(rm -f $(@) && \
		echo "Could not download google maps version code" && \
		echo "-----------------------------------------------" && \
		exit 1)

$(PAPARAZZI_HOME)/conf/maps.xml: $(DATADIR)/maps.google.com
	$(eval GOOGLE_VERSION := $(shell grep -E "http://khm[0-9]+.google.com/kh/v=[0-9]+.x26" $(DATADIR)/maps.google.com | sed -E 's#.*http://khm[0-9]+.google.com/kh/v=##;s#.x26.*##'))
	$(eval $@_TMP := $(shell $(MKTEMP)))
	@echo "Updated google maps version to $(GOOGLE_VERSION)"
	@echo "-----------------------------------------------"
	$(Q)echo "<!DOCTYPE maps SYSTEM \"maps.dtd\">" > $($@_TMP)
	$(Q)echo "" >> $($@_TMP)
	$(Q)echo "<maps google_version=\"$(GOOGLE_VERSION)\"/>" >> $($@_TMP)
	$(Q)echo "" >> $($@_TMP)
	$(Q)mv $($@_TMP) $@
	$(Q)chmod a+r $@

FORCE:
.PHONY: all clean
