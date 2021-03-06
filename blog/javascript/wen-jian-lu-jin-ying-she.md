# 路径映射

1. 在node后台的书写中，为了更好的路由模块化，每一个接口都能写在单独的文件里方便阅读和修改。我在腾讯提供的小程序后台demo里发现了这比较好的方法。

   > 目的：读取某个文件夹下的文件，并映射成为 {文件名：文件内容} 的形式  
   > 以下是其中的源码
   >
   > \`\`\` const \_ = require\('lodash'\) const fs = require\('fs'\) const path = require\('path'\)

/\*\*

* 映射 d 文件夹下的文件为模块 \*/ const mapDir = d =&gt; { const tree = {}

  // 获得当前文件夹下的所有的文件夹和文件 const \[dirs, files\] = \_\(fs.readdirSync\(d\)\).partition\(p =&gt; fs.statSync\(path.join\(d, p\)\).isDirectory\(\)\)

  // 映射文件夹 dirs.forEach\(dir =&gt; { tree\[dir\] = mapDir\(path.join\(d, dir\)\) }\)

  // 映射文件 files.forEach\(file =&gt; { if \(path.extname\(file\) === '.js'\) { tree\[path.basename\(file, '.js'\)\] = require\(path.join\(d, file\)\) } }\)

  return tree }

// 默认导出当前文件夹下的映射 module.exports = mapDir\(path.join\(\_\_dirname\)\)

```text
// 运行结果如下
```

{ dir: { 'file1': \[Function\] }, file1: \[Function\], file2: \[Function\], file3: \[Function\], }

```text
> 这样就能动态的实现一接口对应一文件，以下就来依次解释以下代码中的内容把

### 分布阅读
1. 声明tree对象来储存结果
```

const tree = {}

```text
2. 获得当前文件夹下的所有的文件夹和文件,并储存为数组
```

const \[dirs, files\] = \_\(fs.readdirSync\(d\)\).partition\(p =&gt; fs.statSync\(path.join\(d, p\)\).isDirectory\(\)\) console.log\(dirs, files\) // 运行结果如下 \[ 'dir' \] \[ 'file1.js', 'file2.js', 'file3.js', 'index.js'\]

```text
> 分析下各个函数的作用
// 数组读取所有文件及文件夹
```

fs.readdirSync\(d\) // 数组读取所有文件及文件夹 \[ 'dir', 'file1.js', 'file2.js', 'file3.js', 'index.js'\]

```text
// 判断是否是文件夹
```

fs.statSync\(path.join\(d, p\)\).isDirectory\(\) // 判断是否是文件夹

true false false false false

```text
// 根据是否为文件分为两个数组
```

\_\(fs.readdirSync\(d\)\).partition\(/_true or false_/\)

```text
3. 映射文件夹里的文件
```

// 映射文件夹 dirs.forEach\(dir =&gt; { tree\[dir\] = mapDir\(path.join\(d, dir\)\) }\)

```text
4. 映射js文件
```

files.forEach\(file =&gt; { if \(path.extname\(file\) === '.js'\) { tree\[path.basename\(file, '.js'\)\] = require\(path.join\(d, file\)\) // path.basename\(file, '.js'\) 文件名 去除 .js // path.join\(d, file\) 完整路径 } }\)

\`\`\` 5. 应用 ![&#x6587;&#x4EF6;](https://i.loli.net/2018/09/09/5b949ab716d62.png) ![&#x63A5;&#x53E3;](https://i.loli.net/2018/09/09/5b949ab74e380.png)

