---
title: puppeteer
tags: puppeteer
---

- Environment Variables
Puppeteer 寻找某些环境变量来帮助其操作。 如果 puppeteer 在环境中没有找到它们，这些变量的小写变体将从 npm 配置 中使用。
HTTP_PROXY, HTTPS_PROXY, NO_PROXY - 用于下载定义和运行 Chromium 的 HTTP 代理设置。
PUPPETEER_SKIP_CHROMIUM_DOWNLOAD - 请勿在安装步骤中下载绑定的 Chromium。
PUPPETEER_DOWNLOAD_HOST - 覆盖用于下载 Chromium 的 URL 的主机部分。
PUPPETEER_CHROMIUM_REVISION - 在安装步骤中指定一个你喜欢 puppeteer 使用的特定版本的 Chromium。
PUPPETEER_EXECUTABLE_PATH - 指定一个 Chrome 或者 Chromium 的可执行路径，会被用于 puppeteer.launch。 