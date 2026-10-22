# RiddleMe 出题智能体前端

基于设计稿实现的前端演示项目。当前未接入后端，所有数据访问收敛到 mock 层，接口已按 REST 风格预留，可平滑切换真实后端。纯前端构建产物可直接部署到 GitHub Pages。

## 功能

- 智能生成标签页：动态机器人形象、快捷提问、对话式出题入口
- 右侧题目参数面板：选择知识库 → 文档、勾选题型并设置题数、选择难度，支持向右折叠；左侧导航支持向左折叠
- 题目预览标签页：题型胶囊筛选、题目难度筛选、滚轮滚动列表
- 题目卡片：LaTeX 公式渲染（含矩阵等高等数学公式）、分值右侧行内编辑、答案与解析默认折叠可展开
- 编辑模式：题干、选项、答案、解析均可修改，LaTeX 以源码形式编辑，编辑时自动展开答案与解析
- 删除、收藏、全选
- 组卷弹窗：按题型模块排序（上移/下移）、多选发布学习空间、保存到组卷记录（默认勾选）、发布/取消

## 快速开始

```bash
cd frontend
npm install
npm run dev        # 本地开发，默认 http://localhost:5173
npm run build      # 类型检查 + 产出 dist/
npm run preview    # 本地预览构建产物
```

## 目录结构

```
frontend/src/
├── api/
│   ├── client.ts          # API 端点常量 + ApiClient 接口 + 适配器选择
│   └── mock/
│       ├── mockData.ts    # 知识库、文档、学习空间、题目模板库
│       └── mockAdapter.ts # Mock 适配器（模拟网络延迟）
├── components/
│   ├── layout/            # 顶栏、左侧导航、参数面板、标签栏、装饰
│   ├── tabs/GenerateTab/  # 智能生成（机器人、快捷提问、输入框）
│   ├── tabs/PreviewTab/   # 题目预览（卡片、答案面板、组卷弹窗）
│   └── ui/                # Toast、下拉选择
├── lib/latex.tsx          # KaTeX 渲染组件（$...$ / $$...$$）
├── store/AppContext.tsx   # 全局状态
└── types/index.ts         # 领域类型
```

## 接入真实后端

`src/api/client.ts` 中已列出全部预留端点：

```
GET  /api/knowledge-bases
GET  /api/knowledge-bases/:kbId/documents
GET  /api/study-spaces
POST /api/questions/generate
PUT  /api/questions/:id
DELETE /api/questions/:id
POST /api/papers/publish
GET  /api/assemble-records
```

实现一个 `httpAdapter` 满足 `ApiClient` 接口（用 fetch 或 axios 调上述端点），然后把 `client.ts` 末尾的

```ts
export const api: ApiClient = mockAdapter
```

替换为 `httpAdapter` 即可，页面组件零改动。

## 部署到 GitHub Pages

方式一（推荐）：仓库已包含 `.github/workflows/deploy.yml`。把整个仓库推送到 GitHub 的 `main` 分支，然后在仓库 Settings → Pages 中把 Source 设为 **GitHub Actions**，之后每次推送自动构建发布。

方式二（手动）：

```bash
cd frontend
npm run build
npx gh-pages -d dist
```

`vite.config.ts` 中 `base: './'` 已配置相对路径，部署到仓库子路径（如 `user.github.io/riddleme/`）无需改动。

## 字体

全局使用微软雅黑（Windows 自带），非 Windows 环境自动回退到苹方或系统无衬线字体。
