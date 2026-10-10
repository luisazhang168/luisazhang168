# INNO Machinery website backup

Website: https://innoplasticpyrolysis.com

Admin: https://innoplasticpyrolysis.com/admin

Backup date: 2026-10-09. Source commit: `e65b8338dbe7a7f815269be21f316571fe42ea9c`.

## Files / 文件说明

- `innoplasticpyrolysis-source.zip`: 完整前台、后台源代码，数据库结构迁移、测试、依赖锁文件和构建脚本。解压后可继续开发。
- `innoplasticpyrolysis-frontend-assets.zip`: 英文前台页面、设备图片、样式及管理界面资源。留言和登录依赖后台，不能仅上传此包就获得完整功能。
- `innoplasticpyrolysis-full-deployment.tar.gz`: 已发布版本的完整部署包，同时包含前台、后台服务以及数据库迁移。后台不是独立静态网页，因此完整恢复请使用此包。

## Restore / 恢复部署

当前使用 Sites 托管，服务端为 Cloudflare Workers 兼容 ESM。逻辑数据库绑定为 `DB`，图片对象存储绑定为 `BUCKET`。部署时需由托管平台配置并应用数据库迁移。

管理员账号和密码校验值保存在托管环境变量 `ADMIN_USERNAME` 和 `ADMIN_PASSWORD_HASH` 中。备份不包含生产环境变量或密码，迁移到新环境时需要重新配置。不要把密码放进 GitHub。

Source build: Node.js 22+, `npm ci`, `npm run build`. Tests: `npm test`.

部署包不是普通虚拟主机的 PHP 安装包；如更换托管商，需要支持 Worker、数据库和对象存储，或另外做适配。

## Backup scope / 备份范围

这是源代码和部署文件备份，不包含客户留言、在线修改的内容、后续上传的图片、管理员登录会话或生产密钥。这些数据在运行中的数据库和对象存储中，需要另行导出备份。

把文件保存到 GitHub 不会自动修改或重新发布当前网站。
