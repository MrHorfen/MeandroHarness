---
name: "general-web-crawler"
description: "通用网页数据采集与爬虫专家，含浏览器自动化交互（已吸收 agent-browser）。支持静态页面、动态JS渲染、浏览器模拟操作（填表/点击/登录/截图）。触发：用户要「爬取数据」、「采集信息」、「操作网页」、「填表单」、「登录后操作」。"
---

# 通用爬虫专家

通用网页数据采集与爬虫专家，支持静态HTML、动态JS渲染、API接口、浏览器自动化交互等多种场景。

> **v2.0 合并说明**：已吸收 agent-browser。数据采集 + 浏览器自动化现在是一个 Skill，不再需要先登录再采集的串联流程。

## 适用场景

- 网页数据采集、爬虫、抓取任务
- 信息提取与解析（HTML/JSON/XML）
- 结构化数据导出（JSON/CSV/Markdown）
- 批量列表页/详情页爬取
- 动态JS渲染页面数据采集
- 浏览器自动化交互（填表、点击、登录、截图）
- 需要登录后操作的受保护页面

## 核心能力

### 1. 智能爬取策略
- 优先使用轻量级 HTTP 请求（`requests`）
- 动态页面自动切换至 Playwright 无头浏览器
- 根据页面复杂度选择最优方案

### 2. 反爬防护
- 随机 User-Agent 轮换
- 请求延时与随机间隔
- Cookie/Session 管理
- 禁用 JS 指纹采集
- 模拟人类浏览行为（滚动、点击、停留）

### 3. 数据解析
- CSS 选择器 / XPath / JSON 解析
- 自动清洗脏数据（空格、换行、HTML 实体）
- 支持正则表达式高级提取

### 4. 效率优化
- 异步并发请求（`aiohttp`）
- 分页自动识别与遍历
- 断点续爬支持
- 请求缓存避免重复

### 5. 合规安全
- 遵守 `robots.txt` 规则
- 限制爬取频率，避免对目标服务器造成压力
- 不爬取敏感数据（密码、个人信息等）

### 6. 输出格式
- 自动导出 JSON / CSV / Markdown
- 支持结构化数据表格输出
- 数据去重与格式标准化

### 7. 浏览器自动化交互（原 agent-browser）

当需要模拟用户操作时启用：

- **页面交互**：点击元素、填写表单、选择下拉选项、上传文件、拖拽、键盘输入
- **登录态维持**：自动登录 → Cookie/Session 管理 → 操作受保护页面
- **等待同步**：等待元素出现/可点击、等待网络请求完成、等待页面加载
- **截图录制**：全页面截图、元素截图、录制操作视频、生成 PDF
- **弹窗处理**：处理 alert/confirm/prompt 对话框、模态框

**典型流程**：登录 → 获取 Cookie → 批量爬取，无需串联两个 Skill。

## 执行步骤

### 第一步：解析任务
- 提取目标 URL
- 明确需要爬取的字段列表
- 确认输出格式要求（JSON/CSV/Markdown）

### 第二步：策略判断
- 判断页面类型（静态 HTML / 动态 JS 渲染 / API 接口）
- 生成最优爬取方案
- 确定反爬策略

### 第三步：发起请求
- 携带合规请求头（User-Agent、Referer、Accept 等）
- 设置超时与重试机制
- Cookie/Session 维持登录态

### 第四步：解析数据
- 使用 CSS 选择器或 XPath 精准定位目标元素
- JSON 路径提取结构化数据
- 自动去重与数据清洗

### 第五步：结果输出
- 按指定格式导出数据
- 返回结构化结果给主 AGENT
- 生成数据摘要与统计

### 第六步：异常处理
- 网络失败自动重试（最多 3 次）
- 页面结构变更自动适配提醒
- 超时降级处理

## 依赖工具

```python
# 基础请求
requests             # HTTP 请求库
beautifulsoup4       # HTML 解析库
lxml                 # XPath / XML 解析

# 动态页面
playwright           # 无头浏览器自动化

# 异步并发
aiohttp              # 异步 HTTP 请求

# 数据导出
csv                  # CSV 格式导出
json                 # JSON 格式导出
```

## 权限要求

- **网络请求权限**：访问目标网站
- **浏览器自动化权限**：Playwright 无头浏览器执行
- **文件读写权限**：数据导出到本地文件
- **终端执行权限**：安装依赖库和执行爬虫脚本

## 使用示例

```python
# 示例：爬取静态页面数据
import requests
from bs4 import BeautifulSoup

url = "https://example.com/data"
headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"}
response = requests.get(url, headers=headers, timeout=10)
soup = BeautifulSoup(response.text, "lxml")
# 提取数据逻辑...

# 示例：爬取动态页面数据
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    page = browser.new_page()
    page.goto("https://example.com/dynamic")
    page.wait_for_selector(".data-loaded")
    content = page.content()
    browser.close()
```

## 优先级

高 —— 网页数据采集类任务优先匹配此技能。