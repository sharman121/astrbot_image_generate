# image_generate

AstrBot 生图插件。支持聊天模型调用 `generate_image` Tool 进行文字生图，调用 `edit_image` Tool 进行图生图，也可以通过 `/tushengtu` 命令手动触发图生图。

## 配置

安装插件后，在 AstrBot WebUI 的插件配置中填写：

- `api_key`：中转站 API Key，不要包含 `Bearer` 前缀。
- `api_url`：生图接口地址，默认使用 `https://api.apiqik.com/v1/images/generations`。
- `edit_api_url`：图生图接口地址，默认使用 `https://api.apiqik.com/v1/images/edits`。
- `request_timeout`：请求超时时间，默认 480 秒。

模型固定为 `gpt-image-2`，图片尺寸固定为 `1024x1536`，每次生成 1 张图片。

## 使用

请在 AstrBot 中启用本插件的 `generate_image` 和 `edit_image` 工具，并确保当前聊天模型支持 Tool Calling。

文字生图时直接提出要求，例如：

```text
请生成一张坐在窗边看雨的橘猫图片，水彩插画风格。
```

图生图时，在同一条消息中附带一张或多张图片，或者引用一条包含图片的消息，然后直接用自然语言提出修改要求。聊天模型会调用 `edit_image` Tool。

`edit_image` Tool 可以选择标准尺寸 `1024x1024`、`1024x1536`、`1536x1024`，也可以传入 `original` 使用第一张参考图的真实尺寸。插件会在每次请求中读取所有参考图片的宽高，并把这些原图尺寸加入本次可选尺寸集合；不会修改跨会话共享的全局集合。

也可以使用命令手动触发：

```text
/tushengtu 将参考图片中的主体自然地放在同一个雨夜街景中，统一光线和画风
```

插件会把全部参考图片以 `image[]` 字段上传。支持 JPEG、PNG 和 WebP；单张图片最大 25 MB，所有参考图片合计最大 50 MB。

`edit_image` Tool 可以使用第一张参考图的尺寸作为动态候选。参考图的宽高如果不是 16 的倍数，会分别调整到最接近的 16 倍数后再传给接口，例如 `1001x777` 会调整为 `1008x784`。三个基础候选尺寸 `1024x1024`、`1024x1536`、`1536x1024` 始终保留。
