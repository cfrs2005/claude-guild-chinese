---
title: 自定义域名配置
description: GitHub Pages 域名配置指南
nav_exclude: true
---

# 自定义域名配置

要将您的域名指向 GitHub Pages，按以下步骤操作：

## 🌐 域名配置步骤

### 1. 在 GitHub 仓库中配置域名
```
1. 进入仓库 Settings
2. 找到 "Pages" 部分  
3. 在 "Custom domain" 输入您的域名，如：claude-code.yourdomain.com
4. 勾选 "Enforce HTTPS"
```

### 2. 在域名服务商配置 DNS
```
类型: CNAME
名称: claude-code (或您想要的子域名)
值: cfrs2005.github.io
TTL: 3600 (1小时)
```

### 3. 验证配置
```bash
# 等待 DNS 生效 (通常 10-30 分钟)
dig claude-code.yourdomain.com

# 应该显示指向 cfrs2005.github.io 的 CNAME 记录
```

## 📝 CNAME 文件创建

GitHub Pages 需要一个 CNAME 文件来识别自定义域名：