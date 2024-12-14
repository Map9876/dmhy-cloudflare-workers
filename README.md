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


