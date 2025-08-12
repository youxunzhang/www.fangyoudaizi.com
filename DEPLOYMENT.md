# 部署说明 - www.fangyoudaizi.com

## 问题解决

您遇到的404错误是因为Cloudflare Pages的域名配置问题。以下是解决方案：

## 部署步骤

### 1. 使用Cloudflare Pages部署

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com)
2. 进入 "Pages" 部分
3. 点击 "Create a project"
4. 选择 "Connect to Git" 或 "Direct Upload"
5. 如果使用Git：
   - 连接您的GitHub/GitLab仓库
   - 设置构建命令：留空（因为这是静态文件）
   - 设置发布目录：`.` (根目录)
6. 如果使用Direct Upload：
   - 直接上传所有文件

### 2. 自定义域名设置

1. 在Pages项目设置中，进入 "Custom domains"
2. 添加您的域名：`www.fangyoudaizi.com`
3. 按照Cloudflare的DNS设置说明配置DNS记录

### 3. 环境变量设置（可选）

在Pages项目设置中添加以下环境变量：
- `NODE_VERSION`: `18`

## 文件说明

- `_redirects`: 处理URL重定向规则
- `wrangler.toml`: Cloudflare Pages配置文件
- `index.htm`: 网站首页
- 所有其他文件都是WordPress静态导出的内容

## 常见问题

### 404错误
- 确保所有文件都已上传
- 检查 `_redirects` 文件是否正确
- 验证域名DNS设置

### 样式问题
- 确保所有CSS和JS文件路径正确
- 检查图片文件是否完整

## 技术支持

如果仍有问题，请检查：
1. Cloudflare Pages的构建日志
2. 浏览器开发者工具的网络请求
3. 确保所有静态资源都能正常加载
