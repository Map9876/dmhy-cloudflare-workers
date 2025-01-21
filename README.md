# dmhy-cloudflare-workers
使用cloudflare workers直连动漫花园 dmhy
# https://dmhyorg.pages.dev/
20250122
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
