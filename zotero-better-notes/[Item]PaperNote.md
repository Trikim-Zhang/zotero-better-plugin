# [Item] PaperNote 适配Zotero 6 7 中英文文献 自用SCI笔记模版 简约清晰风格

- `MetaData` : Auto extract`Title`, `Author`, `Journal`, `Journal Tags`, `Local Link`, `DOI`, `Abstract`, `Note Date`, etc.
- `DOI` : Show `url` if the DOI does not exist.
- `Abstract` : Show `abstractTranslation` if the it exists. Otherwise, show `abstractNote`.

这个模版可以自动适配中英文文献，自动提取标题或标题翻译、作者、期刊、期刊分区、本地链接、DOI或者URL、中文摘要或英文摘要或英文摘要的翻译、笔记日期等原数据，模版样式简约清新。

期刊分区需要安装[zotero-style](https://github.com/MuiseDestiny/zotero-style)插件，并配置`期刊标签`功能。标题和英文摘要的翻译需要安装[zotero-pdf-translate](https://github.com/windingwind/zotero-pdf-translate)插件。

关于此模版的一些常见问题，可以参考[discussions/771](https://github.com/windingwind/zotero-better-notes/discussions/771)

## Zotero7 PaperNote Template

```yaml
# Zotero7 PaperNote Template
# @author [jackhanyuan](https://github.com/jackhanyuan)
# @source [zotero-better-plugin](https://github.com/jackhanyuan/zotero-better-plugin)
# This template is specifically for importing/sharing, using better 
# notes 'import from clipboard': copy the content and
# goto Zotero menu bar, click Edit->New Template from Clipboard.  
# Do not copy-paste this to better notes template editor directly.
name: "[Item]PaperNote"
content: |-
  <!-- Title -->
  <h1 style="color:#193c47; background-color:#eef9fd; padding:8px;">
    ${(() => {
      const titleTranslation = topItem.getField("titleTranslation");
      if (titleTranslation) {
        return `(${topItem.getField("date")}) ${topItem.getField("title")} (${titleTranslation})`;
      } else {
        return `(${topItem.getField("date")}) ${topItem.getField("title")}`;
      }
    })()}
  </h1>
  
  <!-- Meta Data -->
  <span>
    <h2 style="color:#1b5e20; background-color:#f1f8e9;">💡 Meta Data</h2>
  </span>
  
  <table>
    <!-- Author -->
    <tr>
      <td style="color:#193c47; background-color:#dbeedd; padding:8px;">
        <b>Author:</b> ${topItem.getCreators().map((v) => v.firstName + " " + v.lastName).join("; ")}
      </td>
    </tr>
  
    <!-- Journal -->
    <tr>
      <td style="color:#193c47; background-color:#f3faf4; padding:8px;">
        <b style="color:#193c47;">Journal: <b style="color:#FF0000">${topItem.getField('publicationTitle')}</b></b><b style="color:#193c47;"> (Publication Date: ${topItem.getField("date")})</b>
      </td>
    </tr>
  
    <!-- Journal Tags -->
    <tr>
      <td style="color:#193c47; background-color:#dbeedd; padding:8px;">
        <b>Journal Tags: </b>
        <!-- In zotero7, the tags of Ethereal Style plugin are referenced -->
        ${ (() => {
            let tags = Zotero.ZoteroStyle.data.ztoolkit.ItemTree.fieldHooks.globalCache.getFieldHooks.PublicationTags("", true, true, topItem, undefined);
            const jsonStartIndex = tags.indexOf('{');
            if (jsonStartIndex !== -1) {
                tags = tags.slice(jsonStartIndex);
            }
            let regex = /"(sci|sciif)":"([^"]+)"/g;
            let matches = tags.match(regex);
  
            <!-- Conditional statement: to prevent errors when unable to find division for dissertations, conference papers, etc. -->
            if (tags) {
                const keyValuePairs = JSON.parse(tags);
                const colors = ["#94635c", "#d0867b", "#dc796a", "#ba8f6c", "#c8825c", "#e78a59", "#e99d22", "#ce9335", "#ff945c"];
                let out = "";
  
                <!-- The division information uses easyscholar, there's a tutorial in zotero style -->
                for (const key in keyValuePairs) {
                    if (key === "sci") {
                        out += `<span style="color:#ffffff; background-color:${colors[0]}; padding:6px;">SCI ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "sciif") {
                        out += `<span style="color:#ffffff; background-color:${colors[2]}; padding:6px;">IF ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "ccf") {
                        out += `<span style="color:#ffffff; background-color:${colors[4]}; padding:6px;">CCF ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "sciUp") {
                        out += `<span style="color:#ffffff; background-color:${colors[6]}; padding:6px;">CAS ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "ssci") {
                        out += `<span style="color:#ffffff; background-color:${colors[8]}; padding:6px;">SSCI ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "eii") {
                        out += `<span style="color:#ffffff; background-color:${colors[1]}; padding:6px;">${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "pku") {
                        out += `<span style="color:#ffffff; background-color:${colors[3]}; padding:6px;">PKU ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "cssci") {
                        out += `<span style="color:#ffffff; background-color:${colors[5]}; padding:6px;">${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "utd24") {
                        out += `<span style="color:#ffffff; background-color:${colors[7]}; padding:6px;">${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "ft50") {
                        out += `<span style="color:#ffffff; background-color:${colors[0]}; padding:6px;">${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "fms") {
                        out += `<span style="color:#ffffff; background-color:${colors[2]}; padding:6px;">FMS ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                }
                return out;
            }
        })()}
      </td>
    </tr>
  
    <!-- Local Link -->
    <tr>
      <td style="color:#193c47; background-color:#f3faf4; padding:8px;">
        <b>Local Link: </b>
        <a href=zotero://open-pdf/0_${Zotero.Items.get(topItem.getAttachments()).filter((i) => i.isPDFAttachment())[0].key}>
          ${Zotero.Items.get(topItem.getAttachments()).filter((i) => i.isPDFAttachment())[0].getFilename()}
        </a>
      </td>
    </tr>
    
    <!-- DOI or URL -->
    <tr>
      <td style="color:#193c47; background-color:#dbeedd; padding:8px;">
        ${(() => {
          const doi = topItem.getField("DOI");
          if (doi) {
            return `<b>DOI: </b><a href="https://doi.org/${topItem.getField('DOI')}">${topItem.getField('DOI')}</a>`;
          } else {
            return `<b>URL: </b><a href="${topItem.getField('url')}">${topItem.getField('url')}</a>`;
          }
        })()}
      </td>
    </tr>
    
    <!-- Abstract -->
    <tr>
      <td style="color:#193c47; background-color:#f3faf4; padding:8px;">
        ${(() => {
          const abstractTranslation = topItem.getField('abstractTranslation');
          if (abstractTranslation) {
            return `<b>Abstract Translation: </b><i>${abstractTranslation}</i>`;
          } else {
            return `<b>Abstract: </b><i>${topItem.getField('abstractNote')}</i>`;
          }
        })()}
      </td>
    </tr>
  
    <!-- Note Date -->
    <tr>
      <td style="color:#193c47; background-color:#dbeedd; padding:8px;">
        <b>Note Date: </b>${new Date().toLocaleString()}
      </td>
    </tr>
  
  </table>
  
  <!-- Main Content -->
  <span>
    <h2 style="color:#e0ffff; background-color:#66cdaa;">📜 Research Core</h2>
    <hr />
  </span>
  <blockquote>Tips: What was done, what problem was solved, innovations and shortcomings?</blockquote>
  <p></p>
  <h3>⚙️ Content</h3>
  <p></p>
  <h3>💡 Innovations</h3>
  <p></p>
  <h3>🧩 Shortcomings</h3>
  <p></p>
  
  <span>
    <h2 style="color:#20b2aa; background-color:#afeeee;">🔁 Research Content</h2>
    <hr />
  </span>
  <p></p>
  <h3>💧 Data</h3>
  <p></p>
  <h3>👩🏻‍💻 Method</h3>
  <p></p>
  <h3>🔬 Experiment</h3>
  <p></p>
  <h3>📜 Conclusion</h3>
  <p></p>
  
  <span>
    <h2 style="color:#004d99; background-color:#87cefa;">🤔 Personal Summary</h2>
    <hr />
  </span>
  <blockquote>Tips: What aspects did you question, how do you think it can be improved?</blockquote>
  <p></p>
  <h3>🙋‍♀️ Key Records</h3>
  <p></p>
  <h3>📌 To be resolved</h3>
  <p></p>
  <h3>💭 Thought Inspiration</h3>
  <p></p>
```

Zotero7 PaperNote Template 中文版本在下面，仅仅把文字替换成了中文

```yaml
# Zotero7 PaperNote Template (Chinese)
# @author [jackhanyuan](https://github.com/jackhanyuan)
# @source [zotero-better-plugin](https://github.com/jackhanyuan/zotero-better-plugin)
# This template is specifically for importing/sharing, using better 
# notes 'import from clipboard': copy the content and
# goto Zotero menu bar, click Edit->New Template from Clipboard.  
# Do not copy-paste this to better notes template editor directly.
name: "[Item]论文笔记"
content: |-
  <!-- 标题 -->
  <h1 style="color:#193c47; background-color:#eef9fd; padding:8px;">
    ${(() => {
      const titleTranslation = topItem.getField("titleTranslation");
      if (titleTranslation) {
        return `(${topItem.getField("date")}) ${topItem.getField("title")} (${titleTranslation})`;
      } else {
        return `(${topItem.getField("date")}) ${topItem.getField("title")}`;
      }
    })()}
  </h1>
  
  <!-- Meta Data -->
  <span>
    <h2 style="color:#1b5e20; background-color:#f1f8e9;">💡 Meta Data</h2>
  </span>
  
  <table>
    <!-- 作者 -->
    <tr>
      <td style="color:#193c47; background-color:#dbeedd; padding:8px;">
        <b>作者:</b> ${topItem.getCreators().map((v) => v.firstName + " " + v.lastName).join("; ")}
      </td>
    </tr>
  
    <!-- 期刊 -->
    <tr>
      <td style="color:#193c47; background-color:#f3faf4; padding:8px;">
        <b style="color:#193c47;">期刊: <b style="color:#FF0000">${topItem.getField('publicationTitle')}</b></b><b style="color:#193c47;"> （发表日期: ${topItem.getField("date")}）</b>
      </td>
    </tr>
  
    <!-- 期刊分区 -->
    <tr>
      <td style="color:#193c47; background-color:#dbeedd; padding:8px;">
        <b>期刊分区: </b>
        <!-- zotero7中，引用了Ethereal Style插件的标签 -->
        ${ (() => {
            let tags = Zotero.ZoteroStyle.data.ztoolkit.ItemTree.fieldHooks.globalCache.getFieldHooks.PublicationTags("", true, true, topItem, undefined);
            const jsonStartIndex = tags.indexOf('{');
            if (jsonStartIndex !== -1) {
                tags = tags.slice(jsonStartIndex);
            }
            let regex = /"(sci|sciif)":"([^"]+)"/g;
            let matches = tags.match(regex);
  
            <!-- 条件语句：防止学位论文、会议论文等找不到分区报错 -->
            if (tags) {
                const keyValuePairs = JSON.parse(tags);
                const colors = ["#94635c", "#d0867b", "#dc796a", "#ba8f6c", "#c8825c", "#e78a59", "#e99d22", "#ce9335", "#ff945c"];
                let out = "";
  
                <!-- 分区信息使用的是easyscholar，zotero style有教程 -->
                for (const key in keyValuePairs) {
                    if (key === "sci") {
                        out += `<span style="color:#ffffff; background-color:${colors[0]}; padding:6px;">SCI ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "sciif") {
                        out += `<span style="color:#ffffff; background-color:${colors[2]}; padding:6px;">IF ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "ccf") {
                        out += `<span style="color:#ffffff; background-color:${colors[4]}; padding:6px;">CCF ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "sciUp") {
                        out += `<span style="color:#ffffff; background-color:${colors[6]}; padding:6px;">中科院 ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "ssci") {
                        out += `<span style="color:#ffffff; background-color:${colors[8]}; padding:6px;">SSCI ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "eii") {
                        out += `<span style="color:#ffffff; background-color:${colors[1]}; padding:6px;">${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "pku") {
                        out += `<span style="color:#ffffff; background-color:${colors[3]}; padding:6px;">北核 ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "cssci") {
                        out += `<span style="color:#ffffff; background-color:${colors[5]}; padding:6px;">${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "utd24") {
                        out += `<span style="color:#ffffff; background-color:${colors[7]}; padding:6px;">${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "ft50") {
                        out += `<span style="color:#ffffff; background-color:${colors[0]}; padding:6px;">${keyValuePairs[key]}</span>&nbsp;`;
                    }
                    if (key === "fms") {
                        out += `<span style="color:#ffffff; background-color:${colors[2]}; padding:6px;">FMS ${keyValuePairs[key]}</span>&nbsp;`;
                    }
                }
                return out;
            }
        })()}
      </td>
    </tr>
  
    <!-- 本地链接 -->
    <tr>
      <td style="color:#193c47; background-color:#f3faf4; padding:8px;">
        <b>本地链接: </b>
        <a href=zotero://open-pdf/0_${Zotero.Items.get(topItem.getAttachments()).filter((i) => i.isPDFAttachment())[0].key}>
          ${Zotero.Items.get(topItem.getAttachments()).filter((i) => i.isPDFAttachment())[0].getFilename()}
        </a>
      </td>
    </tr>
    
    <!-- DOI or URL -->
    <tr>
      <td style="color:#193c47; background-color:#dbeedd; padding:8px;">
        ${(() => {
          const doi = topItem.getField("DOI");
          if (doi) {
            return `<b>DOI: </b><a href="https://doi.org/${topItem.getField('DOI')}">${topItem.getField('DOI')}</a>`;
          } else {
            return `<b>URL: </b><a href="${topItem.getField('url')}">${topItem.getField('url')}</a>`;
          }
        })()}
      </td>
    </tr>
    
    <!-- 摘要 -->
    <tr>
      <td style="color:#193c47; background-color:#f3faf4; padding:8px;">
        ${(() => {
          const abstractTranslation = topItem.getField('abstractTranslation');
          if (abstractTranslation) {
            return `<b>摘要翻译: </b><i>${abstractTranslation}</i>`;
          } else {
            return `<b>摘要: </b><i>${topItem.getField('abstractNote')}</i>`;
          }
        })()}
      </td>
    </tr>
  
    <!-- 笔记日期 -->
    <tr>
      <td style="color:#193c47; background-color:#dbeedd; padding:8px;">
        <b>笔记日期: </b>${new Date().toLocaleString()}
      </td>
    </tr>
  
  </table>
  
  <!-- 正文 -->
  <span>
    <h2 style="color:#e0ffff; background-color:#66cdaa;">📜 研究核心</h2>
    <hr />
  </span>
  <blockquote>Tips: 做了什么，解决了什么问题，创新点与不足？</blockquote>
  <p></p>
  <h3>⚙️ 内容</h3>
  <p></p>
  <h3>💡 创新点</h3>
  <p></p>
  <h3>🧩 不足</h3>
  <p></p>
  
  <span>
    <h2 style="color:#20b2aa; background-color:#afeeee;">🔁 研究内容</h2>
    <hr />
  </span>
  <p></p>
  <h3>💧 数据</h3>
  <p></p>
  <h3>👩🏻‍💻 方法</h3>
  <p></p>
  <h3>🔬 实验</h3>
  <p></p>
  <h3>📜 结论</h3>
  <p></p>
  
  <span>
    <h2 style="color:#004d99; background-color:#87cefa;">🤔 个人总结</h2>
    <hr />
  </span>
  <blockquote>Tips: 你对哪些内容产生了疑问，你认为可以如何改进？</blockquote>
  <p></p>
  <h3>🙋‍♀️ 重点记录</h3>
  <p></p>
  <h3>📌 待解决</h3>
  <p></p>
  <h3>💭 思考启发</h3>
  <p></p>
```

## Zotero6 PaperNote Template

```yaml
# Zotero6 PaperNote Template (Chinese)
# @author [jackhanyuan](https://github.com/jackhanyuan)
# @source [zotero-better-plugin](https://github.com/jackhanyuan/zotero-better-plugin)
# This template is specifically for importing/sharing, using better 
# notes 'import from clipboard': copy the content and
# goto Zotero menu bar, click Edit->New Template from Clipboard.  
# Do not copy-paste this to better notes template editor directly.
name: "[Item]论文笔记"
content: |-
  <!-- 标题 -->
  <h1 style="color:#193c47; background-color:#eef9fd; padding:8px;">
    ${(() => {
      const titleTranslation = topItem.getField("titleTranslation");
      if (titleTranslation) {
        return `(${topItem.getField("date")}) ${topItem.getField("title")} (${titleTranslation})`;
      } else {
        return `(${topItem.getField("date")}) ${topItem.getField("title")}`;
      }
    })()}
  </h1>
  
  <!-- Meta Data -->
  <span>
    <h2 style="color:#1b5e20; background-color:#f1f8e9;">💡 Meta Data</h2>
  </span>
  
  <table>
    <!-- 作者 -->
    <tr>
      <td style="color:#193c47; background-color:#dbeedd; padding:8px;">
        <b>作者:</b> ${topItem.getCreators().map((v) => v.firstName + " " + v.lastName).join("; ")}
      </td>
    </tr>
  
    <!-- 期刊 -->
    <tr>
      <td style="color:#193c47; background-color:#f3faf4; padding:8px;">
        <b style="color:#193c47;">期刊: <b style="color:#FF0000">${topItem.getField('publicationTitle')}</b></b><b style="color:#193c47;"> （发表日期: ${topItem.getField("date")}）</b>
      </td>
    </tr>
  
    <!-- 期刊分区 -->
    <tr>
      <td style="color:#193c47; background-color:#dbeedd; padding:8px;">
        <b>期刊分区: </b>
        <!-- 引用了zotero style插件的标签，请提前安装，否则找不到分区 -->
        ${(() => {let space = " ㅤㅤ ㅤㅤ"
                return Array.prototype.map.call(Zotero.ZoteroStyle.data.ztoolkit.ItemTree.globalCache.renderCellHooks.PublicationTags(
            0,
            Zotero.ZoteroStyle.data.ztoolkit.ItemTree.fieldHooks.globalCache.getFieldHooks.PublicationTags(
            "", true, true,
            topItem, undefined)
            ).childNodes,
            e => {
            e.innerText = space + space + e.innerText + space + space;
            return e.outerHTML
            }).join(space)
            })()
            }
      </td>
    </tr>
  
    <!-- 本地链接 -->
    <tr>
      <td style="color:#193c47; background-color:#f3faf4; padding:8px;">
        <b>本地链接: </b>
        <a href=zotero://open-pdf/0_${Zotero.Items.get(topItem.getAttachments()).filter((i) => i.isPDFAttachment())[0].key}>
          ${Zotero.Items.get(topItem.getAttachments()).filter((i) => i.isPDFAttachment())[0].getFilename()}
        </a>
      </td>
    </tr>
    
    <!-- DOI or URL -->
    <tr>
      <td style="color:#193c47; background-color:#dbeedd; padding:8px;">
        ${(() => {
          const doi = topItem.getField("DOI");
          if (doi) {
            return `<b>DOI: </b><a href="https://doi.org/${topItem.getField('DOI')}">${topItem.getField('DOI')}</a>`;
          } else {
            return `<b>URL: </b><a href="${topItem.getField('url')}">${topItem.getField('url')}</a>`;
          }
        })()}
      </td>
    </tr>
    
    <!-- 摘要 -->
    <tr>
      <td style="color:#193c47; background-color:#f3faf4; padding:8px;">
        ${(() => {
          const abstractTranslation = topItem.getField('abstractTranslation');
          if (abstractTranslation) {
            return `<b>摘要翻译: </b><i>${abstractTranslation}</i>`;
          } else {
            return `<b>摘要: </b><i>${topItem.getField('abstractNote')}</i>`;
          }
        })()}
      </td>
    </tr>
  
    <!-- 笔记日期 -->
    <tr>
      <td style="color:#193c47; background-color:#dbeedd; padding:8px;">
        <b>笔记日期: </b>${new Date().toLocaleString()}
      </td>
    </tr>
  
  </table>
  
  <!-- 正文 -->
  <span>
    <h2 style="color:#e0ffff; background-color:#66cdaa;">📜 研究核心</h2>
    <hr />
  </span>
  <blockquote>Tips: 做了什么，解决了什么问题，创新点与不足？</blockquote>
  <p></p>
  <h3>⚙️ 内容</h3>
  <p></p>
  <h3>💡 创新点</h3>
  <p></p>
  <h3>🧩 不足</h3>
  <p></p>
  
  <span>
    <h2 style="color:#20b2aa; background-color:#afeeee;">🔁 研究内容</h2>
    <hr />
  </span>
  <p></p>
  <h3>💧 数据</h3>
  <p></p>
  <h3>👩🏻‍💻 方法</h3>
  <p></p>
  <h3>🔬 实验</h3>
  <p></p>
  <h3>📜 结论</h3>
  <p></p>
  
  <span>
    <h2 style="color:#004d99; background-color:#87cefa;">🤔 个人总结</h2>
    <hr />
  </span>
  <blockquote>Tips: 你对哪些内容产生了疑问，你认为可以如何改进？</blockquote>
  <p></p>
  <h3>🙋‍♀️ 重点记录</h3>
  <p></p>
  <h3>📌 待解决</h3>
  <p></p>
  <h3>💭 思考启发</h3>
  <p></p>
```
