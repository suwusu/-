# .babelrc

[babel详解](https://juejin.im/post/5a79adeef265da4e93116430)

## 什么是Babel


官方解释,是下一代JavaScript 语法的编译器。
既然是下一代Javascript的标准，浏览器因版本的不同对此会有兼容性问题，JavaScript的新的方法都不能使用，但是目前我们在项目开发一直提倡使用最新的语法糖编写，不但能减少代码量，而且async,await等新特性还解决了回调的编写机制，减轻了代码维护成本。
Babel就因此而生，它可以让你放心使用大部分的JavaScript的新的标准的方法，然后编译成兼容绝大多数的主流浏览器的代码。在项目工程脚手架中，一般会使用.babelrc文件，通过配置一些参数配合webpack进行打包压缩。也通过网上了解，写法各有不同，参数也大不相同，因此，我重新整理一份资料，详细的介绍下各个配置项的意义所在，以便清晰了解如果使用。



## Babel转译器

在.babelrc配置文件中，主要是对预设（presets）和插件（plugins）进行配置，因此不同的转译器作用不同的配置项，大致可分为以下三项：

```
1.语法转义器。主要对javascript最新的语法糖进行编译，并不负责转译javascript新增的api和全局对象。例如let/const就可以被编译，而includes/Object.assign等并不能被编译。常用到的转译器包有，babel-preset-env、babel-preset-es2015、babel-preset-es2016、babel-preset-es2017、babel-preset-latest等。在实际开发中可以只选用babel-preset-env来代替余下的，但是还需要配上javascirpt的制作规范一起使用，同时也是官方推荐

```

```
{
  "presets": ["env", {
      "modules": false
    }],
    "stage-2"
}

```



```
2.补丁转义器。主要负责转译javascript新增的api和全局对象，例如babel-plugin-transform-runtime这个插件能够编译Object.assign,同时也可以引入babel-polyfill进一步对includes这类用法保证在浏览器的兼容性。Object.assign 会被编译成以下代码：

```

```
__WEBPACK_IMPORTED_MODULE_1_babel_runtime_core_js_object_assign___default()

```



```
3.jsx和flow插件，这类转译器用来转译JSX语法和移除类型声明的，使用Rect的时候你将用到它，转译器名称为babel-preset-react

```



## 总结

.babelrc配置文件主要还是以presets和plugins组成，通过和webpack配合进行使用，分享下我们在项目中常用的配置。以上都是通过学习总结出来的，有什么不对的地方希望指出。

vue项目开发使用的配置如下：

```
{
  "presets": [
    ["env", {
      "modules": false
    }],
    "stage-2"
  ],
  // 下面指的是在生成的文件中，不产生注释
  "comments": false,
  "plugins": ["transform-runtime","syntax-dynamic-import"],
  "env": {
    // test 是提前设置的环境变量，如果没有设置BABEL_ENV则使用NODE_ENV，如果都没有设置默认就是development
    "test": {
      "presets": ["env", "stage-2"],
      // instanbul是一个用来测试转码后代码的工具
      "plugins": ["istanbul"]
    }
  }
}

```


react项目开发使用的配置如下：

```
{
  "presets": [
    ["env", { "modules": false }],
    "stage-2",
    "react"
  ],
  "plugins": ["transform-runtime"],
  "comments": false,
  "env": {
    "test": {
      "presets": ["env", "stage-2"],
      "plugins": [ "istanbul" ]
    }
  }
}

```

