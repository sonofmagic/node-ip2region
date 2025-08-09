# ip2region-ts

> forked from [ip2region](https://github.com/lionsoul2014/ip2region), Thanks to [Wu Jian Ping](https://github.com/wujjpp) , [lionsoul2014](https://github.com/lionsoul2014) and all ip2region Contributors!

`ip2region` `node sdk` 的 `typescript` 版本，同时内置 `ip2region.xdb` 作为默认的数据库。

默认自带的 `ip2region.xdb` 版本日期为 `20250809`，后续有改动会进行更新，[源码中的文件位置](https://github.com/lionsoul2014/ip2region/blob/master/data/ip2region.xdb)。

制作此版本是由于 `npm` 上目前的版本比较老，且不能开箱即用，于是 `fork` 一下重新发个包给自己用。

同时此版本兼容 `cjs`(`commonjs`) 和 `esm`(`module`) 两种包引入方式。

## 使用方式

### 安装导入包

```sh
<npm/yarn/pnpm> i ip2region-ts
```

```js
// 导入包
// commonjs
const Searcher = require('ip2region-ts')
// or esm/ts
import * as Searcher from 'ip2region-ts'
```

### 内置 xdb 默认路径

```js
const dbPath = Searcher.defaultDbFile
```

### 完全基于文件的查询

```js
// 指定ip2region数据文件路径
const dbPath = Searcher.defaultDbFile
// or 'path/to/ip2region.xdb file path'

try {
  // 创建searcher对象
  const searcher = Searcher.newWithFileOnly(dbPath)
  // 查询
  const data = await searcher.search('218.4.167.70')
  // data: {region: '中国|0|江苏省|苏州市|电信', ioCount: 3, took: 1.342389}
}
catch (e) {
  console.log(e)
}
```

### 缓存 `VectorIndex` 索引

```js
// 指定ip2region数据文件路径
const dbPath = Searcher.defaultDbFile // 'ip2region.xdb file path'

try {
  // 同步读取vectorIndex
  const vectorIndex = Searcher.loadVectorIndexFromFile(dbPath)
  // 创建searcher对象
  const searcher = Searcher.newWithVectorIndex(dbPath, vectorIndex)
  // 查询 await 或 promise均可
  const data = await searcher.search('218.4.167.70')
  // data: {region: '中国|0|江苏省|苏州市|电信', ioCount: 2, took: 0.402874}
}
catch (e) {
  console.log(e)
}
```

### 缓存整个 `xdb` 数据

```js
// 指定ip2region数据文件路径
const dbPath = Searcher.defaultDbFile // or 'ip2region.xdb file path'

try {
  // 同步读取buffer
  const buffer = Searcher.loadContentFromFile(dbPath)
  // 创建searcher对象
  const searcher = Searcher.newWithBuffer(buffer)
  // 查询 await 或 promise均可
  const data = await searcher.search('218.4.167.70')
  // data: {region:'中国|0|江苏省|苏州市|电信', ioCount: 0, took: 0.063833}
}
catch (e) {
  console.log(e)
}
```
