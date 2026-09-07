<p align="center">
  <a href="#">
    <img width="160" src="./src/static/logo.svg">
  </a>
</p>

<h1 align="center">炸了它 — 让负面情绪炸个干净</h1>

<div align="center">

![node version](https://img.shields.io/badge/node-%3E%3D20-green)
![pnpm version](https://img.shields.io/badge/pnpm-%3E%3D9-green)
![uni-app](https://img.shields.io/badge/uni--app-3.0-2b9939)
![Vue](https://img.shields.io/badge/Vue-3.4-42b883)
![TypeScript](https://img.shields.io/badge/TypeScript-5.8-3178c6)
![License](https://img.shields.io/badge/license-MIT-blue)

</div>

`炸了它`(仓库名 `chuqi-app`)是一款**解压类小程序**,主打「**1-3 分钟碎片时间的安全发泄**」。

打开小程序 → 看到出气包 → 疯狂点击 → 怒气值满 → 触发爆炸粒子特效 → 生成情绪海报分享。

不在倾诉,不在回避,把情绪直接炸掉。

---

## ✨ 核心特性

- **🎯 即时反馈**:单次点击 +2% 怒气值,15ms 短震 + 拟声词弹出,延迟 P95 < 50ms。
- **💥 粒子爆炸**:Canvas 2D 自研粒子系统,标准 200 / 低端机 60 粒子,爆炸特效 1.5s 后自动重置。
- **🎭 多对象切换**:沙袋 / 老板 / 作业本 / 前男友,每个对象有独立形变(挤压、脸红、卷边、心碎)。
- **🎨 情绪海报**:爆炸结束自动生成海报(语录 + 日期 + 小程序码),支持保存到相册 / 分享给好友。
- **🔇 完全可控**:震动、音效可单独关闭;所有用户数据仅本地匿名统计,不上传身份信息。
- **📊 北极星指标**:日均爆炸完成次数 —— 同时反映「核心价值交付」与「用户回访」。

## 🧠 产品理念

| 我们解决的问题 | 现有方案的不足 | 我们的差异化 |
|---------------|---------------|-------------|
| 负面情绪累积,但倾诉成本高、写日记反馈慢 | 捏泡泡纸 / 史莱姆缺少「爆发高潮」 | 「积累 → 爆发 → 释放」三段式闭环 |
| 压力常出现在通勤、课间 1-3 分钟碎片时间 | 解压玩具玩几次就腻 | 每次 1.5s 爆炸即可重启,无限循环 |
| 现代年轻人偏好匿名、私密 | 心理咨询门槛高 | 全程匿名,不出气对象为虚拟 Q 版形象 |

## 🛠️ 技术栈

| 类别 | 选型 |
|------|------|
| 跨端框架 | [uni-app](https://uniapp.dcloud.net.cn/) 3.0 |
| 前端框架 | Vue 3.4 + TypeScript 5.8 |
| 状态管理 | Pinia 2 + pinia-plugin-persistedstate |
| 构建工具 | Vite 5 |
| 脚手架 | [unibest](https://unibest.tech) 4.4.1 |
| UI 组件 | [wot-ui](https://wot-ui.cn) |
| 分页 | z-paging |
| HTTP | alova |
| 样式 | UnoCSS + SCSS |
| 后端 | 微信云开发(云函数 + 云数据库) |
| 测试 | vitest + @vue/test-utils |

## 📁 目录结构

```
src/
├── api/                    # 云函数客户端(getMiniProgramCode / textSecCheck / reportEvent)
├── components/             # 通用组件
│   └── PrivacyModal.vue    # 首次启动隐私政策弹窗
├── composables/            # 组合式函数(全局)
├── http/                   # HTTP 拦截与封装
├── layouts/                # 约定式布局
├── pages/
│   ├── index/              # 首页 —— 主交互(出气包 / 进度条 / 爆炸 / 海报)
│   │   ├── components/     # PunchBag / AngerBar / Onomatopoeia / ParticleExplosion / EmotionPoster / ObjectSwitcher
│   │   └── composables/    # useVibration / useAnger / useParticle
│   ├── settings/           # 设置页(震动/音效开关、清缓存、隐私政策)
│   ├── about/              # 关于页
│   └── me/                 # 我的页
├── pages-demo/             # 示例页(echarts 等)
├── service/                # 业务服务层
├── static/                 # 静态资源(logo / tabbar / 出气对象 PNG / 图标)
├── store/
│   └── angerStore.ts       # 怒气值 / 爆炸态 / 当前对象 / 设置 / 统计
├── style/                  # 全局样式
├── tabbar/                 # 自定义 tabBar
├── types/                  # 类型定义
├── utils/
│   ├── quotes.ts           # 30 条解压语录库
│   ├── audio.ts            # 音效播放(静音读 store.settings.sound)
│   ├── report.ts           # 埋点上报(本地队列 30s 批量)
│   └── device.ts           # 性能档位检测(standard / low)
└── App.vue / main.ts
```

## 🎮 核心数据流

```
用户点击 PunchBag
    ↓ emit('punch')
angerStore.punch()              ← state.angerValue += 2
    ↓
useVibration.vibrate(15ms)      ← 200ms 节流
useAnger 校验有效点击           ← 每 200ms 至多 1 次
Onomatopoeia 随机弹拟声词       ← 每 0.8-1.5s 一发
AngerBar 同步进度
    ↓ angerValue >= 100
「炸了它！」按钮显示 + 红光闪烁
    ↓ 用户点击
useParticle 启动爆炸 (1.5s)
angerStore.isExploding = true
    ↓ 爆炸结束
EmotionPoster 弹出 → 海报分享
angerStore.reset()              ← angerValue 归 0,可再次点击
```

## ⚙️ 环境要求

- **Node** >= 20
- **pnpm** >= 9
- **TypeScript** >= 5.0
- **微信开发者工具**(小程序调试)
- **微信云开发环境**(海报小程序码 + 内容安全 + 埋点上报)

## 🚀 快速开始

```bash
# 安装依赖
pnpm install

# 开发 —— H5
pnpm dev:h5              # 访问 http://localhost:9000

# 开发 —— 微信小程序(主目标平台)
pnpm dev:mp              # 用微信开发者工具导入 dist/dev/mp-weixin

# 开发 —— 字节 / 支付宝小程序
pnpm dev:mp-toutiao
pnpm dev:mp-alipay
```

## 📦 构建发布

```bash
pnpm build:h5            # 产物:dist/build/h5
pnpm build:mp            # 产物:dist/build/mp-weixin(通过微信开发者工具上传)
pnpm build:mp:prod       # 生产模式构建
```

> H5 部署到非根目录时,修改 `manifest.config.ts` 中 `h5.router.base`。

## 🧪 代码质量

```bash
pnpm type-check          # vue-tsc 严格类型检查
pnpm lint                # ESLint
pnpm lint:fix            # 自动修复
pnpm test                # vitest watch
pnpm test:run            # vitest 单次
```

## 🔌 OpenAPI 同步

后端云函数接口若有更新,使用 OpenAPI 同步请求/响应类型:

```bash
pnpm openapi             # 按 openapi-ts-request.config.ts 生成 src/api/ 类型
```

## 🗺️ 路线图

- [x] **V1.0**:点击出气 + 粒子爆炸 + 情绪海报(P0)
- [ ] **V1.0**:多对象切换(沙袋/老板/作业本/前男友)+ 设置页 + 隐私弹窗(P1)
- [ ] **V1.5**:激励视频广告(商业化,触发率稳定 ≥60% 时启动)
- [ ] **V2.0**:语音变声出气(P2)
- [ ] **V2.0**:每日情绪报告(P2)

## 📄 License

[MIT](./LICENSE) © 2025 chuqi-app

## 📚 关联文档

仓库内的产品与开发过程文档统一放在 [`docs/`](./docs) 目录下,作为产品与开发决策留痕:

- [`docs/炸了它产品需求文档V3.0.md`](./docs/炸了它产品需求文档V3.0.md) —— 当前最新版 PRD
- [`docs/炸了它技术方案.md`](./docs/炸了它技术方案.md) —— 架构与接口设计
- [`docs/炸了它PRD审查报告.md`](./docs/炸了它PRD审查报告.md) —— V2.0 → V3.0 的修订依据
- [`docs/炸了它开发任务清单-AI极简版.md`](./docs/炸了它开发任务清单-AI极简版.md) —— AI 全栈协作的执行路径
- [`docs/炸了它产品需求文档V2.0.md`](./docs/炸了它产品需求文档V2.0.md) —— 历史版本(V3.0 已替代,仅供溯源)