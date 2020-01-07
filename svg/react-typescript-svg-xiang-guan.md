# react typescript svg相关

引入了`svgr` 

```bash
yarn add @svgr/webpack -D
```

添加`custom.d.ts`

```typescript
declare interface SvgrComponent
  extends React.StatelessComponent<React.SVGAttributes<SVGElement>> {}

declare module '*.svg' {
  const content: SvgrComponent
  export default content
}
```

编辑`tsconfig.json` 

```javascript
{
    "files": ["custom.d.ts"]
}
```

编辑`storybook` `webpack.config.js`

```javascript
config.module.rules = config.module.rules.map(data => {
    if (/svg\|/.test(String(data.test)))
      data.test = /\.(ico|jpg|jpeg|png|gif|eot|otf|webp|ttf|woff|woff2|cur|ani)(\?.*)?$/;
    return data;
  });

  config.module.rules.push({
    test: /\.svg$/,
    use: [
      {
        loader: '@svgr/webpack'
      }
    ]
  });
```

在另外的工程使用了`parcel` ，也要引入`svgr` 

```javascript
yarn add @svgr/parcel-plugin-svgr -D
```

