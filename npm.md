**package.json 选项**

```js
“scripts”: {
  “prebuild”:….,   // npm run build 前执行
   “build”:….,
   “postbuild”:….  // npm run build 后执行
}
```

**npm script**

```shell
# npm 参数 --argname
npm run --test 
process.argv.includes('--test') // false . node 会把所有--参数当成node参数而不是运行参数

# 自定义参数 -- 会把之后的所有参数存入process.argv
npm run command -- --someArg 
npm run -- --test 
process.argv.includes('--test') // true
```



