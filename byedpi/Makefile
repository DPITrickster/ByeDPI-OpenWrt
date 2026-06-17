include $(TOPDIR)/rules.mk

PKG_NAME:=byedpi
# Placeholders below are filled in by the CI "Prepare package" step via sed.
#   release:    VERSION=0.17.3  SOURCE_REF=v0.17.3  SOURCE_DIR=0.17.3
#   prerelease: VERSION=0.17.3  SOURCE_REF=<sha40>  SOURCE_DIR=<sha40>
# VERSION stays clean (no git suffix) in both modes to keep opkg/apk version
# strings valid; pre-releases are distinguished only by their GitHub release tag.
# GitHub strips the leading "v" from tag archives, so SOURCE_DIR has no "v".
PKG_VERSION:=@VERSION@
PKG_RELEASE:=1
PKG_MAINTAINER:=DPITrickster <DPITrickster@tutamail.com>

PKG_SOURCE:=@SOURCE_REF@.tar.gz
PKG_SOURCE_URL:=https://github.com/hufrea/byedpi/archive/
PKG_HASH:=skip

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-@SOURCE_DIR@
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/byedpi
	SECTION:=net
	CATEGORY:=Network
	TITLE:=ByeDPI
	URL:=https://github.com/hufrea/byedpi
endef

define Package/byedpi/description
	Local SOCKS proxy server to bypass DPI (Deep Packet Inspection)
endef

define Package/byedpi/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ciadpi $(1)/usr/bin

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/byedpi.init $(1)/etc/init.d/byedpi

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/byedpi.config $(1)/etc/config/byedpi
endef

$(eval $(call BuildPackage,byedpi))
