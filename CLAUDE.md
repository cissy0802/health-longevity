# 本仓自治规则（内容层：自停 · 自愈 · 英文泄漏自查）

TOPICS.md 是本仓选题的**唯一真相源**。以下三条每次运行都适用；trigger 的启用/停用由外部 `verify-routine-caps` 守卫按 N_pub vs N_top 统一管，**你不要去改 trigger**。

## 1. 自停（写完 TOPICS 就不再生成）
运行开头先算：
- `N_pub` = 已发布页最高编号：`ls *-day*.html`（或本仓命名 week/issue/book/topic）去 `.en.html`、去前导零取最大。
- `N_top` = TOPICS.md 最大编号：`grep '^[-*#]* *(Day|Week|Issue|Book|Topic) *[0-9]+'` 取最大。

**若 N_pub ≥ N_top（TOPICS 已全部写完）**：本次**不生成任何页面、不修改 TOPICS.md、绝不自己发明新主题**；直接结束（至多发一条『本站已写完 TOPICS 路线图，补充新主题后自动续写』的通知，且仅在你判断这是刚写完的当次时发，不要每次都发）。**这条优先级高于本仓 routine prompt 里任何"超表就自造新主题"的旧措辞。** 新主题只由 BigCat 手动或 deep-research 反哺加进 TOPICS.md；加了之后守卫会自动重开、routine 自然续写。

## 2. 自愈（保持 TOPICS ⊇ 已发布）
**只在越界时动手**：若本期 Day/编号按 TOPICS 计划写、TOPICS 里本就有这行 → 不动。只有当你发布的编号是 **TOPICS.md 里没有的**（用户手动指定主题、或 deep-research 反哺内容，当时漏记进 TOPICS）时：
1. 用本期页面标题/副标题，在 TOPICS.md **末尾 append** 一行 `- Day N: <主题> — <要点>`（前缀 Day/Week/Issue/Book/Topic 随本仓约定，先 `grep` 看已有行照抄）。
2. 与本期内容改动**一并 commit / push 到 main**。
3. **只 append，绝不修改已有行。** 否则越界页占了 `dayN`、TOPICS 缺行，日后往 TOPICS 补的 Day N 新主题会撞车永远写不出。

## 3. 英文页中文泄漏自查
生成 `*.en.html` 后、publish 前，对它 grep 这些指纹（命中=模板槽把中文漏进了英文页）：
`class="en">[一-鿿]` / `class="name-en">[一-鿿]` / `class="name-zh">[一-鿿]` / `class="cn">[一-鿿]` / `Reflections — [一-鿿]` / `class="lang-tag">ZH`
命中就修掉（删中文节点或译成英文）再 publish。**正常情况不算泄漏**：经文/古典原典+英译、孙子兵法原文+英译、term(中文) 括注、代码/分词演示、语言切换标签『中文』、主题本身是中文的页。


## index 维护：roadmap-first（写时把灰色占位转成链接，勿 append 重复）

本仓 `index.html` / `index.en.html` 已改为「路线图先出」：TOPICS.md 里**还没写**的条目，已作为灰色占位行 `<div class="entry todo">…</div>`（无 href、不可点、`todo` 类）预先列在列表里。

**写某编号 N 时**：在**两个** index 里找到该 N 对应的那行灰色占位 `<div class="entry todo">`，**原地改成可点链接**——把 `<div class="entry todo">…</div>` 换成 `<a class="entry" href="{本期文件名}">…</a>`（去掉 `todo` 类、加 href，内部 span 结构保持不变）。`index.html` 用中文页文件名、`index.en.html` 用 `.en.html` 文件名。

**绝不要**在列表末尾另 append 一行——否则会和灰色占位行重复出现两条。

只有当该 N 在 index 里**没有**对应灰色占位行时（越界 / 新反哺主题当时没进占位），才在列表末尾 append 新的 `<a class="entry">` 行。


### 每次运行先「对齐」灰色占位（TOPICS 增长时自动补灰行）

每次运行开头，先扫 `TOPICS.md`：凡是**还没写**（无对应页）**且** index 里**还没有对应行**的编号，就按编号顺序在列表相应位置补一条灰色占位行 `<div class="entry todo">…</div>`（无 href、`todo` 类），内部 span 结构照现有条目。`index.en.html` 里顺带把标题/要点翻成 house-style 英文（**勿泄漏中文**），`index.html` 用 TOPICS 中文文本裁成本仓 zh 风格。**两个 index 都要补。**

这样 BigCat / deep-research 往 TOPICS 末尾加的新主题，下次运行就会自动作为灰色条目出现；等真正写它时再按上面的规则**原地**转成链接（勿另 append）。

## 新页面必带共享脚本（免触发 inject-comments 机器人提交）

生成任何 `*.html`（含 `.en.html`）时，在 `</body>` 前直接写入这 4 行，勿遗漏：

```html
<script src="https://hub.cissychen.com/comments.js" defer></script>
<script src="https://hub.cissychen.com/search.js" defer></script>
<script src="https://hub.cissychen.com/index-button.js" defer></script>
<script src="https://hub.cissychen.com/i18n-tts.js" defer></script>
```

这样 CI 的 inject-comments 不会再对新页面追加自动提交。

## 文末两节：越界 + 十条要点（2026-10-17 起，每篇新页必写）

本仓页面结构（照最近一期抄）：`header` → 若干张主题 `.card`（每张含一句话结论 / 科学背景 + 机制 / 可执行协议 / 女性特别提示 + 常见误区 / 本周一试 + 思考题）→ `.card.think` 深入思考（有的期没有）→ `.nav`。**2026-10-17 起**，在最后一张主题 `.card` 之后、`<div class="card think">`（深入思考）之前（没有深入思考就放在 `.nav` 之前），按顺序插入下面两节；中英两页都要有。样式照 `.card.think` 的「card 里用 `.name` 当标题」写法：

```html
<div class="card crossx"><div class="name">越界 · 跨学科的联想</div><ul>
<li><b>学科 · 概念/人物</b>：……</li>
</ul></div>

<div class="card tk"><div class="name">十条要点</div><ol class="takeaways">
<li>……</li>
</ol></div>
```

1. **`.crossx` 越界 · 跨学科的联想**（2026-10-17 起，**必写，3–5 条**）：写本期某个生理机制、证据或协议，和另一个领域里的什么东西是同一个结构——像在哪、不像在哪。每条约 60–90 字（一两句），只有展开的那 1 条可到约 120 字。往 BigCat 的深度兴趣挂：意识、佛学、量子力学、神经科学、东西方哲学、心理学、生物进化、复杂性科学、分布式系统；领导力、投资、商业可作旁支。**别挂回医学、营养、运动、健康本身**（神经科学、进化挂的是机制的同构，不是再讲一遍本期的健康建议）。具体研究、人物、年份必须真实准确，拿不准就换一条。本期正文里引用的研究、指南属于正文；我们自己联想到的才进越界，绝不写进卡片主线。英文页标题 `Crossovers`。
2. **`.takeaways` 十条要点**（2026-10-17 起，**必写，正好 10 条**）：读者拿它直接记笔记。每条一句完整的话，约 20–45 字，**单独抄出去也能看懂**（写出药名、指标名、机制名，不用「它」「这种药」这类靠上下文的代词）。人名不必每条都当主语开头：研究者/试验名出现两三次就够，其余各条直接写观点、机制或事实（写「鼻用激素治鼻塞优于口服抗组胺药」，不写「Weiner 1998 发现鼻用激素……」）。覆盖各张卡片最值得记的结论、机制和协议，可含越界一两条；顺序跟正文走。**只总结正文已经写过的东西**，不加新信息、不放引语；别把每张卡的「一句话结论」原样抄进来凑数。英文页标题 `Ten takeaways`。

CSS（2026-10-17 起）：本仓没有 TEMPLATE，新页的 `<style>` 照上一期写；若上一期还没有下面这段，就把它追加到新页 `<style>` 末尾（之后各期自然沿用）。照常写浅色，深色由 darken.py 转。越界用紫色跟正文青绿、女性提示的品红区分，十条要点用本仓主强调青 `#0d6e6e`：

```css
.card.crossx{border-top-color:#7a5fb8}
.card.crossx .name,.card.tk .name{margin-bottom:12px}
.card.crossx .name{color:#5b4396}
.crossx ul{list-style:none;margin:0;padding:0}
.crossx li{position:relative;padding:9px 0 9px 20px;border-top:1px dashed #e2dcf0;font-size:0.92rem;line-height:1.75;color:#2c4646}
.crossx li:first-child{border-top:none}
.crossx li::before{content:"↗";position:absolute;left:0;top:9px;color:#7a5fb8;font-size:0.85rem}
.crossx li b{color:#5b4396}
.card.tk{border-top-color:#0d6e6e}
.takeaways{counter-reset:tk;list-style:none;margin:0;padding:0;border:1px solid rgba(13,110,110,0.22);border-radius:10px;background:rgba(13,110,110,0.04)}
.takeaways li{counter-increment:tk;position:relative;margin:0;padding:10px 16px 10px 48px;border-top:1px solid rgba(13,110,110,0.12);font-size:0.92rem;line-height:1.75;color:#2c4646}
.takeaways li:first-child{border-top:none}
.takeaways li::before{content:counter(tk,decimal-leading-zero);position:absolute;left:16px;top:11px;font-family:"SF Mono",Menlo,monospace;font-size:0.78rem;font-weight:700;color:#0d6e6e}
```

篇幅：越界约 400 字、十条要点约 350 字。`.maxchars` 为这两节从 3000 上调到 3750（2026-10-17）；主题卡片的篇幅不变（仍按原来 3000 字以内的目标写），别借新上限把正文写长。`publish.sh` 会拦截：新的中英页 `<ol class="takeaways">` 不是正好 10 个 `<li>`、或 `.crossx` 不是 3–5 个 `<li>`，都不让发布（只查从未提交过的新文件，旧页不受影响）。

## 配色：深色由 CI 兜底，不用你操心

本仓全站深色。**生成页面时照常写你觉得合适的浅色配色**——push 之后 `darken.py`
会把内联 `<style>`、`style=""` 和内联 SVG 的 `fill=`/`stroke=` 一起按亮度推到深色，
幂等，已经转过的页面会跳过。

不要为了"配合深色"自己手写死一套黑：那会让每篇文章长得一模一样。
darken.py 保留色相和彩度，原设计的性格还在，只换明暗。

**别在 `<style>` 顶部手写 `/* bigcat-dark v1 */`。** 那是"已转换"的标记，
写上去这一页就永远跳过转换、永远停在浅色。

`darken.py` 顶部的 `TINT_HUE` 是**本站的身份色相**（本仓 178°，青绿，取自
`#0d6e6e` / `#3aa17e` 这套强调色）。原始底色是近白的浅青，几乎不带彩度，
照原色相推暗会和其他深色站收敛成同一种黑；所以「接近中性」的面改用这个
色相重新上色，真正有颜色的元素保持自己的颜色。改它等于改本站的整体调性。
