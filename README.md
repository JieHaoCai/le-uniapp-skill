# Le UniApp Skill

一键创建 **uni-app + Vue 3 + TypeScript** 跨平台应用项目模版的 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) Skill。

## 安装

```bash
npx skills add JieHaoCai/le-uniapp-skill
```

## 使用

在 Claude Code 中输入：

```
/le-uniapp-skill my-project
```

即可在当前目录下创建一个名为 `my-project` 的 uni-app 标准化结构项目。


## 模版技术栈

| 技术 | 说明 |
|------|------|
| uni-app | 跨平台框架 |
| Vue 3 | 组合式 API（Composition API） |
| TypeScript | 类型安全 |
| Pinia | 状态管理 |
| uview-plus | UI 组件库 |
| z-paging | 上拉/下拉刷新、虚拟列表 |
| dayjs | 日期处理 |
| Vite | 构建工具 |

## 支持平台

- APP（安卓 / iOS）
- 小程序（微信）
- H5

## 项目结构

创建的项目包含以下目录结构：

```
├── api/                # 接口层（按业务模块拆分）
├── components/         # 公共展示组件（纯 UI）
├── composables/        # 组合式函数
├── data/               # 静态数据
├── pages/              # 页面（业务逻辑）
├── static/             # 静态资源
├── stores/             # Pinia Store
├── styles/             # 全局样式变量
├── types/              # TypeScript 类型定义
├── utils/              # 工具函数
├── uni_modules/        # uni-app 插件
├── App.vue             # 全局样式 + 应用生命周期
├── main.js             # 入口文件
├── pages.json          # 路由 + tabBar 配置
├── manifest.json       # 应用配置
├── vite.config.js      # 构建配置
└── uni.scss            # uni-app 变量 + 主题导入
```

## 内置开发规范

创建的项目内置 `CLAUDE.md` 开发规范，Claude 在后续开发中会自动遵循。核心规范包括：

- **单向数据流**：页面处理业务逻辑，组件只做 UI 展示
- **Store 规范**：组件用 `useXxxStore()`，非组件文件用 `getXxxStore()` 懒加载
- **API 规范**：统一通过 `utils/request.ts` 发起请求，命名格式 `xxxApi`
- **组件规范**：遵循 easycom 规范 `components/组件名/组件名.vue`
- **样式规范**：使用 rpx 单位，全局 SCSS 变量，可点击元素必须有反馈效果
- **跨平台**：通过 `#ifdef` / `#endif` 条件编译处理多端差异
- **生命周期**：遵循 uni-app 生命周期，不混用 Vue 原生生命周期
- **Git 提交**：`【type】描述` 格式（add / modify / fix / del）

## 注意事项

### Pinia Store 禁止在模块顶层调用

在 `.ts` / `.js` 文件的模块顶层直接调用 `useXxxStore()` 会导致 **App 端白屏崩溃**。

```ts
// ❌ 错误：模块顶层调用
import { useBaseStore } from '@/stores/base'
const baseStore = useBaseStore()

// ✅ 正确：函数内部调用
export const getHomeList = () => {
  const baseStore = useBaseStore()
  return http.get('/api/list')
}
```

Store 的 `useXxxStore()` 只能在以下位置调用：
- Vue 组件的 `setup()` / `<script setup>` 中
- 函数体内部（确保运行时 Pinia 已初始化）
- Pinia store 的 `actions` 中

## License

MIT
