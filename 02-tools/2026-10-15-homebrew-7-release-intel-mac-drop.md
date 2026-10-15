# Homebrew 7.0 发布：速度翻倍，但告别 Intel Mac（极客老哥的抽屉）

- **日期**：2026-10-15
- **来源**：微信公众号「极客老哥的抽屉」
- **分类**：02 工具与产品
- **原文**：https://mp.weixin.qq.com/s/e5Qv7rn22ciw5akStgBmzg

## 概述

Homebrew 7.0.0 发布。两大关键词：**快**（安装并发化、日常命令更敏捷）和**狠**（Intel Mac 降为 Tier 3，2027-09-01 完全移除；macOS 10.15 及更早直接移除）。另新增内置漏洞扫描 `brew vulns` 与官方 GUI BrewUI。

## 关键内容

### 性能
- **批量安装**：`brew install A B C` 由排队安装改为「并行下载，智能排队安装」（官方说法 greater concurrency，未给具体倍数）
- **命令响应**：`brew config`、`brew update` 等启动和响应更快，减少后台进程等待
- **清理缓存**：`brew cleanup` 不再反复扫描缓存，包多时提升明显

### 安全
- 内置基于 **OSV.dev** 的漏洞数据库，`brew vulns` 扫描已安装软件的已知漏洞

### 不兼容变化（避坑）
| 变化 | 影响 |
|---|---|
| Intel Mac 降为 Tier 3 | 不再构建预编译 Bottles，新装软件多需源码编译；**2027-09-01 完全移除** |
| 移除 macOS 10.15 (Catalina) 及更早 | 需升级到 macOS 11 (Big Sur)+ |
| Linux 沙盒 Bubblewrap → **Landlock** | 需内核 6.1+，旧内核只能退回不那么安全的旧模式 |
| Formula 中 Ruby 代码块 `post_install` 废弃 | 改为声明式步骤；老写法警告，2027 年底失效 |

### 上手指南
- 升级：`brew update` 自动升到 7.0.0
- 安全体检：`brew vulns`
- GUI：`brew install homebrew-app` 安装官方图形界面 BrewUI
- Intel Mac 用户：考虑迁移到 Apple Silicon 或换 MacPorts

## 备注
- Apple 与 GitHub 都在放弃 Intel，Homebrew 作为志愿者项目跟进。
- 对本人环境：McCleary（RHEL8，内核 4.18）若用 Linuxbrew，Landlock 沙盒不可用，会走旧模式。
- 数据来自博主转述，具体以 Homebrew 官方 release notes 为准，未独立核实。
