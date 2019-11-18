---
title: Calling api within Create-react-app 
date: 2019-11-19 17:41:03
thumbnail: 'https://i.imgur.com/yRDVADV.png'
tags: technical
---

Most of frontend application need to fetching data from server side to display data. After some try and error this is how I developed and deployed react application

<!-- more -->

## Development - proxy for stay away from CORS

The most painful things is CORS, at the first time I always need to inject `Access-Control-Allow-Origin` to the header for supporting calling api from localhost.

Using the proxy like this you can call the api from `/api/v1`

```
app.use(
  '/api/v1',
  proxy({
    target: 'http://xx.yy.zz.zzz/',
    changeOrigin: true,
    onProxyReq(proxyReq, req) {
      if (proxyReq.getHeader("origin")) {
        proxyReq.removeHeader("origin")
      }
    }
  })
);
```

Every requests to `/api/v1` will be forwarded to `http://xx.yy.zz.zzz/api/v1`. 
Be notice how the path will be applied. The more information can be found [here](https://create-react-app.dev/docs/proxying-api-requests-in-development/).

## Deployment - using nginx for serving and forward api

I used an nginx instance to serve static file and forward the api. The configuration can be like this

```
// nginx.conf

server 
{
  listen 80;
  location / {
      root /usr/share/nginx/html;
      index index.html index.htm;
      try_files $uri $uri/ /index.html =404;
  }

  location /api/v1/ {
    proxy_pass http://xx.yy.zz.zzz/api/v1/
  }
}
``` 

Every requests will be forwarded also. Do notice of the path, that's a slight difference between 2 methods.

Tell me if you have any better ideas.