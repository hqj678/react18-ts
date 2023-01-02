## 一、使用create-react-app xxx --template typescript
```
create-react-app xxx --template typescript
```
通过--template typescript使得我们的项目支持ts


![1.PNG](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8acf10ee4e844c03b634b10d2b0666d4~tplv-k3u1fbpfcp-watermark.image?)

![2.PNG](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/69165db525214b1db4fad7c547371ce7~tplv-k3u1fbpfcp-watermark.image?)
创建成功
- 打开该项目 open folder 终端 cd xxx
- 删除我们不必要的文件，删除严格模式，最终得到我们的目录结构

![3.PNG](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4c4fa10134474319bcdd16716b5589bf~tplv-k3u1fbpfcp-watermark.image?)
## 二、通过craco进行我们基本的webpack配置
```
npm i @craco/craco@alpha -D
```

![4.PNG](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/59819426542643bab9989680efb29927~tplv-k3u1fbpfcp-watermark.image?)
为什么要加@alpha版本，是因为craco只支持webpack4版本，通过社区解决方案加上@alpha可以支持webpack5
### 1、配置别名

然后在根目录加上craco.config.js文件，去进行我们需要的webpack配置

打开package.json修改我们的启动脚本
```
//package.json文件
  "scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "react-scripts eject"
  },
```
```
//craco.config.js文件
const path = require("path");

const resolve = (dir) => path.resolve(__dirname, dir);

module.exports = {
  webpack: {
    alias: {
      "@": resolve('src'),
    }
  }
}
```
此时，我们修改我们的路径，发现报错

![5.PNG](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/41627927fa504016a71c381f90eb49d4~tplv-k3u1fbpfcp-watermark.image?)

因为我们的typescript进行路径检测报错，它不认识我们的@，所以我们打开tsconfig.json，增加
```
//tsconfig.json文件
"baseUrl": ".",
    "paths": {
      "@/*": [
        "src/*"
      ]
    }
```

![6.PNG](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2c88e6e3c2cc41c1b7921bb1b8e04777~tplv-k3u1fbpfcp-watermark.image?)

然后npm run start启动成功，报错解决
## 三、代码规范工具配置
### 1、集成editorconfig配置
在项目的根目录上增加.editorconfig文件
```
//.editorconfig文件
# http://editorconfig.org

root = true

[*] # 表示所有文件适用
charset = utf-8 # 设置文件字符集为utf-8
indent_style = space # 缩进风格（tab | space）
indent_size = 2 # 缩进大小
end_of_line = lf # 控制换行类型（lf | cr | crlf）
trim_trailing_whitespace = true # 去除行尾的任意空白字符
insert_final_newline = true # 始终在文件末尾插入一个新行

[*.md] # 表示仅.md文件适用以下规则
max_line_length = off
trim_trailing_whitespace = false
```
### 2、使用prettier工具
对ctrl-s后的代码进行格式化
#### 1、安装prettier
```
npm i prettier -D
```
#### 2、增加.prettierrc
```
//.prettierrc文件
{
  "useTabs": false,
  "tabWidth": 2,
  "printWidth": 80,
  "singleQuote": true,
  "trailingComma": "none",
  "semi": false
}
```
#### 3、增加.prettierignore
```
//.prettierignore
/build/*
.local
.output.js
/node_modules/*

**/*.svg
**/*.sh

/public/*
```
#### 4、在package.json文件中增加脚本
```
//package.json文件
"scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "react-scripts eject",
    "prettier": "prettier --write ."
  },
```
#### 5、运行脚本
```
npm run prettier
```
格式化成功

![9.PNG](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3ac0c17538964ed581afc4cd89a8aa8c~tplv-k3u1fbpfcp-watermark.image?)

### 3、使用ESLint检测
#### 1、安装ESLint
```
npm i eslint -D
```
#### 2、初始化ESLint配置
```
npx eslint --init
```

![10.PNG](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/93ef3be5401b40cbb9c83a5c5516a117~tplv-k3u1fbpfcp-watermark.image?)
由于我们选择的是esm环境，代码中module.exports等commonjs代码会被ESLint检测到报错
所以我们在eslint的env环境中增加node环境
```
//.eslintrc.js文件
env: {
    browser: true,
    es2021: true,
    node: true
  },
```
初始化成功
### 4、解决ESLint和prettier冲突的问题
```
npm install eslint-plugin-prettier eslint-config-prettier -D
```
添加prettier插件
```
//.eslintrc.js文件
extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:prettier/recommended'
  ],
```
## 四、配置项目结构并重置css
### 1、增加常见项目结构

![11.PNG](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/83da6021b70a45719ad0f673189e9c32~tplv-k3u1fbpfcp-watermark.image?)
### 2、css样式重置
1、安装并配置less
```
npm install craco-less@2.1.0-alpha.0
```
然后在craco.config.js中配置
```
//craco.config.js文件
const CracoLessPlugin = require('craco-less')
module.exports = {
  plugins: [{ plugin: CracoLessPlugin }],
  webpack: {
    alias: {
      '@': resolve('src')
    }
  }
}
```
2、安装normalize.css
```
npm i normalize.css
```
在index.tsx中引入重置样式
```
import 'normalize.css'
import '@/assets/css/index.less'
```
## 五、其它配置
安装路由
```
npm i react-router-dom
```
引入antd
```
npm install antd --save
```
安装redux
```
npm i redux react-redux @reduxjs/toolkit
```
