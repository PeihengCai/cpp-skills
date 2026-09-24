# cpp-skills

用于 AI 编程助手的个人技能库，将可复用的工作流程整理为 `SKILL.md`，帮助助手按统一要求完成任务。

技能源文件保存在本仓库，通过 [Skills CLI](https://github.com/vercel-labs/skills) 安装到支持的 agent。`npx` 负责运行安装工具，技能内容从 GitHub 获取，无需将本仓库发布为 npm 包。

## 现有技能

| Skill | 简介 | 完整说明 |
| --- | --- | --- |
| `optimize-requirements` | 优化已有产品或游戏需求文档，梳理行为主流程、用户可感知的表现与边界，形成可验收的要求。 | [需求文档优化](skills/optimize-requirements/SKILL.md) |

### optimize-requirements · 需求文档优化

适用于已经有原文的需求整理、改写和补充，主要包括：

- 将内部实现描述转为用户可感知、可验证的行为要求，保留必要的平台、性能和兼容约束。
- 按主流程组织内容，合并重复规则，明确触发条件、处理对象、状态变化、动作时序和结束结果。
- 从用户视角写清界面、交互、声音、运动等表现，定义必要概念、参照和环节之间的衔接。
- 检查重复操作、中断、恢复、多对象竞争及规模边界；缺少产品决策时提出具体问题，保留待确认项。
- 有对应 APK 时，核对原文与参考实现的一致性，并依据可靠证据分析边界。APK 分析需要运行环境提供相应工具与文件访问能力，本仓库不附带解析器。
- 交付优化后的文档，以及按用途分类的视觉素材清单，说明需要哪些截图、示意图、分镜或录屏。

需要提供已有需求正文或可访问的文档；可选提供对应 APK、参考资料和已确认的产品选择。该技能不用于从零撰写需求、实现功能或单纯代码审查，也不会默认制作视觉素材。

对缺失决策，技能会先提问。选择“待确定”或问题实际等待 5 分钟仍未答复时，可查证 APK 中同场景行为；证据不足则保留待确认。原文与 APK 的明确冲突仍需用户决定。

使用示例：

```text
使用 optimize-requirements 优化这份需求文档：梳理主流程，写清用户可见的状态变化和衔接，列出需要确认的边界以及视觉素材清单。
```

在支持 `$` 技能调用的客户端，也可以使用 `$optimize-requirements`。

## 使用 npx 安装

先安装 [Node.js](https://nodejs.org/)（包含 npm/npx）和 [Git](https://git-scm.com/)，并确保能访问 npm 与 GitHub。私有仓库还需要相应的访问权限和 Git 认证。

### 安装整个技能库

选择仓库中所有技能，并按终端提示选择目标 agent、安装范围和安装方式：

```sh
npx skills@latest add PeihengCai/cpp-skills --skill '*'
```

如果希望自己勾选要安装的技能：

```sh
npx skills@latest add PeihengCai/cpp-skills
```

### 安装单个技能

```sh
npx skills@latest add PeihengCai/cpp-skills --skill optimize-requirements
```

只指定技能仍可按提示选择安装到哪个 agent。当前仓库只有一个技能，因此整库安装与单个安装的内容相同；以后新增技能后，两者范围会不同。

### 指定 agent 和安装范围

例如，为 Codex 全局安装单个技能：

```sh
npx skills@latest add PeihengCai/cpp-skills --skill optimize-requirements -g -a codex
```

- `-g`：用户级安装，跨项目使用；项目级安装适用于当前项目。
- `-a codex`：指定 Codex；其他目标可通过交互界面选择。
- `--skill '*'`：选择整个库的技能。不要用 `--all` 代替，它还会选择所有 agent 并跳过提示。

查看远程仓库提供的技能，不执行安装：

```sh
npx skills@latest add PeihengCai/cpp-skills --list
```

## 更新

以下命令适用于通过 Skills CLI 安装的技能。

更新全局安装的 `optimize-requirements`：

```sh
npx skills@latest update optimize-requirements -g
```

更新项目级安装时，在对应项目目录运行：

```sh
npx skills@latest update optimize-requirements -p
```

更新全部全局技能：

```sh
npx skills@latest update -g
```

最后一条命令会更新该工具管理的全部全局技能，包括来自其他仓库的技能。仓库以后新增的技能，可重新执行整库安装命令安装。

维护者修改后需提交并推送到 GitHub，使用者执行更新后才会获取新内容。`skills@latest` 表示安装工具的版本，技能内容来自仓库，并非本仓库的 npm 版本。

## 不使用命令：手动安装

手动安装不需要 Node.js、npm 或 Git。以下以 Codex 为例。

1. 打开 [cpp-skills 仓库](https://github.com/PeihengCai/cpp-skills)，选择 **Code → Download ZIP**。
2. 解压 ZIP，进入其中的 `skills` 文件夹。
3. 选择安装范围，并用文件管理器打开对应目录；不存在时创建目录：

   | 范围 | Codex 技能目录 |
   | --- | --- |
   | 用户级，跨项目使用 | `~/.agents/skills/` |
   | 项目级 | `<项目根目录>/.agents/skills/` |

   `~` 表示用户主目录。Windows 用户可在资源管理器地址栏输入 `%USERPROFILE%\.agents\skills`。

4. 安装单个技能时，复制完整的 `optimize-requirements` 文件夹；安装整个库时，复制下载内容中 `skills` 下的所有技能文件夹。
5. 在 agent 中确认技能可用。若未显示，重新打开会话或重启客户端。

安装完成后的结构应为：

```text
.agents/
└── skills/
    └── optimize-requirements/
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```

请保留整个技能文件夹中的配套文件，不要只复制 `SKILL.md`。同名技能保留一个有效安装入口，避免重复加载。其他 agent 请使用各自支持的技能目录，不要默认沿用 Codex 的路径。

**手动更新：**重新下载 ZIP，将旧版技能文件夹移到技能扫描目录之外备份，再放入新版完整文件夹。仅手动复制的技能没有通过安装工具建立来源记录，不应依赖 `npx skills update` 更新。

## 仓库结构

```text
cpp-skills/
├── readme.md
└── skills/
    └── optimize-requirements/
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```

## 参考

- [Skills CLI：安装参数与更新命令](https://github.com/vercel-labs/skills)
- [Codex 技能目录与加载方式](https://learn.chatgpt.com/docs/build-skills)
