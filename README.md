# dmhy-cloudflare-workers
使用cloudflare workers直连动漫花园 dmhy
# https://dmhyorg.pages.dev/
20250122

https://github.com/copilot/c/9953f2c2-bd80-4d40-baa0-2ef28bfd5c8f 
<script src="./dist/index.js" type="module">
    import { tify } from './dist/index.js'
    </script>

    <script>
   
    const title = '简体字标题';
    name = tify(title);  // 使用转换后的标题
        async function getData() { 
            const xmlUrl = 'https://c.map987.us.kg/https://share.dmhy.org/topics/rss/team_id/816/rss.xm  是这样用吗问题是Uncaught ReferenceError: tify is not defined
    at （索引

直接使用这个库的dist/index.js文件
import { tify } from 'chinese-conv/dist';

export function convertTitle(title) {
    // 将简体字转换为繁体字
    return tify(title);
}
npm install chinese-conv
dist下的文件是已经生成好的可以用在单html中的js文件

202501221454


```
￼GPT 4o
￼
Copilot Chat
JavaScript module export errors and solutions
convertTitle is not defined
    at （索引）:650:5
（索引）:647 Uncaught SyntaxError: The requested module './dist/bundle.js' does not provide an export named 'convertTitle' (at （索引）:647:13)
（索引）:549 Uncaught ReferenceError: initPage is not defined
    at onload 
    
然后而我     <script type="module">
    import {convertTitle} from "./dist/bundle.js";</script>

    <script>
    name = convertTitle();  // 使用转换后的标题其中js中的     /******/ (() => { // webpackBootstrap
/******/ 	"use strict";
/******/ 	var __webpack_modules__ = ([
/* 0 */,
/* 1 */
/***/ ((__unused_webpack_module, __webpack_exports__, __webpack_require__) => {

__webpack_require__.r(__webpack_exports__);
/* harmony export */ __webpack_require__.d(__webpack_exports__, {
/* harmony export */   convertTitle: () => (/* binding */ convertTitle)
/* harmony export */ });
/* harmony import */ var chinese_conv_dist__WEBPACK_IMPORTED_MODULE_0__ = __webpack_require__(2);
// convert-title.js

function convertTitle(title) {
  // 将简体字转换为繁体字
  return (0,chinese_conv_dist__WEBPACK_IMPORTED_MODULE_0__.tify)(title);
  
这是怎么解决
The error messages indicate that the convertTitle function is not being exported correctly from the ./dist/bundle.js module, and there are some syntax issues in your HTML file. Below are the steps to resolve these issues:

Ensure Proper Export in bundle.js:
Make sure that the convertTitle function is correctly exported in your bundle.js file. It should look something like this:

JavaScript

// convert-title.js
import { tify } from 'chinese-conv/dist';

export function convertTitle(title) {
    // 将简体字转换为繁体字
    return tify(title);
}
Check the Build Configuration:
If you are using a bundler like Webpack, ensure that your configuration is correctly set to export the convertTitle function. Here is an example of how you might configure Webpack:

JavaScript
￼
// webpack.config.js
module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist'),
        library: {
            name: 'myLibrary',
            type: 'module',
        },
    },
    experiments: {
        outputModule: true,
    },
};
Correct the HTML File:
Modify your HTML file to correctly import and use the convertTitle function. Here is an example:

HTML

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script type="module">
        import { convertTitle } from "./dist/bundle.js";

        // Example usage of convertTitle function
        const title = "这是一个例子标题";
        const convertedTitle = convertTitle(title);
        console.log(convertedTitle);
    </script>
</body>
</html>
Resolve the initPage Function Error:
If you have an initPage function that is not defined, ensure that it is defined somewhere in your code and is correctly imported if necessary.

By following these steps, you should be able to resolve the errors related to the convertTitle function and any other reference errors in your JavaScript and HTML files.

￼
￼
￼
我这个是纯js没有import { tify } from 'chinese-conv/dist';
这些
明白了，如果你是用纯 JavaScript 而不是模块化的方式来实现这个功能，首先你需要确保 chinese-conv 这个库已经通过 <script> 标签引入到你的 HTML 文件中。然后，你可以定义 convertTitle 函数并使用 tify 方法进行转换。

下面是一个简单的示例，演示如何在纯 JavaScript 和 HTML 中使用 chinese-conv 库来实现这个功能：

在 HTML 文件中引入 chinese-conv 库：

HTML
￼
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Convert Title Example</title>
    <!-- 引入 chinese-conv 库 -->
    <script src="https://cdn.jsdelivr.net/npm/chinese-conv@latest/dist/index.min.js"></script>
</head>
<body>
    <script>
        // 定义 convertTitle 函数
        function convertTitle(title) {
            // 使用 chinese-conv 库的 tify 方法将简体字转换为繁体字
            return window.chineseConv.tify(title);
        }

        // 示例用法
        const title = "这是一个例子标题";
        const convertedTitle = convertTitle(title);
        console.log(convertedTitle); // 输出转换后的标题

        // 可以在 HTML 中展示转换后的标题
        document.body.innerHTML += `<h1>${convertedTitle}</h1>`;
    </script>
</body>
</html>
在这个示例中，我们通过 <script> 标签引入了 chinese-conv 库，然后定义了一个 convertTitle 函数，该函数使用 window.chineseConv.tify 方法将简体字转换为繁体字。最后，我们在控制台输出转换后的标题，并将其添加到 HTML 页面中。

确保你的 HTML 文件中正确引入了 chinese-conv 库，并且在使用 convertTitle 函数之前，库已经成功加载。

￼
￼
￼
chinese-conv 库my，只要bundle.js
好的，既然你希望使用 bundle.js 文件并且是纯 JavaScript 实现，那么我们可以确保 convertTitle 函数正确地在 bundle.js 中导出，并在 HTML 中正确地引入和使用它。

假设你的 bundle.js 文件内容如下：

JavaScript
￼
// convert-title.js
function convertTitle(title) {
    // 这里假设 tify 是一个全局可用的方法
    return tify(title);
}

// 将 convertTitle 方法导出
export { convertTitle };
接下来，确保你的 bundle.js 文件已经被正确编译和打包，并且包含在你的 HTML 文件中。

以下是如何在 HTML 文件中使用这个 bundle.js 文件：

HTML
￼
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Convert Title Example</title>
    <!-- 引入 bundle.js 文件 -->
    <script type="module">
        import { convertTitle } from './dist/bundle.js';

        document.addEventListener('DOMContentLoaded', () => {
            const title = "这是一个例子标题";
            const convertedTitle = convertTitle(title);
            console.log(convertedTitle); // 输出转换后的标题

            // 可以在 HTML 中展示转换后的标题
            document.body.innerHTML += `<h1>${convertedTitle}</h1>`;
        });
    </script>
</head>
<body>
    <!-- 你的 HTML 内容 -->
</body>
</html>
确保以下几点：

bundle.js 文件正确导出 convertTitle 函数。
HTML 文件中正确地引用了 bundle.js 文件，并且使用了 type="module" 类型的 <script> 标签来进行模块化的导入。
tify 方法是全局可用的（例如通过其他 <script> 标签引入的库）。
如果你在打包过程中遇到任何问题，请确保你的打包工具（如 Webpack）配置正确，并且能够正确导出和引用模块。如果需要进一步的帮助，请提供更多详细信息以便更好地指导你。

￼
￼
￼
/******/ (() => { // webpackBootstrap
/******/ 	"use strict";
/******/ 	var __webpack_modules__ = ([
/* 0 */,
/* 1 */
/***/ ((__unused_webpack_module, __webpack_exports__, __webpack_require__) => {

__webpack_require__.r(__webpack_exports__);
/* harmony export */ __webpack_require__.d(__webpack_exports__, {
/* harmony export */   convertTitle: () => (/* binding */ convertTitle)
/* harmony export */ });
/* harmony import */ var chinese_conv_dist__WEBPACK_IMPORTED_MODULE_0__ = __webpack_require__(2);
// convert-title.js

function convertTitle(title) {
  // 将简体字转换为繁体字
  return (0,chinese_conv_dist__WEBPACK_IMPORTED_MODULE_0__.tify)(ti  我是说这个怎么export
为了在纯 JavaScript 项目中正确导出和使用 convertTitle 函数，你需要确保以下几点：

确保 convertTitle 函数正确导出：
如果你在使用打包工具（如 Webpack），需要确保 convertTitle 函数正确导出。你已经在示例代码中使用了 ES 模块的导出方式。

JavaScript
￼
// convert-title.js
function convertTitle(title) {
  // 将简体字转换为繁体字
  return tify(title); // 假设 tify 是一个全局可用的方法
}

// 将 convertTitle 方法导出
export { convertTitle };
确保在 HTML 文件中正确引入打包后的 bundle.js 文件：
在 HTML 文件中使用 <script type="module"> 标签来引入打包后的 bundle.js 文件，并导入 convertTitle 函数。

假设你的 bundle.js 文件已经正确打包并位于 ./dist/bundle.js 路径下，下面是正确的 HTML 文件示例：

HTML
￼
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Convert Title Example</title>
</head>
<body>
    <script type="module">
        import { convertTitle } from './dist/bundle.js';

        document.addEventListener('DOMContentLoaded', () => {
            const title = "这是一个例子标题";
            const convertedTitle = convertTitle(title);
            console.log(convertedTitle); // 输出转换后的标题

            // 可以在 HTML 中展示转换后的标题
            document.body.innerHTML += `<h1>${convertedTitle}</h1>`;
        });
    </script>
</body>
</html>
确保 tify 方法全局可用：
确保 tify 方法是全局可用的。例如，你可以通过 <script> 标签在 HTML 文件中引入 tify 库：

HTML
￼
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Convert Title Example</title>
    <!-- 引入 tify 库 -->
    <script src="https://cdn.jsdelivr.net/npm/chinese-conv@latest/dist/index.min.js"></script>
</head>
<body>
    <script type="module">
        import { convertTitle } from './dist/bundle.js';

        document.addEventListener('DOMContentLoaded', () => {
            const title = "这是一个例子标题";
            const convertedTitle = convertTitle(title);
            console.log(convertedTitle); // 输出转换后的标题

            // 可以在 HTML 中展示转换后的标题
            document.body.innerHTML += `<h1>${convertedTitle}</h1>`;
        });
    </script>
</body>
</html>
确保 convertTitle 函数在 bundle.js 文件中正确导出，并在 HTML 文件中正确导入和使用它。如果 tify 方法不是全局可用的，请确保通过正确的方式引入该方法。

￼
￼
￼
我是说我是这样运行的// convert-title.js
import { tify } from 'chinese-conv/dist';

export function convertTitle(title) {
    // 将简体字转换为繁体字
    return tify(title);
}然后//index.js

import { convertTitle } from './convert-title';

// 示例使用
const title = '简体字标题';
const convertedTitle = convertTitle(title);
console.log(convertedTitle);
export { convertTitle };
问题是我再 webpack它们没法直接使用 convertTitle.，我的问题是怎么直接用在我的html中
为了在 HTML 文件中直接使用通过 Webpack 打包的 convertTitle 函数，需要确保以下几点：

确保 Webpack 正确打包：
你需要配置 Webpack，使其能够正确地将模块打包，并在浏览器中以模块化方式加载。

在 HTML 文件中正确引入打包后的文件：

下面是详细的步骤和示例代码来实现这一点：

1. Webpack 配置
确保你的 webpack.config.js 文件配置正确，能够将你的模块打包成一个可以在浏览器中使用的文件。

JavaScript

// webpack.config.js
const path = require('path');

module.exports = {
    entry: './src/index.js', // 入口文件
    output: {
        filename: 'bundle.js', // 输出文件名
        path: path.resolve(__dirname, 'dist'), // 输出路径
        library: {
            name: 'MyLibrary',
            type: 'var', // 使用全局变量方式
        },
    },
    mode: 'development', // 开发模式
};

2. JavaScript 文件
确保你的 convert-title.js 和 index.js 文件正确导出和使用 convertTitle 函数。

// src/convert-title.js
import { tify } from 'chinese-conv/dist';

export function convertTitle(title) {
    // 将简体字转换为繁体字
    return tify(title);
}


// src/index.js
import { convertTitle } from './convert-title';

// 示例使用
const title = '简体字标题';
const convertedTitle = convertTitle(title);
console.log(convertedTitle);

export { convertTitle };
Public code references from 2 repositories
3. 打包项目
运行 Webpack 将文件打包：

npx webpack

4. 在 HTML 文件中引入打包后的文件
在 HTML 文件中通过 <script> 标签引入打包后的 bundle.js 文件，并使用 MyLibrary.convertTitle 函数。

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Convert Title Example</title>
</head>
<body>
    <script src="./dist/bundle.js"></script>
    <script>
        // 使用 MyLibrary.convertTitle 函数
        document.addEventListener('DOMContentLoaded', () => {
            const title = '简体字标题';
            const convertedTitle = MyLibrary.convertTitle(title);
            console.log(convertedTitle); // 输出转换后的标题

            // 可以在 HTML 中展示转换后的标题
            document.body.innerHTML += `<h1>${convertedTitle}</h1>`;
        });
    </script>
</body>
</html>
总结
通过上面的步骤，你可以确保 convertTitle 函数在打包后能够在 HTML 文件中以全局变量 MyLibrary.convertTitle 的形式使用。确保 Webpack 配置正确，并在 HTML 文件中正确引入打包后的 bundle.js 文件。
```

```
https://github.com/copilot/c/9953f2c2-bd80-4d40-baa0-2ef28bfd5c8f
本来以为要webpack生成js到dist文件，其实库里就有dist文件夹下的js文件了

GPT 4o
￼
Copilot Chat
Using ES6 modules in script tags
    <script src="./dist/index.js" type="module">
    import { tify } from './dist/index.js'
    </script>

    <script>
   
    const title = '简体字标题';
    name = tify(title);  // 使用转换后的标题
        async function getData() { 
            const xmlUrl = 'https://c.map987.us.kg/https://share.dmhy.org/topics/rss/team_id/816/rss.xm  是这样用吗问题是Uncaught ReferenceError: tify is not defined
    at （索引
▋

￼
It looks like you're trying to use ES6 module syntax within a script tag, but you have a few issues in your code that need to be addressed. Here are some steps to correct your code:

Correct type="module" usage: When using ES6 modules, the type="module" attribute needs to be in the <script> tag that contains the import statement.
Define tify properly: Ensure that tify is imported correctly in the script where it is used.
Here's the corrected code:

HTML
￼
<!-- Correct the script tag for importing the module -->
<script type="module">
    import { tify } from './dist/index.js';

    const title = '简体字标题';
    const name = tify(title);  // 使用转换后的标题

    async function getData() { 
        const xmlUrl = 'https://c.map987.us.kg/https://share.dmhy.org/topics/rss/team_id/816/rss.xml';
        // Your fetch or other code to use xmlUrl
    }

    // Call getData function or use it as needed
    getData();
</script>

<!-- Remove the additional script tag as it is not needed -->
Explanation:
type="module": The <script> tag with type="module" should contain the import statement and any other code that uses the imported module.

Import Statement: The import { tify } from './dist/index.js'; statement correctly imports tify from the specified module.

Using Imported Function: The tify function is used to convert the title, and the result is stored in the name variable.

Function Definition and Call: The getData function is defined to handle fetching data from the xmlUrl, and it is called at the end of the script.

Additional Notes:
Make sure that the path './dist/index.js' is correct and the module file exists at that location.
Ensure that the tify function is exported properly from index.js.
This should help resolve the ReferenceError: tify is not defined issue. If you encounter any other errors, please provide more details so I can assist you further.

￼
￼
￼
中文
你好！你可以问我任何关于编程的问题。如果你有具体的代码问题或需要解释的概念，请告诉我。我会尽力帮助你。

￼
￼
￼
console.log(name) 
                name = sify(name);[ANi] Hana wa Saku Shura no Gotoku /  群花綻放、彷如修羅 - 03 [1080P][Baha][WEB-DL][AAC AVC][CHT][MP4]
（索引）:696 Uncaught (in promise) TypeError: Assignment to constant variable.
    at （索引）:696:22
    at NodeList.forEach (<anonymous>)
    at buildDataSource (（索引）:693:19)
    at main (（索引）:708:32)
根据你提供的错误信息，Uncaught (in promise) TypeError: Assignment to constant variable., 这是因为你尝试重新赋值一个用 const 声明的变量。const 声明的变量不能被重新赋值。

以下是修复代码的步骤：

将 const 改为 let 或者 var 以允许重新赋值。
确保 name 在适当的地方声明和使用。
另外，确保 sify 函数正确导入和使用。

以下是修复后的代码示例：

HTML
￼
<script type="module">
    import { tify, sify } from './dist/index.js';

    // 使用 let 来声明可以重新赋值的变量
    let name = '简体字标题';
    name = tify(name);  // 使用转换后的标题

    console.log(name);
    name = sify(name);  // 重新转换标题

    console.log(name);

    async function getData() { 
        const xmlUrl = 'https://c.map987.us.kg/https://share.dmhy.org/topics/rss/team_id/816/rss.xml';
        // 你的 fetch 或其他代码来使用 xmlUrl
    }

    // 调用 getData 函数或按需要使用它
    getData();
</script>
解释：
声明和重新赋值变量：将 const name = '简体字标题'; 改为 let name = '简体字标题';。这样变量 name 可以被重新赋值。
导入 sify 函数：确保 sify 函数从 ./dist/index.js 模块中正确导入。
日志输出：使用 console.log(name); 输出转换后的标题，方便调试和查看结果。
注意事项：
确保 tify 和 sify 函数在 ./dist/index.js 中正确导出。
确保路径 ./dist/index.js 是正确的，并且文件存在。
这样修改后，应该能够解决 Assignment to constant variable 错误。如果还有其他问题，请提供更多详细信息，我会进一
```

###导入script下面这种不行
<script src="./dist/index.js" type="module">
    import { tify } from './dist/index.js'
    </script>
    一个script要不到另外一个script的
，需要把现在的script添加一个type="module"
   <script type="module">
    import { tify, sify } from './dist/index.js';
    然后后面跟着原来的script，然后后续script如果有function，还要再把它单独分一个script标签，不要跟着  <script type="module">这个script
    
### 

## cloudflare workers直连部分 :
1. 使用 :

https://github.com/ymyuuu/Cloudflare-Workers-Proxy/

复制js文件文本到cloudflare workers中，之后通过

https://bfghgf.workers.dev/https://share.dmhy.org 能访问

2. 注册us.kg域名，在cloudflare workers设置域名，然后就能用https://c.map987.us.kg/https://share.dmhy.org  来直连访问

## 显示为一个html网页 
html css样式抄自 https://cdn.mlsub.net

html文本放在一个index.html命名的文件中，打包为压缩文件上传cloudflare pages即可

其中末尾的

```<script> 
        // 假设数据源 
async function getData() { 
  const xmlUrl = 'https://c.map987.us.kg/https://share.dmhy.org/topics/rss/rss.xml?keyword=%E4%B8%80%E5%85%86';

```
修改为自己绑定域名后的workers地址，后跟动漫花园rss链接

## https://b.map987.us.kg/
流程 :
https://b.map987.us.kg → pages.dev → 打开html会加载rss链接 https://c.map987.us.kg/https://share.dmhy.org → https://workers.dev/https://share.dmhy.org

问题:
1. 用js添加每个名字到html中太慢， <script src="https://cdn.jsdelivr.net/npm/handlebars@latest/dist/handlebars.js"></script> 这个js文件比较大，源网站https://cdn.mlsub.net是纯html页面比较快
2. 添加类似于tvkingdom , itv6.jp的番组表css样式。或者直接，周几的名，都显示在网页最上面，比较方便看，类似

周一 xxx xxx
周二 xxx xxx


搜索 cloudflare workers 代理OpenAI
https://www.mikcool.com/archives/18/
使用 Cloudflare Workers 直接使用 OpenAI API - Mikcool™


```javascript

const TELEGRAPH_URL = 'https://api.openai.com';

addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const url = new URL(request.url);
  url.host = TELEGRAPH_URL.replace(/^https?:\/\//, '');

  const modifiedRequest = new Request(url.toString(), {
    headers: request.headers,
    method: request.method,
    body: request.body,
    redirect: 'follow'
  });

  const response = await fetch(modifiedRequest);
  const modifiedResponse = new Response(response.body, response);

  // 添加允许跨域访问的响应头
  modifiedResponse.headers.set('Access-Control-Allow-Origin', '*');

  return modifiedResponse;
}
```

想要请求 ChatGPT 的 API 时，把官方文档中的 https://api.openai.com/v1/chat/completions 替换成 https://openai.workers.dev 即可

2.搜索workers镜像网站
https://yooer.me/cloudflare-workers-proxy.html
使用CloudFlare的Workers反向代理网站的三种源码1. 镜像整个网站

```javascript
//2023 年 3 月 5 日 23:18:00 修正了 不带 Post 数据的 Bug

// 替换成你想镜像的站点
const upstream = 'www.google.com'

// 如果那个站点有专门的移动适配站点，否则保持和上面一致
const upstream_mobile = 'www.google.com'

// 你希望禁止哪些国家访问
const blocked_region = ['RU']

// 禁止自访问
const blocked_ip_address = ['0.0.0.0', '127.0.0.1']

// 替换成你想镜像的站点
const replace_dict = {
    '$upstream': '$custom_domain',
    '//www.google.com': ''
}

//以下内容都不用动
addEventListener('fetch', event => {
    event.respondWith(fetchAndApply(event.request));
})

async function fetchAndApply(request) {

    const region = request.headers.get('cf-ipcountry').toUpperCase();
    const ip_address = request.headers.get('cf-connecting-ip');
    const user_agent = request.headers.get('user-agent');

    let response = null;
    let url = new URL(request.url);
    let url_host = url.host;

    if (url.protocol == 'http:') {
        url.protocol = 'https:'
        response = Response.redirect(url.href);
        return response;
    }

    if (await device_status(user_agent)) {
        upstream_domain = upstream
    } else {
        upstream_domain = upstream_mobile
    }

    url.host = upstream_domain;

    if (blocked_region.includes(region)) {
        response = new Response('Access denied: WorkersProxy is not available in your region yet.', {
            status: 403
        });
    } else if(blocked_ip_address.includes(ip_address)){
        response = new Response('Access denied: Your IP address is blocked by WorkersProxy.', {
            status: 403
        });
    } else{
        let method = request.method;
        let body = request.body;
        let request_headers = request.headers;
        let new_request_headers = new Headers(request_headers);

        new_request_headers.set('Host', upstream_domain);
        new_request_headers.set('Referer', url.href);

        let original_response = await fetch(url.href, {
            method: method,
            body: body,
            headers: new_request_headers
        })

        let original_response_clone = original_response.clone();
        let original_text = null;
        let response_headers = original_response.headers;
        let new_response_headers = new Headers(response_headers);
        let status = original_response.status;

        new_response_headers.set('access-control-allow-origin', '*');
        new_response_headers.set('access-control-allow-credentials', true);
        new_response_headers.delete('content-security-policy');
        new_response_headers.delete('content-security-policy-report-only');
        new_response_headers.delete('clear-site-data');

        const content_type = new_response_headers.get('content-type');
        if (content_type.includes('text/html') && content_type.includes('UTF-8')) {
            original_text = await replace_response_text(original_response_clone, upstream_domain, url_host);
        } else {
            original_text = original_response_clone.body
        }

        response = new Response(original_text, {
            status,
            headers: new_response_headers
        })
    }
    return response;
}

async function replace_response_text(response, upstream_domain, host_name) {
    let text = await response.text()

    var i, j;
    for (i in replace_dict) {
        j = replace_dict[i]
        if (i == '$upstream') {
            i = upstream_domain
        } else if (i == '$custom_domain') {
            i = host_name
        }

        if (j == '$upstream') {
            j = upstream_domain
        } else if (j == '$custom_domain') {
            j = host_name
        }

        let re = new RegExp(i, 'g')
        text = text.replace(re, j);
    }
    return text;
}

async function device_status (user_agent_info) {
    var agents = ["Android", "iPhone", "SymbianOS", "Windows Phone", "iPad", "iPod"];
    var flag = true;
    for (var v = 0; v < agents.length; v++) { if (user_agent_info.indexOf(agents[v]) > 0) {
            flag = false;
            break;
        }


都有cookie携带，主要是header


https://github.com/copilot/c/ccb4b813-b2ee-4d42-90a6-071bb35f14b1https://nijigennomori.com/kimetsu_awaji/https://github.com/copilot/c/b2e6afff-5b49-4258-bb64-d68334e6623d


function showEpisodes(button) {
            const row = button.closest('tr');
            const name = row.querySelector('.name').textContent.trim();
            const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
            if (episodePattern) {
                const seriesName = episodePattern[1].trim();
                const allRows = document.querySelectorAll('tr.file');
                const episodeList = [];
                allRows.forEach((r) => {
                    const episodeName = r.querySelector('.name').textContent.trim();
                    if (episodeName.includes(seriesName)) {
                        const magnetLink = r.querySelector('a').getAttribute('href');
                        episodeList.push({ name: episodeName, magnet: magnetLink, seriesName: seriesName });
                    }
                });
                displayEpisodes(episodeList);
            }
        }

        function displayEpisodes(episodes) {
            const episodeListDiv = document.getElementById('episodeList');
            episodeListDiv.innerHTML = '<h3 class="episode-list-title"></h3><ul>' +
                episodes.map(e => `<li><a href="${e.magnet}">${e.name}</a></li>`).join('') + '</ul>';
            document.getElementById('episodeModal').style.display = 'block';
            document.body.style.overflow = 'hidden'; // Prevent background scrolling
        }

        function closeModal() {
            document.getElementById('episodeModal').style.display = 'none';
            document.body.style.overflow = ''; // Restore background scrolling
        }

        // Close modal when clicking outside
        window.onclick = function(event) {
            const modal = document.getElementById('episodeModal');
            if (event.target === modal) {
                closeModal();
            }
        }
    </script>，
    现在是显示出
集数列表
[ANi] 超超超超超喜歡你的 100 個女朋友 - 14 [1080P][Baha][WEB-DL][AAC AVC][CHT][MP4]
[ANi] 超超超超超喜歡你的 100 個女朋友 - 13 [1080P][Baha][WEB-DL][AAC AVC][CHT][MP4]

我需要的是仅仅显示一次标题，然后就只显示集数即可

function showEpisodes(button) {
    const row = button.closest('tr');
    const name = row.querySelector('.name').textContent.trim();
    const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
    if (episodePattern) {
        const seriesName = episodePattern[1].trim();
       # dmhy-cloudflare-workers
使用cloudflare workers直连动漫花园 dmhy

## cloudflare workers直连部分 :
1. 使用 :

https://github.com/ymyuuu/Cloudflare-Workers-Proxy/

复制js文件文本到cloudflare workers中，之后通过

https://bfghgf.workers.dev/https://share.dmhy.org 能访问

2. 注册us.kg域名，在cloudflare workers设置域名，然后就能用https://c.map987.us.kg/https://share.dmhy.org  来直连访问

## 显示为一个html# dmhy-cloudflare-workers
使用cloudflare workers直连动漫花园 dmhy

## cloudflare workers直连部分 :
1. 使用 :

https://github.com/ymyuuu/Cloudflare-Workers-Proxy/

复制js文件文本到cloudflare workers中，之后通过

https://bfghgf.workers.dev/https://share.dmhy.org 能访问

2. 注册us.kg域名，在cloudflare workers设置域名，然后就能用https://c.map987.us.kg/https://share.dmhy.org  来直连访问

## 显示为一个html网页 
html css样式抄自 https://cdn.mlsub.net

html文本放在一个index.html命名的文件中，打包为压缩文件上传cloudflare pages即可

其中末尾的

```<script> 
        // 假设数据源 
async function getData() { 
  const xmlUrl = 'https://c.map987.us.kg/https://share.dmhy.org/topics/rss/rss.xml?keyword=%E4%B8%80%E5%85%86';

```
修改为自己绑定域名后的workers地址，后跟动漫花园rss链接

## https://b.map987.us.kg/
流程 :
https://b.map987.us.kg → pages.dev → 打开html会加载rss链接 https://c.map987.us.kg/https://share.dmhy.org → https://workers.dev/https://share.dmhy.org

问题:
1. 用js添加每个名字到html中太慢， <script src="https://cdn.jsdelivr.net/npm/handlebars@latest/dist/handlebars.js"></script> 这个js文件比较大，源网站https://cdn.mlsub.net是纯html页面比较快
2. 添加类似于tvkingdom , itv6.jp的番组表css样式。或者直接，周几的名，都显示在网页最上面，比较方便看，类似

周一 xxx xxx
周二 xxx xxx


搜索 cloudflare workers 代理OpenAI
https://www.mikcool.com/archives/18/
使用 Cloudflare Workers 直接使用 OpenAI API - Mikcool™


```javascript

const TELEGRAPH_URL = 'https://api.openai.com';

addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const url = new URL(request.url);
  url.host = TELEGRAPH_URL.replace(/^https?:\/\//, '');

  const modifiedRequest = new Request(url.toString(), {
    headers: request.headers,
    method: request.method,
    body: request.body,
    redirect: 'follow'
  });

  const response = await fetch(modifiedRequest);
  const modifiedResponse = new Response(response.body, response);

  // 添加允许跨域访问的响应头
  modifiedResponse.headers.set('Access-Control-Allow-Origin', '*');

  return modifiedResponse;
}
```

想要请求 ChatGPT 的 API 时，把官方文档中的 https://api.openai.com/v1/chat/completions 替换成 https://openai.workers.dev 即可

2.搜索workers镜像网站
https://yooer.me/cloudflare-workers-proxy.html
使用CloudFlare的Workers反向代理网站的三种源码1. 镜像整个网站

```javascript
//2023 年 3 月 5 日 23:18:00 修正了 不带 Post 数据的 Bug

// 替换成你想镜像的站点
const upstream = 'www.google.com'

// 如果那个站点有专门的移动适配站点，否则保持和上面一致
const upstream_mobile = 'www.google.com'

// 你希望禁止哪些国家访问
const blocked_region = ['RU']

// 禁止自访问
const blocked_ip_address = ['0.0.0.0', '127.0.0.1']

// 替换成你想镜像的站点
const replace_dict = {
    '$upstream': '$custom_domain',
    '//www.google.com': ''
}

//以下内容都不用动
addEventListener('fetch', event => {
    event.respondWith(fetchAndApply(event.request));
})

async function fetchAndApply(request) {

    const region = request.headers.get('cf-ipcountry').toUpperCase();
    const ip_address = request.headers.get('cf-connecting-ip');
    const user_agent = request.headers.get('user-agent');

    let response = null;
    let url = new URL(request.url);
    let url_host = url.host;

    if (url.protocol == 'http:') {
        url.protocol = 'https:'
        response = Response.redirect(url.href);
        return response;
    }

    if (await device_status(user_agent)) {
        upstream_domain = upstream
    } else {
        upstream_domain = upstream_mobile
    }

    url.host = upstream_domain;

    if (blocked_region.includes(region)) {
        response = new Response('Access denied: WorkersProxy is not available in your region yet.', {
            status: 403
        });
    } else if(blocked_ip_address.includes(ip_address)){
        response = new Response('Access denied: Your IP address is blocked by WorkersProxy.', {
            status: 403
        });
    } else{
        let method = request.method;
        let body = request.body;
        let request_headers = request.headers;
        let new_request_headers = new Headers(request_headers);

        new_request_headers.set('Host', upstream_domain);
        new_request_headers.set('Referer', url.href);

        let original_response = await fetch(url.href, {
            method: method,
            body: body,
            headers: new_request_headers
        })

        let original_response_clone = original_response.clone();
        let original_text = null;
        let response_headers = original_response.headers;
        let new_response_headers = new Headers(response_headers);
        let status = original_response.status;

        new_response_headers.set('access-control-allow-origin', '*');
        new_response_headers.set('access-control-allow-credentials', true);
        new_response_headers.delete('content-security-policy');
        new_response_headers.delete('content-security-policy-report-only');
        new_response_headers.delete('clear-site-data');

        const content_type = new_response_headers.get('content-type');
        if (content_type.includes('text/html') && content_type.includes('UTF-8')) {
            original_text = await replace_response_text(original_response_clone, upstream_domain, url_host);
        } else {
            original_text = original_response_clone.body
        }

        response = new Response(original_text, {
            status,
            headers: new_response_headers
        })
    }
    return response;
}

async function replace_response_text(response, upstream_domain, host_name) {
    let text = await response.text()

    var i, j;
    for (i in replace_dict) {
        j = replace_dict[i]
        if (i == '$upstream') {
            i = upstream_domain
        } else if (i == '$custom_domain') {
            i = host_name
        }

        if (j == '$upstream') {
            j = upstream_domain
        } else if (j == '$custom_domain') {
            j = host_name
        }

        let re = new RegExp(i, 'g')
        text = text.replace(re, j);
    }
    return text;
}

async function device_status (user_agent_info) {
    var agents = ["Android", "iPhone", "SymbianOS", "Windows Phone", "iPad", "iPod"];
    var flag = true;
    for (var v = 0; v < agents.length; v++) { if (user_agent_info.indexOf(agents[v]) > 0) {
            flag = false;
            break;
        }


都有cookie携带，主要是header


https://github.com/copilot/c/ccb4b813-b2ee-4d42-90a6-071bb35f14b1https://nijigennomori.com/kimetsu_awaji/https://github.com/copilot/c/b2e6afff-5b49-4258-bb64-d68334e6623d


function showEpisodes(button) {
            const row = button.closest('tr');
            const name = row.querySelector('.name').textContent.trim();
            const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
            if (episodePattern) {
                const seriesName = episodePattern[1].trim();
                const allRows = document.querySelectorAll('tr.file');
                const episodeList = [];
                allRows.forEach((r) => {
                    const episodeName = r.querySelector('.name').textContent.trim();
                    if (episodeName.includes(seriesName)) {
                        const magnetLink = r.querySelector('a').getAttribute('href');
                        episodeList.push({ name: episodeName, magnet: magnetLink, seriesName: seriesName });
                    }
                });
                displayEpisodes(episodeList);
            }
        }

        function displayEpisodes(episodes) {
            const episodeListDiv = document.getElementById('episodeList');
            episodeListDiv.innerHTML = '<h3 class="episode-list-title"></h3><ul>' +
                episodes.map(e => `<li><a href="${e.magnet}">${e.name}</a></li>`).join('') + '</ul>';
            document.getElementById('episodeModal').style.display = 'block';
            document.body.style.overflow = 'hidden'; // Prevent background scrolling
        }

        function closeModal() {
            document.getElementById('episodeModal').style.display = 'none';
            document.body.style.overflow = ''; // Restore background scrolling
        }

        // Close modal when clicking outside
        window.onclick = function(event) {
            const modal = document.getElementById('episodeModal');
            if (event.target === modal) {
                closeModal();
            }
        }
    </script>，
    现在是显示出
集数列表
[ANi] 超超超超超喜歡你的 100 個女朋友 - 14 [1080P][Baha][WEB-DL][AAC AVC][CHT][MP4]
[ANi] 超超超超超喜歡你的 100 個女朋友 - 13 [1080P][Baha][WEB-DL][AAC AVC][CHT][MP4]

我需要的是仅仅显示一次标题，然后就只显示集数即可

function showEpisodes(button) {
    const row = button.closest('tr');
    const name = row.querySelector('.name').textContent.trim();
    const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
    if (episodePattern) {
        const seriesName = episodePattern[1].trim();
       # dmhy-cloudflare-workers
使用cloudflare workers直连动漫花园 dmhy

## cloudflare workers直连部分 :
1. 使用 :

https://github.com/ymyuuu/Cloudflare-Workers-Proxy/

复制js文件文本到cloudflare workers中，之后通过

https://bfghgf.workers.dev/https://share.dmhy.org 能访问

2. 注册us.kg域名，在cloudflare workers设置域名，然后就能用https://c.map987.us.kg/https://share.dmhy.org  来直连访问

## 显示为一个html网页 
html css样式抄自 https://cdn.mlsub.net

html文本放在一个index.html命名的文件中，打包为压缩文件上传cloudflare pages即可

其中末尾的

```<script> 
        // 假设数据源 
async function getData() { 
  const xmlUrl = 'https://c.map987.us.kg/https://share.dmhy.org/topics/rss/rss.xml?keyword=%E4%B8%80%E5%85%86';

```
修改为自己绑定域名后的workers地址，后跟动漫花园rss链接

## https://b.map987.us.kg/
流程 :
https://b.map987.us.kg → pages.dev → 打开html会加载rss链接 https://c.map987.us.kg/https://share.dmhy.org → https://workers.dev/https://share.dmhy.org

问题:
1. 用js添加每个名字到html中太慢， <script src="https://cdn.jsdelivr.net/npm/handlebars@latest/dist/handlebars.js"></script> 这个js文件比较大，源网站https://cdn.mlsub.net是纯html页面比较快
2. 添加类似于tvkingdom , itv6.jp的番组表css样式。或者直接，周几的名，都显示在网页最上面，比较方便看，类似

周一 xxx xxx
周二 xxx xxx


搜索 cloudflare workers 代理OpenAI
https://www.mikcool.com/archives/18/
使用 Cloudflare Workers 直接使用 OpenAI API - Mikcool™


```javascript

const TELEGRAPH_URL = 'https://api.openai.com';

addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const url = new URL(request.url);
  url.host = TELEGRAPH_URL.replace(/^https?:\/\//, '');

  const modifiedRequest = new Request(url.toString(), {
    headers: request.headers,
    method: request.method,
    body: request.body,
    redirect: 'follow'
  });

  const response = await fetch(modifiedRequest);
  const modifiedResponse = new Response(response.body, response);

  // 添加允许跨域访问的响应头
  modifiedResponse.headers.set('Access-Control-Allow-Origin', '*');

  return modifiedResponse;
}
```

想要请求 ChatGPT 的 API 时，把官方文档中的 https://api.openai.com/v1/chat/completions 替换成 https://openai.workers.dev 即可

2.搜索workers镜像网站
https://yooer.me/cloudflare-workers-proxy.html
使用CloudFlare的Workers反向代理网站的三种源码1. 镜像整个网站

```javascript
//2023 年 3 月 5 日 23:18:00 修正了 不带 Post 数据的 Bug

// 替换成你想镜像的站点
const upstream = 'www.google.com'

// 如果那个站点有专门的移动适配站点，否则保持和上面一致
const upstream_mobile = 'www.google.com'

// 你希望禁止哪些国家访问
const blocked_region = ['RU']

// 禁止自访问
const blocked_ip_address = ['0.0.0.0', '127.0.0.1']

// 替换成你想镜像的站点
const replace_dict = {
    '$upstream': '$custom_domain',
    '//www.google.com': ''
}

//以下内容都不用动
addEventListener('fetch', event => {
    event.respondWith(fetchAndApply(event.request));
})

async function fetchAndApply(request) {

    const region = request.headers.get('cf-ipcountry').toUpperCase();
    const ip_address = request.headers.get('cf-connecting-ip');
    const user_agent = request.headers.get('user-agent');

    let response = null;
    let url = new URL(request.url);
    let url_host = url.host;

    if (url.protocol == 'http:') {
        url.protocol = 'https:'
        response = Response.redirect(url.href);
        return response;
    }

    if (await device_status(user_agent)) {
        upstream_domain = upstream
    } else {
        upstream_domain = upstream_mobile
    }

    url.host = upstream_domain;

    if (blocked_region.includes(region)) {
        response = new Response('Access denied: WorkersProxy is not available in your region yet.', {
            status: 403
        });
    } else if(blocked_ip_address.includes(ip_address)){
        response = new Response('Access denied: Your IP address is blocked by WorkersProxy.', {
            status: 403
        });
    } else{
        let method = request.method;
        let body = request.body;
        let request_headers = request.headers;
        let new_request_headers = new Headers(request_headers);

        new_request_headers.set('Host', upstream_domain);
        new_request_headers.set('Referer', url.href);

        let original_response = await fetch(url.href, {
            method: method,
            body: body,
            headers: new_request_headers
        })

        let original_response_clone = original_response.clone();
        let original_text = null;
        let response_headers = original_response.headers;
        let new_response_headers = new Headers(response_headers);
        let status = original_response.status;

        new_response_headers.set('access-control-allow-origin', '*');
        new_response_headers.set('access-control-allow-credentials', true);
        new_response_headers.delete('content-security-policy');
        new_response_headers.delete('content-security-policy-report-only');
        new_response_headers.delete('clear-site-data');

        const content_type = new_response_headers.get('content-type');
        if (content_type.includes('text/html') && content_type.includes('UTF-8')) {
            original_text = await replace_response_text(original_response_clone, upstream_domain, url_host);
        } else {
            original_text = original_response_clone.body
        }

        response = new Response(original_text, {
            status,
            headers: new_response_headers
        })
    }
    return response;
}

async function replace_response_text(response, upstream_domain, host_name) {
    let text = await response.text()

    var i, j;
    for (i in replace_dict) {
        j = replace_dict[i]
        if (i == '$upstream') {
            i = upstream_domain
        } else if (i == '$custom_domain') {
            i = host_name
        }

        if (j == '$upstream') {
            j = upstream_domain
        } else if (j == '$custom_domain') {
            j = host_name
        }

        let re = new RegExp(i, 'g')
        text = text.replace(re, j);
    }
    return text;
}

async function device_status (user_agent_info) {
    var agents = ["Android", "iPhone", "SymbianOS", "Windows Phone", "iPad", "iPod"];
    var flag = true;
    for (var v = 0; v < agents.length; v++) { if (user_agent_info.indexOf(agents[v]) > 0) {
            flag = false;
            break;
        }


都有cookie携带，主要是header


https://github.com/copilot/c/ccb4b813-b2ee-4d42-90a6-071bb35f14b1https://nijigennomori.com/kimetsu_awaji/https://github.com/copilot/c/b2e6afff-5b49-4258-bb64-d68334e6623d


function showEpisodes(button) {
            const row = button.closest('tr');
            const name = row.querySelector('.name').textContent.trim();
            const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
            if (episodePattern) {
                const seriesName = episodePattern[1].trim();
                const allRows = document.querySelectorAll('tr.file');
                const episodeList = [];
                allRows.forEach((r) => {
                    const episodeName = r.querySelector('.name').textContent.trim();
                    if (episodeName.includes(seriesName)) {
                        const magnetLink = r.querySelector('a').getAttribute('href');
                        episodeList.push({ name: episodeName, magnet: magnetLink, seriesName: seriesName });
                    }
                });
                displayEpisodes(episodeList);
            }
        }

        function displayEpisodes(episodes) {
            const episodeListDiv = document.getElementById('episodeList');
            episodeListDiv.innerHTML = '<h3 class="episode-list-title"></h3><ul>' +
                episodes.map(e => `<li><a href="${e.magnet}">${e.name}</a></li>`).join('') + '</ul>';
            document.getElementById('episodeModal').style.display = 'block';
            document.body.style.overflow = 'hidden'; // Prevent background scrolling
        }

        function closeModal() {
            document.getElementById('episodeModal').style.display = 'none';
            document.body.style.overflow = ''; // Restore background scrolling
        }

        // Close modal when clicking outside
        window.onclick = function(event) {
            const modal = document.getElementById('episodeModal');
            if (event.target === modal) {
                closeModal();
            }
        }
    </script>，
    现在是显示出
集数列表
[ANi] 超超超超超喜歡你的 100 個女朋友 - 14 [1080P][Baha][WEB-DL][AAC AVC][CHT][MP4]
[ANi] 超超超超超喜歡你的 100 個女朋友 - 13 [1080P][Baha][WEB-DL][AAC AVC][CHT][MP4]

我需要的是仅仅显示一次标题，然后就只显示集数即可

function showEpisodes(button) {
    const row = button.closest('tr');
    const name = row.querySelector('.name').textContent.trim();
    const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
    if (episodePattern) {
        const seriesName = episodePattern[1].trim();
        const allRows = document.querySelectorAll('tr.file');
        const episodeList = [];
        allRows.forEach((r) => {
            const episodeName = r.querySelector('.name').textContent.trim();
            if (episodeName.includes(seriesName)) {
                const magnetLink = r.querySelector('a').getAttribute('href');
                const episodeNumber = episodeName.match(/-\s*(\d+)/)[1]; // 提取集数
                episodeList.push({ episodeNumber: episodeNumber, magnet: magnetLink });
            }
        });
        displayEpisodes(seriesName, episodeList);
    }
}

function displayEpisodes(seriesName, episodes) {
    const episodeListDiv = document.getElementById('episodeList');
    episodeListDiv.innerHTML = `<h3 class="episode-list-title">${seriesName}</h3><ul>` +
        episodes.map(e => `<li><a href="${e.magnet}">Episode ${e.episodeNumber}</a></li>`).join('') + '</ul>';
    document.getElementById('episodeModal').style.display = 'block';
    document.body.style.overflow = 'hidden'; // Prevent background scrolling
}

function closeModal() {
    document.getElementById('episodeModal').style.display = 'none';
    document.body.style.overflow = ''; // Restore background scrolling
}

// Close modal when clicking outside
window.onclick = function(event) {
    const modal = document.getElementById('episodeModal');
    if (event.target === modal) {
        closeModal();
    }
}


transform: translateY(-50%);show-episodes-btnepisodeList<script>
    function showEpisodes(button) {
        const row = button.closest('tr');
        const name = row.querySelector('.name').textContent.trim();
        const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
        if (episodePattern) {
            const seriesName = episodePattern[1].trim();
            const allRows = document.querySelectorAll('tr.file');
            const episodeList = [];
            allRows.forEach((r) => {
                const episodeName = r.querySelector('.name').textContent.trim();
                if (episodeName.includes(seriesName)) {
                    const magnetLink = r.querySelector('a').getAttribute('href');
                    episodeList.push({ name: episodeName, magnet: magnetLink });
                }
            });
            displayEpisodes(episodeList);
        }
    }

    function displayEpisodes(episodes) {
        const episodeListDiv = document.getElementById('episodeList');
        episodeListDiv.innerHTML = '<h3 class="episode-list-title">集数列表</h3><ul>' +
            episodes.map(e => `<li><a href="${e.magnet}">${e.name}</a></li>`).join('') + '</ul>';
        document.getElementById('episodeModal').style.display = 'block';
        document.body.style.overflow = 'hidden'; // Prevent background scrolling
    }

    function closeModal() {
        document.getElementById('episodeModal').style.display = 'none';
        document.body.style.overflow = ''; // Restore background scrolling
    }

    // Close modal when clicking outside
    window.onclick = function(event) {
        const modal = document.getElementById('episodeModal');
        if (event.target === modal) {
            closeModal();
        }
    }
</script>

<style>
/* Modal styles */
.modal {
    display: none;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgb(0,0,0);
    background-color: rgba(0,0,0,0.4);
    padding-top: 60px;
}

.modal-content {
    background-color: #fefefe;
    margin: 5% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
    max-height: 80%;
    overflow-y: auto;
    border-left: 3px solid #BB2649; /* Left border */
    border-right: 3px solid #BB2649; /* Right border */
}

.episode-list-title {
    border-top: 5px solid #BB2649; /* Top border for the title */
    padding-top: 10px;
}

.close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
}

.close:hover,
.close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
}

/* Button styles */
.show-episodes-btn {
    background-color: #BB2649;
    color: white;
    border: none;
    padding: 5px 10px;
    cursor: pointer;
    font-size: 14px;
    margin-right: 10px;
}

.show-episodes-btn:hover {
    background-color: #a02345;
}
</style



<!-- Modal for displaying episodes -->
<div id="episodeModal" class="modal">
    <div class="modal-content">
        <span class="close" onclick="closeModal()">&times;</span>
        <div id="episodeList"></div>
    </div>
</div>

<script>
    function showEpisodes(button) {
        const row = button.closest('tr');
        const name = row.querySelector('.name').textContent.trim();
        const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
        if (episodePattern) {
            const seriesName = episodePattern[1].trim();
            const allRows = document.querySelectorAll('tr.file');
            const episodeList = [];
            allRows.forEach((r) => {
                const episodeName = r.querySelector('.name').textContent.trim();
                if (episodeName.includes(seriesName)) {
                    const magnetLink = r.querySelector('a').getAttribute('href');
                    episodeList.push({ name: episodeName, magnet: magnetLink });
                }
            });
            displayEpisodes(episodeList);
        }
    }

    function displayEpisodes(episodes) {
        const episodeListDiv = document.getElementById('episodeList');
        episodeListDiv.innerHTML = '<h3>集数列表</h3><ul>' +
            episodes.map(e => `<li><a href="${e.magnet}">${e.name}</a></li>`).join('') + '</ul>';
        document.getElementById('episodeModal').style.display = 'block';
        document.body.style.overflow = 'hidden'; // Prevent background scrolling
    }

    function closeModal() {
        document.getElementById('episodeModal').style.display = 'none';
        document.body.style.overflow = ''; // Restore background scrolling
    }

    // Close modal when clicking outside
    window.onclick = function(event) {
        const modal = document.getElementById('episodeModal');
        if (event.target === modal) {
            closeModal();
        }
    }
</script>

<style>
/* Modal styles */
.modal {
    display: none;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgb(0,0,0);
    background-color: rgba(0,0,0,0.4);
    padding-top: 60px;
}

.modal-content {
    background-color: #fefefe;
    margin: 5% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
    max-height: 80%;
    overflow-y: auto;
    border-top: 5px solid #BB2649; /* Top border */
    border-left: 3px solid #BB2649; /* Left border */
    border-right: 3px solid #BB2649; /* Right border */
}

.close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
}

.close:hover,
.close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
}
</style>


 const name = row.querySelector('.name').textContent.trim();
        const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
        
        if (episodePattern) {
            const seriesName = episodePattern[1].trim();
            console.log("正则表达式检查点", seriesName);

            [ANi] 金肉人 完美超人始祖篇 Season 2 - 13 [1080P][Baha][WEB-DL][AAC AVC][CHT][MP4]
            ↑name输入
            ↓seriesName匹配，打印出：
            正则表达式检查点 金肉人 完美超人始祖篇 Season 2         const allRows = document.querySelectorAll('tr.file');
        const episodeList = [];
        allRows.forEach((r) => {
            const episodeName = r.querySelector('.name').textContent.trim();
            if (episodeName.includes(seriesName)) {
                const magnetLink = r.querySelector('a').getAttribute('href');
                const episodeNumber = episodeName.match(/-\s*(\d+)/)[1]; // 提取集数
                episodeList.push({ episodeNumber: episodeNumber, magnet: magnetLink });
            }
        });
        displayEpisodes(seriesName, episodeList);
    }
}

function displayEpisodes(seriesName, episodes) {
    const episodeListDiv = document.getElementById('episodeList');
    episodeListDiv.innerHTML = `<h3 class="episode-list-title">${seriesName}</h3><ul>` +
        episodes.map(e => `<li><a href="${e.magnet}">Episode ${e.episodeNumber}</a></li>`).join('') + '</ul>';
    document.getElementById('episodeModal').style.display = 'block';
    document.body.style.overflow = 'hidden'; // Prevent background scrolling
}

function closeModal() {
    document.getElementById('episodeModal').style.display = 'none';
    document.body.style.overflow = ''; // Restore background scrolling
}

// Close modal when clicking outside
window.onclick = function(event) {
    const modal = document.getElementById('episodeModal');
    if (event.target === modal) {
        closeModal();
    }
}


transform: translateY(-50%);show-episodes-btnepisodeList<script>
    function showEpisodes(button) {
        const row = button.closest('tr');
        const name = row.querySelector('.name').textContent.trim();
        const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
        if (episodePattern) {
            const seriesName = episodePattern[1].trim();
            const allRows = document.querySelectorAll('tr.file');
            const episodeList = [];
            allRows.forEach((r) => {
                const episodeName = r.querySelector('.name').textContent.trim();
                if (episodeName.includes(seriesName)) {
                    const magnetLink = r.querySelector('a').getAttribute('href');
                    episodeList.push({ name: episodeName, magnet: magnetLink });
                }
            });
            displayEpisodes(episodeList);
        }
    }

    function displayEpisodes(episodes) {
        const episodeListDiv = document.getElementById('episodeList');
        episodeListDiv.innerHTML = '<h3 class="episode-list-title">集数列表</h3><ul>' +
            episodes.map(e => `<li><a href="${e.magnet}">${e.name}</a></li>`).join('') + '</ul>';
        document.getElementById('episodeModal').style.display = 'block';
        document.body.style.overflow = 'hidden'; // Prevent background scrolling
    }

    function closeModal() {
        document.getElementById('episodeModal').style.display = 'none';
        document.body.style.overflow = ''; // Restore background scrolling
    }

    // Close modal when clicking outside
    window.onclick = function(event) {
        const modal = document.getElementById('episodeModal');
        if (event.target === modal) {
            closeModal();
        }
    }
</script>

<style>
/* Modal styles */
.modal {
    display: none;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgb(0,0,0);
    background-color: rgba(0,0,0,0.4);
    padding-top: 60px;
}

.modal-content {
    background-color: #fefefe;
    margin: 5% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
    max-height: 80%;
    overflow-y: auto;
    border-left: 3px solid #BB2649; /* Left border */
    border-right: 3px solid #BB2649; /* Right border */
}

.episode-list-title {
    border-top: 5px solid #BB2649; /* Top border for the title */
    padding-top: 10px;
}

.close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
}

.close:hover,
.close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
}

/* Button styles */
.show-episodes-btn {
    background-color: #BB2649;
    color: white;
    border: none;
    padding: 5px 10px;
    cursor: pointer;
    font-size: 14px;
    margin-right: 10px;
}

.show-episodes-btn:hover {
    background-color: #a02345;
}
</style



<!-- Modal for displaying episodes -->
<div id="episodeModal" class="modal">
    <div class="modal-content">
        <span class="close" onclick="closeModal()">&times;</span>
        <div id="episodeList"></div>
    </div>
</div>

<script>
    function showEpisodes(button) {
        const row = button.closest('tr');
        const name = row.querySelector('.name').textContent.trim();
        const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
        if (episodePattern) {
            const seriesName = episodePattern[1].trim();
            const allRows = document.querySelectorAll('tr.file');
            const episodeList = [];
            allRows.forEach((r) => {
                const episodeName = r.querySelector('.name').textContent.trim();
                if (episodeName.includes(seriesName)) {
                    const magnetLink = r.querySelector('a').getAttribute('href');
                    episodeList.push({ name: episodeName, magnet: magnetLink });
                }
            });
            displayEpisodes(episodeList);
        }
    }

    function displayEpisodes(episodes) {
        const episodeListDiv = document.getElementById('episodeList');
        episodeListDiv.innerHTML = '<h3>集数列表</h3><ul>' +
            episodes.map(e => `<li><a href="${e.magnet}">${e.name}</a></li>`).join('') + '</ul>';
        document.getElementById('episodeModal').style.display = 'block';
        document.body.style.overflow = 'hidden'; // Prevent background scrolling
    }

    function closeModal() {
        document.getElementById('episodeModal').style.display = 'none';
        document.body.style.overflow = ''; // Restore background scrolling
    }

    // Close modal when clicking outside
    window.onclick = function(event) {
        const modal = document.getElementById('episodeModal');
        if (event.target === modal) {
            closeModal();
        }
    }
</script>

<style>
/* Modal styles */
.modal {
    display: none;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgb(0,0,0);
    background-color: rgba(0,0,0,0.4);
    padding-top: 60px;
}

.modal-content {
    background-color: #fefefe;
    margin: 5% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
    max-height: 80%;
    overflow-y: auto;
    border-top: 5px solid #BB2649; /* Top border */
    border-left: 3px solid #BB2649; /* Left border */
    border-right: 3px solid #BB2649; /* Right border */
}

.close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
}

.close:hover,
.close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
}
</style>


 const name = row.querySelector('.name').textContent.trim();
        const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
        
        if (episodePattern) {
            const seriesName = episodePattern[1].trim();
            console.log("正则表达式检查点", seriesName);

            [ANi] 金肉人 完美超人始祖篇 Season 2 - 13 [1080P][Baha][WEB-DL][AAC AVC][CHT][MP4]
            ↑name输入
            ↓seriesName匹配，打印出：
            正则表达式检查点 金肉人 完美超人始祖篇 Season 2        网页 
html css样式抄自 https://cdn.mlsub.net

html文本放在一个index.html命名的文件中，打包为压缩文件上传cloudflare pages即可

其中末尾的

```<script> 
        // 假设数据源 
async function getData() { 
  const xmlUrl = 'https://c.map987.us.kg/https://share.dmhy.org/topics/rss/rss.xml?keyword=%E4%B8%80%E5%85%86';

```
修改为自己绑定域名后的workers地址，后跟动漫花园rss链接

## https://b.map987.us.kg/
流程 :
https://b.map987.us.kg → pages.dev → 打开html会加载rss链接 https://c.map987.us.kg/https://share.dmhy.org → https://workers.dev/https://share.dmhy.org

问题:
1. 用js添加每个名字到html中太慢， <script src="https://cdn.jsdelivr.net/npm/handlebars@latest/dist/handlebars.js"></script> 这个js文件比较大，源网站https://cdn.mlsub.net是纯html页面比较快
2. 添加类似于tvkingdom , itv6.jp的番组表css样式。或者直接，周几的名，都显示在网页最上面，比较方便看，类似

周一 xxx xxx
周二 xxx xxx


搜索 cloudflare workers 代理OpenAI
https://www.mikcool.com/archives/18/
使用 Cloudflare Workers 直接使用 OpenAI API - Mikcool™


```javascript

const TELEGRAPH_URL = 'https://api.openai.com';

addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const url = new URL(request.url);
  url.host = TELEGRAPH_URL.replace(/^https?:\/\//, '');

  const modifiedRequest = new Request(url.toString(), {
    headers: request.headers,
    method: request.method,
    body: request.body,
    redirect: 'follow'
  });

  const response = await fetch(modifiedRequest);
  const modifiedResponse = new Response(response.body, response);

  // 添加允许跨域访问的响应头
  modifiedResponse.headers.set('Access-Control-Allow-Origin', '*');

  return modifiedResponse;
}
```

想要请求 ChatGPT 的 API 时，把官方文档中的 https://api.openai.com/v1/chat/completions 替换成 https://openai.workers.dev 即可

2.搜索workers镜像网站
https://yooer.me/cloudflare-workers-proxy.html
使用CloudFlare的Workers反向代理网站的三种源码1. 镜像整个网站

```javascript
//2023 年 3 月 5 日 23:18:00 修正了 不带 Post 数据的 Bug

// 替换成你想镜像的站点
const upstream = 'www.google.com'

// 如果那个站点有专门的移动适配站点，否则保持和上面一致
const upstream_mobile = 'www.google.com'

// 你希望禁止哪些国家访问
const blocked_region = ['RU']

// 禁止自访问
const blocked_ip_address = ['0.0.0.0', '127.0.0.1']

// 替换成你想镜像的站点
const replace_dict = {
    '$upstream': '$custom_domain',
    '//www.google.com': ''
}

//以下内容都不用动
addEventListener('fetch', event => {
    event.respondWith(fetchAndApply(event.request));
})

async function fetchAndApply(request) {

    const region = request.headers.get('cf-ipcountry').toUpperCase();
    const ip_address = request.headers.get('cf-connecting-ip');
    const user_agent = request.headers.get('user-agent');

    let response = null;
    let url = new URL(request.url);
    let url_host = url.host;

    if (url.protocol == 'http:') {
        url.protocol = 'https:'
        response = Response.redirect(url.href);
        return response;
    }

    if (await device_status(user_agent)) {
        upstream_domain = upstream
    } else {
        upstream_domain = upstream_mobile
    }

    url.host = upstream_domain;

    if (blocked_region.includes(region)) {
        response = new Response('Access denied: WorkersProxy is not available in your region yet.', {
            status: 403
        });
    } else if(blocked_ip_address.includes(ip_address)){
        response = new Response('Access denied: Your IP address is blocked by WorkersProxy.', {
            status: 403
        });
    } else{
        let method = request.method;
        let body = request.body;
        let request_headers = request.headers;
        let new_request_headers = new Headers(request_headers);

        new_request_headers.set('Host', upstream_domain);
        new_request_headers.set('Referer', url.href);

        let original_response = await fetch(url.href, {
            method: method,
            body: body,
            headers: new_request_headers
        })

        let original_response_clone = original_response.clone();
        let original_text = null;
        let response_headers = original_response.headers;
        let new_response_headers = new Headers(response_headers);
        let status = original_response.status;

        new_response_headers.set('access-control-allow-origin', '*');
        new_response_headers.set('access-control-allow-credentials', true);
        new_response_headers.delete('content-security-policy');
        new_response_headers.delete('content-security-policy-report-only');
        new_response_headers.delete('clear-site-data');

        const content_type = new_response_headers.get('content-type');
        if (content_type.includes('text/html') && content_type.includes('UTF-8')) {
            original_text = await replace_response_text(original_response_clone, upstream_domain, url_host);
        } else {
            original_text = original_response_clone.body
        }

        response = new Response(original_text, {
            status,
            headers: new_response_headers
        })
    }
    return response;
}

async function replace_response_text(response, upstream_domain, host_name) {
    let text = await response.text()

    var i, j;
    for (i in replace_dict) {
        j = replace_dict[i]
        if (i == '$upstream') {
            i = upstream_domain
        } else if (i == '$custom_domain') {
            i = host_name
        }

        if (j == '$upstream') {
            j = upstream_domain
        } else if (j == '$custom_domain') {
            j = host_name
        }

        let re = new RegExp(i, 'g')
        text = text.replace(re, j);
    }
    return text;
}

async function device_status (user_agent_info) {
    var agents = ["Android", "iPhone", "SymbianOS", "Windows Phone", "iPad", "iPod"];
    var flag = true;
    for (var v = 0; v < agents.length; v++) { if (user_agent_info.indexOf(agents[v]) > 0) {
            flag = false;
            break;
        }


都有cookie携带，主要是header


https://github.com/copilot/c/ccb4b813-b2ee-4d42-90a6-071bb35f14b1https://nijigennomori.com/kimetsu_awaji/https://github.com/copilot/c/b2e6afff-5b49-4258-bb64-d68334e6623d


function showEpisodes(button) {
            const row = button.closest('tr');
            const name = row.querySelector('.name').textContent.trim();
            const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
            if (episodePattern) {
                const seriesName = episodePattern[1].trim();
                const allRows = document.querySelectorAll('tr.file');
                const episodeList = [];
                allRows.forEach((r) => {
                    const episodeName = r.querySelector('.name').textContent.trim();
                    if (episodeName.includes(seriesName)) {
                        const magnetLink = r.querySelector('a').getAttribute('href');
                        episodeList.push({ name: episodeName, magnet: magnetLink, seriesName: seriesName });
                    }
                });
                displayEpisodes(episodeList);
            }
        }

        function displayEpisodes(episodes) {
            const episodeListDiv = document.getElementById('episodeList');
            episodeListDiv.innerHTML = '<h3 class="episode-list-title"></h3><ul>' +
                episodes.map(e => `<li><a href="${e.magnet}">${e.name}</a></li>`).join('') + '</ul>';
            document.getElementById('episodeModal').style.display = 'block';
            document.body.style.overflow = 'hidden'; // Prevent background scrolling
        }

        function closeModal() {
            document.getElementById('episodeModal').style.display = 'none';
            document.body.style.overflow = ''; // Restore background scrolling
        }

        // Close modal when clicking outside
        window.onclick = function(event) {
            const modal = document.getElementById('episodeModal');
            if (event.target === modal) {
                closeModal();
            }
        }
    </script>，
    现在是显示出
集数列表
[ANi] 超超超超超喜歡你的 100 個女朋友 - 14 [1080P][Baha][WEB-DL][AAC AVC][CHT][MP4]
[ANi] 超超超超超喜歡你的 100 個女朋友 - 13 [1080P][Baha][WEB-DL][AAC AVC][CHT][MP4]

我需要的是仅仅显示一次标题，然后就只显示集数即可

function showEpisodes(button) {
    const row = button.closest('tr');
    const name = row.querySelector('.name').textContent.trim();
    const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
    if (episodePattern) {
        const seriesName = episodePattern[1].trim();
        const allRows = document.querySelectorAll('tr.file');
        const episodeList = [];
        allRows.forEach((r) => {
            const episodeName = r.querySelector('.name').textContent.trim();
            if (episodeName.includes(seriesName)) {
                const magnetLink = r.querySelector('a').getAttribute('href');
                const episodeNumber = episodeName.match(/-\s*(\d+)/)[1]; // 提取集数
                episodeList.push({ episodeNumber: episodeNumber, magnet: magnetLink });
            }
        });
        displayEpisodes(seriesName, episodeList);
    }
}

function displayEpisodes(seriesName, episodes) {
    const episodeListDiv = document.getElementById('episodeList');
    episodeListDiv.innerHTML = `<h3 class="episode-list-title">${seriesName}</h3><ul>` +
        episodes.map(e => `<li><a href="${e.magnet}">Episode ${e.episodeNumber}</a></li>`).join('') + '</ul>';
    document.getElementById('episodeModal').style.display = 'block';
    document.body.style.overflow = 'hidden'; // Prevent background scrolling
}

function closeModal() {
    document.getElementById('episodeModal').style.display = 'none';
    document.body.style.overflow = ''; // Restore background scrolling
}

// Close modal when clicking outside
window.onclick = function(event) {
    const modal = document.getElementById('episodeModal');
    if (event.target === modal) {
        closeModal();
    }
}


transform: translateY(-50%);show-episodes-btnepisodeList<script>
    function showEpisodes(button) {
        const row = button.closest('tr');
        const name = row.querySelector('.name').textContent.trim();
        const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
        if (episodePattern) {
            const seriesName = episodePattern[1].trim();
            const allRows = document.querySelectorAll('tr.file');
            const episodeList = [];
            allRows.forEach((r) => {
                const episodeName = r.querySelector('.name').textContent.trim();
                if (episodeName.includes(seriesName)) {
                    const magnetLink = r.querySelector('a').getAttribute('href');
                    episodeList.push({ name: episodeName, magnet: magnetLink });
                }
            });
            displayEpisodes(episodeList);
        }
    }

    function displayEpisodes(episodes) {
        const episodeListDiv = document.getElementById('episodeList');
        episodeListDiv.innerHTML = '<h3 class="episode-list-title">集数列表</h3><ul>' +
            episodes.map(e => `<li><a href="${e.magnet}">${e.name}</a></li>`).join('') + '</ul>';
        document.getElementById('episodeModal').style.display = 'block';
        document.body.style.overflow = 'hidden'; // Prevent background scrolling
    }

    function closeModal() {
        document.getElementById('episodeModal').style.display = 'none';
        document.body.style.overflow = ''; // Restore background scrolling
    }

    // Close modal when clicking outside
    window.onclick = function(event) {
        const modal = document.getElementById('episodeModal');
        if (event.target === modal) {
            closeModal();
        }
    }
</script>

<style>
/* Modal styles */
.modal {
    display: none;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgb(0,0,0);
    background-color: rgba(0,0,0,0.4);
    padding-top: 60px;
}

.modal-content {
    background-color: #fefefe;
    margin: 5% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
    max-height: 80%;
    overflow-y: auto;
    border-left: 3px solid #BB2649; /* Left border */
    border-right: 3px solid #BB2649; /* Right border */
}

.episode-list-title {
    border-top: 5px solid #BB2649; /* Top border for the title */
    padding-top: 10px;
}

.close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
}

.close:hover,
.close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
}

/* Button styles */
.show-episodes-btn {
    background-color: #BB2649;
    color: white;
    border: none;
    padding: 5px 10px;
    cursor: pointer;
    font-size: 14px;
    margin-right: 10px;
}

.show-episodes-btn:hover {
    background-color: #a02345;
}
</style



<!-- Modal for displaying episodes -->
<div id="episodeModal" class="modal">
    <div class="modal-content">
        <span class="close" onclick="closeModal()">&times;</span>
        <div id="episodeList"></div>
    </div>
</div>

<script>
    function showEpisodes(button) {
        const row = button.closest('tr');
        const name = row.querySelector('.name').textContent.trim();
        const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
        if (episodePattern) {
            const seriesName = episodePattern[1].trim();
            const allRows = document.querySelectorAll('tr.file');
            const episodeList = [];
            allRows.forEach((r) => {
                const episodeName = r.querySelector('.name').textContent.trim();
                if (episodeName.includes(seriesName)) {
                    const magnetLink = r.querySelector('a').getAttribute('href');
                    episodeList.push({ name: episodeName, magnet: magnetLink });
                }
            });
            displayEpisodes(episodeList);
        }
    }

    function displayEpisodes(episodes) {
        const episodeListDiv = document.getElementById('episodeList');
        episodeListDiv.innerHTML = '<h3>集数列表</h3><ul>' +
            episodes.map(e => `<li><a href="${e.magnet}">${e.name}</a></li>`).join('') + '</ul>';
        document.getElementById('episodeModal').style.display = 'block';
        document.body.style.overflow = 'hidden'; // Prevent background scrolling
    }

    function closeModal() {
        document.getElementById('episodeModal').style.display = 'none';
        document.body.style.overflow = ''; // Restore background scrolling
    }

    // Close modal when clicking outside
    window.onclick = function(event) {
        const modal = document.getElementById('episodeModal');
        if (event.target === modal) {
            closeModal();
        }
    }
</script>

<style>
/* Modal styles */
.modal {
    display: none;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgb(0,0,0);
    background-color: rgba(0,0,0,0.4);
    padding-top: 60px;
}

.modal-content {
    background-color: #fefefe;
    margin: 5% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
    max-height: 80%;
    overflow-y: auto;
    border-top: 5px solid #BB2649; /* Top border */
    border-left: 3px solid #BB2649; /* Left border */
    border-right: 3px solid #BB2649; /* Right border */
}

.close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
}

.close:hover,
.close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
}
</style>


 const name = row.querySelector('.name').textContent.trim();
        const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
        
        if (episodePattern) {
            const seriesName = episodePattern[1].trim();
            console.log("正则表达式检查点", seriesName);

            [ANi] 金肉人 完美超人始祖篇 Season 2 - 13 [1080P][Baha][WEB-DL][AAC AVC][CHT][MP4]
            ↑name输入
            ↓seriesName匹配，打印出：
            正则表达式检查点 金肉人 完美超人始祖篇 Season 2         const allRows = document.querySelectorAll('tr.file');
        const episodeList = [];
        allRows.forEach((r) => {
            const episodeName = r.querySelector('.name').textContent.trim();
            if (episodeName.includes(seriesName)) {
                const magnetLink = r.querySelector('a').getAttribute('href');
                const episodeNumber = episodeName.match(/-\s*(\d+)/)[1]; // 提取集数
                episodeList.push({ episodeNumber: episodeNumber, magnet: magnetLink });
            }
        });
        displayEpisodes(seriesName, episodeList);
    }
}

function displayEpisodes(seriesName, episodes) {
    const episodeListDiv = document.getElementById('episodeList');
    episodeListDiv.innerHTML = `<h3 class="episode-list-title">${seriesName}</h3><ul>` +
        episodes.map(e => `<li><a href="${e.magnet}">Episode ${e.episodeNumber}</a></li>`).join('') + '</ul>';
    document.getElementById('episodeModal').style.display = 'block';
    document.body.style.overflow = 'hidden'; // Prevent background scrolling
}

function closeModal() {
    document.getElementById('episodeModal').style.display = 'none';
    document.body.style.overflow = ''; // Restore background scrolling
}

// Close modal when clicking outside
window.onclick = function(event) {
    const modal = document.getElementById('episodeModal');
    if (event.target === modal) {
        closeModal();
    }
}


transform: translateY(-50%);show-episodes-btnepisodeList<script>
    function showEpisodes(button) {
        const row = button.closest('tr');
        const name = row.querySelector('.name').textContent.trim();
        const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
        if (episodePattern) {
            const seriesName = episodePattern[1].trim();
            const allRows = document.querySelectorAll('tr.file');
            const episodeList = [];
            allRows.forEach((r) => {
                const episodeName = r.querySelector('.name').textContent.trim();
                if (episodeName.includes(seriesName)) {
                    const magnetLink = r.querySelector('a').getAttribute('href');
                    episodeList.push({ name: episodeName, magnet: magnetLink });
                }
            });
            displayEpisodes(episodeList);
        }
    }

    function displayEpisodes(episodes) {
        const episodeListDiv = document.getElementById('episodeList');
        episodeListDiv.innerHTML = '<h3 class="episode-list-title">集数列表</h3><ul>' +
            episodes.map(e => `<li><a href="${e.magnet}">${e.name}</a></li>`).join('') + '</ul>';
        document.getElementById('episodeModal').style.display = 'block';
        document.body.style.overflow = 'hidden'; // Prevent background scrolling
    }

    function closeModal() {
        document.getElementById('episodeModal').style.display = 'none';
        document.body.style.overflow = ''; // Restore background scrolling
    }

    // Close modal when clicking outside
    window.onclick = function(event) {
        const modal = document.getElementById('episodeModal');
        if (event.target === modal) {
            closeModal();
        }
    }
</script>

<style>
/* Modal styles */
.modal {
    display: none;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgb(0,0,0);
    background-color: rgba(0,0,0,0.4);
    padding-top: 60px;
}

.modal-content {
    background-color: #fefefe;
    margin: 5% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
    max-height: 80%;
    overflow-y: auto;
    border-left: 3px solid #BB2649; /* Left border */
    border-right: 3px solid #BB2649; /* Right border */
}

.episode-list-title {
    border-top: 5px solid #BB2649; /* Top border for the title */
    padding-top: 10px;
}

.close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
}

.close:hover,
.close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
}

/* Button styles */
.show-episodes-btn {
    background-color: #BB2649;
    color: white;
    border: none;
    padding: 5px 10px;
    cursor: pointer;
    font-size: 14px;
    margin-right: 10px;
}

.show-episodes-btn:hover {
    background-color: #a02345;
}
</style



<!-- Modal for displaying episodes -->
<div id="episodeModal" class="modal">
    <div class="modal-content">
        <span class="close" onclick="closeModal()">&times;</span>
        <div id="episodeList"></div>
    </div>
</div>

<script>
    function showEpisodes(button) {
        const row = button.closest('tr');
        const name = row.querySelector('.name').textContent.trim();
        const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
        if (episodePattern) {
            const seriesName = episodePattern[1].trim();
            const allRows = document.querySelectorAll('tr.file');
            const episodeList = [];
            allRows.forEach((r) => {
                const episodeName = r.querySelector('.name').textContent.trim();
                if (episodeName.includes(seriesName)) {
                    const magnetLink = r.querySelector('a').getAttribute('href');
                    episodeList.push({ name: episodeName, magnet: magnetLink });
                }
            });
            displayEpisodes(episodeList);
        }
    }

    function displayEpisodes(episodes) {
        const episodeListDiv = document.getElementById('episodeList');
        episodeListDiv.innerHTML = '<h3>集数列表</h3><ul>' +
            episodes.map(e => `<li><a href="${e.magnet}">${e.name}</a></li>`).join('') + '</ul>';
        document.getElementById('episodeModal').style.display = 'block';
        document.body.style.overflow = 'hidden'; // Prevent background scrolling
    }

    function closeModal() {
        document.getElementById('episodeModal').style.display = 'none';
        document.body.style.overflow = ''; // Restore background scrolling
    }

    // Close modal when clicking outside
    window.onclick = function(event) {
        const modal = document.getElementById('episodeModal');
        if (event.target === modal) {
            closeModal();
        }
    }
</script>

<style>
/* Modal styles */
.modal {
    display: none;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgb(0,0,0);
    background-color: rgba(0,0,0,0.4);
    padding-top: 60px;
}

.modal-content {
    background-color: #fefefe;
    margin: 5% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
    max-height: 80%;
    overflow-y: auto;
    border-top: 5px solid #BB2649; /* Top border */
    border-left: 3px solid #BB2649; /* Left border */
    border-right: 3px solid #BB2649; /* Right border */
}

.close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
}

.close:hover,
.close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
}
</style>


 const name = row.querySelector('.name').textContent.trim();
        const episodePattern = name.match(/\[ANi\]\s*(.*?)\s*-\s*\d+/);
        
        if (episodePattern) {
            const seriesName = episodePattern[1].trim();
            console.log("正则表达式检查点", seriesName);

            [ANi] 金肉人 完美超人始祖篇 Season 2 - 13 [1080P][Baha][WEB-DL][AAC AVC][CHT][MP4]
            ↑name输入
            ↓seriesName匹配，打印出：
            正则表达式检查点 金肉人 完美超人始祖篇 Season 2





github中
/.github/workflow/run.yml放置下面这个

```
name: 部署 cloudflare Pages 个人网页
on:
push:
    branches:
    - main

jobs:
    publish:
        runs-on: ubuntu-latest
        permissions:
            contents: read
            deployments: write
        name: Publish to Cloudflare Pages
        steps:
            - name: Checkout
            uses: actions/checkout@v3

                # Run a build step here if your project requires

            - name: Publish to Cloudflare Pages
            uses: cloudflare/pages-action@v1
            with:
                apiToken: ${{ secrets.CF_TOKEN }}
                accountId: ${{ secrets.CF_ID }}
                projectName: ${{ secrets.CF_NAME }}
                directory: ./
                wranglerVersion: '3'


```
注意projectName是dmhyorg.pages.dev中的dmhyorg，

api是cloudflare申请的api ，accountId就是cloudflare控制台主页，链接后面始终跟着，每个用户唯一

https://dash.cloudflare.com/xxxxxx/

在github项目设置中设置，填写好serect分别为 CF_TOKEN ，CF_ID，CF_NAME，然后填写对应的值，这样之后每次github更新，会通过api链接推送到pages。
