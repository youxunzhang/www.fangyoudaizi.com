# 故障排除指南 - 重定向循环问题

## 🚨 问题描述

您遇到的 "ERR_TOO_MANY_REDIRECTS" 错误是由于重定向规则配置不当导致的循环重定向。

## 🔧 解决方案

### 1. 立即修复步骤

1. **清除浏览器缓存和Cookie**
   - 按 `Ctrl+Shift+Delete` (Windows) 或 `Cmd+Shift+Delete` (Mac)
   - 选择"所有时间"并清除所有数据
   - 重新访问网站

2. **使用无痕/隐私模式**
   - 打开浏览器的无痕模式
   - 访问网站测试是否正常

### 2. 重新部署网站

如果问题仍然存在，请重新部署网站：

1. **删除现有部署**
   - 登录 Cloudflare Dashboard
   - 进入 Pages 项目
   - 删除当前部署

2. **重新上传文件**
   - 确保使用修复后的文件
   - 重新部署项目

### 3. 验证文件配置

确保以下文件配置正确：

#### `_redirects` 文件
```
# Simplified redirects to avoid loops

# Redirect WordPress admin and login pages to home
/wp-admin/* / 301
/wp-login.php* / 301

# Redirect feed URLs to home
/feed / 301
/comments/feed / 301

# Redirect search to home
/?s=* / 301
/search/* / 301

# Redirect XML-RPC to home
/xmlrpc.php* / 301
/wp-json/* / 301

# Redirect other WordPress endpoints to home
/wp-* / 301
```

#### `wrangler.toml` 文件
```toml
name = "www-fangyoudaizi-com"
compatibility_date = "2024-01-01"

[build]
command = ""
publish = "."

[build.environment]
NODE_VERSION = "18"

# Simplified redirects to avoid loops
[[redirects]]
from = "/wp-admin/*"
to = "/"
status = 301

[[redirects]]
from = "/wp-login.php*"
to = "/"
status = 301

[[redirects]]
from = "/feed"
to = "/"
status = 301

[[redirects]]
from = "/comments/feed"
to = "/"
status = 301

[[redirects]]
from = "/search/*"
to = "/"
status = 301

[[redirects]]
from = "/xmlrpc.php*"
to = "/"
status = 301

[[redirects]]
from = "/wp-json/*"
to = "/"
status = 301
```

## 🔍 问题原因分析

### 导致循环重定向的常见原因：

1. **路径重定向循环**
   ```
   /page/* -> /page/$1 -> /page/* (循环)
   ```

2. **通配符匹配过宽**
   ```
   /* -> /$1 -> /* (循环)
   ```

3. **重定向链过长**
   ```
   /a -> /b -> /c -> /a (循环)
   ```

## ✅ 验证修复

修复后，网站应该：

1. **正常访问首页**
   - `https://your-domain.pages.dev/` 应该显示现代化网站

2. **WordPress旧链接重定向到首页**
   - `/wp-admin/` → 首页
   - `/wp-login.php` → 首页
   - `/feed` → 首页

3. **无循环重定向**
   - 浏览器不会显示重定向错误
   - 页面正常加载

## 🆘 如果问题仍然存在

### 临时解决方案

1. **完全移除重定向文件**
   - 删除 `_redirects` 文件
   - 删除 `wrangler.toml` 中的重定向配置
   - 重新部署

2. **使用Cloudflare Dashboard配置**
   - 在Cloudflare Dashboard中手动配置重定向规则
   - 避免使用文件配置

### 联系支持

如果问题仍然存在，请：

1. 检查 Cloudflare Pages 构建日志
2. 查看浏览器开发者工具的网络请求
3. 联系 Cloudflare 支持

## 📞 紧急联系

如果网站完全无法访问，请：

1. **立即删除重定向配置**
2. **重新部署基础文件**
3. **逐步添加重定向规则**

---

**重要提醒**：重定向循环是严重问题，会影响用户体验和SEO。请优先解决此问题再考虑其他功能。
