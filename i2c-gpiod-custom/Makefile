#
# This is free software, licensed under the GNU General Public License v2. 
# See /LICENSE for more information. 
#

include $(TOPDIR)/rules.mk

PKG_NAME:=i2c-gpiod-custom
PKG_RELEASE:=1

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/i2c-gpiod-custom
  SUBMENU:=Other modules
  TITLE:=i2c-gpiod-custom driver with gpiod support
  FILES:=$(PKG_BUILD_DIR)/i2c-gpiod-custom.ko
  DEPENDS:=+kmod-i2c-gpio
endef

define KernelPackage/i2c-gpiod-custom/description
	i2c-gpiod-custom kernel module
endef

define Build/Prepare 
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./src/* $(PKG_BUILD_DIR)/
endef

MAKE_OPTS:= \
	ARCH="$(LINUX_KARCH)" \
	CROSS_COMPILE="$(TARGET_CROSS)" \
	M="$(PKG_BUILD_DIR)"

define Build/Compile 
	$(MAKE) -C "$(LINUX_DIR)" \
			$(MAKE_OPTS) \
			modules
endef

$(eval $(call KernelPackage,i2c-gpiod-custom))
