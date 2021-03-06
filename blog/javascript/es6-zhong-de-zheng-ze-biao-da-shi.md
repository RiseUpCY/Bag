# es6中的命名捕获

1. \(?&lt;别名&gt;\) 在正则表达式中以这样的形势就能给子表达式取一个别名，以获得更好的代码可读性

   > 现在有一个字符串，'2018-09-06'，我需要分别获取他的年月日，以前的做法就是：
   >
   > ```text
   > let str =  '2018-09-06'
   > let reg = /(\d{4})-(\d{2})-(\d{2})/
   > let arr = str.match(reg)
   > let [year, month, day] = [arr[1], arr[2], arr[3]]
   > ```
   >
   > 但是现在新版的es中提供了一个更加便于理解的方法,现在很多浏览器还不支持，新版谷歌可用
   >
   > \`\`\` let str = '2018-09-06' let reg1 = /\(?\d{4}\)-\(?\d{2}\)-\(?\d{2}\)/

let arr = str.match\(reg1\) let {year, month, day} = arr.groups

```text
#### 命名捕获在replace中的使用
> 之前的做法
```

let str = '2018-09-06' let reg = /\(\d{4}\)-\(\d{2}\)-\(\d{2}\)/ let str1 = str.replace\(reg,'$2/$3/$1'\);

console.log\(str1\) // 09/06/2018

```text
> es2018的做法
```

let str = '2018-09-06' let reg = /\(?\d{4}\)-\(?\d{2}\)-\(?\d{2}\)/ let newstr = str.replace\(reg, '$/$/$'\)

console.log\(newstr\) // 09/06/2018

```text
> 当然replace的回掉函数方式也是可以实现的
```

let str = '2018-09-06'; let reg = /\(?\d{4}\)-\(?\d{2}\)-\(?\d{2}\)/ let str2 = str.replace\(reg,\(...args\)=&gt;{ let {year,month,day} = args\[args.length - 1\] return `${year}/${month}/${day}`; }\) console.log\(str2\) // 2018/09/06

```text
#### 反向引用
1. 以前反向引用子表达式的做法如下
```

let str2 = 'welcome-welcome-welcame' let reg2 = /\(welcome\)\(-\)\1\2/

console.log\(str2.match\(reg2\)\) // welcome-welcome-

```text
2. 新es标准里可以通过命名捕获来反向引用, 只需要 \k<别名>就好了
```

let str2 = 'welcome-welcome-welcame' let reg2 = /\(?welcome\)-\k-/

console.log\(str2.match\(reg2\)\) // welcome-welcome- \`\`\`

