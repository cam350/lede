include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=rtl8188eus
PKG_RELEASE:=1

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
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./src/* $(PKG_BUILD_DIR)/
endef

define Build/Compile
	$(MAKE) -C $(LINUX_DIR) \
		ARCH=$(LINUX_KARCH) \
		CROSS_COMPILE=$(KERNEL_CROSS_COMPILE) \
		SUBDIRS=$(PKG_BUILD_DIR) \
		modules
endef

$(eval $(call KernelPackage,rtl8188eus))
