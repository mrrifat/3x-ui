[English](/README.md) | [فارسی](/README.fa_IR.md) | [العربية](/README.ar_EG.md) |  [中文](/README.zh_CN.md) | [Español](/README.es_ES.md) | [Русский](/README.ru_RU.md)

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./media/3x-ui-dark.png">
    <img alt="3x-ui" src="./media/3x-ui-light.png">
  </picture>
</p>

[![Release](https://img.shields.io/github/v/release/mrrifat/3x-ui.svg)](https://github.com/mrrifat/3x-ui/releases)
[![Downloads](https://img.shields.io/github/downloads/mrrifat/3x-ui/total.svg)](https://github.com/mrrifat/3x-ui/releases/latest)
[![License](https://img.shields.io/badge/license-GPL%20V3-blue.svg?longCache=true)](https://www.gnu.org/licenses/gpl-3.0.en.html)
[![Stars](https://img.shields.io/github/stars/mrrifat/3x-ui.svg)](https://github.com/mrrifat/3x-ui/stargazers)
[![Forks](https://img.shields.io/github/forks/mrrifat/3x-ui.svg)](https://github.com/mrrifat/3x-ui/network/members)

**3X-UI** — 一个基于网页的高级开源控制面板，专为管理 Xray-core 服务器而设计。它提供了用户友好的界面，用于配置和监控各种 VPN 和代理协议。

> [!IMPORTANT]
> 本项目仅用于个人使用和通信，请勿将其用于非法目的，请勿在生产环境中使用。

作为原始 X-UI 项目的增强版本，3X-UI 提供了更好的稳定性、更广泛的协议支持和额外的功能。

## 快速开始

```
bash <(curl -Ls https://raw.githubusercontent.com/mrrifat/3x-ui/main/install.sh)
```

完整文档请参阅:
- 📖 [中国优化指南](docs/CHINA-OPTIMIZATION.md) - 详细配置说明
- 📖 [原项目Wiki](https://github.com/MHSanaei/3x-ui/wiki) - 上游文档

## 🇨🇳 中国优化版特性

本仓库是 3x-ui 的个人优化分支,针对中国网络环境进行了深度优化:

✅ **智能分流 DNS** - 国内域名 10ms (AliDNS),国际域名 50ms (DoH加密)
✅ **自动下载中国 geo 文件** - 安装时自动配置
✅ **智能路由规则** - 国内流量直连,国际流量代理
✅ **协议模板** - 预配置 VLESS Reality, VMess/Trojan WS+TLS
✅ **零配置** - 开箱即用

基于 [MHSanaei/3x-ui](https://github.com/MHSanaei/3x-ui) 开发。

## 特别感谢

- [alireza0](https://github.com/alireza0/)

## 致谢

- [Iran v2ray rules](https://github.com/chocolate4u/Iran-v2ray-rules) (许可证: **GPL-3.0**): _增强的 v2ray/xray 和 v2ray/xray-clients 路由规则，内置伊朗域名，专注于安全性和广告拦截。_
- [Russia v2ray rules](https://github.com/runetfreedom/russia-v2ray-rules-dat) (许可证: **GPL-3.0**): _此仓库包含基于俄罗斯被阻止域名和地址数据自动更新的 V2Ray 路由规则。_

## 支持项目

**如果这个项目对您有帮助，您可以给它一个**:star2:

## 随时间变化的星标数

[![Stargazers over time](https://starchart.cc/mrrifat/3x-ui.svg?variant=adaptive)](https://starchart.cc/mrrifat/3x-ui) 
