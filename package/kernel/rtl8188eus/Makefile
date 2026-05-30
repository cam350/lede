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
    # 下载源码
    $(call Build/Prepare/Default)
    # 清理之前的编译产物
    $(MAKE) -C $(PKG_BUILD_DIR) clean || true
endef

define Build/Compile
    @echo "=== Starting compilation ==="
    @echo "Linux dir: $(LINUX_DIR)"
    @echo "Build dir: $(PKG_BUILD_DIR)"
    
    # 为内核6.6添加必要的编译标志
    $(MAKE) -C $(PKG_BUILD_DIR) \
        ARCH=$(LINUX_KARCH) \
        CROSS_COMPILE=$(TARGET_CROSS) \
        KSRC=$(LINUX_DIR) \
        KVER=$(LINUX_VERSION) \
        CONFIG_PLATFORM_I386_PC=y \
        CONFIG_POWER_SAVING=n \
        USER_EXTRA_CFLAGS="-Wno-error=incompatible-pointer-types -Wno-error=address-of-packed-member"
    
    @echo "=== Compilation finished ==="
    @ls -la $(PKG_BUILD_DIR)/*.ko 2>/dev/null || echo "No .ko file found"
endef

$(eval $(call KernelPackage,rtl8188eus))
