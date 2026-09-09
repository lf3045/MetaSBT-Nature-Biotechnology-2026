# Genome Biology 2026：原核基因组注释工具大规模benchmark

论文：Jundzill M, Hölzer M, Mangul S, et al. **Large-scale benchmarking of prokaryotic annotation tools across thousands of species.** *Genome Biology* 27, 284 (2026). DOI: 10.1186/s13059-026-04262-0.

## 文件结构

- `article.md`：在线排版版，图片使用 GitHub Raw 公网地址，适合复制到 didispace/OpenWrite 等在线 Markdown 编辑器。
- `article_local.md`：本地长期维护版，图片使用 `images/...` 相对路径。
- `images/`：论文主文 Fig.1–5。
- `figure_manifest.md`：图片对应关系、图意和许可信息。
- `downloads/GenomeBiology-Prokaryotic-Annotation-Benchmark-2026.zip`：完整发布包。

## 后续怎么修改而不丢图片

长期维护建议优先修改 `article_local.md`。只要文件仍然和 `images/` 保持当前目录结构，Typora、Obsidian、VS Code 等本地编辑器都会继续正常显示图片。

仓库中的 GitHub Actions 会根据 `article_local.md` 自动生成 `article.md`，把相对图片路径替换为 `raw.githubusercontent.com` 公网图片链接，并重新生成 ZIP。

## 在线排版

`article.md` 可以直接复制到支持远程 Markdown 图片的在线编辑器。图片地址指向本仓库的公开 Raw 文件，因此无需再次手工上传图片。

## 图片许可

论文为 CC BY 4.0 Open Access。Springer Nature 权限页说明，文章中的图片和第三方材料在没有单独 credit line 排除的情况下均包含在该许可中。Fig.1 原文注明使用 BioRender 制作，转载和二次修改时建议保留原论文、作者和许可说明。
