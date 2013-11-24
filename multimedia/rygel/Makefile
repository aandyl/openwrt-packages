#
# Copyright (C) 2006-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=rygel
PKG_VERSION:=0.20.2
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=@GNOME/rygel/0.20/
PKG_MD5SUM:=a719121013449c35dbad0f9a22effa0c

PKG_FIXUP:=autoreconf
PKG_BUILD_DEPENDS:=vala/host intltool/host vala

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

TARGET_LDFLAGS+=\
        -Wl,-rpath-link=$(STAGING_DIR)/usr/lib

define Package/rygel/Default
  SECTION:=multimedia
  CATEGORY:=Multimedia
  TITLE:=GNOME UPnP/DLNA services
  URL:=http://live.gnome.org/Rygel
endef

define Package/rygel/description/Default
 Rygel is a collection of DLNA (UPnP AV) services (devices in UPnP speak),
 implemented through a plug-in mechanism.
endef

define Package/rygel
  $(call Package/rygel/Default)
  DEPENDS:=+librygel-core +librygel-server
endef

define Package/rygel/conffiles
/etc/config/rygel
endef

define Package/rygel/description
$(call Package/rygel/description/Default)
 .
 This package contains the main rygel binary.
endef

define Package/librygel-core
  $(call Package/rygel/Default)
  TITLE+= - core library
  DEPENDS:=+libgssdp +libgupnp +libgupnp-av +libgupnp-dlna +libgee +libsoup \
           +gstreamer +libgstvideo +libgstpbutils
endef

define Package/librygel-core/description
$(call Package/rygel/description/Default)
 .
 This package contains the librygel-core library.
endef

define Package/librygel-renderer
  $(call Package/rygel/Default)
  TITLE+= - media renderer library
  DEPENDS:=+librygel-core
endef

define Package/librygel-renderer/description
$(call Package/rygel/description/Default)
 .
 This package contains the librygel-renderer library, which is used by the
 playbin (media renderer) plugin.
endef

define Package/librygel-renderer-gst
  $(call Package/rygel/Default)
  TITLE+= - media renderer library (GStreamer)
  DEPENDS:=+librygel-core +librygel-renderer
endef

define Package/librygel-renderer-gst/description
$(call Package/rygel/description/Default)
 .
 This package contains the librygel-renderer-gst library, which provides
 GStreamer support for the playbin (media renderer) plugin.
endef

define Package/librygel-server
  $(call Package/rygel/Default)
  TITLE+= - media server library
  DEPENDS:=+librygel-core
endef

define Package/librygel-server/description
$(call Package/rygel/description/Default)
 .
 This package contains the librygel-server library, which is used by the media
 server plugins.
endef

define Package/librygel-media-engine-gst
  $(call Package/rygel/Default)
  TITLE+= - GStreamer media engine
  DEPENDS:=+librygel-core +librygel-server +gstreamer +libgstpbutils +libgstapp
endef

define Package/librygel-media-engine-gst/descriptino
$(call Package/rygel/description/Default)
 .
 This package contains the rygel media engine for GStreamer.
endef

define RemoveValaStamps
	find $(PKG_BUILD_DIR) -name "*_vala.stamp" | xargs rm -f
endef

Hooks/Configure/Pre += RemoveValaStamps

define Build/Configure
	$(call Build/Configure/Default,--without-ui --disable-tracker-plugin,)
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		all install
endef

define Package/rygel/install
	$(INSTALL_DIR) $(1)/usr/bin

	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/usr/bin/rygel \
		$(1)/usr/bin

	$(INSTALL_DIR) $(1)/usr/share/rygel/xml

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/share/rygel/xml/*.xml \
		$(1)/usr/share/rygel/xml

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DATA) files/etc/rygel.conf $(1)/etc

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) files/etc/init.d/rygel $(1)/etc/init.d/rygel

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) files/etc/config/rygel $(1)/etc/config/rygel

	$(INSTALL_DIR) $(1)/usr/share/icons/hicolor/{32x32,48x48,128x128}/apps
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/share/icons/hicolor/32x32/apps/rygel.png \
		$(1)/usr/share/icons/hicolor/32x32/apps/rygel.png
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/share/icons/hicolor/48x48/apps/rygel.png \
		$(1)/usr/share/icons/hicolor/48x48/apps/rygel.png
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/share/icons/hicolor/128x128/apps/rygel.png \
		$(1)/usr/share/icons/hicolor/128x128/apps/rygel.png
endef

LIB_SUFFIX := 2.0

define Package/librygel-core/install
	$(INSTALL_DIR) $(1)/usr/lib

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/librygel-core-$(LIB_SUFFIX).so{,.1,.1.0.0} \
		$(1)/usr/lib
endef

define Package/librygel-renderer/install
	$(INSTALL_DIR) $(1)/usr/lib

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/librygel-renderer-$(LIB_SUFFIX).so{,.1,.1.0.0} \
		$(1)/usr/lib
endef

define Package/librygel-renderer-gst/install
	$(INSTALL_DIR) $(1)/usr/lib

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/librygel-renderer-gst-$(LIB_SUFFIX).so{,.1,.1.0.0} \
		$(1)/usr/lib
endef

define Package/librygel-server/install
	$(INSTALL_DIR) $(1)/usr/lib

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/librygel-server-$(LIB_SUFFIX).so{,.1,.1.0.0} \
		$(1)/usr/lib
endef

define Package/librygel-media-engine-gst/install
	$(INSTALL_DIR) $(1)/usr/lib/rygel-$(LIB_SUFFIX)/engines

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/rygel-$(LIB_SUFFIX)/engines/librygel-media-engine-gst.so \
		$(1)/usr/lib/rygel-$(LIB_SUFFIX)/engines
endef

# 1: plugin name as used in shared library filename
# 2: additional dependencies, if any
define RygelBuildPlugin

  define Package/rygel-$(1)
    $(call Package/rygel/Default)
    TITLE+= - $(1) plugin
    DEPENDS:=+rygel $(2)
  endef

  define Package/rygel-$(1)/install
	$(INSTALL_DIR) $$(1)/usr/lib/rygel-$(LIB_SUFFIX)/plugins
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/rygel-$(LIB_SUFFIX)/plugins/librygel-$(1).so \
		$$(1)/usr/lib/rygel-$(LIB_SUFFIX)/plugins
  endef

  $$(eval $$(call BuildPackage,rygel-$(1)))
endef

define Package/rygel-external/description
  $(call Package/rygel/description/Default)
  .
  This package contains the external plugin, a MediaServer plugin that exports
  media provided by applications implementing the D-Bus MediaServer interface.
endef
$(eval $(call RygelBuildPlugin,external,+librygel-server))

define Package/rygel-media-export/description
  $(call Package/rygel/description/Default)
  .
  This package contains the media-export plugin, a MediaServer plugin that
  recursively exports files and folders specified in the user configuration.
endef
$(eval $(call RygelBuildPlugin,media-export,+libsqlite3 +libgstapp +libgsttag +librygel-server))

define Package/rygel-mpris/description
  $(call Package/rygel/description/Default)
  .
  This package contains the mpris plugin, a MediaServer plugin that exports
  media provided by applications implementing the D-Bus MPRIS2 interface.
endef
$(eval $(call RygelBuildPlugin,mpris,+librygel-server))

# As of 0.15, the tracker plugin has a new dependency on libtracker-sparql,
# which is not packaged for OpenWrt.
#define Package/rygel-tracker/description
#  $(call Package/rygel/description/Default)
#  .
#  This package contains the tracker plugin, a MediaServer plugin that exports
#  media known to Tracker.
#endef
#$(eval $(call RygelBuildPlugin,tracker,+librygel-server))

define Package/rygel-playbin/description
  $(call Package/rygel/description/Default)
  .
  This package contains the playbin plugin, the MediaRenderer plugin.
endef
$(eval $(call RygelBuildPlugin,playbin,+librygel-renderer +librygel-renderer-gst))

define Package/rygel-playbin-gst-suggested/description
  $(call Package/rygel/description/Default)
  .
  This optional metapackage installs a suggested set of gstreamer plugins to
  support audio playback in rygel-playbin.
endef

define Package/rygel-playbin-gst-suggested
  $(call Package/rygel/Default)
  TITLE:=Suggested GStreamer plugins for rygel-playbin
  DEPENDS:= \
    +librygel-media-engine-gst \
    +gst-mod-alsa \
    +gst-mod-audioconvert \
    +gst-mod-audioparsers \
    +gst-mod-audioresample \
    +gst-mod-autodetect \
    +gst-mod-flac \
    +gst-mod-icydemux \
    +gst-mod-id3demux \
    +gst-mod-playback \
    +gst-mod-isomp4 \
    +gst-mod-ogg \
    +gst-mod-playback \
    +gst-mod-souphttpsrc \
    +gst-mod-typefindfunctions \
    +gst-mod-volume \
    +gst-mod-vorbis \
    +BUILD_PATENTED:gst-mod-faad \
    +BUILD_PATENTED:gst-mod-mad
endef
$(eval $(call BuildPackage,rygel-playbin-gst-suggested))
 
$(eval $(call BuildPackage,rygel))
$(eval $(call BuildPackage,librygel-core))
$(eval $(call BuildPackage,librygel-renderer))
$(eval $(call BuildPackage,librygel-renderer-gst))
$(eval $(call BuildPackage,librygel-server))
$(eval $(call BuildPackage,librygel-media-engine-gst))
