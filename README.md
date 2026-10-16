# INNO Machinery — 网站代码与部署备份

网站：https://innoplasticpyrolysis.com  
后台：https://innoplasticpyrolysis.com/admin

分类整理日期：2026-10-16。代码版本：9a92cc41245d529432eac94e221e76f418314c51（2026-10-14 已发布版本 9）。

## 分开保存的代码包

| 文件 | 内容 | 用途 |
| --- | --- | --- |
| [frontend-source.zip](frontend-source.zip) | 首页、公司介绍、产品、案例、联系我们；原始设备图片；样式、交互及默认公司资料 | 修改客户看到的网站 |
| [backend-source.zip](backend-source.zip) | 后台管理界面、登录验证、留言接口、内容及图片管理接口、数据库结构和迁移、后台使用说明 | 修改管理功能及服务端逻辑 |
| [shared-build-config.zip](shared-build-config.zip) | 前后台共用构建脚本、依赖锁定、测试、托管配置、环境变量示例 | 让前后台一起构建和部署 |

三个包保留原始目录路径。将它们解压到同一个空文件夹即可组合为可构建的源代码项目。不要将三个包分别部署：当前网站前后台共用一个 Worker，登录和留言需要服务端、数据库与对象存储。

## 完整备份（同一版本）

- [innoplasticpyrolysis-source.zip](innoplasticpyrolysis-source.zip)：完整源代码及当时的构建产物，保留用于恢复。
- [innoplasticpyrolysis-full-deployment.tar.gz](innoplasticpyrolysis-full-deployment.tar.gz)：已发布版本的完整部署包，包括前台、后台和数据库迁移。
- [innoplasticpyrolysis-frontend-assets.zip](innoplasticpyrolysis-frontend-assets.zip)：当前版本的静态资源备份，包含前台网页和后台界面资源。需要单独查看前台代码时，请使用上面的 frontend-source.zip。

## 代码与线上数据的区别

这里保存的是代码、默认内容和原始图片；不是线上数据库的完整备份。

客户留言、在后台修改的文字和联系方式、后来上传的图片，保存在运行中的数据库和对象存储里，不会自动写入这些压缩包。数据库结构文件只负责建表，不包含客户留言记录。

本公开仓库不保存生产密码、密码校验值、密钥、客户留言或管理员会话。线上内容及图片需要单独导出并保存在受限访问的位置。

## 以后怎样更新

以后通过开发修改前台或后台代码时，可以在同一次工作中：修改源代码、检查并构建、发布网站、更新本仓库中的分类代码包和完整部署包，并记录代码版本与日期。每次都需要实际提交成功；本仓库目前没有自动同步或自动部署。

在网站后台直接编辑内容不会触发 GitHub 更新。若需要这些线上数据的备份，需要另外配置数据库与图片存储备份；目前尚未配置该自动备份。

## 恢复部署

源代码环境：Node.js 22+。在组合后的源代码目录执行：

```sh
npm ci
npm run build
npm test
```

当前使用 Sites 托管，服务端为 Cloudflare Workers 兼容 ESM。数据库绑定为 DB，对象存储绑定为 BUCKET。迁移部署时需要配置存储、应用数据库迁移，并在托管平台配置 ADMIN_USERNAME 和 ADMIN_PASSWORD_HASH，生产凭据不包含在备份中。

这是 Worker 应用，不是普通 PHP 虚拟主机安装包。更换平台可能需要适配。仅将文件存入 GitHub 不会改变线上网站，也不会自动恢复线上留言和内容数据。

## Pixel 和 CAPI 恢复说明

本次备份包含 Pixel 页面访问统计及 CAPI 询盘 Lead 自动回传。像素编号：1406803950899656。2026-10-14 已在 Meta 测试事件中验证服务器 Lead 处理成功，随后移除测试代码并发布正式配置。

迁移时必须另行在服务器配置秘密变量 META_CAPI_ACCESS_TOKEN；访问口令不在这些备份内。生产环境不要设置 META_CAPI_TEST_EVENT_CODE。原有 ADMIN_USERNAME、ADMIN_PASSWORD_HASH 也需在服务器配置。所有数据库迁移（包括 capi_outbox）需要应用。

仅对勾选数据分享同意框的新询盘发送 Lead；不会回传历史留言。失败事件在后续网站访问时按退避间隔重试，没有独立定时任务。发送成功或超过六天后清除队列中的匹配数据。详细实现说明见后台包内 CAPI.md。

此次仅备份代码和部署文件，不包含线上留言、已编辑内容、后续上传图片或 CAPI 队列数据。