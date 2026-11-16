# dsh-url-router-win

**DSH 地址栏路由插件的 Windows 版。** 每个界面都有自己的链接 —— 会话、主面板、插件页与组件页、任务看板、设置 —— 整行就是真链接；桌面版把地址栏画进 **Windows 标题栏**（`#dsh-url-router-titlebar`），点一下全选，并列出最近用过的界面和粘贴过的链接。

> 这是 `dsh-url-router` 的**Windows 专用**构建。macOS 标题条那条路径（固定线、空段搜索、corner / 右栏夹取、重叠兜底、条模式）**不在这个包里** —— 那部分交给 [`dsh-topbar`](https://github.com/DDDMUC/dsh-topbar)。完整文档、路由契约、给 AI 用的清单接口，见原仓：<https://github.com/DDDMUC/dsh-url-router>。

## 安装

```sh
dsh plugin install dsh-url-router-win
# 或者在本仓目录里：
dsh plugin install .
```

装完**重启 DSH**（浏览器半区在页面加载时生效；桌面版要整体重启，刷新不算）。

## 和原版的关系

| | `dsh-url-router` | 本包 |
| --- | --- | --- |
| 路由 / 链接 / 最近 / 清单 / 电源命令 | ✓ | ✓（同一套） |
| Windows 标题栏地址栏 | ✓ | ✓ |
| macOS 标题条地址栏 | ✓ | ✗（这条路径已从本包移除） |
| 与 `dsh-topbar` 的共存协议 | 无 | 不需要（不碰 mac 标题条） |

**两个包不要同时装**：它们都声明自己是 fragment 的唯一写者，也都画同一个 `#dsh-url-router-titlebar`。同时启用会互相覆盖 URL，并且屏幕上会出现两个框。

## 它做什么

- **地址栏 = 当前界面**：`#/session/<id>`、`#/session/<id>/turn/<n>`、`#/panel/<panelId>`、`#/panel/plugins/<包名>/<组件>`、`#/settings/<section>/<tab>`、`#/power`、`#/act/…`。
- **整行链接**：侧栏会话行、插件行、任务行、设置节、动作项都是真 `<a>`，点击走路由，行内原始控件照常可用。
- **最近列表**：下拉里是最近用过的界面（面板页会写出是哪一页）和粘贴过的链接，条数在插件页里调。
- **宽度**：默认 700px，右侧握把可拖（拖动时画框、每帧一次写、握把 14px），长度按这条 strip 的比例记住，双击恢复。
- **给 AI 的清单**：`window.__dshUrlRouter.links()` 返回这一屏能链接到什么（`links` + `actions`，动作带 `safe` 标记）。

## 开发

无构建步骤，`lib/client.js` 既是源也是产物。

```sh
node --check lib/client.js
node --test "test/*.test.js"     # 215 个测试（mac 专属的 52 个已随路径移除）
```

演练台：`dev/harness/index.html?case=drag|inventory|search|messages|…`（headless Edge：`--headless=new --dump-dom`，读 `#report`）。

## 许可

MIT。上游：`dsh-url-router`（同作者）。
