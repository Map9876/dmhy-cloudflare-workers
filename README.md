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


都有cookie携带，主要是headers
