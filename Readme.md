# [简体中文](https://github.com/shunianssy/Decker/blob/main/Readme_Zn.md)
以下是原版简介

Decker
======
Decker is a multimedia platform for creating and sharing interactive documents, with sound, images, hypertext, and scripted behavior.

![Decker, complete with toolbars](images/wings.gif)

You can learn more about Decker on [my website](http://beyondloom.com/decker/), on the [community forum](https://internet-janitor.itch.io/decker/community), or you can just dive in and [try it online](http://beyondloom.com/decker/tour.html). Periodic binary releases of Decker for MacOS and Windows are available on [Itch.io](https://internet-janitor.itch.io/decker).

If you're interested in _Lil_, Decker's scripting language, you can access documentation and play with it in your browser at [trylil](http://beyondloom.com/tools/trylil.html).


Web-Decker
----------
Decker is available as [a web application](http://beyondloom.com/decker/tour.html) (written in vanilla JavaScript) which is distributed as a single freestanding HTML file. Web-Decker can be built with a `make` script. The test suite uses [Node.js](https://nodejs.org/en/):

```
make testjs
make web-decker
make runweb      # (optional) open in your default browser
```


Native-Decker
-------------
Decker is also available as a native application, written in C. Building Native-Decker from source requires:

- a c compiler and libc
- the `xxd` utility (standard with MacOS and most \*nix distros)
- [SDL2](https://www.libsdl.org/download-2.0.php)
- [SDL2_image](https://github.com/libsdl-org/SDL_image)

On MacOS, BSD, or Linux, fetch the appropriate SDL2 packages and then build with `make`. This has also been reported to build and run successfully under WSL:

```
brew install sdl2 sdl2_image                                   # MacOS/Homebrew
sudo apt install libsdl2-2.0-0 libsdl2-dev libsdl2-image-dev   # Debian
nix-shell                                                      # Nix

make lilt            # (optional) command-line tools
make docs            # (optional) build documentation (requires Lilt)
make decker          # build decker itself
make test            # (optional) regression test suite
sudo make install    # (optional) install lilt, decker, and lil syntax profiles
```

If SDL2 is not available, Native-Decker can also be built with [reduced functionality](c/io_sdl1.h) against SDL1.2 and a corresponding version of `SDL_image`. This compatibility shim is presently designed with the [OLPC XO-4](https://wiki.laptop.org/go/XO-4_Touch) and its default Fedora 18 OS image in mind; expect to do some tinkering with the makefile for other platforms:
```
sudo yum install SDL-devel SDL_image-devel

make decker
```


Lilt
----
Decker's scripting language, [Lil](http://beyondloom.com/tools/trylil.html), is available as a standalone interpreter, with extended IO functionality to make it suitable for general-purpose programming and scripting: this package is called [Lilt](http://beyondloom.com/decker/lilt.html). Lilt only requires libc and `xxd` to build from source:
```
make lilt
```

Lilt can be used to programmatically create, inspect, and manipulate decks, as well as package them as Web-Decker self-executing documents:
```
$ lilt
 d:read["examples/decks/color.deck"]
<deck>
 d.card:d.cards.colhex
<card>
 d.card.widgets.hex.text:"FFAA00"
"FFAA00"
 d.card.widgets.hex.event["change"]
0
 d.card.widgets.rgb.text
"16755200"
 write["color.html" d]
1
```

You can build Lilt against [Cosmopolitan Libc](https://github.com/jart/cosmopolitan), producing a single binary that will run on most popular operating systems:
```
$ ./apelilt.sh
successfully compiled lilt.com
running tests against ./lilt.com...
all interpreter tests passed.
all dom tests passed.
all roundtrip tests passed.

$ sh ./lilt.com
  range 10
(0,1,2,3,4,5,6,7,8,9)
```

The Danger Zone
---------------
Decker normally sandboxes the execution of scripts within decks to prevent low-level access to the host computer and ensure parity between the capabilities of Web-Decker and Native-Decker. Both implementations offer opt-in APIs for performing more "dangerous" or non-portable operations called [The Danger Zone](http://beyondloom.com/decker/decker.html#thedangerzone).

When building Native-Decker from source, you can enable _The Danger Zone_ by defining the `DANGER_ZONE` preprocessor flag:
```
FLAGS:=$(FLAGS) -DDANGER_ZONE
```

A "dangerous" build of Native-Decker can export "dangerous" Web-Decker builds. You can also temporarily enable _The Danger Zone_ for Web-Decker by calling the `endanger()` function from your browser's JavaScript console or modifying the `DANGEROUS=0` constant in the .html file to `DANGEROUS=1`. [The Forbidden Library](http://beyondloom.com/decker/forbidden.html) offers a suite of bindings for useful JavaScript APIs based on this interface.


Contributing
------------
The Decker project is released under the MIT license. Any contributions to this repository are understood to fall under the same license.

- Bug fixes and typo corrections are always welcome.
- Bug reports must include simple steps for reproduction and clearly indicate the OS and/or web browser where the bug arises.
- PRs should match the style of existing code.
- PRs should be as small as possible, and must not contain bundled unrelated changes.
- PRs must include updates for _both_ the C and JavaScript versions of Decker (or its associated tools) whenever relevant.
- PRs must include updates for documentation (see: the `docs` directory) wherever relevant.
- PRs must pass the entire test suite (see: `make test`/`make testjs`).
- PRs _must not_ incorporate _any_ material generated by or with the assistance of _any_ so-called "generative AI" tool or language model. Place your trash in an appropriate receptacle.
- When modifying the JavaScript version of Decker, _please_ test your changes in multiple web browsers and avoid using bleeding-edge features. As a rule of thumb, if it didn't exist 5 years ago, don't use it now. If it _only_ works in Chrome, it's better not to do it at all.
- When modifying the C version of Decker, avoid generating warnings and _do not use_ compiler-specific features such as GCC extensions.

Please refrain from submitting Pull Requests to this repository containing new features without first discussing their inclusion in an Issue. Decker is intended to be small, simple, and cozy. There are an infinite number of features that could potentially be added, but creative constraints are also valuable. If you have a differing vision, feel empowered to explore it in your own fork of the project- that's what permissive licenses are for.

```bash
make testjs      # 运行 JavaScript 测试
make web-decker  # 构建 Web 版 Decker
make runweb      # （可选）在默认浏览器中打开
```

---

**原生版 Decker（Native-Decker）**  
-------------  
Decker 同时提供 C 语言编写的**原生应用版本**。从源码构建需满足以下依赖：

- C 编译器与 libc  
- `xxd` 工具（macOS 及多数 \*nix 系统默认自带）  
- [SDL2](https://www.libsdl.org/download-2.0.php)  
- [SDL2_image](https://github.com/libsdl-org/SDL_image)

在 macOS、BSD 或 Linux 上安装对应 SDL2 包后，直接执行 `make` 即可构建。该版本亦有用户报告可在 WSL 下成功编译运行：

```bash
# macOS/Homebrew
brew install sdl2 sdl2_image

# Debian/Ubuntu
sudo apt install libsdl2-2.0-0 libsdl2-dev libsdl2-image-dev

# Nix
nix-shell

# 可选：构建命令行工具
make lilt

# 可选：构建文档（需 Lilt）
make docs

# 构建 Decker 本体
make decker

# 可选：运行回归测试套件
make test

# 可选：安装 lilt、decker 及 lil 语法配置
sudo make install
```

若系统未提供 SDL2，亦可退而使用 [SDL1.2 与配套 SDL_image](c/io_sdl1.h) 构建功能精简版。此兼容层当前主要面向 [OLPC XO-4](https://wiki.laptop.org/go/XO-4_Touch) 及其预装的 Fedora 18 系统设计；其他平台可能需手动调整 Makefile：

```bash
# Fedora 示例
sudo yum install SDL-devel SDL_image-devel

make decker
```

---

**Lilt**  
----  
Decker 的脚本语言 [Lil](http://beyondloom.com/tools/trylil.html) 另有独立发行版——[**Lilt**](http://beyondloom.com/decker/lilt.html)，提供扩展 I/O 功能，适用于通用编程与脚本任务。仅需 libc 与 `xxd` 即可从源码构建：

```bash
make lilt
```

Lilt 可用于**程序化创建、检查与操作 deck 文件**，并可将其打包为 Web-Decker 自执行文档：

```bash
$ lilt
 d:read["examples/decks/color.deck"]
<deck>
 d.card:d.cards.colhex
<card>
 d.card.widgets.hex.text:"FFAA00"
"FFAA00"
 d.card.widgets.hex.event["change"]
0
 d.card.widgets.rgb.text
"16755200"
 write["color.html" d]
1
```

您还可基于 [Cosmopolitan Libc](https://github.com/jart/cosmopolitan) 构建 Lilt，生成**单一可执行二进制文件**，兼容绝大多数主流操作系统：

```bash
$ ./apelilt.sh
successfully compiled lilt.com
running tests against ./lilt.com...
all interpreter tests passed.
all dom tests passed.
all roundtrip tests passed.

$ sh ./lilt.com
  range 10
(0,1,2,3,4,5,6,7,8,9)
```

---

**危险区域（The Danger Zone）**  
---------------  
为保障安全与跨平台一致性，Decker **默认对 deck 内脚本执行沙箱隔离**，禁止底层系统访问，并确保 Web 与原生版本能力对等。

两版实现均提供**可选的高风险 API**——即 **“危险区域”**（[The Danger Zone](http://beyondloom.com/decker/decker.html#thedangerzone)），用于执行非便携或高权限操作。

- **构建原生版时启用**：在编译命令中定义预处理器宏 `DANGER_ZONE`：
  ```makefile
  FLAGS:=$(FLAGS) -DDANGER_ZONE
  ```
  启用后，原生版可导出具备“危险能力”的 Web 版 Decker。

- **Web 版临时启用**：  
  ① 在浏览器开发者控制台调用 `endanger()` 函数；  
  ② 或直接修改 HTML 文件中 `DANGEROUS=0` 为 `DANGEROUS=1`。  

基于此接口的实用 JavaScript API 绑定集合详见：[《禁术图书馆》](http://beyondloom.com/decker/forbidden.html)（The Forbidden Library）。

---

**贡献指南**  
------------  
本项目采用 **MIT 许可证**发布。所有向本仓库提交的内容均视为遵循相同许可。

- 欢迎提交 Bug 修复与笔误更正。  
- Bug 报告**必须**包含：复现步骤、操作系统/浏览器信息。  
- Pull Request（PR）应**严格遵循现有代码风格**。  
- PR 尽量**小巧专一**，禁止捆绑无关变更。  
- 若修改涉及 C/JS 双端实现（或工具链），**必须同步更新双方**。  
- 若涉及功能或 API 变更，**必须同步更新文档**（详见 `docs/` 目录）。  
- PR **必须通过全部测试**（`make test` / `make testjs`）。  
- **严禁**使用任何生成式 AI 工具（或语言模型）辅助生成提交内容——请将“垃圾”投入合适的回收站。  
- 修改 JS 版本时：  
  - 请在**多款浏览器**中验证兼容性；  
  - **避免使用前沿特性**——经验法则：若 5 年前不存在，现在就别用；若仅 Chrome 支持，最好别用。  
- 修改 C 版本时：  
  - 避免编译警告；  
  - **禁用编译器专属扩展**（如 GCC 特性）。

⚠️ **重要提醒**：未经 Issue 讨论并达成共识前，请勿直接提交新功能 PR。  
Decker 的核心追求是**小巧、简洁、亲切**。尽管可拓展的功能无穷无尽，但创作约束同样珍贵。若您有不同愿景，欢迎在您自己的分支中自由探索——这正是宽松许可证赋予您的权利。

--- 

如需进一步技术协助（如构建问题、环境配置等），我可结合您的系统环境（如 Deepin、手机端调试经验等）提供针对性建议。
