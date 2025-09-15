---
title: 'UnoCSS与样式'
description: '更改网站外观和创建自定义样式'
order: 6
---

## UnoCSS更改外观

你可以通过修改 `src/assets/styles/app.css` 中的CSS文件来自定义主题默认的UnoCSS调色板。例如，如果你想更改默认主题颜色，可以修改以下代码:

```css title="src/assets/styles/app.css"
:root {
  /* ... */
  --primary: 200 29% 45%; /* [!code --] */
  --primary: <你喜欢的颜色使用原始hsl>; /* [!code ++] */
}
```

同样地，要更改默认字体系列，可以修改以下代码:

```css title="src/assets/styles/app.css"
:root {
  /* ... */
  font-family: 'Satoshi'; /* [!code --] */
  src: url('/fonts/Satoshi-Variable.ttf'); /* [!code --] */
  font-family: '<你喜欢的字体系列>'; /* [!code ++] */
  src: url('/fonts/<你喜欢的字体>.ttf'); /* [!code ++] */
}
```

确保将你的自定义字体文件放在 `public/fonts` 目录中。

## UnoCSS配置

文件: `uno.config.ts`

了解更多:

- [UnoCSS: Astro集成](https://unocss.dev/integrations/astro)
- [UnoCSS: 配置UnoCSS](https://unocss.dev/config)

## `@unocss/preset-typography`

排版配置可以在 `uno.config.ts` 中修改:

```js title="uno.config.ts"
const typographyConfig = {
  // prettier-ignore
  cssExtend: { // [!code highlight:3]
    // ...
  }
}
```

但如果你想通过UnoCSS自定义排版，或只是更改预设排版主题，可以在 `src/site.config.ts` 中进行:

```ts title="src/site.config.ts"
export const integ: IntegrationUserConfig = {
  // ...
  typography: {
    class: 'prose text-base text-muted-foreground' // [!code highlight]
  }
}
```

查看[排版预设](https://unocss.dev/presets/typography)了解更多信息。