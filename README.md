# 🌐 漫游实验室 RoamLab - 完整部署指南

> 全球漫游 · 自由生长

## 📦 文件清单

```
├── index.html           ← 主网站（前端）
├── admin.html           ← 管理后台（需登录）
├── login.html           ← 登录页面
├── hero-cyber.mp4       ← 赛博朋克视频背景
├── hero-satellite.mp4   ← 卫星动画
├── hero-connect.mp4     ← 连接线动画
└── README.md            ← 本文件
```

---

## 🚀 快速部署

### 方案 1：Cloudflare Pages（推荐）

1. **创建 GitHub 仓库**
   - 新建 GitHub repo，命名为 `roamlab`
   - 将所有文件上传到仓库根目录

2. **连接到 Cloudflare Pages**
   - 登录 [Cloudflare Dashboard](https://dash.cloudflare.com)
   - 左侧菜单 → Pages → Connect to Git
   - 选择你的 GitHub 账户和 `roamlab` 仓库
   - Build command: **留空**
   - Build output directory: `/`
   - Environment variables: 不需要
   - Deploy 即可

3. **自定义域名**（可选）
   - Pages 会给你一个 `*.pages.dev` 的域名
   - 在 Custom domain 中输入你自己的域名（如 roamlab.com）
   - 按照指示更改 DNS 记录

---

## 🔐 后台管理系统

### 登录凭证（演示）
- **URL**: `/login.html`
- **邮箱**: `admin@roamlab.com`
- **密码**: `roamlab2026`

### 功能模块

#### 📊 仪表板
- 快速查看数据统计
- 快速操作按钮

#### ⚙️ 站点设置
- 编辑网站标题、描述、标语
- 配置社交媒体链接（Telegram、Twitter、Email）
- 修改统计数据（覆盖国家数、套餐数等）
- **导出/导入数据**（备份功能）

#### 📝 文章管理
- 发布新文章
- 分类：电话卡指南、金融工具、数字游民、选城攻略
- 添加标签
- 编辑和删除

#### 📡 电话卡库
- 添加电话卡套餐
- 记录：国家、供应商、类型、价格、更新日期
- 前端自动读取

#### 🏦 金融工具
- 添加金融工具（汇款、虚拟卡、数字银行）
- 记录：工具名、类型、支持国家、评分
- 前端动态显示

#### 🗺️ 地区数据
- 添加覆盖地区
- 记录：国旗、地区名、推荐电话卡、推荐金融工具

#### 📈 数据分析
- 预留 Google Analytics 集成位置
- 连接后可查看访问量、用户数等

---

## 💾 数据存储说明

### 本地存储（LocalStorage）
所有数据保存在浏览器的 LocalStorage 中，**仅在登录设备有效**

```javascript
localStorage.getItem('roamlab_data')  // 所有管理数据
localStorage.getItem('admin_token')   // 登录状态
```

### 数据结构
```json
{
  "articles": [
    {
      "id": 1234567890,
      "title": "日本东京必备的3张电话卡",
      "category": "电话卡指南",
      "content": "...",
      "tags": "eSIM, 实体卡, 日本",
      "date": "2026/5/27"
    }
  ],
  "simCards": [...],
  "financeTools": [...],
  "regions": [...],
  "settings": {
    "title": "漫游实验室 RoamLab",
    "desc": "全球连接，自由生长",
    "countries": "50",
    "sim": "200",
    "finance": "30"
  }
}
```

---

## 🔄 前后端数据流

### 流程
1. **管理员在后台编辑** → 数据保存到 LocalStorage
2. **前端加载页面** → 自动读取 LocalStorage 中的数据
3. **前端显示动态内容** → 统计数据、英文标语、Hero 描述等实时更新

### 示例：修改英文标语
1. 管理后台 → 站点设置
2. 修改 "英文标语" 字段
3. 点击保存
4. 前端页面自动读取新数据（刷新可见更新）

---

## 📲 跨设备同步方案

> 当前使用 LocalStorage，仅限单设备

### 升级为云端同步（可选）

#### 方案 A：使用 Supabase（推荐）
```javascript
// 示例代码
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(URL, KEY)
const { data, error } = await supabase
  .from('admin_data')
  .select('*')
```

#### 方案 B：使用 Firebase
```javascript
import { initializeApp } from "firebase/app"
import { getFirestore, collection, getDocs } from "firebase/firestore"

const db = getFirestore(app)
const querySnapshot = await getDocs(collection(db, "roamlab"))
```

---

## 🔒 安全加强

### 当前状态
- ✅ 简单演示登录（不加密）
- ✅ LocalStorage 存储

### 生产环保建议

1. **更换密码验证**
   ```javascript
   // login.html 中替换验证逻辑
   // 改为调用真实 API
   const response = await fetch('your-api/auth/login', {
     method: 'POST',
     body: JSON.stringify({ email, password })
   })
   ```

2. **使用 JWT Token**
   - 用 API 返回 JWT Token
   - 每次请求都在 header 中验证

3. **加密敏感数据**
   - 使用 bcrypt 加密密码
   - 启用 HTTPS

4. **限制访问**
   - IP 白名单
   - 两因素认证（2FA）

---

## 📱 响应式设计

网站已针对以下设备优化：
- ✅ 桌面（1920px 及以上）
- ✅ 平板（768px - 1023px）
- ✅ 手机（320px - 767px）

视频在移动设备上自动调整。

---

## 🎨 自定义修改

### 修改配色
编辑 `index.html` 中的 CSS 变量：

```css
:root{
  --accent:#00f5c4;      /* 青绿色 */
  --accent2:#4f8fff;     /* 蓝色 */
  --accent3:#ff4f7b;     /* 红色 */
  --gold:#f5c842;        /* 金色 */
}
```

### 修改文案
1. 方案 A：直接编辑 HTML 中的文本
2. 方案 B：通过管理后台的"站点设置"修改（推荐）

### 增加新功能模块

在 `admin.html` 中：

```html
<!-- 添加导航项 -->
<button class="nav-link" data-panel="new-module">
  <span class="icon">🆕</span> 新模块
</button>

<!-- 添加面板内容 -->
<div class="panel" id="new-module">
  <!-- 内容 -->
</div>
```

在 JavaScript 中添加对应的处理函数。

---

## 🐛 常见问题

### Q: 后台数据只在登录设备有效？
**A**: 是的，当前使用 LocalStorage。要跨设备同步，需升级为云数据库（见上面"跨设备同步方案"）。

### Q: 能否添加到线上数据库？
**A**: 可以。建议使用 Supabase 或 Firebase（免费额度足够小网站使用）。

### Q: 视频大小太大？
**A**: 可以压缩视频。使用 FFmpeg：
```bash
ffmpeg -i input.mp4 -crf 28 -preset fast output.mp4
```

### Q: 如何备份数据？
**A**: 在管理后台 → 站点设置 → 导出数据，会下载 JSON 文件。

### Q: 怎样与前端同步数据？
**A**: 前端在 DOMContentLoaded 时自动读取 LocalStorage：
```javascript
const adminData = JSON.parse(localStorage.getItem('roamlab_data'))
```

---

## 📞 后续支持

如需修改、扩展功能、或迁移到实际后端，可以：

1. **直接修改 HTML/CSS/JS** - 所有代码都在单文件中
2. **连接真实 API** - 替换 localStorage 为 fetch 调用
3. **升级数据库** - 从 LocalStorage 迁移到 Supabase/Firebase

---

## 📋 清单

部署前检查：

- [ ] 所有视频文件已上传到 Cloudflare
- [ ] GitHub 仓库已创建
- [ ] Cloudflare Pages 已连接
- [ ] 自定义域名已配置（可选）
- [ ] 修改了后台登录密码
- [ ] 测试了前后台数据同步
- [ ] 在手机上测试响应式

---

**祝你漫游愉快！🚀**

---

Made with ❤️ for Digital Nomads  
Version 1.0 · 2026
