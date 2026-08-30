# MetaSBT Nature Biotechnology 2026 中文文章发布包

本仓库保存论文中文解读 Markdown 和原论文主图，方便后续反复修改，同时保持图片链接稳定。

## 推荐使用

- `article.md`：**在线发布版**。图片使用 GitHub Raw 公网地址，适合复制到 didispace/OpenWrite 等在线 Markdown 编辑器。
- `article_local.md`：**本地编辑版**。图片使用 `images/...` 相对路径，适合 Typora、Obsidian、VS Code 等。
- `images/`：论文 Fig.1–5。
- `figure_manifest.md`：图号、用途和授权说明。

## 在线 Markdown 编辑器

直接打开 `article.md`，复制全文到在线 Markdown 编辑器。图片链接形如：

```markdown
![MetaSBT整体框架](https://raw.githubusercontent.com/lf3045/MetaSBT-Nature-Biotechnology-2026/main/images/fig01_metasbt_framework.png)
```

只要目标编辑器允许加载远程图片，图片会跟随正文出现。

## 本地长期编辑

下载整个仓库，保持：

```text
article_local.md
images/
```

的相对位置不变，图片就不会因为移动 Markdown 文件而丢失。

## 后续修改文章

建议优先修改 `article.md`。仓库中的 GitHub Actions 会自动生成对应的 `article_local.md`，把公网图片链接转换为本地相对路径。

如果只改文字，不需要动 `images/`。只要图片文件名不变，已经发布的图片链接也不会变化。

## 图片来源与授权

论文：Cumbo F, Blankenberg D. *Characterization of microbial dark matter at scale with MetaSBT and taxonomy-aware Sequence Bloom Trees*. **Nature Biotechnology** (2026). DOI: `10.1038/s41587-026-03245-7`。

论文采用 **CC BY 4.0**。本仓库 Fig.1–5 来自 Nature 论文公开图片资源；转载时应保留作者、论文来源和许可证说明。如果某张图片存在单独第三方 credit line，应以原文说明为准。
