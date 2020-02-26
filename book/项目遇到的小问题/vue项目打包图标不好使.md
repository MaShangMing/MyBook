在vue打包完，发现很多问题，来整理下：

第一：使用element框架的icon时候，开发环境下是没有问题的，打包完以后出现小方块，页面不显示，

解决办法：找到utils.js文件，加 publicPath: '../../'



function generateLoaders (loader, loaderOptions) {
    const loaders = options.usePostCSS ? [cssLoader, postcssLoader] : [cssLoader]

```js
if (loader) {
  loaders.push({
    loader: loader + '-loader',
    options: Object.assign({}, loaderOptions, {
      sourceMap: options.sourceMap
    })
  })
}
 
// Extract CSS when that option is specified
// (which is the case during production build)
if (options.extract) {
  return ExtractTextPlugin.extract({
    use: loaders,
    fallback: 'vue-style-loader',
    publicPath: '../../',//解决ele小图标出不来问题
  })
} else {
  return ['vue-style-loader'].concat(loaders)
}
```
  }
第二：打包完以后路径不对了，加载文件，图片不出来

解决办法：找到config里面的index.js，加入assetsPublicPath: './',



build: {
    // Template for index.html
    index: path.resolve(__dirname, '../dist/index.html'),

```js
// Paths
assetsRoot: path.resolve(__dirname, '../dist'),
assetsSubDirectory: 'static',
assetsPublicPath: './', //解决打包完路径不对的问题
```