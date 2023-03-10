# 通用博文标签标准

```yaml
创建时间: 2023年1月14日
版本: 0.0.1-beta
```

<ruby>通用博文标签标准<rp>(</rp><rt>General Blog Post Tag Standard</rt><rp>)</rp></ruby>是由 [Save The Web Project]、[中文博客列表导航] 以及其他的中文博客聚合项目设计的开放标准。

[Save The Web Project]: https://github.com/saveweb
[中文博客列表导航]: https://github.com/zh-blogs

通用博文标签标准有三种设想的使用场景，分别是：

1.  博主通过博客程序，在博文中使用 HTML 的 meta 标签等元数据来主动声明博客、博文的特征。
2.  读者阅读后，对任意页面进行标记，可能是共享给其他读者，也可以本地储存。
3.  博客聚合项目通过整合标签，方便读者使用标签搜索，以快速发现感兴趣的博客。

此标准如果没有其他声明，分发协议均为 CC0。

## 简介

「通用博文标签标准」是为了通用性、精确描述以及可扩展性而编写的标准，所以此标准不会考虑具体的软件实现，仅仅考虑标签的样貌。

## GBPTS 0000 通用博文标签标准索引

```yaml
标题: 通用博文标签标准索引
创建时间: 2023年1月1日
```

GBPTS 0000 是索引，会收录所有的 GBPTS 内容，并对可行性进行评议，每条 GBPTS 都会被分类，分别是「正式」「草案」「延期」和「拒绝」。[^bep0000]

[^bep0000]: 借用了许多 [BEP 0000](https://www.bittorrent.org/beps/bep_0000.html)（BitTorrent 增强建议索引）的设计。

### 正式的 GBPTS

| 序号 | 标题                                     |
| ---- | ---------------------------------------- |
| 1    | [标签规则](#gbpts-0001-标签规则)         |
| 2    | [传递特性](#gbpts-0002-传递特性)         |
| 101  | [语言属性标签](#gbpts-0101-语言属性标签) |

### 草案的 GBPTS

以下 GBPTS 正在考虑标准化。

| 序号 | 标题                                                     |
| ---- | -------------------------------------------------------- |
| 0    | [通用博文标签标准索引](#gbpts-0000-通用博文标签标准索引) |
| 3    | [博文标签上限](#gbpts-0003-博文标签上限)                 |
| 4    | [元数据表示方法](#gbpts-0004-html-元数据表示方法)        |
| 5    | [使用第三方元数据](#gbpts-0005-使用第三方元数据)         |
| 102  | [地域属性标签](#gbpts-0102-地域属性标签)                 |
| 103  | [内容属性标签](#gbpts-0103-内容属性标签)                 |
| 104  | [篇幅属性标签](#gbpts-0104-篇幅属性标签)                 |
| 105  | [标题属性标签](#gbpts-0105-标题属性标签)                 |

### 延期的 GBPTS

小组认为以下 GBPTS 没有朝着标准化的方向发展，但他们还没有撤回。

### 拒绝的 GBPTS

目前，没有 GBPTS 被拒绝。

## GBPTS 0001 标签规则

```yaml
标题: 标签规则
创建时间: 2023年1月1日
```

标签由英文字母、数字、一个冒号、短横线和下划线组成，通常是 `键:值` 的结构，少数情况存在仅 `键` 的结构。

### 标签代码规范

标签代码有以下规范：

+   「键」「值」由英文字母、数字、短横线和下划线组成
+   「键」的部分禁止使用下划线
+   其他字符需要被删除
+   将空格替换成下划线
+   大小写不敏感

### 标签代码翻译

考虑到多语言支持，所以标签的本体都使用英文表示，比如：

| 标签             | 标签代码                        |
| ---------------- | ------------------------------- |
| 艺术分类         | `category:Art`                  |
| 简体中文语言     | `language:zh-CN`                |
| 中国广州（地域） | `region:PRC`+`region:Guangzhou` |

然后展示时再以合适的语言进行替换翻译：

| 标签代码                        | 简体中文       |
| ------------------------------- | -------------- |
| `category:art`                  | 分类：艺术     |
| `language:zh-CN`                | 语言：简体中文 |
| `region:PRC`+`region:Guangzhou` | 地域：中国广州 |

这需要维护 CSV 表格文件来实现此功能。（或者某种相似的表）

### 无冒号兼容模式

少数情况，会遇到无法使用冒号的情况，比如 Telegram 的标签系统不支持冒号以及首位数字，所以只需要修改成下面的样子：

| 标签代码       | 无冒号兼容模式  |
| -------------- | --------------- |
| `category:art` | `_category_art` |

即在「键」的前后增加一个下划线，并移除冒号。

## GBPTS 0002 传递特性

```yaml
标题: 传递特性
创建时间: 2023年1月3日
```

通常博客的结构如下所示：

```mermaid
flowchart TD

a["博客"] ---> o["文章／博文"]
a --> b["栏目／分类／板块等"] --> o
```

+   博客
    +   栏目／分类／板块等
        +   文章／博文

如果文章未定义语言属性标签，那么会从上面的结构里寻找标签，原则是就近原则，比如名为《植物图鉴》的博文位于「图鉴」栏目下，所以优先从「图鉴」栏目的标签里寻找。

因为从上到下是逐渐精确的路径，所以可以「文章」的标签可以覆盖「博客／栏目」的标签，比如博客主要使用中文语言，但文章设置的英文语言属性标签就能覆盖。

部分标签不会进行传递，需要看每个标签的传递定义，但是在键的后面添加上 -tc (transfer characteristic 传递参数) 就能强制传递，比如博客的分类是 `category-tc:Games` 就能传递给栏目、文章等，但会受到文章覆盖的影响。

## GBPTS 0003 博文标签上限

```yaml
标题: 博文标签上限
创建时间: 2023年1月14日
```

为了使标签精确，不被滥用，所以需要限制博文标签的上限，这里主要限制 GBPTS 0103 内容属性标签。

### 草案

解析器只会记录博文标签里面的，前 10 个「内容属性标签」，也许这样能让作者克制，只标记文章主要的标签。

也许能与「篇幅属性标签」进行参与计算，对于「微型博客」可能会减少内容属性标签的数量，而长文应该增加数量。

比如默认情况只记录前 10 个「内容属性标签」，而微型博客只记录 5 个。

## GBPTS 0004 HTML 元数据表示方法

```yaml
标题: HTML 元数据表示方法
创建时间: 2023年1月16日
```

网站的拥有者可以通过 \<meta> 标签，可以根据需要添加元数据，让软件读取。

目前 [WHATWG 的 MetaExtensions 标准][] 是大多数人的共识，所以为了减少重复功能，应该利用上面已有的数据，比如 Google, Twitter 和 Facebook 的相关标准。

[WHATWG 的 MetaExtensions 标准]: https://wiki.whatwg.org/wiki/MetaExtensions

如果没有相同功能的标准，那么将 GBPTS 于 HTML 的 \<meta> 标签中使用，就需要在头部加上 `gbpts.` 的前缀，比如：

| 标签         | HTML 的 \<meta> 标签                               |
| ------------ | -------------------------------------------------- |
| 艺术分类     | `<meta name="gbpts.category" content="Art">`       |
| 简体中文语言 | `<meta name="gbpts.language" content="zh-CN">`     |
| 这是一个标题 | `<meta name="gbpts.title" content="这是一个标题">` |

## GBPTS 0005 使用第三方元数据

```yaml
标题: 使用第三方元数据
创建时间: 2023年1月17日
```

0005 是 [0004](#gbpts-0004-html-元数据表示方法) 的补充，会解释那些元数据可以使用、如何使用。

### Open Graph

Open Graph 是 Facebook 提出的协议，作用是让站外链接在 Facebook 里具有美观的标题、配图预览效果。

Open Graph 的元数据的「键」在 \<meta> 标签的 property 属性里，所以标题属性标签就像下面一样：

```html
<meta property="og:title" content="这是一个标题" />
```

下面是可能有兼容性的标签列表：

| 属性名                  | 数据类型     | 含义     | GBPTS 序号                       |
| ----------------------- | ------------ | -------- | -------------------------------- |
| og:title                | string       | 标题     | [0105](#gbpts-0105-标题属性标签) |
| og:locale               | string       | 语言     | [0101](#gbpts-0101-语言属性标签) |
| og:site_name            | string       | 网站名称 |                                  |
| og:url                  | URL          | 链接     |                                  |
| og:image                | URL          | 标题图   |                                  |
| og:description          | string       | 简介     |                                  |
| og:type                 | string       | 网页类型 |                                  |
| article:published_time  | datetime     | 发布时间 |                                  |
| article:modified_time   | datetime     | 修改时间 |                                  |
| article:expiration_time | datetime     | 过期时间 |                                  |
| article:section         | string       | 分类     |                                  |
| article:tag             | string array | 标签     |                                  |
| article:author          | URL array    | 作者     |                                  |
| profile:first_name      | string       | 名       |                                  |
| profile:last_name       | string       | 姓       |                                  |
| profile:username        | string       | 用户名   |                                  |

`og:type` 在博客网站中通常有 article, profile 和 website 这几种属性，article 用在博文页面，profile 用在个人简介页面，而 website 用在其他页面，比如主页、翻页、存档等位置。

`article:author` 相对比较特殊，它的值是一个链接，作者名称的数据在那个页面里寻找，这是 Facebook 的某种分离性的设计，下面是作者页面 `example.com/profile.html` 的演示：[^opgep]

[^opgep]: <https://github.com/niallkennedy/open-graph-protocol-examples/blob/master/profile.html>

```html
<!DOCTYPE html>
<html lang="zh">
<head prefix="og: http://ogp.me/ns# profile: http://ogp.me/ns/profile#">
<meta charset="utf-8">
<title>张三的个人页面</title>
<meta property="og:title" content="张三的个人页面">
<meta property="og:site_name" content="Open Graph protocol examples">
<meta property="og:type" content="profile">
<meta property="og:locale" content="zh_CN">
<link rel="canonical" href="http://example.com/profile.html">
<meta property="og:url" content="http://example.com/profile.html">
<meta property="og:image" content="http://example.com/media/images/50.png">
<meta property="og:image:secure_url" content="https://example.com/media/images/50.png">
<meta property="og:image:width" content="50">
<meta property="og:image:height" content="50">
<meta property="og:image:type" content="image/png">
<meta property="profile:first_name" content="John">
<meta property="profile:last_name" content="Doe">
<meta property="profile:gender" content="male">
<meta property="profile:username" content="johndoe">
</head>
<body>
<p>张三的个人页面</p>
</body>
</html>
```

其他内容：播客 (Podcast) 和 Vlog（视频博客）的 Open Graph 元数据似乎并不普遍，所以暂不探索。

## GBPTS 0006 数据类型

在 GBPTS 中定义属性时使用以下数据类型。

| 类型     | 描述                           | 演示内容                       |
| -------- | ------------------------------ | ------------------------------ |
| Boolean  | 布尔值                         | `true`, `false`, `1`, `0`      |
| DateTime | 符合 [ISO 8601][]              | `2023-01-17`                   |
| Enum     | 枚举，在给定的字符串集合中选择 |                                |
| Float    | 浮点数                         | `1.234`                        |
| Integer  | 整数                           | `1234`                         |
| String   | 字符串                         |                                |
| URL      | 链接                           | `http://example.com/test.html` |

[ISO 8601]: https://en.wikipedia.org/wiki/ISO_8601

## GBPTS 0101 语言属性标签

```yaml
标题: 语言属性标签
创建时间: 2023年1月1日
可选标签: false
传递特性: true
重复允许: true
数据类型: enum
```

由于 ISO 的版权比较麻烦，[ISO 639][] 的完整内容相对不是能够开箱取得的，所以还是使用开放的 [BCP 47][] 的标准，这也是网站语言属性标签的规范。

[ISO 639]: https://zh.wikipedia.org/wiki/ISO_639-1代码表
[BCP 47]: https://www.rfc-editor.org/info/bcp47

BCP 47 的代码通常是语言+地区，但日语是 ja，因为日语通常只有在日本使用，或者说只有在日本的日语具有核心性，而英语有多种地区，所以开头都是 en 之后在加上地区名，比如英语（英式）的代码是 en-GB，GB 表示<ruby>大不列颠<rp>(</rp><rt>Great Britain</rt><rp>)</rp></ruby>

| 语言   | 简体中文 | 正體/繁體中文 | 英语（美式） | 英语（英式） | 日语 |
| ------ | -------- | ------------- | ------------ | ------------ | ---- |
| BCP 47 | zh-CN    | zh-TW         | en-US        | en-GB        | ja   |

BCP 47 对中文还有更精准的表述，分别是：

+   zh-cmn-Hans-CN (Chinese, Mandarin, Simplified script, as used in China)
+   cmn-Hans-CN (Mandarin Chinese, Simplified script, as used in China)
+   zh-yue-HK (Chinese, Cantonese, as used in Hong Kong SAR)
+   yue-HK (Cantonese Chinese, as used in Hong Kong SAR)

不过部分内容被 iana 废弃了，现在只有单独的 cmn 还被使用，[^ianalsr] 所以不需要使用这些表述方式。

[^ianalsr]: <https://www.iana.org/assignments/language-subtag-registry/language-subtag-registry>

```yaml
%%
Type: language
Subtag: cmn
Description: Mandarin Chinese
Added: 2009-07-29
Macrolanguage: zh
%%
Type: extlang
Subtag: cmn
Description: Mandarin Chinese
Added: 2009-07-29
Preferred-Value: cmn
Prefix: zh
Macrolanguage: zh
%%
Type: redundant
Tag: zh-cmn
Description: Mandarin Chinese
Added: 2005-07-15
Deprecated: 2009-07-29
Preferred-Value: cmn
%%
Type: redundant
Tag: zh-cmn-Hans
Description: Mandarin Chinese (Simplified)
Added: 2005-07-15
Deprecated: 2009-07-29
Preferred-Value: cmn-Hans
%%
Type: redundant
Tag: zh-cmn-Hant
Description: Mandarin Chinese (Traditional)
Added: 2005-07-15
Deprecated: 2009-07-29
Preferred-Value: cmn-Hant
%%
```

### 定义方法

通常语言属性标签是在 HTML 中已经声明，但许多静态博客生成器是在配置里写入的语言属性标签，所以博主使用其他语言编写博文时，语言属性标签不会产生变化，导致语言属性标签与实际的内容不符的情况，所以通常得使用正则式匹配字符，或者人力确认语言。

除了定义语言之外，可以可对一些支持多语言的属性标签进行多语言标注，那么其 YAML 片段大概如下：

```yaml
- 标题:
  - language-zh-CN: "2022 年度最佳"
  - language-en-US: "The Best of 2022"
- 简介:
  - language-zh-CN: "回顾今年最佳的内容吧！"
  - language-en-US: "Check out this year's best content!"
```

### 表示方式

只要在语言代码前加上 `language:` 的前缀就好。比如简体中文的网站使用 `language:zh-CN`。

如果是作为属性标签的多语言标注，那么将冒号变更为短横线，作为「键」来使用。

## GBPTS 0102 地域属性标签

```yaml
标题: 地域属性标签
创建时间: 2023年1月1日
可选标签: true
传递特性: true
重复允许: true
数据类型: string
```

### 定义方法

根据文章中是否有地域描述，来决定是否写上地域属性标签，因为这是可选标签，不是必须的。

+   缺省值：`region:null`

### 表示方式1

只要在英文地域前加上 `region:` 的前缀就好，比如中国广州使用（这些写法都是等价的）：

+   `region:Mainland_China`+`region:Guangzhou`
+   `region:PRC`+`region:Guangzhou`
+   `region:Peoples_epublic_of_China`+`region:Guangzhou`

备注：如果也可以只表示广州 `region:Guangzhou`，然后使用解析器添加中国。

缺点是多地域的支持会比较复杂，对解析器的要求较高，并且单独写下城市的方法可能与同名的城市或地区出现混淆。

### 表示方式2

只要在英文地域前加上 `region:` 的前缀就好，比如中国广州使用（这些写法都是等价的）：

+   `region:Mainland_China/Guangzhou`
+   `region:PRC/Guangzhou`
+   `region:Peoples_epublic_of_China/Guangzhou`

缺点是 Telegram 等需要兼容性的地方无法使用。

### 表示方式3

只要在英文地域前加上 `region:` 的前缀就好，比如中国广州使用（这些写法都是等价的）：

+   `region:Mainland_China:Guangzhou`
+   `region:PRC:Guangzhou`
+   `region:Peoples_epublic_of_China:Guangzhou`

缺点是 Telegram 等需要兼容性的地方无法使用。

### 表示方法的备注

上面的表示方法，等到正式版理应只保留一种。

### 备注

〔理论上应该支持多个地域，不过这个实现有点复杂的样子〕

## GBPTS 0103 内容属性标签

```yaml
标题: 内容属性标签
创建时间: 2023年1月3日
可选标签: true
传递特性: false
重复允许: true
数据类型: enum
```

Telegram 的频道分类比较成熟，所以直接照搬了其分类，有少量调整。然后合并了 [ooh.directory][] 的博客分类。分类作为内容属性标签的一级标签，其下还可以有很多可以描述具体内容的二级标签。

[ooh.directory]: https://ooh.directory/

### 细节

考虑到表格转换到爬虫读取的数据过于麻烦，为了便于维护，还是直接建立一个数据结构比较好，我们使用 YAML 创建了包含标签、译名、简介和子类型的数据，应该会有些帮助。

目前只使用了 `language-zh-CN` 表示中文翻译，`summary` 表示简介，`subtype` 表示子分类，所以应该不会很复杂。

你可以在 [/categories.yml](categories.yml) 查看目前已规定的 `category`。

### 定义方法

根据文章的描述来分类，这是可选标签，不是必须填写的。

而内容属性标签没有传递特性，所以博客的内容属性标签不会传递给博文，除非使用强制开启传递特性。

### 表示方式

内容属性标签分为 `category` 和 `tag`，`category` 作为博文的主题，所以通常很少，比如 1~2 个，而 `tag` 可以很多，并且 `tag` 可以创建任意字符。

如果你选择使用 `Extension` 作为博文的 `category`，那么你需要在 `Extension` 后面用冒号描述你所要拓展的 `category`，例如 `category:Extension:Life` 就可以表示博文属于生活 `category`，这个 `category` 是自己拓展的，解析器不能在标准中发现。

博文的 `category:Games` 标签，不能覆盖博文的标签。

博客的 `category-tc:Games` 标签，可以覆盖博文的标签。

## GBPTS 0104 篇幅属性标签

```yaml
标题: 篇幅属性标签
创建时间: 2023年1月14日
可选标签: true
传递特性: true
重复允许: true
数据类型: string
```

考虑到合理的优化「内容属性标签」数量，给不同篇幅的博文设立「内容属性标签」限制也许是可行的方法。

### 定义方法

根据「微型博客」性质以及文字数量来定义。

### 表示方式

只要在篇幅表述前加上 `length:` 的前缀就好。比如：

| 篇幅表述      | 篇幅属性标签           |
| ------------- | ---------------------- |
| 微型博客      | `length:microblog`     |
| 书            | `length:book`          |
| 1000 字       | `length:1000character` |
| 300 词        | `length:300word`       |
| 80 行[^lines] | `length:80lines`       |

[^lines]: 通常不需要使用，除非像新语思那种博客，使用的是硬换行。

这些标签可以单独使用，也可以混合使用。

## GBPTS 0105 标题属性标签

```yaml
标题: 标题属性标签
创建时间: 2023年1月16日
可选标签: 根据情况判断
传递特性: false
重复允许: false
数据类型: string
```

标题也许要标签来定义吗？绝大部分都不需要，但网页的 \<title> 标签经常是「标题 + 网站名」的形式，所以干净的标题属性往往需要在 \<h1> 里面寻找。

### 第三方定义方法

与上面的说法相同，干净的标题通常页面在 \<h1> 里寻找。

### 网站拥有者定义方法

直接在元数据里使用 `title` 来声明。

标题「这是一个标题」表示方式是：`<meta name="gbpts.title" content="这是一个标题">`
