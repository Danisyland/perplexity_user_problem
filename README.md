# perplexity_user_problem
如何一次性导出所有对话记录


# 🚀 终极指南：零成本批量导出 Perplexity 聊天记录

**适用工具：** GitHub 开源项目 `perplexity-ai-export`
**核心痛点解决：** 突破了官方无法批量导出的限制，并解决了国内网络环境下的“3000ms 超时闪退”问题。

---

## 🛠️ 准备工作 (环境搭建)
1. 确保电脑已安装 **Node.js**。
2. 下载项目源码（通过 `git clone` 或直接下载 ZIP 压缩包并解压）。

---

## 📍 第一步：初始化与安装依赖
在项目文件夹（`perplexity-ai-export-master`）中，打开终端（PowerShell / CMD），依次运行以下三条命令：

```bash
# 1. 安装所需依赖包
npm install

# 2. 编译项目（将 TypeScript 转为可运行的 JavaScript）
npm run build

# 3. 安装 Playwright 自动化浏览器引擎
npx playwright install
```

---

## 🔑 第二步：获取网页 Cookie (身份令牌)
脚本需要你的登录状态才能抓取私有记录：
1. 打开 Edge/Chrome 浏览器，登录 Perplexity。
2. 按 `F12` 打开开发者工具，点击顶部标签页的 **“应用程序” (Application)**。
3. 在左侧边栏展开 **“Cookie”**，点击 `https://www.perplexity.ai`。
4. 在右侧列表中找到名称为 **`__Secure-next-auth.session-token`** 的项。
5. 双击复制它对应的超长字符串 **值 (Value)**。

---

## ⚙️ 第三步：配置 `.env` 文件 (核心设置)
在项目根目录找到 `.env.example` 文件，重命名为 `.env`（或新建一个）。用记事本打开，填入并修改为以下关键配置：

```env
# 基础配置
WAIT_MODE=static
RATE_LIMIT_MS=10000

# 浏览器模式（必须设为 false，方便随时手动过真人验证）
HEADLESS=false

# 填入你刚才复制的 Cookie (等号后直接贴，不要有空格)
PERPLEXITY_SESSION_TOKEN=eyJhbGciOiJkaXIiLCJlbmMiOiJ...

# 【关键防断连配置】填入你的本地科学上网代理端口（如 Clash 是 7890，v2ray 是 10808）
HTTP_PROXY=[http://127.0.0.1:7890](http://127.0.0.1:7890)
HTTPS_PROXY=[http://127.0.0.1:7890](http://127.0.0.1:7890)
```

---

## ⏱️ 第四步：修改源码，破解“致命 3 秒”超时限制 (防白屏闪退)
由于国内网络波动，默认的 3 秒等待时间极易导致浏览器直接崩溃。需要手动延长：
1. 打开项目文件夹，进入路径：`dist/scraper/`。
2. 用记事本打开 **`browser.js`** 文件。
3. 按 `Ctrl + F` 搜索数字 **`3000`**。
4. 找到类似 `timeout: 3000` 的代码，把它加上一个零，改为 **`30000`**（30秒）或 **`60000`**。
5. 保存并关闭文件。*(注：如果是较旧版本，可能还需要在 `dist/repl/commands.js` 中做同样修改)*。

---

## 🚜 第五步：启动收割机！
回到终端，输入启动命令：

```bash
npm start
```

1. 在出现的交互菜单中，按回车选择第一项：**`❯ Start scraper (Library)`**。
2. **盯住弹出的浏览器窗口**：如果出现 Cloudflare 的“请确认你不是机器人”复选框，立刻用鼠标点一下。
3. 验证通过后，终端会显示 `Already logged in!`，随后自动盘点所有的对话数量（如 29 threads），并开启多线程极速拉取。

---

## ✅ 第六步：验收与善后
* **提取文件**：看到终端提示 `✨ Export complete!` 后，打开项目里的 **`exports`** 文件夹，你所有的记录都已经变成了排版精美的 `.md` 文件。
* **清理云端**：前往 Perplexity 网页版 -> 左侧 `Library` -> 右上角 `...` -> 选择 `Delete all threads`，彻底清空云端隐私数据。

---



你可以直接复制上面的代码块。需要我为你提供一份将这些导出的 Markdown 文件高效整理进 Obsidian 或 Notion 的分类模板和建议吗？
