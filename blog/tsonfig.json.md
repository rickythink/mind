# TypeScript tsconfig.json 整理

## 引入配置

文件相关的配置确保 TypeScript 能包含正确的文件

### include

设置一个包含文件名的数组或匹配规则，指明那些文件是引入到程序中的。文件名基于包含`tsconfig.json`文件的路径解析

```
// settings
{
	"include": ["src/**/*", "tests/**/*"]
}

// results
.
├── scripts                ⨯
│   ├── lint.ts            ⨯
│   ├── update_deps.ts     ⨯
│   └── utils.ts           ⨯
├── src                    ✓
│   ├── client             ✓
│   │    ├── index.ts      ✓
│   │    └── utils.ts      ✓
│   ├── server             ✓
│   │    └── index.ts      ✓
├── tests                  ✓
│   ├── app.test.ts        ✓
│   ├── utils.ts           ✓
│   └── tests.d.ts         ✓
├── package.json
├── tsconfig.json
└── yarn.lock
```

**默认值**

如果定义了`files`配置，则为 `[ ]` ；否则为 `[" ** / * "]`

### exclude

设置一个包含文件名的数组或匹配规则,指明忽略哪些`include`中的配置

`exclude` 仅对 `include` 配置中的文件生效。一个被 `exclude` 设置的文件依然可以被代码引入依赖，因此，这并不是一个避免文件被使用的设置，这仅仅是用来改变 `include` 的设置

**默认值**

`["node_modules", "bower_components", "jspm_packages"]` 和 outdir

### files

设置一个包含文件名的数组，指明包含到程序中的文件。这通常应用于你仅需少量文件时，否则使用`include`是更好的建议

```
{
  "compilerOptions": {},
  "files": [
    "core.ts",
    "sys.ts",
    "types.ts",
    "scanner.ts",
    "parser.ts",
    "utilities.ts",
    "binder.ts",
    "checker.ts",
    "tsc.ts"
  ]
}
```

**默认**

false

### extends

继承通用的设置。如果有重名的设置，则新定义的设置会覆盖被继承的配置。extends 配置对于大型项目的 `tcsonfig.json` 的复用帮助很大

```
{
  "compilerOptions": {
    "noImplicitAny": true,
    "strictNullChecks": true
  }
}
```

```
{
  "extends": "./configs/base",
  "files": ["main.ts", "supplemental.ts"]
}
```

## 项目配置

项目配置决定了你希望项目是如何运行的

### allowJS

是否允许引入 JavaScript 文件。默认仅允许引入 `.ts` 和 `.tsx` 文件

**默认**

`false`

### checkJS

是否报告 JavaScript 文件中的错误。此选项依赖 `allowJS` 为 `true`

例如，根据 TypeScript 自带的 `parseFloat` 类型定义，这是不正确的 JavaScript：

```
// parseFloat 仅接受一个字符串作为参数
module.exports.pi = parseFloat(3.124);
```

当引入到一个 TypeScript 模块：

```
// @filename: constants.js
module.exports.pi = parseFloat(3.124);

// @filename: index.ts
import { pi } from "./constants";
console.log(pi);Try
```

你将不会得到任何错误。但是如果你开启了 `checkJs` 选项，那么你可以从 JavaScript 文件中得到错误信息。

**默认**

false

### declaration

是否要在编译时输出`.d.ts`文件。`.d.ts` 文件用于向外部模块描述API的类型定义文件。

当 `declaration` 设置为 `true` 时，用编译器执行下面的 TypeScript 代码：

```
export let helloWorld = "hi";Try
```

将会生成如下这样的 `index.js` 文件：

```
export let helloWorld = "hi";Try
```

以及一个相应的 `helloWorld.d.ts`：

```
export declare let helloWorld: string;
```

**默认值**

false

### lib

TypeScript 包括一组默认的内建 JS 接口（例如 Math）的类型定义，以及在浏览器环境中存在的对象的类型定义（例如 document）

TypeScript 还包括与你指定的 target 选项相匹配的较新的 JS 特性的 API。例如如果target 为 ES6 或更新的环境，那么 Map 的类型定义是可用的。

最小化的依赖引入能避免下游包被污染的可能性。我们需要谨慎的选择`lib`需要包含的库。

[高阶库](https://www.notion.so/dce02bcf3bf14a7a9d20a3b1f18dd371)

### jsx

控制 JSX 在 JavaScript 文件中的输出方式。 这只影响 .tsx 文件的 JS 文件输出。

* `react`: 将 JSX 改为等价的对 `React.createElement` 的调用并生成 `.js` 文件。
* `react-jsx`: 改为 `__jsx` 调用并生成 `.js` 文件。
* `react-jsxdev`: 改为 `__jsx` 调用并生成 `.js` 文件。
* `preserve`: 不对 JSX 进行改变并生成 `.jsx` 文件。
* `react-native`: 不对 JSX 进行改变并生成 `.js` 文件。

示例代码：

```
export const helloWorld = () => <h1>Hello world</h1>;
```

默认为： `"react"`

```
export const helloWorld = () => React.createElement("h1", null, "Hello world");Try

```

保留: `"preserve"`

```
export const helloWorld = () => <h1>Hello world</h1>;Try

```

React Native: `"react-native"`

```
export const helloWorld = () => <h1>Hello world</h1>;Try

```

React 17 转换: `"react-jsx"`[\[1\]](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)

```
import { jsx as _jsx } from "react/jsx-runtime";
export const helloWorld = () => _jsx("h1", { children: "Hello world" }, void 0);Try

```

React 17 开发模式转换: `"react-jsxdev"`[\[1\]](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)

```
import { jsxDEV as _jsxDEV } from "react/jsx-dev-runtime";
const _jsxFileName = "/home/runner/work/TypeScript-Website/TypeScript-Website/packages/typescriptlang-org/index.tsx";
export const helloWorld = () => _jsxDEV("h1", { children: "Hello world" },

```

### module

这设置了`tsc`编译打包的模块系统。改变`module`同时会影响`moduleResolution`

示例

```
// @filename: index.ts
import { valueOfPi } from "./constants";

export const twoPi = valueOfPi * 2;
```

#### CommonJS

```
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
exports.twoPi = void 0;
const constants_1 = require("./constants");
exports.twoPi = constants_1.valueOfPi * 2;
```

#### UMD

```
(function (factory) {
    if (typeof module === "object" && typeof module.exports === "object") {
        var v = factory(require, exports);
        if (v !== undefined) module.exports = v;
    }
    else if (typeof define === "function" && define.amd) {
        define(["require", "exports", "./constants"], factory);
    }
})(function (require, exports) {
    "use strict";
    Object.defineProperty(exports, "__esModule", { value: true });
    exports.twoPi = void 0;
    const constants_1 = require("./constants");
    exports.twoPi = constants_1.valueOfPi * 2;
});
```

#### AMD

```
define(["require", "exports", "./constants"], function (require, exports, constants_1) {
    "use strict";
    Object.defineProperty(exports, "__esModule", { value: true });
    exports.twoPi = void 0;
    exports.twoPi = constants_1.valueOfPi * 2;
});
```

#### System

```
System.register(["./constants"], function (exports_1, context_1) {
    "use strict";
    var constants_1, twoPi;
    var __moduleName = context_1 && context_1.id;
    return {
        setters: [
            function (constants_1_1) {
                constants_1 = constants_1_1;
            }
        ],
        execute: function () {
            exports_1("twoPi", twoPi = constants_1.valueOfPi * 2);
        }
    };
});
```

#### ESNext

```
import { valueOfPi } from "./constants";
export const twoPi = valueOfPi * 2;Try
```

#### ES2020

```
import { valueOfPi } from "./constants";
export const twoPi = valueOfPi * 2;Try
```

#### None

```
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
exports.twoPi = void 0;
const constants_1 = require("./constants");
exports.twoPi = constants_1.valueOfPi * 2;
```

**默认值**

如果`target` 为 ES3 或者 ES5，默认值为 `CommonJS`

如果`target` 为 ES6 或者更高，则默认值为 `ES6`/`ES2015`

### moduleResolution

指定模块解析策略：'node' （Node.js） 或 'classic' （在 TypeScript 1.6 版本之前使用）。 你可能不需要在新代码中使用 classic。

**默认值**

如果`module`为`AMD`/`UMD`/`System`/`ES6`, 则为 `Classic`

其他情况下为 `Node`

### noEmit

是否禁止编译生成文件。如果你想使用`babel`来编译`TypeScript`，可以设置这个选项为`true`

**默认值**

false

### outDir

指定编译输出的目录。若没有指定，`.js` 将被生成至于生成它们的 `.ts` 文件相同的目录中：

```
$ tsc

example
├── index.js
└── index.ts
```

使用类似这样的 `tsconfig.json`：

```
{
  "compilerOptions": {
    "outDir": "dist"
  }
}
```

使用这些配置运行 `tsc` 时，会将文件移动到指定的 `dist` 文件夹中：

```
$ tsc

example
├── dist
│   └── index.js
├── index.ts
└── tsconfig.json
```

**默认值**

n/a

### outFile

设置打包输出至单个文件中。

除非`module`为`None`, `System`或`AMD`，否则不能使用 `outFile`

**默认值**

n/a

### plugins

设置编辑器内运行的语言服务插件列表

语言服务插件是一种基于现有 TypeScript 文件向用户提供额外信息的方法。它们可以改进 TypeScript 和编辑器之间的现有信息，或提供自己的错误信息。

* [ts-sql-plugin](https://github.com/xialvjun/ts-sql-plugin#readme) — 增加了用模板字符串做 SQL 构建器时的风格检查。
* [typescript-styled-plugin](https://github.com/Microsoft/typescript-styled-plugin) — 在目标字符串中提供 CSS 风格检查。
* [typescript-eslint-language-service](https://github.com/Quramy/typescript-eslint-language-service) — 在编译器的输出中提供 eslint 的错误信息和修复信息。
* [ts-graphql-plugin](https://github.com/Quramy/ts-graphql-plugin) — 在 GraphQL 查询目标字符串中提供验证和补全。

### removeComments

设置当打包 JavaScript 时，忽略所有 TypeScript 文件中的注释。

例如，这是一个有 JSDoc 注释的 TypeScript 文件：

```
/** 'Hello world' 的葡萄牙语翻译 */
export const helloWorldPTBR = "Olá Mundo";
```

当然 `removeComments` 被设置为 `true`：

```
export const helloWorldPTBR = "Olá Mundo";Try
```

未设置 `removeComments` 或被设置为 `false`：

```
/** 'Hello world' 的葡萄牙语翻译 */
export const helloWorldPTBR = "Olá Mundo";Try
```

这意味着你的注释将呈现在 JavaScript 中。

**默认值**

false

### rootDir

设定 TypeScript 打包输出的根文件目录。

这么说有点抽象。举例来说，假设你有一些输入文件：

```
MyProj
├── tsconfig.json
├── core
│   ├── a.ts
│   ├── b.ts
│   ├── sub
│   │   ├── c.ts
├── types.d.ts
```

`rootDir` 推断的结构是所有非声明输入文件的最长公共路径，在例子中为 `core/`。

如果你的 `outDir` 是 `dist`，TypeScript 将会生成这样的文件树：

```
MyProj
├── dist
│   ├── a.ts
│   ├── b.ts
│   ├── sub
│   │   ├── c.ts
```

但你可能希望让 `core` 成为输出目录结构的一部分。 通过在 `tsconfig.json` 中指定 `rootDir: "."`，TypeScript 将会生成这样的文件树：

```
MyProj
├── dist
│   ├── core
│   │   ├── a.js
│   │   ├── b.js
│   │   ├── sub
│   │   │   ├── c.js
```

**注意**

rootDir 不会影响哪些文件被包含在编译中。如果设置了`rootDir`，但是程序又依赖了`rootDir` 之外的文件，这个文件会被输出至`outDir`外。因此，`rootDir`务必要保证依赖的文件都在`rootDir`下

### sourceMap

是否生成sourcemap

**默认值**

false

## 严格检查
