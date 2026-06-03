# OpenAI Creator Skills

这个仓库收集可复用的创作类 Agent Skills。目前包含：

- `script-five-dimensional-analysis`：短剧剧本五维分析技能。用于把完整短剧剧本按全集和逐集拆解，输出导演、分镜师、AI 视频生成师、演员、配音师可直接参考的 Markdown 分析报告。

## 仓库结构

```text
skills/
└── script-five-dimensional-analysis/
    ├── SKILL.md
    ├── agents/
    │   └── openai.yaml
    └── references/
        └── analysis_prompt.md
```

## 技能能力

`script-five-dimensional-analysis` 会围绕五个维度拆解短剧全集：

1. 叙事节奏维度
2. 镜头视听维度
3. 人物表演维度
4. 台词表达维度
5. 肢体动作维度

适用请求示例：

```text
使用 $script-five-dimensional-analysis 分析这个短剧剧本，输出全集逐集五维分析报告。
```

```text
给这个短剧做逐集五维分析，报告要适合导演、分镜师、AI视频生成师使用。
```

## Codex 使用步骤

### 1. 克隆仓库

```bash
git clone git@github.com:huawuhen/oepnai-creator-skills.git
cd oepnai-creator-skills
```

### 2. 安装到 Codex skills 目录

```bash
mkdir -p ~/.codex/skills
rm -rf ~/.codex/skills/script-five-dimensional-analysis
cp -R skills/script-five-dimensional-analysis ~/.codex/skills/
```

### 3. 重启 Codex

Codex 会在启动时扫描 `~/.codex/skills`。安装或更新 skill 后，需要重启 Codex 才能让新的 agent 自动加载。

### 4. 调用方式

显式调用：

```text
使用 $script-five-dimensional-analysis 分析下面的短剧全集剧本。
```

自然语言触发：

```text
给这个剧本做五维分析，必须逐集分析，不要只分析第一集。
```

## OpenClaw 使用步骤

OpenClaw 的不同版本可能有两种常见加载方式：项目级 `skill.md`，或全局/项目 `skills/` 目录。这个仓库按 Agent Skills 标准组织，两种方式都可以适配。

### 方式 A：复制到 OpenClaw skills 目录

```bash
git clone git@github.com:huawuhen/oepnai-creator-skills.git
mkdir -p ~/.openclaw/skills
rm -rf ~/.openclaw/skills/script-five-dimensional-analysis
cp -R oepnai-creator-skills/skills/script-five-dimensional-analysis ~/.openclaw/skills/
```

然后重启或刷新 OpenClaw agent，让它重新扫描 skills。

### 方式 B：作为项目级 skill.md 使用

如果你的 OpenClaw 项目读取项目根目录的 `skill.md`：

```bash
cd /path/to/openclaw-project
cp /path/to/oepnai-creator-skills/skills/script-five-dimensional-analysis/SKILL.md ./skill.md
mkdir -p references
cp /path/to/oepnai-creator-skills/skills/script-five-dimensional-analysis/references/analysis_prompt.md ./references/
```

然后在 OpenClaw 对话中使用：

```text
使用 script-five-dimensional-analysis 对这个短剧全集做五维分析。
```

或直接说：

```text
按叙事节奏、镜头视听、人物表演、台词表达、肢体动作五个维度逐集拆解这个剧本。
```

## Hermes 使用步骤

Hermes 默认从 `~/.hermes/skills` 读取用户 skills，也支持在 `~/.hermes/config.yaml` 中配置外部 skills 目录。

### 方式 A：复制到 Hermes 默认 skills 目录

```bash
git clone git@github.com:huawuhen/oepnai-creator-skills.git
mkdir -p ~/.hermes/skills
rm -rf ~/.hermes/skills/script-five-dimensional-analysis
cp -R oepnai-creator-skills/skills/script-five-dimensional-analysis ~/.hermes/skills/
```

检查是否可见：

```bash
hermes skills list | grep script-five-dimensional-analysis
```

单次调用：

```bash
hermes --skills script-five-dimensional-analysis \
  -z "给这个短剧全集做逐集五维分析，输出 Markdown 报告。"
```

### 方式 B：配置 external_dirs 直接引用仓库

如果不想复制文件，可以把仓库的 `skills` 目录加入 Hermes 外部技能目录。编辑 `~/.hermes/config.yaml`：

```yaml
skills:
  external_dirs:
    - /path/to/oepnai-creator-skills/skills
```

保存后重启 Hermes 或开启新会话。

### Hermes 自动化示例

```bash
hermes cron create "0 2 * * *" \
  "读取指定目录里的短剧剧本，使用五维分析技能生成 Markdown 报告。" \
  --skills "script-five-dimensional-analysis" \
  --name "短剧五维分析"
```

## 更新技能

拉取仓库最新内容后，重新复制到对应 agent 的 skills 目录：

```bash
git pull

# Codex
rm -rf ~/.codex/skills/script-five-dimensional-analysis
cp -R skills/script-five-dimensional-analysis ~/.codex/skills/

# OpenClaw
rm -rf ~/.openclaw/skills/script-five-dimensional-analysis
cp -R skills/script-five-dimensional-analysis ~/.openclaw/skills/

# Hermes
rm -rf ~/.hermes/skills/script-five-dimensional-analysis
cp -R skills/script-five-dimensional-analysis ~/.hermes/skills/
```

更新后请重启对应 agent，或开启新的 agent 会话。

## 质量要求

使用这个 skill 生成报告时，请检查：

- 是否分析了全集，而不是只分析第一集。
- 是否每一集都有五个维度。
- 是否不是单纯剧情复述。
- 是否包含导演执行、AI 视频生成、台词口型、肢体动作风控建议。
- 是否对暴力、亲密、擦边、血腥等内容做了镜头规避和 AI 生成风险处理。
