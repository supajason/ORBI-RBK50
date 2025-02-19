#
# Copyright (C) 2006-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

BASE_NAME:=openssl
PKG_NAME:=openssl-1.1.x
PKG_BASE:=1.1.1
PKG_BUGFIX:=m
PKG_VERSION:=$(PKG_BASE)$(PKG_BUGFIX)
PKG_RELEASE:=1
PKG_USE_MIPS16:=0
ENGINES_DIR=engines-1.1
PKG_BUILD_DIR := $(BUILD_DIR)/$(BASE_NAME)-$(PKG_VERSION)

PKG_BUILD_PARALLEL:=1

PKG_SOURCE:=$(BASE_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:= \
	http://ftp.fi.muni.cz/pub/openssl/source/ \
	http://ftp.linux.hr/pub/openssl/source/ \
	ftp://ftp.pca.dfn.de/pub/tools/net/openssl/source/ \
	http://www.openssl.org/source/ \
	http://www.openssl.org/source/old/$(PKG_BASE)/
PKG_MD5SUM:=8ec70f665c145c3103f6e330f538a9db

PKG_LICENSE:=OpenSSL
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Eneas U de Queiroz <cote2004-github@yahoo.com>
PKG_CPE_ID:=cpe:/a:openssl:openssl
PKG_CONFIG_DEPENDS:= \
	CONFIG_OPENSSL_11X_ENGINE \
	CONFIG_OPENSSL_11X_ENGINE_BUILTIN \
	CONFIG_OPENSSL_11X_ENGINE_BUILTIN_AFALG \
	CONFIG_OPENSSL_11X_ENGINE_BUILTIN_DEVCRYPTO \
	CONFIG_OPENSSL_11X_ENGINE_BUILTIN_PADLOCK \
	CONFIG_OPENSSL_11X_NO_DEPRECATED \
	CONFIG_OPENSSL_11X_OPTIMIZE_SPEED \
	CONFIG_OPENSSL_11X_PREFER_CHACHA_OVER_GCM \
	CONFIG_OPENSSL_11X_WITH_ARIA \
	CONFIG_OPENSSL_11X_WITH_ASM \
	CONFIG_OPENSSL_11X_WITH_ASYNC \
	CONFIG_OPENSSL_11X_WITH_BLAKE2 \
	CONFIG_OPENSSL_11X_WITH_CAMELLIA \
	CONFIG_OPENSSL_11X_WITH_CHACHA_POLY1305 \
	CONFIG_OPENSSL_11X_WITH_CMS \
	CONFIG_OPENSSL_11X_WITH_COMPRESSION \
	CONFIG_OPENSSL_11X_WITH_DTLS \
	CONFIG_OPENSSL_11X_WITH_EC \
	CONFIG_OPENSSL_11X_WITH_EC2M \
	CONFIG_OPENSSL_11X_WITH_ERROR_MESSAGES \
	CONFIG_OPENSSL_11X_WITH_GOST \
	CONFIG_OPENSSL_11X_WITH_IDEA \
	CONFIG_OPENSSL_11X_WITH_MDC2 \
	CONFIG_OPENSSL_11X_WITH_NPN \
	CONFIG_OPENSSL_11X_WITH_PSK \
	CONFIG_OPENSSL_11X_WITH_RFC3779 \
	CONFIG_OPENSSL_11X_WITH_SEED \
	CONFIG_OPENSSL_11X_WITH_SM234 \
	CONFIG_OPENSSL_11X_WITH_SRP \
	CONFIG_OPENSSL_11X_WITH_SSE2 \
	CONFIG_OPENSSL_11X_WITH_TLS13 \
	CONFIG_OPENSSL_11X_WITH_WHIRLPOOL

include $(INCLUDE_DIR)/package.mk

ifneq ($(CONFIG_CCACHE),)
HOSTCC=$(HOSTCC_NOCACHE)
HOSTCXX=$(HOSTCXX_NOCACHE)
endif

define Package/openssl-1.1.x/Default
  TITLE:=Open source SSL toolkit
  URL:=http://www.openssl.org/
  SECTION:=libs
  CATEGORY:=Libraries
endef

define Package/libopenssl-1.1.x/config
source "$(SOURCE)/Config.in"
endef

define Package/openssl-1.1.x/Default/description
The OpenSSL Project is a collaborative effort to develop a robust,
commercial-grade, full-featured, and Open Source toolkit implementing the
Transport Layer Security (TLS) protocol as well as a full-strength
general-purpose cryptography library.
endef

define Package/libopenssl-1.1.x
$(call Package/openssl-1.1.x/Default)
  SUBMENU:=SSL
  DEPENDS:=+OPENSSL_11X_WITH_COMPRESSION:zlib \
	   +OPENSSL_11X_ENGINE_BUILTIN_AFALG:kmod-crypto-user \
	   +OPENSSL_11X_ENGINE_BUILTIN_DEVCRYPTO:kmod-crypto-ocf-cryptodev \
	   +OPENSSL_11X_ENGINE_BUILTIN_PADLOCK:kmod-crypto-hw-padlock \
	   +libpthread
  TITLE+= (libraries)
  ABI_VERSION:=1.1
  MENU:=1
endef

define Package/libopenssl-1.1.x/description
$(call Package/openssl-1.1.x/Default/description)
This package contains the OpenSSL shared libraries, needed by other programs.
endef

define Package/openssl-util-1.1.x
  $(call Package/openssl-1.1.x/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=+libopenssl-1.1.x +libopenssl-conf-1.1.x
  TITLE+= (utility)
endef

define Package/openssl-util-1.1.x/description
$(call Package/openssl-1.1.x/Default/description)
This package contains the OpenSSL command-line utility.
endef

define Package/libopenssl-conf-1.1.x
  $(call Package/openssl-1.1.x/Default)
  SUBMENU:=SSL
  TITLE:=/etc/ssl/openssl.cnf config file
  DEPENDS:=libopenssl-1.1.x
endef

define Package/libopenssl-conf-1.1.x/conffiles
/etc/ssl/openssl.cnf
endef

define Package/libopenssl-conf-1.1.x/description
$(call Package/openssl-1.1.x/Default/description)
This package installs the OpenSSL configuration file /etc/ssl/openssl.cnf.
endef

define Package/libopenssl-afalg-1.1.x
  $(call Package/openssl-1.1.x/Default)
  SUBMENU:=SSL
  TITLE:=AFALG hardware acceleration engine
  DEPENDS:=libopenssl-1.1.x @OPENSSL_11X_ENGINE @KERNEL_AIO \
	   +PACKAGE_libopenssl-afalg-1.1.x:kmod-crypto-user +libopenssl-conf-1.1.x @!OPENSSL_11X_ENGINE_BUILTIN
endef

define Package/libopenssl-afalg-1.1.x/description
This package adds an engine that enables hardware acceleration
through the AF_ALG kernel interface.
To use it, you need to configure the engine in /etc/ssl/openssl.cnf
See https://www.openssl.org/docs/man1.1.1/man5/config.html#Engine-Configuration-Module
and https://openwrt.org/docs/techref/hardware/cryptographic.hardware.accelerators
The engine_id is "afalg"
endef

define Package/libopenssl-devcrypto-1.1.x
  $(call Package/openssl-1.1.x/Default)
  SUBMENU:=SSL
  TITLE:=/dev/crypto hardware acceleration engine
  DEPENDS:=libopenssl-1.1.x @OPENSSL_11X_ENGINE +PACKAGE_libopenssl-devcrypto-1.1.x:kmod-cryptodev +libopenssl-conf-1.1.x \
	   @!OPENSSL_11X_ENGINE_BUILTIN
endef

define Package/libopenssl-devcrypto-1.1.x/description
This package adds an engine that enables hardware acceleration
through the /dev/crypto kernel interface.
To use it, you need to configure the engine in /etc/ssl/openssl.cnf
See https://www.openssl.org/docs/man1.1.1/man5/config.html#Engine-Configuration-Module
and https://openwrt.org/docs/techref/hardware/cryptographic.hardware.accelerators
The engine_id is "devcrypto"
endef

define Package/libopenssl-padlock-1.1.x
  $(call Package/openssl-1.1.x/Default)
  SUBMENU:=SSL
  TITLE:=VIA Padlock hardware acceleration engine
  DEPENDS:=libopenssl-1.1.x @OPENSSL_11X_ENGINE @TARGET_x86 +PACKAGE_libopenssl-padlock-1.1.x:kmod-crypto-hw-padlock \
	   +libopenssl-conf-1.1.x @!OPENSSL_11X_ENGINE_BUILTIN
endef

define Package/libopenssl-padlock-1.1.x/description
This package adds an engine that enables VIA Padlock hardware acceleration.
To use it, you need to configure it in /etc/ssl/openssl.cnf.
See https://www.openssl.org/docs/man1.1.1/man5/config.html#Engine-Configuration-Module
and https://openwrt.org/docs/techref/hardware/cryptographic.hardware.accelerators
The engine_id is "padlock"
endef

OPENSSL_11X_OPTIONS:= shared

ifndef CONFIG_OPENSSL_11X_WITH_BLAKE2
  OPENSSL_11X_OPTIONS += no-blake2
endif

ifndef CONFIG_OPENSSL_11X_WITH_CHACHA_POLY1305
  OPENSSL_11X_OPTIONS += no-chacha no-poly1305
else
  ifdef CONFIG_OPENSSL_11X_PREFER_CHACHA_OVER_GCM
    OPENSSL_11X_OPTIONS += -DOPENSSL_PREFER_CHACHA_OVER_GCM
  endif
endif

ifndef CONFIG_OPENSSL_11X_WITH_ASYNC
  OPENSSL_11X_OPTIONS += no-async
endif

ifndef CONFIG_OPENSSL_11X_WITH_EC
  OPENSSL_11X_OPTIONS += no-ec
endif

ifndef CONFIG_OPENSSL_11X_WITH_EC2M
  OPENSSL_11X_OPTIONS += no-ec2m
endif

ifndef CONFIG_OPENSSL_11X_WITH_ERROR_MESSAGES
  OPENSSL_11X_OPTIONS += no-err
endif

ifndef CONFIG_OPENSSL_11X_WITH_TLS13
  OPENSSL_11X_OPTIONS += no-tls1_3
endif

ifndef CONFIG_OPENSSL_11X_WITH_ARIA
  OPENSSL_11X_OPTIONS += no-aria
endif

ifndef CONFIG_OPENSSL_11X_WITH_SM234
  OPENSSL_11X_OPTIONS += no-sm2 no-sm3 no-sm4
endif

ifndef CONFIG_OPENSSL_11X_WITH_CAMELLIA
  OPENSSL_11X_OPTIONS += no-camellia
endif

ifndef CONFIG_OPENSSL_11X_WITH_IDEA
  OPENSSL_11X_OPTIONS += no-idea
endif

ifndef CONFIG_OPENSSL_11X_WITH_SEED
  OPENSSL_11X_OPTIONS += no-seed
endif

ifndef CONFIG_OPENSSL_11X_WITH_MDC2
  OPENSSL_11X_OPTIONS += no-mdc2
endif

ifndef CONFIG_OPENSSL_11X_WITH_WHIRLPOOL
  OPENSSL_11X_OPTIONS += no-whirlpool
endif

ifndef CONFIG_OPENSSL_11X_WITH_CMS
  OPENSSL_11X_OPTIONS += no-cms
endif

ifndef CONFIG_OPENSSL_11X_WITH_RFC3779
  OPENSSL_11X_OPTIONS += no-rfc3779
endif

ifdef CONFIG_OPENSSL_11X_NO_DEPRECATED
  OPENSSL_11X_OPTIONS += no-deprecated
endif

ifeq ($(CONFIG_OPENSSL_11X_OPTIMIZE_SPEED),y)
  TARGET_CFLAGS := $(filter-out -O%,$(TARGET_CFLAGS)) -O3
else
  OPENSSL_11X_OPTIONS += -DOPENSSL_SMALL_FOOTPRINT
endif

ifdef CONFIG_OPENSSL_11X_ENGINE
  ifdef CONFIG_OPENSSL_11X_ENGINE_BUILTIN
    OPENSSL_11X_OPTIONS += disable-dynamic-engine
    ifndef CONFIG_OPENSSL_11X_ENGINE_BUILTIN_AFALG
      OPENSSL_11X_OPTIONS += no-afalgeng
    endif
    ifdef CONFIG_OPENSSL_11X_ENGINE_BUILTIN_DEVCRYPTO
      OPENSSL_11X_OPTIONS += enable-devcryptoeng
    endif
    ifndef CONFIG_OPENSSL_11X_ENGINE_BUILTIN_PADLOCK
      OPENSSL_11X_OPTIONS += no-hw-padlock
    endif
  else
    ifdef CONFIG_PACKAGE_libopenssl-devcrypto-1.1.x
      OPENSSL_11X_OPTIONS += enable-devcryptoeng
    endif
    ifndef CONFIG_PACKAGE_libopenssl-afalg-1.1.x
      OPENSSL_11X_OPTIONS += no-afalgeng
    endif
    ifndef CONFIG_PACKAGE_libopenssl-padlock-1.1.x
      OPENSSL_11X_OPTIONS += no-hw-padlock
    endif
  endif
else
  OPENSSL_11X_OPTIONS += no-engine
endif

ifndef CONFIG_OPENSSL_11X_WITH_GOST
  OPENSSL_11X_OPTIONS += no-gost
endif

ifndef CONFIG_OPENSSL_11X_WITH_DTLS
  OPENSSL_11X_OPTIONS += no-dtls
endif

ifdef CONFIG_OPENSSL_11X_WITH_COMPRESSION
  OPENSSL_11X_OPTIONS += zlib-dynamic
else
  OPENSSL_11X_OPTIONS += no-comp
endif

ifndef CONFIG_OPENSSL_11X_WITH_NPN
  OPENSSL_11X_OPTIONS += no-nextprotoneg
endif

ifndef CONFIG_OPENSSL_11X_WITH_PSK
  OPENSSL_11X_OPTIONS += no-psk
endif

ifndef CONFIG_OPENSSL_11X_WITH_SRP
  OPENSSL_11X_OPTIONS += no-srp
endif

ifndef CONFIG_OPENSSL_11X_WITH_ASM
  OPENSSL_11X_OPTIONS += no-asm
endif

ifdef CONFIG_i386
  ifndef CONFIG_OPENSSL_11X_WITH_SSE2
    OPENSSL_11X_OPTIONS += no-sse2
  endif
endif

OPENSSL_11X_TARGET:=linux-$(call qstrip,$(CONFIG_ARCH))-openwrt

STAMP_CONFIGURED := $(STAMP_CONFIGURED)_$(shell echo $(OPENSSL_11X_OPTIONS) | mkhash md5)

TARGET_CFLAGS := -I$(CURDIR)/include $(TARGET_CFLAGS)
TARGET_CFLAGS += $(FPIC) -ffunction-sections -fdata-sections
TARGET_LDFLAGS += -Wl,--gc-sections

define Build/Configure
	(cd $(PKG_BUILD_DIR); \
		./Configure $(OPENSSL_11X_TARGET) \
			--prefix=/usr \
			--libdir=lib \
			--openssldir=/etc/ssl \
			-D__ARM_MAX_ARCH__=7 \
			$(TARGET_CPPFLAGS) \
			$(TARGET_LDFLAGS) \
			$(OPENSSL_11X_OPTIONS) && \
		{ [ -f $(STAMP_CONFIGURED) ] || make clean; } \
	)
endef

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		CC="$(TARGET_CC)" \
		SOURCE_DATE_EPOCH=$(SOURCE_DATE_EPOCH) \
		OPENWRT_OPTIMIZATION_FLAGS="$(TARGET_CFLAGS)" \
		$(OPENSSL_MAKEFLAGS) \
		all
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		CC="$(TARGET_CC)" \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		$(OPENSSL_MAKEFLAGS) \
		install_sw install_ssldirs
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/openssl-1.1.x
	$(CP) $(PKG_INSTALL_DIR)/usr/include/openssl $(1)/usr/include/openssl-1.1.x/
	$(INSTALL_DIR) $(1)/usr/lib/openssl-1.1.x
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/lib{crypto,ssl}.{a,so*} $(1)/usr/lib/openssl-1.1.x/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/lib{crypto,ssl}.so.1.1 $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/openssl.pc $(1)/usr/lib/pkgconfig/openssl-1.1.x.pc
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libcrypto.pc $(1)/usr/lib/pkgconfig/libcrypto-1.1.x.pc
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libssl.pc $(1)/usr/lib/pkgconfig/libssl-1.1.x.pc
	[ -n "$(TARGET_LDFLAGS)" ] && $(SED) 's#$(TARGET_LDFLAGS)##g' $(1)/usr/lib/pkgconfig/{openssl-1.1.x,libcrypto-1.1.x,libssl-1.1.x}.pc || true
endef

define Package/libopenssl-1.1.x/install
	$(INSTALL_DIR) $(1)/etc/ssl/certs
	$(INSTALL_DIR) $(1)/etc/ssl/private
	chmod 0700 $(1)/etc/ssl/private
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/libcrypto.so.* $(1)/usr/lib/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/libssl.so.* $(1)/usr/lib/
	$(if $(CONFIG_OPENSSL_11X_ENGINE),$(INSTALL_DIR) $(1)/usr/lib/$(ENGINES_DIR))
endef

define Package/libopenssl-conf-1.1.x/install
	$(INSTALL_DIR) $(1)/etc/ssl
	$(CP) $(PKG_INSTALL_DIR)/etc/ssl/openssl.cnf $(1)/etc/ssl/
endef

define Package/openssl-util-1.1.x/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/openssl $(1)/usr/bin/openssl-1.1.x
endef

define Package/libopenssl-afalg-1.1.x/install
	$(INSTALL_DIR) $(1)/usr/lib/$(ENGINES_DIR)
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/$(ENGINES_DIR)/afalg.so $(1)/usr/lib/$(ENGINES_DIR)
endef

define Package/libopenssl-devcrypto-1.1.x/install
	$(INSTALL_DIR) $(1)/usr/lib/$(ENGINES_DIR)
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/$(ENGINES_DIR)/devcrypto.so $(1)/usr/lib/$(ENGINES_DIR)
endef

define Package/libopenssl-padlock-1.1.x/install
	$(INSTALL_DIR) $(1)/usr/lib/$(ENGINES_DIR)
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/$(ENGINES_DIR)/*padlock.so $(1)/usr/lib/$(ENGINES_DIR)
endef

$(eval $(call BuildPackage,libopenssl-1.1.x))
$(eval $(call BuildPackage,libopenssl-conf-1.1.x))
$(eval $(call BuildPackage,libopenssl-afalg-1.1.x))
$(eval $(call BuildPackage,libopenssl-devcrypto-1.1.x))
$(eval $(call BuildPackage,libopenssl-padlock-1.1.x))
$(eval $(call BuildPackage,openssl-util-1.1.x))
