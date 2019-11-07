### 组件定制
#### 基本介绍

开发店铺装修模版组件

#### 文件目录

```bash
├── dist                  构建结果
├── src                   源码目录
│   └── xxx               自定义组件目录
│       └── App.vue       自定义组件指定入口文件
├── webpack               webpack打包配置
└── package.json
```

#### 准备

`npm install` 安装项目依赖

本地开发
`npm run dev` 

开发打包
`npm run build` 

## 如何开发自定义组件

在 `src` 目录下可以开发自定义组件，组件入口文件名必须为`App.vue`，使用 `export default` 直接导出对象，且包含直接用字符串声明值的 name
