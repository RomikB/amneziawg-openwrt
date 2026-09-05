include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=kmod-amneziawg
PKG_VERSION:=3.1.20260828
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/amnezia-vpn/amneziawg-linux-kernel-module/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=726cb8aee21181382b214bbea856e9f99a464605a99461b23ccc342aa6da1699

PKG_BUILD_DIR:=$(KERNEL_BUILD_DIR)/amneziawg-linux-kernel-module-$(PKG_VERSION)
MAKE_PATH:=src

include $(INCLUDE_DIR)/package.mk

define KernelPackage/amneziawg
	SUBMENU:=Network Support
	TITLE:=AmneziaWG VPN Kernel Module
	FILES:=$(PKG_BUILD_DIR)/$(MAKE_PATH)/amneziawg.ko
	DEPENDS:= \
		+kmod-udptunnel4 \
		+kmod-udptunnel6 \
		$(if $(call kernel_patchver_ge,5.6), \
			+kmod-crypto-lib-chacha20poly1305 \
			+kmod-crypto-lib-curve25519 \
		)
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	# Kernel sourcetree is only needed for Linux >= 5.6
	$(if $(call kernel_patchver_ge,5.6), \
		ln -sf $(LINUX_DIR) $(PKG_BUILD_DIR)/$(MAKE_PATH)/kernel; \
	)
endef

define Build/Compile
	$(MAKE_VARS) $(MAKE) -C "$(LINUX_DIR)" \
		$(KERNEL_MAKE_FLAGS) \
		M="$(PKG_BUILD_DIR)/$(MAKE_PATH)" \
		EXTRA_CFLAGS="$(BUILDFLAGS)" \
		WIREGUARD_VERSION="$(WIREGUARD_VERSION)" \
		modules
endef

$(eval $(call KernelPackage,amneziawg))
