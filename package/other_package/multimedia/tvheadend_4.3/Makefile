#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=tvheadend
PKG_VERSION:=4.3
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL=https://github.com/tvheadend/tvheadend.git
PKG_SOURCE_DATE:=2021-05-31
PKG_SOURCE_VERSION:=9476680f88d3c2363f86bdb1d4ea93dd3c7d2c95
PKG_MIRROR_HASH:=fbb6712c1db8a75e761a174335499fa25baa6dd7feab4b9aa6f9167af97c474f

PKG_LICENSE:=GPL-3.0
PKG_LICENSE_FILES:=LICENSE.md

PKG_FIXUP:=autoreconf

PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/tvheadend_by_sqter
  SECTION:=multimedia
  CATEGORY:=Multimedia
  TITLE:=Tvheadend is a TV streaming server for Linux
  DEPENDS:=+libopenssl +libpcre +libpcre2 +librt +zlib +TVHEADEND_AVAHI_SUPPORT:libavahi-client +TVHEADEND_DVBCSA_SUPPORT:libdvbcsa_mod $(ICONV_DEPENDS)
  URL:=https://tvheadend.org
  MAINTAINER:=Jan Čermák <jan.cermak@nic.cz>, Sandy Gould <sjjgould@gmail.com>
endef

define Package/tvheadend_by_sqter/description
  Tvheadend is a TV streaming server and recorder for Linux, FreeBSD and Android
  supporting DVB-S, DVB-S2, DVB-C, DVB-T, ATSC, IPTV, SAT>IP and HDHomeRun as input sources.

  Tvheadend offers the HTTP (VLC, MPlayer), HTSP (Kodi, Movian) and SAT>IP streaming.
 
  As of Tvheadend 2.4, the Tvheadend 'bundle' (the web front-end) requires CONFIG_BUILD_NLS (i.e., native language support) to be enabled.
  This allows the correct version of libiconv to be built and linked.
  Tvheadend will function without the bundle being enabled, and therefore without CONFIG_BUILD_NLS, you will just see a blank page if you attempt to log into the front-end.
endef

define Package/tvheadend_by_sqter/config
  menu "Configuration"
  depends on PACKAGE_tvheadend_by_sqter
  source "$(SOURCE)/Config.in"
  endmenu
endef

ifeq ($(CONFIG_BUILD_NLS),y)
  ifeq ($(CONFIG_TVHEADEND_ENABLE_BUNDLE),y)
    CONFIGURE_ARGS += --enable-bundle
    TARGET_LDFLAGS += $(if $(ICONV_FULL),-liconv)
  endif
endif

ifeq ($(CONFIG_TVHEADEND_CWC_SUPPORT),)
  CONFIGURE_ARGS += --disable-cwc
endif

ifeq ($(CONFIG_TVHEADEND_LINUXDVB_SUPPORT),)
  CONFIGURE_ARGS += --disable-linuxdvb
endif

ifeq ($(CONFIG_TVHEADEND_DVBSCAN_SUPPORT),)
  CONFIGURE_ARGS += --disable-dvbscan
endif

ifeq ($(CONFIG_TVHEADEND_DVBCSA_SUPPORT),y)
  CONFIGURE_ARGS += --enable-dvbcsa
else
  CONFIGURE_ARGS += --disable-dvbcsa
endif

ifeq ($(CONFIG_TVHEADEND_AVAHI_SUPPORT),)
  CONFIGURE_ARGS += --disable-avahi
else
  CONFIGURE_ARGS += --enable-avahi
endif

ifeq ($(CONFIG_TVHEADEND_HDHOMERUN_CLIENT_SUPPORT),y)
  CONFIGURE_ARGS += --enable-hdhomerun_client
else
  CONFIGURE_ARGS += --disable-hdhomerun_static
endif

ifeq ($(CONFIG_TVHEADEND_DISABLE_SATIP_SERVER_SUPPORT),y)
  CONFIGURE_ARGS += --disable-satip_server
endif

ifeq ($(CONFIG_TVHEADEND_DISABLE_SATIP_CLIENT_SUPPORT),y)
  CONFIGURE_ARGS += --disable-satip_client
endif

CONFIGURE_ARGS += \
	--arch=$(ARCH) \
	--disable-dbus_1 \
	--disable-libav \
	--disable-ffmpeg_static \
 	--disable-libx264 \
        --disable-libx264_static \
        --disable-libx265 \
        --disable-libx265_static \
        --disable-libvpx \
        --disable-libvpx_static \
        --disable-libtheora \
        --disable-libtheora_static \
        --disable-libvorbis \
        --disable-libvorbis_static \
        --disable-libfdkaac \
        --disable-libfdkaac_static \
	--disable-libmfx_static \
	--disable-imagecache \

TARGET_CFLAGS += -Wno-error=pointer-compare
TARGET_CFLAGS += -ffunction-sections -fdata-sections
TARGET_LDFLAGS += -Wl,--gc-sections


define Build/Prepare
	$(call Build/Prepare/Default)
	echo 'Tvheadend $(shell echo $(PKG_VERSION))_by_SqTER' \
		> $(PKG_BUILD_DIR)/debian/changelog
endef

define Package/conffiles
/etc/config/tvheadend
endef

define Package/tvheadend_by_sqter/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/tvheadend.init $(1)/etc/init.d/tvheadend
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/tvheadend.config $(1)/etc/config/tvheadend

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/build.linux/tvheadend $(1)/usr/bin/
endef

$(eval $(call BuildPackage,tvheadend_by_sqter))
