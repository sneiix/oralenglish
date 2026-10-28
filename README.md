# oralenglish-skill — 专四口语思辨陪练

一个可开源的 Agent Skill，用于**英语专业四级（TEM-4）口语思辨训练**。

## 1. 项目解决什么问题

英语专业四级的“口语”不止考发音，更考**批判性思维与即兴观点表达**：要在短时间内对一个话题亮出观点、给出理由、条理清晰地用口头英语讲出来。很多学生的问题在于——想得出观点，却不知道“怎么说”；说出来的英文带着明显的中式英语，自己却意识不到。

本项目把 TEM-4 口语的思辨训练做成一个**一轮一轮、可循环的对话式陪练**：AI 出思辨话题并给出一正一反的参考句型，用户口头作答后，AI 原样转写、给出一版优化的口头英语、并点明中式英语问题，帮用户把“会想”转化为“会说、说对、说得地道”。

纯对话型技能，**不需要**任何网页抓取、PDF 解析或图形界面。

## 2. 主要功能

- **逐轮思辨出题**：AI 给出纯英文思辨话题，并附两句纯英文参考句型——一句肯定立场、一句否定立场，作为开口的句式骨架。
- **口述原样转写**：用户作答后，AI 输出【我的原句】，逐字保留用户原始表达，不做修改。
- **中式英语优化**：AI 输出【优化句】——仅 1 版、适配专四口语的改写，优先修正直译语序、中式搭配、逻辑断层、母语负迁移四类问题；拒绝浅层同义词替换，改动的核心内容加粗，语言口语化、不过度书面。
- **简短点评**：AI 点明本次的中式英语问题，并简述逻辑、句式、用词地道度。
- **双分支循环**：每轮反馈后给出【继续】【重新回答】两个选项——
  - 【继续】：基于当前话题深度追问，开启新一轮作答；
  - 【重新回答】：不换话题，等用户再次作答并重新反馈，循环直到输入【继续】。

## 3. 安装方法

本项目由 `skill-creator-for-work` 的 `init_skill.py` 初始化生成，包含 `SKILL.md` 与 `agents/openai.yaml`。安装到任意 Agent 环境的步骤：

1. 将 `oralenglish-skill` 整个目录**复制**到目标 Agent 的技能目录。常用位置是 `<环境目录>/workspace/.user_skills/`（不同运行环境前缀可能不同，以该环境已有的技能路径为准）。最终路径形如：
   ```
   <环境目录>/workspace/.user_skills/oralenglish-skill/
   ```
2. 重启或重载该 Agent，使其扫描到新技能。
3. 验证安装：技能目录中必须存在 `SKILL.md`（frontmatter 含 `name` 与 `description`）；`agents/openai.yaml` 为可选的 Agent 清单，缺省不影响使用。

> 技能为标准文件夹，不打包、不生成 `.skill` 文件，保持完整目录结构即可被加载。

## 4. 使用方法

对已安装该技能的 Agent 说“**帮我做专四口语思辨训练**”（或“口语训练 / 思辨陪练 / TEM-4 口语”等）即可触发。随后按以下多轮流程进行：

1. **出题**：AI 先给 `【Topic】`（纯英文思辨话题），再给两句纯英文参考句型——一句肯定立场、一句否定立场，等待作答。
2. **作答**：用户用口头英语作答（可直接打字输入你的口述内容）。
3. **三栏反馈**：AI 输出【我的原句】【优化句】【简短点评】。
4. **分支选择**：AI 给出【继续】【重新回答】两个选项，用户二选一：
   - 输入 **【继续】** → AI 基于当前话题深度追问并重新出题；
   - 输入 **【重新回答】** → AI 不换话题，等你再次作答并重新反馈，循环直到输入【继续】。

## 5. 输入输出示例

**AI 出题：**
```
【Topic】Should students be allowed to use smartphones in class?

参考句型：
• 肯定立场：I believe students should be allowed to use smartphones in class, because they can look up information and make lessons more interactive.
• 否定立场：In my opinion, smartphones in class are more of a distraction than a help, because they shift students' attention away from the teacher.
```

**用户作答（输入）：**
```
I think students shouldn't use phone in class. Because the phone will let students' attention run away. Study is important, play phone is not good.
```

**AI 三栏反馈（输出）：**
```
①【我的原句】I think students shouldn't use phone in class. Because the phone will let students' attention run away. Study is important, play phone is not good.

②【优化句】I think students shouldn't use their phones in class, because phones make it hard to stay focused. Since study comes first, spending time on the phone only hurts our learning.

③【简短点评】中式英语问题：①"use phone"漏复数与限定词；②"let attention run away"是中文直译，英语用 distract / lose focus；③"play phone"是中式搭配，应说 use / spend time on the phone。逻辑：有观点有理由但句子破碎；句式：口语化但结构散；用词地道度：需重点提升。
```

**分支选择（输出）：**
```
【继续】  基于当前话题进行深度追问，开始新一轮作答
【重新回答】 不换话题，请再次口述作答
```

## 验证

- 已通过 `quick_validate.py` 校验（Skill 结构、frontmatter、命名）——结果 `Skill is valid!`。
- 已用两个不同思辨话题（智能手机进课堂、居家办公 vs 坐班）跑通完整流程，含【重新回答】与【继续】两个分支。

## 目录结构

```
oralenglish-skill/
├── SKILL.md            # 技能定义与完整流程规范（Agent 读取执行）
├── agents/
│   └── openai.yaml     # Agent 清单（name/description/instructions/tools）
├── LICENSE             # MIT 开源协议
└── README.md           # 本文件（位于项目根目录）
```

## License

MIT
