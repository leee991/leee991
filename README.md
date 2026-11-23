### 关于我

平时主要做后端开发、老项目重构和线上运维，Node.js、Go、Python、PHP、Java 这几个技术栈的线上系统都实际维护过。

这里整理了最近经手的 5 套线上项目的架构图、踩坑改造记录、接口定义和配置模板，方便自己复盘，也当作日常技术笔记（核心业务实现和线上配置没放上来）。

---

### 项目列表

| 项目 | 主要做什么的 | 用到的技术 | 重点改了些啥 |
|---|---|---|---|
| **[shenzhen-exchange-showcase](https://github.com/leee991/shenzhen-exchange-showcase)** | OneEx / Apexa1 交易平台（行情、合约、客服、防刷） | PHP (ThinkPHP)、Python、Node.js、MySQL、Redis、Docker、Nginx | 把 2 秒一次的 `docker exec` 刷行情改成了常驻缓存；用 Nginx + Python 写了 PoW 防刷校验；秒合约结算拆成独立进程 |
| **[u8-sports-showcase](https://github.com/leee991/u8-sports-showcase)** | U8 业务平台（会员账本、返水、客服、游戏接入） | Node.js 24 (`node:sqlite`)、Vite、Python、PHP、Docker、Nginx | 客服拆出去独立跑 SQLite，不碰财务库；做了个隔离的 EBPay 回调沙箱用来联调；拆了 60 多个财务和风控子模块 |
| **[japan-mall-showcase](https://github.com/leee991/japan-mall-showcase)** | TGShop 商城（PC/H5、商户后台、API） | Java 8、Tomcat 9、Spring、Dubbo、ZooKeeper、MySQL、Redis、Nginx | 把图片和静态 JS 从 Tomcat 剥出来走 Nginx 缓存直出；Nginx 层拦掉了被刷爆的评价接口；调了 JVM G1GC 和 Fastjson 安全模式 |
| **[guzhuan-lottery-showcase](https://github.com/leee991/guzhuan-lottery-showcase)** | 开奖站 + 独立 Go CMS + Telegram 查询机器人 | Node.js 24、Go 1.26、Python 3、SQLite WAL、Nginx | Node 主站从整库 JSON 迁到 SQLite WAL 并加上幂等防重；用 Go 单独写了内容管理后台（乐观锁、图片重编码、P95 监控）；零依赖 Python 机器人 |
| **[guzhuan-stock-showcase](https://github.com/leee991/guzhuan-stock-showcase)** | 股票行情与模拟交易平台 `tf-course` | Node.js ESM、SQLite、Nginx、Systemd | 在老站上加了增量融合后台（不动旧表，只加 `fusion_*` 表）；按单号前缀兼容新老持仓；把提现冻结和防负余额结算补齐了 |

---

### 平时常用的技术栈

- **后端**：Node.js（原生 ESM / `node:sqlite`）、Go、Python、PHP（ThinkPHP / FastAdmin）、Java（Spring / Dubbo / Tomcat）
- **数据**：MySQL、Redis、SQLite（WAL 模式）、ZooKeeper
- **运维与安全**：Nginx（缓存、限流、`auth_request`、Brotli）、Docker Compose、Systemd、基础排障与备份恢复

*最近整理时间：2026-11-23*
