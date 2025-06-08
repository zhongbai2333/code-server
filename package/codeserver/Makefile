include $(TOPDIR)/rules.mk

PKG_NAME:=codeserver
PKG_VERSION:=4.100.3
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/zhongbai2333/code-server.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=409c64e0df4d53530e59c16acc2b5d5766f717b0

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=lang
	CATEGORY:=Development
	TITLE:=code-server Web IDE
	MENU:=1
	DEPENDS:=+node +node-npm
endef

define Build/Prepare
	$(call Build/Prepare/Default)
endef

define Build/Compile
	cd $(PKG_BUILD_DIR) && \
	# 清理缓存与锁文件后，仅安装 production 依赖
	npm cache clean --force && \
	npm cache verify && \
	rm -rf node_modules package-lock.json npm-shrinkwrap.json && \
	npm ci --only=production --unsafe-perm && \
	npm run build
endef


define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/lib/code-server
	$(CP) $(PKG_BUILD_DIR)/out/* $(1)/usr/lib/code-server/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/out/bin/code-server $(1)/usr/bin/code-server
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
