include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=rtl8188eus
PKG_RELEASE:=1
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/aircrack-ng/rtl8188eus.git
PKG_SOURCE_VERSION:=master
PKG_SOURCE:=$(PKG_NAME)-$(PKG_SOURCE_VERSION).tar.xz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)

PKG_BUILD_DIR:=$(KERNEL_BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8188eus
  SUBMENU:=Wireless Drivers
  TITLE:=Realtek RTL8188EUS USB WiFi driver (aircrack-ng)
  DEPENDS:=@USB_SUPPORT +kmod-usb-core +kmod-cfg80211 +kmod-mac80211
  FILES:=$(PKG_BUILD_DIR)/rtl8188eus.ko
  AUTOLOAD:=$(call AutoProbe,rtl8188eus)
endef

define Build/Prepare
    $(call Build/Prepare/Default)
    # 修复 Makefile 以适配 OpenWrt
    $(SED) 's/KSRC ?= \/lib\/modules\/$$(shell uname -r)\/build/KSRC=$(LINUX_DIR)/g' $(PKG_BUILD_DIR)/Makefile
    $(SED) 's/KVER ?= $$(shell uname -r)/KVER=$(LINUX_VERSION)/g' $(PKG_BUILD_DIR)/Makefile
    # 设置平台为 PC 架构
    $(SED) 's/CONFIG_PLATFORM_I386_PC = n/CONFIG_PLATFORM_I386_PC = y/g' $(PKG_BUILD_DIR)/Makefile
endef

define Build/Compile
    $(MAKE) -C $(PKG_BUILD_DIR) \
        ARCH=$(LINUX_KARCH) \
        CROSS_COMPILE=$(TARGET_CROSS) \
        KSRC=$(LINUX_DIR) \
        KVER=$(LINUX_VERSION) \
        CONFIG_PLATFORM_I386_PC=y \
        USER_EXTRA_CFLAGS="-Wno-error=incompatible-pointer-types -Wno-error=address-of-packed-member -Wno-error=array-parameter"
endef

$(eval $(call KernelPackage,rtl8188eus))
