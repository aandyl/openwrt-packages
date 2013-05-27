#
# Copyright (C) 2006-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=gupnp-av
PKG_VERSION:=0.12.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=@GNOME/gupnp-av/0.12/
PKG_MD5SUM:=2d15a94d743720febb400b2cacde1cdc

# When set, this package (and all dependencies) will be built on the host, in
# order to regenerate the Vala API definition file (.vapi) using
# gobject-introspection and vapigen.
#
# When clear, pregenerated vapi provided by the package maintainer will be
# used.
BUILD_VAPI:=1

PKG_FIXUP:=autoreconf
HOST_BUILD_DEPENDS:=libgupnp/host vala/host gobject-introspection/host

ifdef BUILD_VAPI
  PKG_BUILD_DEPENDS+=libgupnp-av/host
endif

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

TARGET_LDFLAGS+=\
        -Wl,-rpath-link=$(STAGING_DIR)/usr/lib

define Host/Exports
  $(call Host/Exports/Default, $(1))
  $(1) : export XDG_DATA_DIRS=$(STAGING_DIR_HOST)/share
endef

define Package/libgupnp-av
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=Library for dealing with UPnP/AV profiles
  URL:=http://www.gupnp.org/
  DEPENDS:=+libgupnp
endef

define Package/libgupnp-av/description
GUPnP A/V is a small utility library that aims to ease the handling and
implementation of UPnP A/V profiles.
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		all install
endef

ifdef BUILD_VAPI
  VAPI_SOURCE:=$(HOST_BUILD_DIR)/vala
else
  VAPI_SOURCE:=./files
endif

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/{lib/pkgconfig,include/gupnp-av-1.0/libgupnp-av}

	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/libgupnp-av-1.0.{so*,la,a} \
		$(1)/usr/lib/

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/*.pc \
		$(1)/usr/lib/pkgconfig/

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/include/gupnp-av-1.0/libgupnp-av/*.h \
		$(1)/usr/include/gupnp-av-1.0/libgupnp-av/

	$(INSTALL_DIR) $(1)/usr/share/vala/vapi

	$(INSTALL_DATA) \
		$(VAPI_SOURCE)/gupnp-av-1.0.{vapi,deps} \
		$(1)/usr/share/vala/vapi
endef

define Package/libgupnp-av/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/libgupnp-av-1.0.so* \
		$(1)/usr/lib/
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,libgupnp-av))
