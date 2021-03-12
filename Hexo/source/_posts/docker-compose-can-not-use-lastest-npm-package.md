---
title: docker-compose can not use lastest npm package
date: 2021-03-12 21:04:12
tags: [cmder, webstorm, sublime]
categories: Tools
e_title: docker-compose-can-not-use-lastest-npm-package
---

## 正文

前端本地开发时，使用了 docker，但遇到了一个奇怪的问题：本地 npm 包更新后，本地开发一直无法拿到最新代码

```json
"dependencies": {
  "month-view-page-common": "file:packages/common",
},
```

最后发现，原来是 docker 使用了缓存的 volumes。解决方法：

```bash
# rebuild, while making sure it doesn't use the internal cache 
docker-compose build --no-cache
docker-compose up
```

> docker 不熟悉呀，需要加强 2021-03-12

## 参考资料

1. [NodeJS: Making docker-compose Volumes Ignore the node_modules Directory](https://medium.com/@semur.nabiev/how-to-make-docker-compose-volumes-ignore-the-node-modules-directory-99f9ec224561)   
2. ["docker-compose up" not rebuilding container that has an underlying updated image #4337](https://github.com/docker/compose/issues/4337)
