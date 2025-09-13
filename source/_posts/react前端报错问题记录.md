
---
title: "记录前端变异后报错问题"
date: "2025-09-13 12:57:03"
tags: [typescript]  # 可多个
categories: [前端]    # 可多个
---

### 报错信息
```shell
Uncaught ReferenceError: Cannot access 'ln' before initialization
    at color.js:63:3
```

### 导致原因 (猜测)
```shell
本人js运行时一般使用bun，但是偶然发现打包后color.js报错，指向最后一行FastColor
1.首先排除依赖问题，因为依赖多个版本未做改变
2.因为之前也遇到过vite启动报加密库错误的问题，所以推断是bun兼容性问题导致的
```

### 处理步骤
- 删除现有打包文件夹
- 删除node_modules依赖文件夹
- 使用pnpm i 下载依赖 (这里是因为上面提到的vite兼容就是bun装依赖后的问题)
- 使用pnpm run build 编译
- serve -s xxx -l 5174 (serve使用 npm i -g serve 安装; xxx为打包后的文件夹,通常为dist)
- 验证问题不存在