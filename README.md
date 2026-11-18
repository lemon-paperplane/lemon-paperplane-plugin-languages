# lemon-paperplane-plugin-languages

Lemon Paperplane 文件编辑器的通用格式着色（插件 id `languages`，名称「通用语言着色」/ Common Languages），内置随 App 安装。

没有业务逻辑：只在 `plugin.json` 的 `contributes.languages` 里声明语言与文件匹配规则，着色模块放在 `editor/`。这是纯声明插件：`plugin.json` 不写 `executable`，没有程序，内核从不启动进程，只按需提供 `editor/` 下的文件。

## 语言

Markdown（代码块按语言名懒加载其他插件的着色）、YAML、XML（含 pom.xml、.svg、.plist、.csproj 等）、HTML、CSS / SCSS / Sass / Less、Shell（.sh/.zsh/.bash、.zshrc 等）、SQL、Dockerfile、TOML、INI / .properties / .env、Makefile、Nginx、Protocol Buffers、Rust、C/C++、C#、Swift、PHP、Ruby、Lua、Dart，以及 Scala、Objective-C、Perl、PowerShell、R、CMake、Haskell、Clojure、Erlang、Julia、HTTP、Visual Basic、Pascal、Fortran、Verilog、VHDL、Tcl、Elm、Crystal、D、CoffeeScript、Pug、Stylus、Gherkin、Puppet、LaTeX、Jinja、NSIS。

文件匹配顺序：完整文件名 > 文件名通配 > 扩展名（取最长）；同级取 `priority` 高的。语言插件（java、nodejs、go…）自带本语言的着色，这里不重复。

## 构建着色模块

`editor/*.js` 由编辑器工程的脚本生成（单文件 ES module，`export default function (cm) { return LanguageSupport }`，CodeMirror 运行时取自编辑器的 `window.cm`）：

```bash
cd ../lemon-paperplane-main/lemon-paperplane-main-editor
npm ci
node scripts/build-lang.mjs languages      # 按 scripts/lang-modules.mjs 重建本插件的模块
```

新增语言：在 `scripts/lang-modules.mjs` 加一项（`@codemirror/lang-*` 包、`@codemirror/legacy-modes` 模式或 `lang/` 下自己写的模式），重建后在 `plugin.json` 的 `languages` 里声明扩展名/文件名与 `module`。
