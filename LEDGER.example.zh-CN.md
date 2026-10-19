# Agent Install Ledger · 安装台帐 —— 合成示例

> **本文数据全是编的**，只为演示字段格式。
> 复制走，删掉行，开始记你自己的。

| 名称 | 作用简述 | 安装日期 | 安装原因 | 关联项目 | 安装位置 | 占用空间 | 留存性质 | 可删除原因 | 卸载命令 | 状态 |
|---|---|---|---|---|---|---|---|---|---|---|
| Playwright (Python) | 浏览器自动化——用代码控制真实浏览器 | 2026-07-22 | 一次性报告的截图 | 季度报告 | `~/Library/Python/3.9/lib/python/site-packages/playwright` | 134 M | 临时 | 报告已交付，不再使用 | `pip uninstall playwright` | 在用 |
| Chromium 内核 | Playwright 实际驱动的浏览器（headless shell + ffmpeg） | 2026-07-22 | 同上 | 季度报告 | `~/Library/Caches/ms-playwright/` | 554 M | 临时 | 截图已完成——当前最大可回收项 | `python3 -m playwright uninstall --all` | 在用 |
| imageio-ffmpeg | 视频 / 帧处理 | 2026-07-22 | 延时视频合成 | 季度报告 | `…/imageio_ffmpeg/binaries/` | 73 M | 阶段性 | 视频交付后即可删 | `pip uninstall imageio-ffmpeg` | 在用（至 08-12） |
| pip 下载缓存 | pip 留的安装包副本，免得重复下载 | 累积 | 上面几次安装的副产物 | — | `~/Library/Caches/pip` | 113 M | 临时 | 只是下载缓存，删了不影响已装包 | `pip cache purge` | 可清理（待用户确认） |
| ffmpeg（静态二进制） | 把定时截图串成延时视频 | 2026-07-23 | 延时视频渲染 | 季度报告 | `~/.local/bin/ffmpeg` | 77 M | 阶段性 | 视频交付后即可删 | `trash ~/.local/bin/ffmpeg` | 在用 |
| poppler | PDF 命令行工具组（pdfinfo / pdftotext / pdftoppm） | 2026-08-21 | 简历 HTML→PDF 导出与质检管线 | 求职 | `/usr/local/Cellar/poppler/26.08.0` | ≈276 M（含同时新装的依赖树） | 长期 | — | `brew uninstall poppler && brew autoremove` | 在用 |
| 二维码生成（临时依赖） | 生成本域网测试用的二维码 PNG | 2026-08-11 | 用手机测一个页面 | 个人站点 | `/tmp/qr-gen` | 15 M | 临时 | 二维码 PNG 已生成 | `trash /tmp/qr-gen` | 已删除（2026-08-11） |

## 该看出什么

**1. 占用是量出来的，不是猜出来的。**
`du -sh` 胜过厂商写的下载体积。`poppler` 本体才 33 M，算上它拖进来的依赖树是 276 M。**那中间的差，才是硬盘真正被吃掉的地方。**

**2. 「可删除原因」是写给未来的你。**
「报告已交付」是原因。「不需要了」不是原因——那是半年后你会不同意的那句话。

**3. 「卸载命令」得是命令，不是描述。**
写不出确切命令，说明你还不知道怎么删掉它。**趁安装才十分钟，现在就去搞清楚。**

**4. 已删除的行要留着。**
`已删除（2026-08-11）` 加日期，是**复审真的发生过、决定真的被执行过**的痕迹。一份会悄悄丢行的台帐没法审计。

## 复审 log

```
2026-08-11  复审 7 项。用 du 逐项核对，两行已滞后（Chromium 已涨到 592 M）。
            删除：二维码生成临时依赖（15 M，成果物已产出）。保留：poppler 与
            ffmpeg（仍在用）。pip 缓存标记为可清理，等用户点头。
```

一次复审一行。日期、查了什么、删了什么、留了什么、什么还在等人拍板。
