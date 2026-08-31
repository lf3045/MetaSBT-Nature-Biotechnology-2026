# Nature CRISIS 2026｜Markdown 发布包

论文：**CRISPR–Cas regulates expression of embedded anti-phage defence systems**  
DOI：10.1038/s41586-026-10833-9

## 文件

- `article.md`：公共 GitHub / 在线 Markdown 版。由于论文不是 CC BY，不重新托管原文 Fig.1–5，只保留图号、图意和版权说明。
- 本地完整包另含 `article_local.md`：使用 `images/...` 相对路径插入 Fig.1–5。
- 本地完整包的 `images/`：从用户提供 PDF 裁切的主文 Fig.1–5，仅用于个人阅读、笔记和编辑。
- `figure_manifest.md`：图片位置、来源和版权说明。

## 在线排版

`article.md` 可以直接复制到 didispace/OpenWrite 等在线 Markdown 编辑器。由于公共版未托管原论文图片，在线版对应位置显示文字图注。

如果后续获得 Springer Nature/作者对原图公开再发布的明确许可，可以把 `article.md` 中的文字图注替换为有权公开托管的图片地址。

## 本地完整包结构

```text
Nature-CRISIS-2026/
├── article.md
├── article_local.md
├── README.md
├── figure_manifest.md
└── images/
    ├── fig01_crlRNA_regulation.png
    ├── fig02_crisis_diversity.png
    ├── fig03_antiphage_domains.png
    ├── fig04_sir2_hera_layered_immunity.png
    └── fig05_wukong_plasmid_tradeoff.png
```

## 版权提示

用户提供的论文 PDF 版权页写明：`© The Author(s), under exclusive licence to Springer Nature Limited 2026`。该文件未显示 Creative Commons Attribution 许可。因此本地包中的论文原图默认按个人研究/编辑素材处理，不建议直接复制到公共 GitHub 或公开文章中，除非你的使用场景具有相应许可或法律依据。
