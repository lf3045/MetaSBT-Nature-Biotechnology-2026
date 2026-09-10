# Nature Genetics 2022：PanGenie 泛基因组基因分型

论文：Ebler J, Ebert P, Clarke WE, et al. **Pangenome-based genome inference allows efficient and accurate genotyping across a wide spectrum of variant classes.** *Nature Genetics* 54, 518–525 (2022). DOI: 10.1038/s41588-022-01043-w.

## 文件结构

- `article.md`：在线排版版，图片使用 GitHub Raw 公网地址，可复制到 didispace/OpenWrite。
- `article_local.md`：本地长期维护版，图片使用 `images/...` 相对路径。
- `images/`：论文主文 Fig.1–5。
- `figure_manifest.md`：图号、文件名、用途和许可说明。
- `downloads/NatureGenetics-PanGenie-2022.zip`：完整 Markdown + 图片发布包。

## 推荐维护方式

长期编辑建议以 `article_local.md` 为主。只要保持它与 `images/` 的相对目录结构不变，在 Typora、Obsidian、VS Code 等本地 Markdown 编辑器中修改正文不会丢图。

仓库中的 GitHub Actions 会自动下载论文 Fig.1–5、生成使用 Raw 图片地址的 `article.md`，并重新打包 ZIP。

## 许可

原论文为 CC BY 4.0 Open Access。Nature 页面说明，文章中的图片和第三方材料在没有单独 credit line 排除的情况下包含于该许可。公开转载时请保留作者、论文来源、CC BY 4.0许可信息，并在修改图片时说明改动。
