# 《炸了它》开发任务清单 —— AI 全栈极简版

- **场景**：1 名开发者 + AI 全栈开发（不分工、不开会、不评审）
- **基线**：unibest 模板已就绪（`C:\Users\26483\chuqi-app`）
- **目标**：以最少的 AI 轮次 + 最少的人为决策点，把 V1.0 干到提审
- **生成日期**：2026年9月7日
- **配套阅读**：原协作版任务清单 `炸了它开发任务清单V1.0.md`（保留作为依赖关系参考）

---

## 核心原则

1. **不再分 Phase / pd 估时**，改用「AI 轮次」与「人参与决策点」两个度量。
2. **凡能并行的就并行**：架构 + 后端 + 资源，可三路 AI 同时跑。
3. **凡能合并的就合并**：store + composables + utils + api 客户端 = 一个 AI 提示词。
4. **凡需要人拍板的就前置**：PRD 的 8 项待确认 + 3 项内部矛盾，必须在第一行代码前定稿。
5. **凡能用 AI 替代评审的就替代**：lint + type-check + vitest 充当 CI 门禁。

---

## 全局节奏

| 步骤 | 动作 | 谁做 | 估时 |
|---|---|---|---|
| **D0** | PRD 待确认 8 项 + 内部矛盾 3 项锁定 | 人 | 1 个下午 |
| **D1** | 后端云开发环境 + AI 出资源图 + AI 出 UI 设计稿 | 人开通 + AI 三路并行 | 1 天 |
| **D2** | 架构层 + 工具层 + API 客户端（一个 AI 提示词一次出） | AI | 1 个 AI 轮次 |
| **D3** | P0 核心交互：首页 + PunchBag + AngerBar + 点击逻辑 | AI | 1 个 AI 轮次 |
| **D4** | 粒子爆炸 + 拟声词 + 满值流程 | AI | 1 个 AI 轮次 |
| **D5** | 情绪海报 + 分享 + 埋点 | AI | 1 个 AI 轮次 |
| **D6** | P1：对象切换 + 设置页 + 隐私弹窗 | AI | 1 个 AI 轮次 |
| **D7** | 性能优化 + 低端机适配 + 包体积 | AI + 真机手动 | 1 天 |
| **D8** | 隐私政策文本 + 提审材料 + 提交审核 | 人 | 1 个下午 |
| **D9** | 内部 alpha（自己 + 邀请 10 个朋友） | 人 | 1 天 |
| **D10** | 灰度 beta（100 内测码）+ 数据看板 | 人监控 + AI 修 bug | 1 周观察 |
| **D11** | 全量 GA | 人一键 | 1 天 |

**总计**：**11 个步骤、约 8 个 AI 轮次 + 5 个人为决策点 + 1 周观察**。比协作版压缩 50% 以上。

---

## 第一刀：D0 —— PRD 锁定（必须的人为决策）

> AI 不能替你做这些决策。先把答案给 AI，它才能开干。

| 决策点 | 必须答案 | 来自 PRD |
|---|---|---|
| 后端是否从零搭建云开发 | 是 / 否 | §8.1 |
| 小程序码用微信 `getUnlimited` 还是第三方 | 选一个 | §8.2 API-1 |
| 6 个成功指标目标值（触发率 60%？D1 25%？） | 锁定数字 | §2.2 |
| 用户访谈结论 | 至少 2 句代表性原话 | §1.3 |
| 各里程碑负责人 | 自填「本人」即可 | §14.1 |
| 竞品均值数据（可空，标 [待补]） | 数字或「暂不引用」 | §13 |
| 语音变声最终方案（V1.1 才用，可现在定） | 本地 / 后端 | §5.4.1 |
| 隐私政策文本（可让 AI 起草，人审） | AI 草稿 + 人签字 | §11.2 |
| 震动默认值（V3.0 已定稿：默认开） | 已确认 | 矛盾 C1 |
| 粒子选型（V3.0 已定稿：Canvas 自研） | 已确认 | 矛盾 C3 |
| 情绪报告入口位置（V1.0 不放） | 已确认 | 矛盾 C2 |

**输出**：一份 `PRD-已确认决议.md`（10 行即可），后续所有 AI 提示词都以这个为真理来源。

---

## 第二刀：D1 —— 三路并行（环境 + 后端 + 资源）

> 三个互不依赖的任务，AI/人同时干。

### 路径 A：人 —— 后端云开发环境
- 注册小程序账号（个人主体即可）
- 微信开发者后台开通云开发，拿到环境 ID
- 在 `cloudfunctions/` 下创建三个占位函数

### 路径 B：AI —— 出气对象素材（PNG）
- 单条提示词：

```
为"炸了它"小程序生成 4 个 Q 版出气对象：沙袋、老板、作业本、前男友。
每个对象 3 帧表情：平静 / 受力（被击打瞬间） / 爆裂前。
要求：
- 圆头大眼卡通风格，统一橙色 #FF6B35 主色
- 尺寸 512×512，PNG 透明背景
- 文件命名：{对象名}-{状态}.png
- 适合小程序包体积，输出单张 < 50KB
```

### 路径 C：AI —— UI 设计稿
- 单条提示词：

```
为"炸了它"小程序出 Figma 设计稿（Figma Make / 即时设计 / MasterGo 均可）。
页面：①首页（含切换器、沙袋区、进度条、炸了它按钮）②设置页 ③情绪海报（含小程序码占位）。
视觉规范：
- 主色 #FF3B30 → 释放蓝 #4A90D9 渐变
- 背景 #1A1A1A，文字 #FFFFFF，高亮 #FFD700
- 圆角 24rpx
- 字号：标题 48rpx 粗体，按钮文案带感叹号
- 适配刘海屏/挖孔屏的安全区
```

### 路径 D：AI —— 音效资源
- 单条提示词：

```
合成 8 条短音效（MP3，各 ≤ 30KB）：
- 点击（啪）：5 条不同变体
- 满值提示：1 条（急促上扬）
- 爆炸：1 条（爆破+碎片感）
- 连击 50 次彩蛋：1 条（夸张欢呼）
要求：无版权风险，纯 CC0 风格。
```

**这一刀做完，工作区里就有 4 个对象 PNG + 一份设计稿 + 8 条音效 + 一个云开发环境 ID。**

---

## 第三刀：D2 —— 架构层一次性（1 个 AI 轮次）

> 把 PRD §9.2 模块图里所有非页面文件一次生成。

**单条 AI 提示词**：

```
项目根：C:\Users\26483\chuqi-app，技术栈 uni-app + Vue3 + TypeScript + Pinia + Vite + unibest。

请一次性创建以下文件（不要写页面，只写底层模块）：

1. src/store/angerStore.ts —— Pinia store，state 包含：
   - angerValue (number, 0-100)
   - isExploding (boolean)
   - currentObject ('sandbag'|'boss'|'homework'|'ex')
   - settings: { vibration: boolean (默认 true), sound: boolean (默认 true) }
   - stats: { todayExplode: number, totalClick: number, maxCombo: number, lastDate: string }
   actions: punch() / reset() / setSetting(key, value) / clearCache() / switchObject(type)
   用 pinia-plugin-persistedstate 持久化 settings（其他不持久化）

2. src/pages/index/composables/useVibration.ts —— 200ms 节流的 uni.vibrateShort 封装

4. src/pages/index/composables/useAnger.ts —— 单次+2%，每 200ms 最多 1 次有效点击，达到 100% 回调

5. src/pages/index/composables/useParticle.ts —— Canvas 2D 自研粒子系统，标准 200 / 低端 60

6. src/utils/quotes.ts —— 30 条语录库，提供 getRandomQuote()

7. src/utils/audio.ts —— play(name) / stop()，静音读 store.settings.sound

8. src/utils/report.ts —— report(event, payload)，本地队列 30s 批量上报

9. src/utils/device.ts —— detectPerformance() 返回 'standard' | 'low'

10. src/api/index.ts —— 三个云函数封装：
    - getMiniProgramCode(scene)
    - textSecCheck(content)
    - reportEvent(events[])
    统一错误处理，TS 类型完整

11. 配套单元测试（vitest）覆盖：useAnger 节流、angerStore 4 个 actions、quotes 长度。

完成后跑 pnpm type-check 与 pnpm test:run，必须零错误。
```

**这一步产出**：底层全部就绪。下一步开始堆 UI。

---

## 第四刀：D3 —— P0 核心交互（1 个 AI 轮次）

**单条 AI 提示词**：

```
基于已建好的 store/composables/utils，实现首页核心交互。

请创建/修改：

1. src/pages/index/index.vue —— 页面骨架：
   顶部：占位（出气对象切换器，先空着）+ 设置齿轮入口
   中部：<PunchBag /> 居中（占屏幕 40%）+ <canvas> 粒子层覆盖 + 拟声词浮动层
   底部：<AngerBar /> 进度条 + 「炸了它！」按钮（v-if angerValue >= 100 && !isExploding）

2. src/pages/index/components/PunchBag.vue —— props: objectType。
   内部状态：表情帧（idle/hit/critical）。
   点击触发：emit('punch') + scaleX 0.7 挤压 + 200ms 回弹。
   切换帧：受击后 200ms 切 hit，连续 5 次未爆切 critical。

3. src/pages/index/components/AngerBar.vue —— props: value (0-100)。
   红色渐变进度条 + 大号百分比数字；>= 95% 时数字变红闪烁。

4. 点击联通：
   PunchBag @punch → angerStore.punch() → 自动触发 useVibration + 拟声词 + AngerBar 同步

5. 满值流程：
   - 怒气值 >= 100 → 屏幕红光闪烁（CSS 动画 0.5s × 2）
   - 出现「炸了它！」按钮
   - 点击按钮 → angerStore.isExploding = true → 触发粒子爆炸

6. 拟声词浮动层 src/pages/index/components/Onomatopoeia.vue：
   每 0.8-1.5s 随机一个（啪/咻/砰/Duang），同屏最多 4 个，单个存活 1s，位置：左右各 20% 边缘区

7. src/pages/index/components/ParticleExplosion.vue —— 引用 useParticle，1.5s 持续。

完成后真机扫码验证 PRD §5.2.1 全部 6 条验收。
```

---

## 第五刀：D4 —— 海报 + 分享 + 埋点（1 个 AI 轮次）

**单条 AI 提示词**：

```
实现情绪海报 + 分享 + 9 个埋点事件。

请创建/修改：

1. src/pages/index/components/EmotionPoster.vue —— props: { qrcodeUrl, quote, date }。
   模态框：标题「今日怒气已清空！」+ 语录 + 日期 + 小程序码（qrcodeUrl 加载失败时降级为占位）。
   右上角关闭按钮（emit close）。

2. 海报保存到相册：
   - 用 canvas 把海报渲染成图片（canvasToTempFilePath）
   - 调用 uni.saveImageToPhotosAlbum
   - 未授权时降级为「仅显示海报」并弹授权引导

3. 海报分享好友：
   - 在 EmotionPoster 内用 <button open-type="share">
   - onShareAppMessage 返回海报图 + 「忍一时…退一步…但今天我选择炸掉它」

4. 9 个埋点事件接通 utils/report.ts：
   app_launch（onLaunch）、punch_click（每次有效点击）、anger_full（达 100%）、anger_explode（点炸按钮）、poster_show（海报弹出）、poster_save（保存）、poster_share（分享）、object_switch（P1 阶段再接）、settings_change（P1 阶段再接）

5. src/pages/settings/index.vue —— P1 占位（先放「建设中」），仅留路由。

完成后跑埋点字典表自检，确认 7 个事件（P0 部分）全部接通。
```

---

## 第六刀：D5 —— 后端云函数（1 个 AI 轮次）

> 这一刀可以放在 D1 后并行启动。

**单条 AI 提示词**：

```
在 C:\Users\26483\chuqi-app\cloudfunctions\ 下创建三个云函数（用微信云开发 Node 18）。

1. getMiniProgramCode/index.js —— 入参 { scene }，调用微信 getUnlimitedQRCode，
   返回 { qrcodeUrl, expireAt: now + 24h }，本地缓存到云数据库 scene_qrcodes。

2. textSecCheck/index.js —— 入参 { content, openid }，调用微信 msgSecCheck，
   返回 { pass: boolean }。

3. reportEvent/index.js —— 入参 { events: [{ name, payload, ts }] }，
   写入云数据库 event_logs 集合；同时按 name+date 聚合到 daily_stats。

每个函数：加注释、错误处理、参数校验、环境变量从云开发后台读取（不要硬编码）。
```

---

## 第七刀：D6 —— P1 增值（1 个 AI 轮次）

**单条 AI 提示词**：

```
基于已有 PunchBag 扩展 P1 功能。

1. 出气对象切换器 src/pages/index/components/ObjectSwitcher.vue：
   顶部横向滑动，4 个选项：沙袋 / 老板 / 作业本 / 前男友。
   切换时 200ms 过渡动画。
   点击 emit switch。

2. 老板对象（PunchBag 的差异化 props）：
   挤压时增加「脸红」红色遮罩渐显。
   作业本：纸张卷边 CSS + 轻微抖动。
   前男友：心碎粒子 + 抖动。
   （图片素材 D1 路径 B 已生成）

3. 完成设置页 src/pages/settings/index.vue：
   - 震动开关（读 angerStore.settings.vibration）
   - 音效开关（读 angerStore.settings.sound）
   - 清除缓存（弹确认对话框，调用 angerStore.clearCache()）
   - 隐私政策入口（web-view 打开隐私 URL）

4. 隐私政策弹窗 src/components/PrivacyModal.vue：
   - 首次启动检测本地标记，未同意则弹出
   - 同意后调用 report('privacy_accepted') 并持久化

5. 补 2 个埋点：object_switch（from/to）、settings_change（key/value）

完成后真机验证 PRD §5.3.1 与 §5.3.2 全部验收项。
```

---

## 第八刀：D7 —— 性能优化 + 低端机（人 + AI 协作）

| 子任务 | 谁做 |
|---|---|
| 真机调试 5 个机型（iPhone 8 / 15 / 小米千元 / 华为中端 / OPPO 千元） | 人手动扫码，记录卡顿机型 |
| 低端机粒子减量（200 → 60） | AI 根据报告改 useParticle |
| rAF 优化、Canvas 抗锯齿关闭 | AI |
| 包体积 ≤1.5MB 校验（rollup-plugin-visualizer） | AI |
| 启动时间 ≤2s | AI |
| 刘海屏/挖孔屏适配 | AI |

**单条 AI 提示词**：

```
根据真机测试报告（iPhone 8 fps 35、小米千元机 fps 40）做性能优化：

1. src/pages/index/composables/useParticle.ts：低端机粒子减到 60；关抗锯齿；rAF 改 setTimeout(16)（低端兼容性更好）。

2. 包体积：所有静态资源放 CDN（用现成的 unpkg 或七牛），本地不留图片。

3. 启动时间：pages.json 异步加载非关键页面；首页不引入 echarts。

4. CSS will-change: transform 仅在交互元素上加，不要全页加。

跑 pnpm build:mp-weixin 后用 visualizer 检查主包 ≤1.5MB，iPhone 8 真机 fps ≥ 55。
```

---

## 第九刀：D8 —— 提审材料（人为，必做）

| 子任务 | 谁做 | 估时 |
|---|---|---|
| 隐私政策文本（AI 起草 → 人审 → 法务可省） | 人审 | 30 min |
| 用户协议 | AI 起草，人审 | 20 min |
| 5 张截图（首页 / 爆炸中 / 海报 / 设置 / 对象切换） | AI 用截图工具生成 | 20 min |
| 微信开发者后台填类目（工具）、标签、简介 | 人 | 30 min |
| 提交审核 | 人一键 | 5 min |

**AI 提示词**：

```
为"炸了它"小程序撰写隐私政策（HTML）和用户协议（Markdown）。

隐私政策要点：
- 仅收集匿名行为统计（爆炸/点击/分享次数），不收集身份信息
- 不收集地理位置、不获取通讯录、不申请无关权限
- 用户可随时在设置页清除本地缓存
- 接入微信内容安全 API（msgSecCheck）
- 联系邮箱：[填]

用户协议要点：
- 工具类定位，不提供付费内容
- 出气对象为 Q 版虚拟形象，不针对任何真实个人
- 禁止用户上传自定义内容（V1.0）
```

---

## 第十刀：D9-D11 —— 灰度 & 放量

| 阶段 | 动作 | 谁做 |
|---|---|---|
| **D9 alpha** | 邀请 10 个朋友扫码体验，收集反馈 | 人 |
| **D10 beta** | 微信内测码 100 人，监控爆炸触发率/分享率/崩溃率 | 人监控 + AI 修 bug |
| **D11 GA** | 指标达标（触发率 ≥60%、崩溃率 <1%）→ 提交全量发布 | 人一键 |

**AI 辅助**：修 bug、写数据看板查询语句。

---

## 关键依赖（一图看清）

```
D0 (PRD 锁定) ─── 阻塞所有
  ↓
D1 三路并行：后端环境 / 资源 PNG / UI 设计稿 / 音效
  ↓
D2 架构层一次性（store/composables/utils/api）
  ↓                                ↓
D3 首页+点击+震动+拟声词     D5 云函数（可与 D2 同天）
  ↓
D4 海报+分享+埋点
  ↓
D6 P1（切换+设置+隐私）
  ↓
D7 性能优化
  ↓
D8 提审材料
  ↓
D9-D11 灰度放量
```

---

## 关键风险与单点

| 风险 | 应对 |
|---|---|
| AI 一次出整页代码容易埋坑 | 每刀结束都跑 `pnpm type-check && pnpm test:run`，AI 兜底 |
| 设计稿 AI 出的不够好看 | D1 先用 AI 出 3 版，选最好的；D3 再调整 |
| 粒子爆炸视觉不如预期 | D4 完成后人肉看效果；不行就让 AI 重写或换 WebGL |
| 微信审核被拒（涉「老板」） | 出气对象文案审核前置；老板文案改成「甲方」或「KPI」 |
| 云开发环境不开通 | 备选：先用 mock 数据演示，海报降级到无小程序码 |

---

## 不做的清单（明确砍掉）

- ❌ 多角色 PM/前端/后端/测试分工
- ❌ 站会、评审会、复盘会
- ❌ Sprint 估点、Velocity
- ❌ RICE 重新打分（这次只看依赖顺序）
- ❌ 任务编号 T-001 这种仪式感
- ❌ 「如果是我会这么干…」的过多扩展
- ✅ 只保留：**真阻塞的决策点 + AI 一次能干的活 + 必须人验收的边界**

---

## 一句话总结

> **8 个 AI 轮次 + 5 个拍板决策 + 1 周观察 = V1.0 全量上线。**

如果你愿意，我可以**立刻开始 D0**：帮你把 PRD 那 10 项决策点填成一份 `PRD-已确认决议.md`，作为所有后续 AI 提示词的真理来源。