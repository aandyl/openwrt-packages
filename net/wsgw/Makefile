#
# Copyright (C) 2012-2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=wsgw
PKG_VERSION:=23
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://deviousfish.com/Downloads/wsgw/
PKG_MD5SUM:=44b9f5516331b40de7a7b1c9f4ffda9f

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/wsgw
  $(call Package/wsgw/Default)
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libpthread
  TITLE:=WebSocket gateway
  URL:=http://deviousfish.com/pianod/wsgw.html
endef

define Package/wsgw/description
  wsgw is a general purpose WebSocket (RFC6455) gateway. It is primarily used with pianod
  to control it from WebSocket clients.
endef

define Package/wsgw/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/wsgw $(1)/usr/bin/
endef

$(eval $(call BuildPackage,wsgw))
