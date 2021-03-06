1. 使用 .editconfig 控制代码样式，比如：缩进、空格等


### vite

- 使用 ```vite serve 目录``` 可以指定启动目录

- Vite 默认支持 ESM 语法，或在 package.json 中配置  type: "module" 来开启 ESM 语法

- 为什么 ant-design-vue 不用在 site 目录下安装依赖就能启动项目
原因：没有根目录装 vue 导致的

### issue
- generateRoutes 单词拼错了


#### Error [ERR_REQUIRE_ESM]: Must use import to load ES Module

解决方法： 使用 esno/esmo  替换 node 执行命令

疑问: 为什么 ant-design-vue 就可以使用 node 执行js 文件 TODO:



#### peerDependencies 的作用是什么

```
  "peerDependencies": {
    "@vue/compiler-sfc": ">=3.1.0",
    "vue": ">=3.1.0"
  },
```

### 功能点拆分

- 使用 vite 搭建 vue 项目
当前使用的 vite 创建的项目，没有自己搭建项目，并且没有使用 ts, TODO:后续补上

    - 用到的插件， 这2个插件的作用是什么
        - @vitejs/plugin-vue
        - @vue/compiler-sfc


## 从 Vue Design Vue 中学习工和化

### 1. 自动把组件生成路由

```js
{
  "scripts":{
    "predev": "node node_modules/esbuild/install.js",
    "dev": "yarn predev && yarn routes && vite serve site",
    "routes": "node site/scripts/genrateRoutes.js"
  }
}
```

- [x] done

### 2. 自动化测试

```js
{
  "scripts": {
    "test": "cross-env NODE_ENV=test jest --config .jest.js",
  }
}
```


### 3. 自动化编译

```js
{
  "scripts": {
    "compile": "node antd-tools/cli/run.js compile",
  }
}
```

### 4. 自动化发布

```js
{
  "scripts": {
    "pub": "node --max_old_space_size=8192 antd-tools/cli/run.js pub",
    "pub-with-ci": "node antd-tools/cli/run.js pub-with-ci",
    "prepublishOnly": "node antd-tools/cli/run.js guard",
    "pre-publish": "node ./scripts/prepub && npm run generator-webtypes",
    "site": "yarn routes && ./node_modules/vite/bin/vite.js build site --base=https://next.antdv.com/",
    "pub:site": "npm run site && node site/scripts/pushToOSS.js",
  }
}
```

### 5. 自动化代码格式化

```js
{
  "scripts": {
    "prettier": "prettier -c --write **/*",
    "pretty-quick": "pretty-quick",
    "lint": "npm run tsc && npm run lint:demo && npm run lint:md && npm run lint:script && npm run lint:site",
    "lint:components": "eslint --fix --ext .jsx,.js,.ts,.tsx ./components",
    "lint:demo": "eslint --fix components/*/demo/*.vue",
    "lint:md": "eslint --fix *.md",
    "lint:script": "eslint . --ext '.js,.jsx,.ts,.tsx'",
    "lint:site": "eslint -c ./.eslintrc.js --fix --ext .jsx,.js,.ts,.tsx,vue ./site",
    "lint:style": "stylelint \"{site,components}/**/*.less\" --syntax less",
  }
}
```
### 6. 代码提交校验

```js
{
  "scripts": {
    "prepare": "husky install"
  }
}
```