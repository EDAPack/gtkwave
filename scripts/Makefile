
MKFILES_DIR=$(abspath $(dir $(lastword $(MAKEFILE_LIST))))
EDAPACK_GTKWAVE_DIR=$(abspath $(MKFILES_DIR)/..)
PACKAGES_DIR=$(EDAPACK_GTKWAVE_DIR)/packages
BUILD_DIR=$(EDAPACK_GTKWAVE_DIR)/build

include $(EDAPACK_GTKWAVE_DIR)/etc/edapack-gtkwave.info

GTKWAVE_DIR=gtkwave-$(version)
GTKWAVE_TGZ=$(GTKWAVE_DIR).tar.gz
GTKWAVE_URL=http://gtkwave.sourceforge.net/$(GTKWAVE_TGZ)

ifeq (true,$(VERBOSE))
Q=
WGET=wget
UNTARGZ=tar xvzf
else
Q=@
WGET=wget -q
UNTARGZ=tar xzf
endif

uname_o:=$(shell uname -o)

ifeq (Linux,$(uname_o))
platform=linux_x86_64
else
ifeq (GNU/Linux,$(uname_o))
platform=linux_x86_64
else
platform=unknown
endif
endif

all : $(BUILD_DIR)/gtkwave-$(platform)-$(version).tar.gz

$(BUILD_DIR)/gtkwave-$(platform)-$(version).tar.gz : $(PACKAGES_DIR)/$(GTKWAVE_TGZ)
	$(Q)rm -rf $(BUILD_DIR)/gtkwave
	$(Q)mkdir -p $(BUILD_DIR)/gtkwave
	$(Q)cd $(BUILD_DIR)/gtkwave; $(UNTARGZ) $^
	$(Q)cd $(BUILD_DIR)/gtkwave/$(GTKWAVE_DIR); \
		./configure --prefix=$(BUILD_DIR)/gtkwave/gtkwave \
			--disable-tcl \
			--disable-xz \
			--disable-mime-update
	$(Q)cd $(BUILD_DIR)/gtkwave/$(GTKWAVE_DIR); $(MAKE) 
	$(Q)cd $(BUILD_DIR)/gtkwave/$(GTKWAVE_DIR); $(MAKE) install
	$(Q)cd $(BUILD_DIR)/gtkwave ; tar czf $@ gtkwave


$(PACKAGES_DIR)/$(GTKWAVE_TGZ) : 
	$(Q)if test ! -d $(PACKAGES_DIR); then mkdir -p $(PACKAGES_DIR); fi
	$(Q)$(WGET) -O $@ $(GTKWAVE_URL)

clean :
	$(Q)rm -rf $(BUILD_DIR)

clean-all :
	$(Q)rm -rf $(BUILD_DIR) $(PACKAGES_DIR)

