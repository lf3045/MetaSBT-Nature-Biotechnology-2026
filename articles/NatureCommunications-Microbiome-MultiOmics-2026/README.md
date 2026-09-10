# Nature Communications 2026：微生物组多组学整合路线图

论文：Van Den Bossche T, Lazau EA, Aho VTE, et al. **Integrating multi-omics technologies to decipher microbiome functions.** *Nature Communications* 17, 9600 (2026). DOI: 10.1038/s41467-026-77539-4.

## 文件结构

- `article_local.md`：本地长期维护版，图片使用 `images/...` 相对路径。
- `article.md`：在线排版版，图片使用 GitHub Raw 公网地址，可复制到 didispace/OpenWrite 等在线 Markdown 编辑器。
- `images/`：论文主文 Fig.1–2。
- `figure_manifest.md`：图片对应关系、图意和许可信息。
- `downloads/NatureCommunications-Microbiome-MultiOmics-2026.zip`：完整发布包。

## 推荐维护方式

长期修改优先编辑 `article_local.md`。保持 `article_local.md` 与 `images/` 的目录关系不变，本地编辑器会持续正常显示图片。

仓库中的 GitHub Actions 会下载主文图片、把本地相对路径转换为 GitHub Raw 公网链接生成 `article.md`，并重新打包 ZIP。

## 图片许可

原论文为 CC BY 4.0 Open Access。文章中的图片和第三方材料在没有单独 credit line 排除的情况下包含于该许可。转载、修改或再次发布时应保留作者、论文来源、许可证，并在修改图片时注明进行了修改。
