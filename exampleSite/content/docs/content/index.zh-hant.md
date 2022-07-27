+++
title = "添加內容"
linkTitle = "內容"
linkTitleIcon = '<i class="fas fa-newspaper fa-fw"></i>'
date = 2021-12-04T10:43:39+08:00
featured = false
comment = true
toc = true
reward = true
pinned = false
categories = [
]
tags = [
  "Authoring",
  "Articles"
]
series = [
  "文檔"
]
images = []
navWeight = 970
authors = ["RazonYang"]
[menu.footer]
  parent = "docs"
  weight = 5
+++

## Main Sections

`mainSections` 參數用於過濾頁面，默認為 `["posts", "docs"]`。

```toml {title="config/_default/params.toml"}
mainSections = ["blog", "posts", "docs", "notes"]
```

## 內容類型

你可能想在其他 Section 中使用 `docs` 布局，比如 `/notes`。這可以通過在 front matter 中設置 `type = "docs"` 實現。 

## 文章原型

我們也可以為 `notes` 創建一個原型，讓 Hugo 處理 `type`。

```bash
$ cp themes/hugo-theme-bootstrap/archetypes/default.md archetypes/notes.md
```

然後在 `archetypes/notes.md` 中添加 `type = "docs"`。現在 `hugo new notes/blah-blah-blah` 將會復製 `archetypes/notes.md` 的內容到新的文章。

同樣地，你也可以為 `posts`、`docs` 等自定義原型。

## Sections 模板

你可能還想在 `notes` 中使用和 `docs` 一樣的列表布局。

```html {title="layouts/notes/list.html"}
{{ define "content" }}
{{- partial "docs/nav" . -}}
<div class="col-lg-7 ms-auto">
  {{ partial "docs/list" . }}
</div>
{{- partial "docs/sidebar" . -}}
{{ end }}
```

## 書寫文章

> 假設默認語言為 `en`。

```bash
$ hugo new posts/new-post/index.md
```

上述命令創建了一篇英文文章，同樣的，我們也可以創建一篇簡體中文的文章：

```bash
$ hugo new posts/new-post/index.zh-tw.md
```

> 請注意：創建的文章一般處於草稿狀態，本地預覽時，`hugo server` 需要指定 `-D` 參數才能預覽草稿文章。文章發佈時，需要將 `draft` 改為 `false`，或者直接移除 `draft` 參數。

## Summary Selection Order

1. If `post.excerpt = "description"` and `description` is not empty, then it'll be used.
1. Manual splitting via <code>&lt;!--more--&gt;</code>.
1. If `summary` on front matter isn't empty, then `summary` will be selected.
1. The text of content will be cut off by `post.excerptMaxLength` and formatted in plain text or HTML when `post.plainifyExcerpt = true`.

```toml {title="config/_default/params.toml"}
[post]
  # excerpt = "description"
  # excerptMaxLength = 120
  # copyright = false # Whether to display copyright section on each post.
  # plainifyExcerpt = false # Format excerpt in HTML if false.
```

## Thumbnail Selection Order

1. The `images` on front matter are preferred.
1. Page images resources that match the filename's patterns: `*feature*`, `*cover*` and `*thumbnail*`. Such as `posts/my-page/feature.png`.

> The page images resources will be resized to several smaller versions to suit the users devices for saving the bandwidth.

## 文章置頂

你可以通過在 front matter 設置 `pinned` 為 `true` 以置頂文章。

```markdown
+++
title = "Pinned Post"
pinned = true
weight = 100
+++
```

> 如果有多個置頂文章，那麽將按照 `weight` 進行降序排序。

### 站點配置

```toml {title="config/_default/params.toml"}
pinnedPost = false # 關閉文章置頂功能
pinnedPostCount = 2 # 首頁顯示的置頂文章的數目
```

## Carousel

將文章于 carousel 顯示。

```markdown
+++
carousel = true
+++
```

## 作者

HBS supports the authors taxonomy. Firstly, you'll need to enable it by setting the following configuration.

{{< code-toggle filename="config" >}}
[taxonomies]
  author = "authors"
{{</ code-toggle >}}

And then define the `authors` on your posts.

```toml
+++
authors = [
  "Foo",
  "Bar"
]
+++
```

Now, the authors will be present on the post meta and sidebar taxonomies.

Finally, we may need to introduce the author in more detail. Take the `Foo` as an example, just create a page with the following content and save it as `/content/authors/foo/index.md`.

```yaml
---
title: Razon Yang
description: Gopher, PHPer, Full Stack Engineer.
social:
  github: razonyang
  twitter: razonyang
---
```

- `title`: The author display name.
- `description`: The introduction.
- `social`: [Social links]({{< ref "/docs/widgets/social-links" >}}).

The author image should be placed at the same folder with pattern `avatar*`, such as `/content/authors/foo/avatar.png`.

## 下一步

- [調整配置]({{< ref "/docs/configuration" >}})
- [菜單]({{< ref "/docs/configuration/menu" >}})
- [觀感]({{< ref "/docs/look-and-feel" >}})