SPK_NAME = lirc
SPK_VERS = 0.9.0
SPK_REV = 3
SPK_ICON = src/lirc.png
FIRMWARE = 4.3-3776

LIRC_SUPPORTED_DRIVERS =

# Abort for ARCHs that won't compile LIRC cleanly
UNSUPPORTED_ARCHS = $(PPC_ARCHS)

# include arch definitions
include ../../mk/spksrc.archs.mk

# Use older LIRC build for ARCHs with kernel < 2.6.36
ifeq ($(findstring $(ARCH),$(ARMv5_ARCHS) $(PPC_ARCHS)),$(ARCH))
SPK_VERS = 0.8.7
DEPENDS = cross/lirc-0.8.7
LIRC_SUPPORTED_DRIVERS = mceusb uirt uirt2
else
DEPENDS = cross/lirc-0.9.0
endif

MAINTAINER = bru7us
DESCRIPTION = LIRC daemon decodes infrared signals and provides them on a Unix domain socket
DISPLAY_NAME = LIRC

HOMEPAGE = https://www.lirc.org/
LICENSE  =

WIZARDS_DIR = $(WORK_DIR)/wizard

INSTALLER_SCRIPT = src/installer.sh
SSS_SCRIPT       = src/dsm-control.sh

INSTALL_PREFIX = /usr/local/$(SPK_NAME)

POST_STRIP_TARGET = lirc_extra_install

include ../../mk/spksrc.spk.mk

.PHONY: lirc_extra_install lirc_abort
lirc_extra_install:
	install -m 755 -d $(STAGING_DIR)/var
	install -m 755 -d $(STAGING_DIR)/var/log
	install -m 755 -d $(STAGING_DIR)/etc/lirc/scripts
	install -m 755 src/send_ir.sh $(STAGING_DIR)/etc/lirc/scripts/send_ir.sh
	install -m 644 src/lircrc $(STAGING_DIR)/etc/lirc/lircrc
	install -m 644 src/lircd.conf $(STAGING_DIR)/etc/lirc/lircd.conf
	cp -R $(WORK_DIR)/$(SPK_NAME)-$(SPK_VERS)/remotes $(STAGING_DIR)/
	install -m 755 -d $(WIZARDS_DIR)
	install src/install_uifile.tmpl $(WIZARDS_DIR)/install_uifile
	@for DRIVER in $(LIRC_SUPPORTED_DRIVERS); do \
	    sed -i "s/@keys@/$${DRIVER} @keys@/" $(WIZARDS_DIR)/install_uifile; \
	done;
	@sed -i 's/@keys@//' $(WIZARDS_DIR)/install_uifile
