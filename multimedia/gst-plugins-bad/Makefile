#
# Copyright (C) 2011-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=gst-plugins-bad
PKG_VERSION:=1.2.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=http://gstreamer.freedesktop.org/src/gst-plugins-bad/
PKG_MD5SUM:=8ee6863cbb917bb4bd235698b87192a4

PKG_BUILD_DEPENDS:=gstreamer gst-plugins-base liboil

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/gstreamer/Default
  CATEGORY:=Multimedia
  SECTION:=multimedia
  TITLE:=GStreamer
  URL:=http://gstreamer.freedesktop.org/
endef

define Package/gstreamer/description/Default
 GStreamer open source multimedia framework
endef


define Package/gst-plugins-bad
$(call Package/gstreamer/Default)
  TITLE+= plugins collection (bad)
  DEPENDS+= $(GST_DEPENDS)
endef

define Package/gst-plugins-bad/description
$(call Package/gstreamer/description/Default)
 .
 This meta package contains only dependencies to the other plugins from
 the bad plugins collection.
endef


GST_VERSION:=1.0

CONFIGURE_ARGS += \
	--disable-examples \
	--disable-nls	\
	\
	--disable-directsound \
	--disable-directdraw \
	--disable-osx_video \
	--disable-quicktime \
	--disable-vcd \
	--disable-alsa \
	--disable-assrender \
	--disable-amrwb \
	--disable-apexsink \
	--disable-bz2 \
	--disable-cdaudio \
	--disable-celt \
	--disable-cog \
	--disable-dc1394 \
	--disable-directfb \
	--disable-dirac \
	--disable-dts \
	--disable-divx \
	--disable-resindvd \
	--disable-metadata \
	--disable-faac \
	--disable-fbdev \
	--disable-flite \
	--disable-gsm \
	--disable-jack \
	--disable-jp2k \
	--disable-kate \
	--disable-ladspa \
	--disable-lv2 \
	--disable-modplug \
	--disable-mimic \
	--disable-mpeg2enc \
	--disable-mplex \
	--disable-musepack \
	--disable-musicbrainz \
	--disable-mythtv \
	--disable-nas \
	--disable-neon \
	--disable-ofa \
	--disable-rsvg \
	--disable-timidity \
	--disable-wildmidi \
	--disable-sdl \
	--disable-sdltest \
	--disable-sndfile \
	--disable-soundtouch \
	--disable-spc \
	--disable-gme \
	--disable-swfdec \
	--disable-theoradec \
	--disable-xvid \
	--disable-dvb \
	--disable-wininet \
	--disable-acm \
	--disable-vdpau \
	--disable-schro \
	--disable-zbar \
	--disable-vp8 \
	--disable-librfb \
	\
	--without-libiconv-prefix \
	--without-libintl-prefix \
	--without-x \

EXTRA_LDFLAGS+= \
	-Wl,-rpath-link=$(STAGING_DIR)/usr/lib \

# Necessary as of Nov 2013, because the default Build command passes all the
# variables (like CC) to make, overriding the CC value of "gcc -std=c99" set by
# configure
define Build/Compile
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) \
		DESTDIR="$(PKG_INSTALL_DIR)"
endef

define Package/gst-plugins-ugly/install
	/bin/true
endef

# 1: short name
# 2: description
# 3: dependencies on other gstreamer libraries (short name)
# 4: dependencies on other packages
define GstBuildLibrary

  GST_DEPENDS += +libgst$(1)

  define Package/libgst$(1)
    $(call Package/gstreamer/Default)
    TITLE+= $(2) library (base)
    DEPENDS+= +libgstreamer $$(foreach p,$(3),+libgst$$(p)) $(4)
  endef

  define Package/libgst$(1)/description
   $(call Package/gstreamer/description/Default)
   .
   This package contains the GStreamer $(2) library.
  endef

  define Package/libgst$(1)/install
	$(INSTALL_DIR) $$(1)/usr/lib
	( cd $(PKG_INSTALL_DIR); $(CP) \
		./usr/lib/libgst$(1)-$(GST_VERSION).so.* \
		$$(1)/usr/lib/ \
	)
  endef

  $$(eval $$(call BuildPackage,libgst$(1)))
endef

$(eval $(call GstBuildLibrary,photography,photography,,))
$(eval $(call GstBuildLibrary,basecamerabinsrc,basecamerabinsrc,app,))

# 1: short name
# 2: description
# 3: dependencies on other gstreamer libraries (short name)
# 4: dependencies on other gstreamer plugins (short name)
# 5: dependencies on other packages
define GstBuildPlugin

  GST_DEPENDS += +gst-mod-$(1)

  define Package/gst-mod-$(1)
    $(call Package/gstreamer/Default)
    TITLE+= $(2) plugin (bad)
    DEPENDS+= +libgstreamer $$(foreach p,$(3),+libgst$$(p)) $$(foreach p,$(4),+gst-mod-$$(p)) $(5)
  endef

  define Package/gst-mod-$(1)/description
   $(call Package/gstreamer/description/Default)
   .
   This package contains the GStreamer $(2) plugin.
  endef

  define Package/gst-mod-$(1)/install
	$(INSTALL_DIR) $$(1)/usr/lib/gstreamer-$(GST_VERSION)
	( cd $(PKG_INSTALL_DIR); $(CP) \
		./usr/lib/gstreamer-$(GST_VERSION)/libgst$(1).so* \
		$$(1)/usr/lib/gstreamer-$(GST_VERSION)/ \
	)
  endef

  $$(eval $$(call BuildPackage,gst-mod-$(1)))
endef

# Plugins marked NONPORTED are not available as of 1.0.7, because they are on
# the GST_PLUGINS_NONPORTED list in configure.in.
$(eval $(call GstBuildPlugin,adpcmdec,adpcm decoding support,audio,,))
$(eval $(call GstBuildPlugin,adpcmenc,adpcm encoding support,audio,,))
#NONPORTED: $(eval $(call GstBuildPlugin,aiff,aiff support,tag,,))
$(eval $(call GstBuildPlugin,asfmux,asf muxing support,rtp,,))
$(eval $(call GstBuildPlugin,autoconvert,autoconvert support,,,))
$(eval $(call GstBuildPlugin,bayer,bayer support,video,,))
$(eval $(call GstBuildPlugin,camerabin2,camerabin support,app basecamerabinsrc pbutils photography tag,,))
#NONPORTED: $(eval $(call GstBuildPlugin,cdxaparse,cdxaparse support,riff,,))
$(eval $(call GstBuildPlugin,dataurisrc,dataurisrc support,,,))
#NONPORTED: $(eval $(call GstBuildPlugin,dccp,dccp support,,,))
$(eval $(call GstBuildPlugin,debugutilsbad,debugutils support,video,,))
$(eval $(call GstBuildPlugin,dvdspu,dvdspu support,video,,))
$(eval $(call GstBuildPlugin,faad,faad support,audio,,+libfaad2))
$(eval $(call GstBuildPlugin,festival,festival support,audio,,))
$(eval $(call GstBuildPlugin,frei0r,frei0r support,video,,))
#NONPORTED: $(eval $(call GstBuildPlugin,hdvparse,hdvparse support,,,))
$(eval $(call GstBuildPlugin,id3tag,id3tag support,tag,,))
$(eval $(call GstBuildPlugin,jpegformat,jpegformat support,tag,,))
$(eval $(call GstBuildPlugin,liveadder,liveadder support,audio,,))
$(eval $(call GstBuildPlugin,mms,mms support,,,+libmms))
$(eval $(call GstBuildPlugin,mpegpsdemux,mpegpsdemux support,tag,,+liboil +librt))
$(eval $(call GstBuildPlugin,mpegpsmux,mpegpsmux support,,,))
$(eval $(call GstBuildPlugin,mpegtsmux,mpegtsmux support,video,,))
#NONPORTED: $(eval $(call GstBuildPlugin,mve,mve support,,,))
#NONPORTED: $(eval $(call GstBuildPlugin,mxf,mxf support,video,,))
#NONPORTED: $(eval $(call GstBuildPlugin,nuvdemux,nuvdemux support,,,))
$(eval $(call GstBuildPlugin,pcapparse,pcapparse support,,,))
$(eval $(call GstBuildPlugin,pnm,pnm support,video,,))
$(eval $(call GstBuildPlugin,rawparse,rawparse support,audio video,,))
$(eval $(call GstBuildPlugin,rtpmux,rtpmux support,rtp,,))
$(eval $(call GstBuildPlugin,scaletempoplugin,scaletempo support,audio,,))
$(eval $(call GstBuildPlugin,sdpelem,sdp support,rtp sdp,,))
$(eval $(call GstBuildPlugin,segmentclip,segmentclip support,audio,,))
$(eval $(call GstBuildPlugin,siren,siren support,audio,,))
$(eval $(call GstBuildPlugin,speed,speed support,audio,,))
$(eval $(call GstBuildPlugin,subenc,subenc support,,,))
#NONPORTED: $(eval $(call GstBuildPlugin,stereo,stereo support,audio,,))
#NONPORTED: $(eval $(call GstBuildPlugin,tta,tta support,,,))
#NONPORTED: $(eval $(call GstBuildPlugin,videomeasure,videomeasure support,video,,))
#NONPORTED: $(eval $(call GstBuildPlugin,videosignal,videosignal support,video,,))
#NONPORTED: $(eval $(call GstBuildPlugin,vmnc,vmnc support,,,))

$(eval $(call BuildPackage,gst-plugins-bad))
