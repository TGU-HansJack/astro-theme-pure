---
title: '常见问题'
description: '常见问题解答'
order: 4
---

## 路由

### 博客特定路由

博客路由格式如 `/blog/:year/:id`

参见 [4.0.2-beta如何使文章链接中包含年份](https://github.com/cworld1/astro-theme-pure/discussions/37#discussioncomment-11905851)。

## 内容

### `heroImage`支持网络图片

应该与 `inferSize: true` 一起使用以获取图片尺寸。例如：

```yaml
heroImage:
  { src: 'https://img.tukuppt.com/ad_preview/00/15/09/5e715a320b68e.jpg!/fw/980', inferSize: true }
```

## Vite

### Vite阻止请求

```log
Blocked request. This host ("xxx")is not allowed.
To allow this host, add "xxx" to `preview.allowedHosts` in vite.config.js.
```

参见 [option server.allowedHosts doesn't take into account "true"](https://github.com/vitejs/vite/issues/19242)

## 包

### `BUN_LINK_PKG`问题

参见 [BUN_LINK_PKG 环境变量无法设置成功](https://github.com/cworld1/astro-theme-pure/issues/51)