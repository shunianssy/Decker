
本页面由[小狐狸］(/shunianssy)进行简体中文汉化

**Decker**  
======  
Decker 是一个多媒体平台，用于创建和分享**交互式文档**，支持声音、图像、超文本与脚本。

![Decker 界面，含工具栏](images/wings.gif)

您可通过以下方式进一步了解 Decker：  
- 官方网站：[http://beyondloom.com/decker/](http://beyondloom.com/decker/)  
- 社区论坛：[https://internet-janitor.itch.io/decker/community](https://internet-janitor.itch.io/decker/community)  
- 直接体验在线版：[http://beyondloom.com/decker/tour.html](http://beyondloom.com/decker/tour.html)  

macOS 与 Windows 的定期二进制发行版可在 [Itch.io](https://internet-janitor.itch.io/decker) 获取。

若您对 Decker 的脚本语言 **Lil** 感兴趣，可访问 [trylil](http://beyondloom.com/tools/trylil.html) 查阅文档并在线试用。

---

**Web 版 Decker**  
----------  
Decker 提供 [网页应用版本](http://beyondloom.com/decker/tour.html)，采用原生 JavaScript 编写，整个应用打包为**单个独立 HTML 文件**。Web-Decker 可通过 `make` 脚本构建；测试套件依赖 [Node.js](https://nodejs.org/en/)：

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
