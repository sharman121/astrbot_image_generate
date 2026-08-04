# image_generate

AstrBot 生图插件。支持llm调用 `generate_image` Tool 进行文字生图，调用 `edit_image` Tool 进行图生图，以及通过 `/tushengtu` 命令手动触发图生图。

## 配置

安装插件后，在插件配置中填写：

- `api_key`：中转站 API Key。
- `api_url`：文生图接口地址，一般使用 `中转站url/v1/images/generations`。
- `edit_api_url`：图生图接口地址，一般使用 `中转站url/v1/images/edits`。
- `request_timeout`：请求超时时间，默认是 480 秒。

模型为 `gpt-image-2`，图片尺寸默认为 `1024x1536`，每次生成 1 张图片。

## 使用

请在 AstrBot 中启用本插件的 `generate_image` 和 `edit_image` 工具，并确保当前llm支持 Tool Calling。

文字生图时直接提出要求，ai会根据你的要求写出提示词，例如：

```text
请生成一张坐在窗边看雨的橘猫图片，水彩插画风格。
```

图生图时，在同一条消息中附带一张或多张图片，或者引用一条包含图片的消息，同样直接提出修改要求就可以了。llm会调用 `edit_image` Tool。

`edit_image` Tool 可以选择默认尺寸 `1024x1024`、`1024x1536`、`1536x1024`，也可以传入 `original` 使用第一张参考图的真实尺寸。插件会在每次请求中读取所有参考图片的宽高，并把这些尺寸加入一个尺寸集合供llm选择使用。参考图尺寸可能会微调以符合openai生图接口的要求。

也可以使用命令，提示词会直接传入`prompt`，例如：

```text
/tushengtu 将参考图片中的主体自然地放在同一个雨夜街景中，保持光线和画风一致
```

插件会把全部参考图片以 `image[]` 字段上传。支持 JPEG、PNG 和 WebP；单张图片最大 25 MB，所有参考图片合计最大 50 MB。
