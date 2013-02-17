include $(TOPDIR)/rules.mk

PKG_NAME:=libsoup
PKG_VERSION:=2.38.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=@GNOME/$(PKG_NAME)/2.38
PKG_MD5SUM:=d13fb4968acea24c26b83268a308f580

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

HOST_BUILD_DEPENDS:=glib2/host libxml2/host libgnutls/host

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

HOST_LDFLAGS+=\
	-Wl,-rpath=$(STAGING_DIR_HOST)/lib

TARGET_LDFLAGS+=\
	-Wl,-rpath-link=$(STAGING_DIR)/usr/lib

define Package/libsoup
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=libsoup
  URL:=http://live.gnome.org/LibSoup
  DEPENDS:=+glib2 +libxml2 +libgnutls
endef

define Build/Configure
	$(call Build/Configure/Default, \
		--enable-ssl \
		--disable-glibtest \
		--without-apache-httpd \
		--without-gnome \
	)
endef

define Host/Configure
	$(call Host/Configure/Default, \
		--disable-tls-check \
		--without-apache-httpd \
		--without-gnome \
	)
endef

define package/libsoup/decription
Libsoup is an HTTP library implementation in C
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/{lib/pkgconfig,include/libsoup-2.4/libsoup}

	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/libsoup-2.4.{so*,la,a} \
		$(1)/usr/lib/

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/* \
		$(1)/usr/lib/pkgconfig/

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/include/libsoup-2.4/libsoup/*.h \
		$(1)/usr/include/libsoup-2.4/libsoup/
endef

define Package/libsoup/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/libsoup-2.4.so* \
		$(1)/usr/lib/
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,libsoup))
