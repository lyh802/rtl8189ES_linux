include $(TOPDIR)/rules.mk

PKG_NAME:=rtl8189es
PKG_RELEASE=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/lyh802/rtl8189ES_linux.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2021-10-30
PKG_SOURCE_VERSION:=4d62118debf0e42542f57863a145fa7f20c71ae8
PKG_MIRROR_HASH:=79eec4e5ed5485a2d894d92d997ac8565b06fa8fcaebb65ca551e44828a3dd0b

PKG_MAINTAINER:=Hauke Mehrtens <hauke@hauke-m.de>
PKG_BUILD_PARALLEL:=1
#PKG_EXTMOD_SUBDIRS:=rtl8189es

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8189es
  SUBMENU:=Wireless Drivers
  TITLE:=Realtek RTL8189ES / RTL8189ETV SDIO WiFi
  DEPENDS:=+kmod-cfg80211 +kmod-mmc +@DRIVER_11N_SUPPORT 
  FILES:=$(PKG_BUILD_DIR)/8189es.ko
#  AUTOLOAD:=$(call AutoProbe,rtl8189es)
  AUTOLOAD:=$(call AutoLoad,50,8189es)
endef

NOSTDINC_FLAGS = \
	-I$(PKG_BUILD_DIR) \
	-I$(PKG_BUILD_DIR)/include \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-include backport/backport.h

NOSTDINC_FLAGS+=-DCONFIG_LITTLE_ENDIAN
NOSTDINC_FLAGS+=-DCONFIG_CONCURRENT_MODE
NOSTDINC_FLAGS+=-DCONFIG_IOCTL_CFG80211 -DRTW_USE_CFG80211_STA_EVENT -DBUILD_OPENWRT

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(KERNEL_MAKE_FLAGS) \
		M="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		CONFIG_RTL8189ES=m \
		modules
endef

$(eval $(call KernelPackage,rtl8189es))
