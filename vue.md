# 1.环境配置

1. nodejs

2. vue-cli

```
npm install --global vue-cli
```

3. webpack

```
npm install -g webpack

npm install webpack-dev-server -g

npm install --save-dev webpack-cli -g
```

4. 创建项目

```
vue init webpack my-project
```

5. 编译或运行

```
npm run dev 或者 npm start
```

6.直接运行在浏览器

```
npm install -g serve
serve -s dist
```



# 10000.j遇见的错误



## 9999.安装node-sass报错Python环境

```
npm i node-sass --sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
```

10000.npm run出现

```
ms\node_modules\webpack-cli\bin\cli.js:137                        const statsPresetToOptions = require("webpack").Stats.presetToOptions;                                                                             ^ TypeError: Cannot read property 'presetToOptions' of undefined
```

解决：去node_modules\webpack-cli\bin\cli.js:137把Stats去掉



## 2.JavaScript heap out of memory

```
Ineffective mark-compacts near heap limit Allocation failed - JavaScript heap out of memory
```

解决：全局安装npm install -g increase-memory-limit ，然后进入项目运行 increase-memory-limit