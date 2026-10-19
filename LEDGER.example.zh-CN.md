# Agent Install Ledger · 安装台帐 —— 合成示例

> **本文数据全是编的**，只为演示字段格式。
> 复制走，删掉行，开始记你自己的。

| 名称 | 作用简述 | 安装日期 | 安装原因 | 关联项目 | 安装位置 | 占用空间 | 留存性质 | 可删除原因 | 卸载命令 | 状态 |
|---|---|---|---|---|---|---|---|---|---|---|
| Playwright (Python) | 浏览器自动化——用代码控制真实浏览器 | 2025-11-04 | 一次性报告的截图 | 季度报告 | `~/Library/Python/3.9/lib/python/site-packages/playwright` | 121 M | 临时 | 报告已交付，不再使用 | `pip uninstall playwright` | 在用 |
| Chromium 内核 | Playwright 实际驱动的浏览器（headless shell + ffmpeg） | 2025-11-04 | 同上 | 季度报告 | `~/Library/Caches/ms-playwright/` | 487 M | 临时 | 截图已完成——当前最大可回收项 | `python3 -m playwright uninstall --all` | 在用 |
| imageio-ffmpeg | 视频 / 帧处理 | 2025-11-04 | 延时视频合成 | 季度报告 | `…/imageio_ffmpeg/binaries/` | 68 M | 阶段性 | 视频交付后即可删 | `pip uninstall imageio-ffmpeg` | 在用 |
| pip 下载缓存 | pip 留的安装包副本，免得重复下载 | — | 上面几次安装的副产物 | — | `~/Library/Caches/pip` | 96 M | 临时 | 只是下载缓存，删了不影响已装包 | `pip cache purge` | 可清理（待用户确认） |
| ffmpeg（静态二进制） | 把定时截图串成延时视频 | 2025-11-05 | 延时视频渲染 | 季度报告 | `~/.local/bin/ffmpeg` | 71 M | 阶段性 | 视频交付后即可删 | `trash ~/.local/bin/ffmpeg` | 在用 |
| poppler | PDF 命令行工具组（pdfinfo / pdftotext / pdftoppm） | 2026-01-14 | 简历 HTML→PDF 导出与质检管线 | 求职 | `/usr/local/Cellar/poppler/25.10.0` | ≈241 M——本体 22 M，其余是它拖进来的依赖树 | 长期 | — | `brew uninstall poppler && brew autoremove` | 在用 |
| 二维码生成（临时依赖） | 生成本域网测试用的二维码 PNG | 2025-12-02 | 用手机测一个页面 | 个人站点 | `/tmp/qr-gen` | 9 M | 临时 | 二维码 PNG 已生成 | `trash /tmp/qr-gen` | 已删除（2025-12-02） |

> 表中 `trash` 是 macOS 15（Sequoia）起内置的命令。更早的 macOS 或 Linux 上用你平台的等价物，比如 `gio trash`。

## 该看出什么

**1. 占用是量出来的，不是猜出来的——而且要分三处量。**
对安装路径跑 `du -sh` 只拿到**本体**：`poppler` 本体 22 M。它拖进来的东西在别处——依赖树，加上它自己的缓存目录。把这些加起来才是 ≈241 M。**单跑一次 `du` 永远不会报出那个数**，而中间这个差，才是硬盘真正被吃掉的地方。

**2. 「可删除原因」是写给未来的你。**
「报告已交付」是原因。「不需要了」不是原因——那是半年后你会不同意的那句话。

**3. 「卸载命令」得是命令，不是描述。**
写不出确切命令，说明你还不知道怎么删掉它。**趁安装才十分钟，现在就去搞清楚。**

**4. 已删除的行要留着。**
`已删除（2025-12-02）` 加日期，是**复审真的发生过、决定真的被执行过**的痕迹。一份会悄悄丢行的台帐没法审计。

## 复审 log

```
2025-12-02  复审 7 项。用 du 逐项核对，两行已滞后（Chromium 已涨到 519 M）。
            删除：二维码生成临时依赖（9 M，成果物已产出）。保留：poppler 与
            ffmpeg（仍在用）。pip 缓存标记为可清理，等用户点头。
```

一次复审一行。日期、查了什么、删了什么、留了什么、什么还在等人拍板。
