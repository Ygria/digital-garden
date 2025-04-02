---
date: ' 2025-04-02'
tags: 
description: 
title: 
draft: false
keywords: []
---
每次写完微信公众号文章，配封面总是一件麻烦事。内容中的图片常常分辨率不匹配，裁剪之后预览效果不好，AI 生图的效果也不稳定。

于是我使用这个方案： LLM 推理文章标题、高亮热词、四字总结，使用 Cloudflare Worker 部署一个服务，支持传入 LLM 推理出的结果生成封面图，测试效果如下：

![image.png](https://images.ygria.site/2025/04/b340549affd0493719e500a565448469.png)


# Cloudflare Worker实现

## 如何绘制？

> [!question] Cloudflare Worker 环境限制
>  Cloudflare Worker 有诸多基于性能和安全考虑的环境限制，比如无法开子进程、无法读写本地文件等，我一开始想的是直接使用大模型生成 html，然后绘制成图。但 Worker 中无法使用浏览器的绘制功能（需要引入Puppeteer 等无头浏览器库），调用外部 API 实现的基本是渲染在一个浏览器上，再截图，这样出来的效果也容易模糊，且依赖外部有不稳定风险。

那么直接用 `svg` 生成，再转成 `png` 或 `jpeg`  呢？（微信封面不支持 svg 格式）

**SVG 有如下好处：**

1. **分辨率灵活，好扩展。** 想要做什么分辨率的图片都可以，不管 1:1 还是 16:9，还是微信公众号封面要求的 2.35:1

2. **矢量图**不管渲染到多大尺寸都**清晰**，占用空间小

3. **各种样式、图案都可以画出来**，只要定义好模板，替换内容即可。如果是文字排版，那更是只要准备几套就行。

## 定义 SVG 模板

在 `GPT` 老师的帮助下生成了 `generateSVG` 方法，主要实现的功能是将传入的 `title` 标题、`hightlights` 高亮、`summary` 四个大字填入 svg 中。

> [!tip]
> SVG 内容可以后续根据版式设计再调整。我这里是截图了一个别人用 AI 生成的封面，让 GPT 尽量为我还原的。图片左侧可以截图当 2.35:1的封面，右侧四个大字是转发时预览的 1:1封面。
> **如下图 , 图片大小为 1283 × 383，这样左侧可以裁出 900 × 383 的 2.35:1封面，右侧是 1 × 1 的消息卡片、转发到朋友圈的视图。注意尺寸必须正确，不然后续发布到草稿箱，裁剪会出错。**
> 
> ![image.png](https://images.ygria.site/2025/04/7c3bf37707415aef5129a2ef308869a6.png)


```typescript
function generateSVG({ highlights, summary, title }) {
    const highlightWords = highlights.split(", ").map(word => word.trim());
    // 高亮 title 中的关键词
    function highlightText(text) {
        let parts = [];
        let lastIndex = 0;
        highlightWords.forEach(word => {
            let index = text.indexOf(word);
            if (index !== -1) {
                if (index > lastIndex) {
                    parts.push(text.slice(lastIndex, index));
                }
                parts.push(`<tspan fill="#FFD700">${word}</tspan>`);
                lastIndex = index + word.length;
            }
        });

        if (lastIndex < text.length) {
            parts.push(text.slice(lastIndex));
        }
        return parts.join("");
    }

    // 计算左侧区域的占比
    
    const textStartX = 50; // 文字起始 x 轴
    const maxCharsPerLine = 10; // 每行最多 10 个字符
    const fontSize = 50; // 字体大小
    const lineSpacing = 60; // 行距
    // 处理 title 自动换行
    const titleLines = title.match(new RegExp(`.{1,${maxCharsPerLine}}`, "g")) || [];
    const titleSVG = titleLines.map((line, index) =>
        `<text x="${textStartX}" y="${80 + index * lineSpacing}" font-size="${fontSize}" fill="#FFFFFF" font-weight="bold">${highlightText(line)}</text>`

    ).join("\n");
    // 右侧大字拆分
    const summaryWords = summary.split("");
    const summaryFirstLine = summaryWords.slice(0, 2).join("");
    const summarySecondLine = summaryWords.slice(2).join("");
    return `<svg width="1283" height="383" viewBox="0 0 1283 383" xmlns="http://www.w3.org/2000/svg">
        <rect width="100%" height="100%" fill="#000"/>
        <!-- 左侧标题 -->
        ${titleSVG}
        <!-- 右侧大字 -->
        <text x="950" y="140" font-size="80" fill="#FFFFFF" font-weight="bold">${summaryFirstLine}</text>
        <text x="950" y="220" font-size="80" fill="#FFD700" font-weight="bold">${summarySecondLine}</text>
    </svg>`;

}
```

## 究极折磨：如何 svg 转 png

> [!question] svg 转 png
> `svg` 转 `png` 在其他环境很简单，比如 `node` 和浏览器环境。但在 `Worker` 里，使用 `sharp` 和 `resvg` 等绘制库都会报**依赖缺失**。最后只能用 `wasm` 来做。我这里在 AI 的忽悠下走了很多弯路，自己用 `go` 手搓再编译成 `wasm`，再在 worker中引入，还需要给一个 `go` 运行环境……总之就是折腾了半天也没成功。最后发现 `resvg` 直接就有  `cf-wasm` 库，贴心注明了适合在 Cloudflare Worker 环境内运行……走了好多弯路！给我的教训是不要迷信 AI ，某些垂直领域掌握一些搜索技巧，可以获得比 AI 精准很多的结果

>  在 npm 使用 `wasm`  和 ` @cf-wasm ` 作为关键字，就可以搜到支持 Cloudflare Worker 环境可以用的 `wasm` 库。
![image.png](https://images.ygria.site/2025/04/5c7bbd2084d7d5498615256f917751cb.png)
![image.png](https://images.ygria.site/2025/04/c4fbeafdd57df672be75a62b36bc14b8.png)

```typescript
import { Resvg } from "@cf-wasm/resvg";

const svg =  generateSVG({highlights, summary, title})
const resvg = new Resvg(svg, opts)
const pngData = resvg.render()
const pngBuffer = pngData.asPng();

```

获取到了 `png` 内容，就可以直接调用微信开放平台 API，获取到的 mediaId 可以在后续调用新增微信草稿时使用，作为封面图传入了。

## 文本出不来？手动写入字体数据

由于 `Cloudflare Worker` 运行环境无系统字体，所以必须传入字体文件内容。`Cloudflare Worker` 也不支持读写本地文件，所以要么通过 `fetch` 从外部环境拉取，要不从存储中读入。

考虑性能，使用 Cloudflare 自带的KV 库存储字体文件。

### Cloudflare Worker 使用 KV

参考 CF 官方文档即可。 👉 https://developers.cloudflare.com/kv/

Worker 绑定 KV，在设置-绑定中。注意检查 Worker项目的 `wrangler.toml` 是否正确。
```toml
[[kv_namespaces]]
binding = "Fonts"
id = "XXXXXX"  # 替换为你的实际 Namespace ID
```

![image.png](https://images.ygria.site/2025/04/aeb997f5a572f3a8b29b0895c1c2c26b.png)




CF Dashboard 只支持新增 Value 类型为 `string` 的条目，字体为二进制文件，所以需要上传。

```bash
npx wrangler kv key put --binding=<BINDING_NAME> "<KEY>" --path ./font.ttf
```

注意，CF 区分了本地测试环境和生产环境，这时虽然控制台显示写入正常，但生产环境找不到这条数据。

**必须加上 `--remote`，才是向生产环境写入！**


```bash
npx wrangler kv key put --binding=<BINDING_NAME> "<KEY>" --path ./font.ttf --remote
```

写入成功后，可以去 Cloudflare Dashbord 看一眼，KV 库里有没有数据。左上角就是 namespace ID，注意别配错了。

![image.png](https://images.ygria.site/2025/04/18d11b89d57385794f3b716b64c2d6a4.png)

我向 KV 中写入了阿里巴巴普惠字体。之后可以考虑传入一些免费设计字体，用于生成更美观的封面。

###  读 KV 库内容，并传到 resvg 渲染
在 Worker 中使用：

```typescript
const font = await env.Fonts.get("Puhui", { type: "arrayBuffer" })
const fontBuffer = new Uint8Array(font);
if (!fontBuffer) {
  return new Response("Font not found in KV", { status: 404 });

}

const opts = {
        background: 'rgba(238, 235, 230, .9)',
        fitTo: {
            mode: 'width',
            value: 1283,
        },
        font: {
            fontBuffers: [fontBuffer]
        }
    }

const resvg = new Resvg(svg, opts)
//  ...
```

## Deepseek V3 生成标题、热词、摘要

现在我们可以在 coze 工作流中配置 LLM，为我的文章内容生成标题、热词和摘要了。

![53161a038f0b0f30f2b859c0a8d8829.jpg](https://images.ygria.site/2025/04/74c55171d4f77989f97cf09c4746cd7a.jpg)

生成的内容可以直接用于下一步调用 Worker 接口的入参。生成图片后，调用微信 API 获得 MediaID。

## 向微信草稿箱写入封面、裁剪参数

写入微信草稿箱，将上一步获取到的 MediaId 传入，另外需要计算裁剪比例，左侧作为消息列表中 2.35:1的文章封面，右边为转发和历史消息里的 1:1 封面。
> 先看一下微信官方文档给出的参数解释：

| pic_crop_235_1 | 否   | 封面裁剪为2.35:1规格的坐标字段。以原始图片（thumb_media_id）左上角（0,0），右下角（1,1）建立平面坐标系，经过裁剪后的图片，其左上角所在的坐标即为（X1,Y1）,右下角所在的坐标则为（X2,Y2），用分隔符_拼接为X1_Y1_X2_Y2，每个坐标值的精度为不超过小数点后6位数字。示例见下图，图中(X1,Y1) 等于（0.1945,0）,(X2,Y2)等于（1,0.5236），所以请求参数值为0.1945_0_1_0.5236。 |
| -------------- | --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| pic_crop_1_1   | 否   | 封面裁剪为1:1规格的坐标字段，裁剪原理同pic_crop_235_1，裁剪后的图片必须符合规格要求。                                                                                                                                                                                 |

同样地，让 `GPT` 老师帮我们算一下。

![image.png](https://images.ygria.site/2025/04/cc39bc13ec1ba50f8f0a96f1a583d9c0.png)
![image.png](https://images.ygria.site/2025/04/803b9046d1b53027e005823705476686.png)

```typescript
let body = {

        articles: [

            {

                title: data.title,
                content: data.content,
                digest: data.digest,
                thumb_media_id: data.thumb_media_id,
                pic_crop_235_1: '0_0_0.7015_1',
                pic_crop_1_1: '0.7015_0_1_1',
                // ... 其他参数

            }

        ],

    };
```

Ok，这样就完成啦！看下效果~
第一张图是消息列表里的图。

![9da521a62f54a8fbbd85817009cb46f.jpg](https://images.ygria.site/2025/04/49e6f6d81dcc97f2a76a4154a5a52d95.jpg)

第二张是转发时看到的。

![image.png](https://images.ygria.site/2025/04/f3674b90c5f11ef44f31fb4725a320e3.png)

仅为测试效果，后续还会继续做样式改进~请朋友们尽情发挥自己的审美吧！