# Icode API Image

通过 Midlight OpenAI-compatible image relay 生成或编辑图片的 Skill。支持 Codex 和 Claude Code。

生图 Skill：<https://github.com/FeynmanNddbb/icode-api-image-skill>

## 使用前准备

1. 在 <https://icodeapi.com/keys> 创建一个生图 API key。
2. 建议只开放你实际需要的模型，并设置费用、余额或调用额度上限。
3. 不要把真实密钥写入代码、README、截图、Git 提交或公共聊天记录。

Skill 会把本地保存的密钥放在仓库之外的凭据文件中，不会写入当前项目。默认路径为 `~/.config/midlight-image/credentials.json`。

## 在 Codex 中安装

在 PowerShell 或终端中运行：

```bash
codex plugin marketplace add FeynmanNddbb/icode-api-image-skill --ref main
codex plugin add icode-api-image@personal
```

也可以启动 Codex 后输入 `/plugins`，在 `Personal` marketplace 中找到并安装 `icode-api-image`。

安装后建议新开一个 Codex 会话。更新插件时运行：

```bash
codex plugin marketplace upgrade personal
```

## 在 Claude Code 中使用

Claude Code 使用仓库中的标准 Skill 目录。将下面的目录复制到项目级或用户级 Skill 目录：

```text
plugins/icode-api-image/skills/icode-api-image/
```

项目级目录：

```text
<your-project>/.claude/skills/icode-api-image/
```

用户级目录：

```text
<your-home>/.claude/skills/icode-api-image/
```

复制后重新打开 Claude Code，让它重新扫描 Skill。

## 推荐使用方式

推荐先在计划模式（Plan mode）中提出生图需求，并开启询问功能。这样在产生费用前，代理会先询问必要的模型、尺寸、比例或分辨率等设置。

示例：

```text
请调用 icode-api-image 生图 Skill，使用此密钥 sk-......... 帮我生成一张赛博朋克城市夜景，并帮我把密钥保存在本地，下次无需再次发送密钥。
```

第一次使用时，Skill 会保存密钥并继续工作。之后可以直接提出需求，不必重复发送密钥。示例中的 `sk-.........` 只是占位符，请替换成你自己的密钥；不要把真实密钥提交到 GitHub。

Codex 和 Claude Code 都可以使用类似的请求：

```text
请调用 icode-api-image 生图 Skill，生成一张 1024x1536 的产品海报，使用 gpt-image-2。
```

如果使用 Gemini 模型，需要同时提供画面比例和 `1K`、`2K` 或 `4K` 分辨率。图片编辑目前只支持 `gpt-image-2`，参考图需要是本地 PNG、JPEG 或 WebP，且不超过 50 MiB。

## 支持的模型

| 模型 | 需要提供的设置 | 默认值 |
| --- | --- | --- |
| `gpt-image-2` | 像素尺寸 | `1024x1024` |
| `gemini-3-pro-image-preview` | 画面比例、分辨率 | `1:1`、`1K` |
| `gemini-3.1-flash-image-preview` | 画面比例、分辨率 | `1:1`、`1K` |

## 安全提示

- API key 具有计费和调用权限，只授予必要的模型权限，并设置金额或额度上限。
- 只在你信任的本地 Codex 或 Claude Code 会话中发送密钥。
- 不要在公共频道、Issue、日志、截图或 Git 提交中暴露 API key。
- 不要把生成结果中的敏感信息上传到不受信任的位置。
- 生图请求可能产生费用；遇到重试或超时，先确认上一次请求是否已经扣费，再决定是否继续。

需要删除本地密钥时，在 Skill 目录中运行：

```bash
python3 scripts/generate_image.py --remove-key
```

## 故障排查

- `401` 或 `403`：检查密钥是否有效，以及是否有目标模型权限。
- `404`：当前密钥组可能不支持所选模型，请换用可用模型。
- `429`：检查额度、余额或速率限制。
- 如果修改了 Skill 或插件目录，重启 Codex/Claude Code，并在新会话中重试。
