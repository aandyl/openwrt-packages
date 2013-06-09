#
# Copyright (C) 2012-2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=pianod
PKG_VERSION:=144
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://deviousfish.com/Downloads/pianod/
PKG_MD5SUM:=847957dba094eb53f0bce50a41175f88

PKG_FIXUP:=autoreconf
PKG_BUILD_DEPENDS:=libao libfaad2 libmad libjson
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/pianod
  $(call Package/pianod/Default)
  SECTION:=sound
  CATEGORY:=Sound
  DEPENDS:=+libao +libfaad2 +libmad +libpiano +libpthread \
           +libgnutls +@LIBGCRYPT_CIPHER_BLOWFISH +libjson
  TITLE:=Pandora radio daemon-mode client
  URL:=http://deviousfish.com/pianod/
endef

define Package/pianod/description
  pianod is an open-source Pandora client with a simple TCP/IP control interface.
  A simple protocol makes for easy clients, mix scripting, integration with a
  home automation system, use as multiple-user music system for home or office.
endef

define Package/pianod/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/pianod $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/piano $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/runmix $(1)/usr/bin/
endef

$(eval $(call BuildPackage,pianod))
