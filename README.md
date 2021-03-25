# Webpack

## 簡易的創建 webpack 方式

先創建一個資料夾 這裡取名為 `webpack-demo`

然後我們開啟終端機 通過 `npm init` 快速創建一個 `package.json` 文件

打開 `package.json` 找到 `scripts` 將其改為如下內容：

```js
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",


  // 加上這行
  "build": "webpack" // 通過 npm run build 執行 webpack 打包編譯

},
```

接下來安裝 `webpack` 與 `webpack-cli` (記得加上 `--save`)

接下來我們在 `webpack-demo` 下創建一個 `src` 資料夾

裡面放入口文件(`main.js`) 與其他需要被編譯的檔案(ex: `helper.js` `all.scss`)

先創建 `helper.js` 文件 在裡面通過 `export default{}` 匯出一個函數簡單 `log` 一段話

`helper.js` 文件內容如下：

```js
export default {
  fn() {
    console.log("this is a js code");
  },
};
```

接下來開啟 `main.js` 輸入以下內容：

```js
import helpers from "./helper";

helpers.fn();
```

完成後在 `webpack-demo` 資料夾中創建一個檔案 `webpack.config.js` (它是 `webpack` 的設定檔)

創建後在裡面貼上以下內容：

```js
// node 的 path
const path = require("path");

module.exports = {
  // webpack 的入口文件
  entry: "./src/main.js",

  // webpack 的出口文件
  output: {
    // 編譯後生成的資料夾為在該項目目錄下的 dist 資料夾
    path: path.resolve(__dirname, "dist"),
    // 編譯後生成的文件名稱
    filename: "main.bundle.js",
  },
};

module.exports = config;
```

然後我們開啟終端機 切換到 `webpack-demo` 資料夾 通過 `npm run build` 執行編譯打包獲得 `dist` 資料夾

現在將 `dist` 資料夾另外用一個編輯器開啟線上瀏覽 即可看見 `helper.js` 中所寫的 `log` 了

以上是 `js` 方面的編譯打包 因為 `webpack` 內建 `js` 編譯

## css 與 sass 的編譯

接下來我們在 `src` 資料夾中創建一個 `all.css` 簡單修改 `body` 的背景色即可

然後上網搜尋 `css-loader` 它的說明文件中會要求你安裝 `css-loader`

即開啟終端機輸入以下內容：

```sell
npm install --save-dev css-loader
```

然後我們開啟 `webpack.config.js` 貼上 `css-loader` 說明文件中的模組規範

現在你的 `webpack.config.js` 內容如下：

```js
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "main.bundle.js",
  },
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
};
```

完成後再次開啟終端機 切換到 `webpack-demo` 資料夾 通過 `npm run build` 執行編譯打包獲得 `dist` 資料夾

再將 `dist` 資料夾另外用一個編輯器開啟線上瀏覽 即可看見背景色更改了

而 sass 則也是上網查詢 sass-loader 的說明文件 依序安裝 sass-loader 等

再到 `webpack.config.js` 中添加上 `sass-loader` 的 `rules`

最後執行 `npm run build` 重新打包即可

## devServe

在每次編譯代碼時都需要手動 npm run build 會比較費時

可以通過 webpack 提供的 webpack-dev-server 讓代碼在每次修改後自動編譯執行

安裝方式為開啟終端機輸入以下代碼：

```shell
npm i webpack-dev-server --save
```

接下來將 webpack 設定檔改為下方內容：

```js
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "main.bundle.js",
  },
  // 主要添加這段
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    compress: true,
    port: 9000,
  },
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
};
```

再開啟 `package.json` 將 `scripts` 改為下方：

```js
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",

  "start": "webpack serve", // 主要加這行

  "build": "webpack"
},
```

現在就可以直接通過 npm start 來執行實時編譯了
