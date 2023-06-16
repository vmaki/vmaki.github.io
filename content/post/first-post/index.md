---
title: Hugo Stack 主题折腾笔记
date: 2023-04-21
slug: stack
image: stack.webp
categories:
    - 折腾
tags:
    - Hugo
    - Stack
---

## 有话要说

> 博客系统基于 [Hugo-Extended](https://gohugo.io/)，版本：<code>0.113.0</code><br>
> 博客主题来自 [Stack](https://github.com/CaiJimmy/hugo-theme-stack)，版本：<code>3.16.0</code>

最早在<code>V2EX</code>了解到这个主题，表示是我喜欢的风格，但无奈中间因为没有时间去折腾博客，也就一直就拖到了现在。
这篇文章仅是为了记录一下博客装修内容，在自己的审美之上，进行的一些修改。

## 折腾须知

最好根据作者为我们预留的文件，来添加自定义样式以及 Header / Footer，以避免更新主题时可能会出现的错误。

-   <code>themes/stack/assets/scss/custom.scss</code>
-   <code>themes/stack/layouts/partials/head/custom.html</code>
-   <code>themes/stack/layouts/partials/footer/custom.html</code>

如果你使用的是<code>Hugo module</code>的安装方式，你需要将你修改的文件复制到根目录下的相对应的文件夹。

官方说明：

> Using this method, there won't be any file under <code>themes</code> directory. In order to modify the theme, you will have to copy the file you want to modify to the same directory under layouts directory.<br>For example, in order to modify the <code>themes/hugo-theme-stack/layouts/partials/header.html</code> file, you will have to copy it to <code>layouts/partials/header.html</code> and modify it there (copy the code from theme's repository). The same applies to <code>assets</code> and <code>static</code> directories.

## 调整内容

-   增加页面顶部加载条
-   增加返回顶部按钮
-   删除分类页面的文章图片和遮罩
-   修改代码高亮以及增加<code>MacOS</code>圆点样式
-   修改滚动条的样式
-   修改友情链接为三栏
-   修改代码块、下拉菜单等圆角
-   修改页面间距和文章内容的间距
-   修改键盘标签样式；<kbd>CTRL</kbd> + <kbd>C</kbd>

### 代码高亮

因为主题默认没有开启代码高亮，可以在[Chroma Playground](https://swapoff.org/chroma/playground/)挑选高亮主题，修改根目录下<code>config.yaml</code>文件中的<code>highlight</code>部分来开启代码高亮。

```yaml{hl_lines=[2]}
highlight:
    style: onedark
	noClasses: true
    codeFences: true
    guessSyntax: true
    lineNoStart: 1
    lineNos: true
    lineNumbersInTable: false
    tabWidth: 4
```

以下为对代码高亮的其他调整：

```scss
/* themes/stack/assets/scss/custom.scss 代码高亮的行数设置为不可选 */
.chroma .lnt,
.chroma .line .ln {
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}

/* ----------------------------------------------- */

/* themes/stack/assets/scss/custom.scss 代码高亮 MacOS 圆点样式 */
.highlight:before {
	content: "";
	display: block;
	background: url(/code-header.svg);
	height: 30px;
	width: 100%;
	background-size: 45px;
	background-repeat: no-repeat;
	margin-bottom: -7px;
	border-radius: 5px;
	background-position: 10px 10px;
	position: absolute;
	top: 0;
	left: 0;
}

/* ----------------------------------------------- */

/* themes/stack/assets/scss/custom.scss highlight 间距调整 */
.article-content .highlight {
	padding-left: 15px;
	padding-bottom: 15px;
}

/* themes/stack/assets/scss/variables.scss:160 代码块背景色调整*/
[data-scheme="light"] {
	--pre-text-color: #f8f8f2;
	--pre-background-color: #282c34;
	@import "partials/highlight/light.scss";
}

[data-scheme="dark"] {
	--pre-text-color: #f8f8f2;
	--pre-background-color: #282c34;
	@import "partials/highlight/dark.scss";
}
```

### 增加样式

修改路径：<code>themes\stack\layouts\partials\footer\custom.html</code>

```html
<!-- ----------------------------------------------- -->

<!-- 加载条 -->
<script
	src="https://cdn.jsdelivr.net/gh/zhixuan2333/gh-blog@v0.1.0/js/nprogress.min.js"
	integrity="sha384-bHDlAEUFxsRI7JfULv3DTpL2IXbbgn4JHQJibgo5iiXSK6Iu8muwqHANhun74Cqg"
	crossorigin="anonymous"></script>
<link
	rel="stylesheet"
	href="https://cdn.jsdelivr.net/gh/zhixuan2333/gh-blog@v0.1.0/css/nprogress.css"
	integrity="sha384-KJyhr2syt5+4M9Pz5dipCvTrtvOmLk/olWVdfhAp858UCa64Ia5GFpTN7+G4BWpE"
	crossorigin="anonymous" />
<script>
	NProgress.start();
	document.addEventListener("readystatechange", () => {
		if (document.readyState === "interactive") NProgress.inc(0.8);
		if (document.readyState === "complete") NProgress.done();
	});
</script>

<!-- ----------------------------------------------- -->

<!--返回顶部按钮 -->
<a href="#" id="back-to-top" title="返回顶部"></a>

<!--返回顶部CSS -->
<style>
	#back-to-top {
		display: none;
		position: fixed;
		bottom: 20px;
		right: 55px;
		width: 55px;
		height: 55px;
		border-radius: 7px;
		background-color: var(--back-to-top-background);
		box-shadow: var(--shadow-l2);
		font-size: 30px;
		text-align: center;
		line-height: 50px;
		cursor: pointer;
	}

	#back-to-top:before {
		content: " ";
		display: inline-block;
		position: relative;
		top: 0;
		transform: rotate(135deg);
		height: 10px;
		width: 10px;
		border-width: 0 0 2px 2px;
		border-color: var(--back-to-top-color);
		border-style: solid;
	}

	#back-to-top:hover:before {
		border-color: #2674e0;
	}

	/* 在屏幕宽度小于 768 像素时，钮位置调整 */
	@media screen and (max-width: 768px) {
		#back-to-top {
			bottom: 20px;
			right: 20px;
			width: 40px;
			height: 40px;
			font-size: 10px;
		}
	}

	/* 在屏幕宽度大于等于 1024 像素时，按钮位置调整 */
	@media screen and (min-width: 1024px) {
		#back-to-top {
			bottom: 20px;
			right: 40px;
		}
	}

	/* 在屏幕宽度大于等于 1280 像素时，按钮位置调整 */
	@media screen and (min-width: 1280px) {
		#back-to-top {
			bottom: 20px;
			right: 55px;
		}
	}

	/* 目录显示时，隐藏按钮 */
	@media screen and (min-width: 1536px) {
		#back-to-top {
			visibility: hidden;
		}
	}
</style>

<!--返回顶部JS -->
<script>
	function backToTop() {
		document.documentElement.scrollIntoView({
			behavior: "smooth",
		});
	}

	window.onload = function () {
		let scrollTop = this.document.documentElement.scrollTop || this.document.body.scrollTop;
		let totopBtn = this.document.getElementById("back-to-top");
		if (scrollTop > 0) {
			totopBtn.style.display = "inline";
		} else {
			totopBtn.style.display = "none";
		}
	};

	window.onscroll = function () {
		let scrollTop = this.document.documentElement.scrollTop || this.document.body.scrollTop;
		let totopBtn = this.document.getElementById("back-to-top");
		if (scrollTop < 200) {
			totopBtn.style.display = "none";
		} else {
			totopBtn.style.display = "inline";
			totopBtn.addEventListener("click", backToTop, false);
		}
	};
</script>
```

### 全部调整

修改路径：<code>themes\stack\assets\scss\custom.scss</code>

```scss
/* Place your custom SCSS in HUGO_SITE_FOLDER/assets/scss/custom.scss */

/* ----------------------------------------------- */

/* 定义滚动条的宽度、高度和颜色 */
::-webkit-scrollbar {
	width: 6px;
	height: 6px;
}

/* 定义滚动条滑块的样式 */
::-webkit-scrollbar-thumb {
	background-color: #aaaaaa;
	border-radius: 5px;
}

/* ----------------------------------------------- */

/* 引用，代码块等不贴合文章边缘 */
.article-page {
	blockquote,
	figure,
	.highlight,
	pre,
	.gallery,
	.video-wrapper,
	.table-wrapper,
	.s_video_simple {
		margin-left: 0;
		margin-right: 0;
		width: 100%;
	}
}

/* ----------------------------------------------- */

/* 代码块，图片圆角样式 */
.highlight,
pre,
.gallery img {
	border-radius: 5px;
}

/* ----------------------------------------------- */

/* 头像圆角样式 */
.site-avatar {
	.site-logo {
		border-radius: 7%;
	}
}

/* ----------------------------------------------- */

/* 下拉菜单圆角样式 */
.menu {
	padding-left: 0;
	list-style: none;
	flex-direction: column;
	overflow-y: auto;
	flex-grow: 1;
	font-size: 1.59rem;
	background-color: var(--card-background);
	box-shadow: var(--shadow-l2);
	display: none;
	margin: 0;
	border-radius: 10px;
	padding: 30px 30px;
	@include respond(xl) {
		padding: 15px 0;
	}

	&,
	.menu-bottom-section {
		gap: 30px;
		@include respond(xl) {
			gap: 25px;
		}
	}

	&.show {
		display: flex;
	}

	@include respond(md) {
		align-items: flex-end;
		display: flex;
		background-color: transparent;
		padding: 0;
		box-shadow: none;
		margin: 0;
	}

	li {
		position: relative;
		vertical-align: middle;
		padding: 0;

		@include respond(md) {
			width: 100%;
		}

		svg {
			stroke-width: 1.33;

			width: 20px;
			height: 20px;
		}

		a {
			height: 100%;
			display: inline-flex;
			align-items: center;
			color: var(--body-text-color);
			gap: var(--menu-icon-separation);
		}

		span {
			flex: 1;
		}

		&.current {
			a {
				color: var(--accent-color);
				font-weight: bold;
			}
		}
	}
}

/* ----------------------------------------------- */

/* 页面间距调整 */
.container {
	margin-left: auto;
	margin-right: auto;

	.left-sidebar {
		order: -3;
		max-width: var(--left-sidebar-max-width);
	}

	.right-sidebar {
		order: -1;
		max-width: var(--right-sidebar-max-width);

		/// Display right sidebar when min-width: lg
		@include respond(xl) {
			display: flex;
		}
	}

	&.extended {
		@include respond(md) {
			max-width: 768px;
			--left-sidebar-max-width: 25%;
			--right-sidebar-max-width: 30%;
		}

		@include respond(lg) {
			max-width: 1024px;
			--left-sidebar-max-width: 20%;
			--right-sidebar-max-width: 30%;
		}

		@include respond(xl) {
			max-width: 1280px;
			--left-sidebar-max-width: 15%;
			--right-sidebar-max-width: 25%;
		}
	}

	&.compact {
		@include respond(md) {
			--left-sidebar-max-width: 25%;
			max-width: 768px;
		}

		@include respond(lg) {
			max-width: 1024px;
			--left-sidebar-max-width: 20%;
		}

		@include respond(xl) {
			max-width: 1280px;
		}
	}
}

/* ----------------------------------------------- */

/* 友情链接修改为三栏 */
@media (min-width: 1024px) {
	.article-list--compact.links {
		display: grid;
		grid-template-columns: 1fr 1fr 1fr;
		background: none;
		box-shadow: none;
		gap: 1rem;

		article {
			background: var(--card-background);
			border: none;
			box-shadow: var(--shadow-l2);
			margin-bottom: 8px;
			border-radius: var(--card-border-radius);
			&:nth-child(odd) {
				margin-right: 8px;
			}
		}
	}
}

/* ----------------------------------------------- */

/* 修复引用块内容 */
a {
	word-break: break-all;
}

code {
	word-break: break-all;
}

/* ----------------------------------------------- */

/* 文章列表封面图尺寸修改 */
.article-list .article-image {
	img {
		width: 100%;
		height: 100px !important;
		object-fit: cover;

		@include respond(md) {
			height: 200px !important;
		}

		@include respond(xl) {
			height: 200px !important;
		}
	}
}
/* ----------------------------------------------- */

/* 分类页面隐藏文章图片 */
.article-list--compact > article .article-image img,
.section-card .section-image img {
	display: none;
}

/* themes\stack\layouts\partials\article\components\links.html:19 add "id="link-img" */
#link-img img {
	display: block !important;
}

/* ----------------------------------------------- */

/* 取消分类图片的渐变色 */
.subsection-list .article-list--tile .has-image .article-details {
	background: linear-gradient(0deg, rgba(0, 0, 0, 0) 0%, rgba(0, 0, 0, 0) 0%) !important;
}

/* ----------------------------------------------- */

/* 代码高亮的行数设置为不可选 */
.chroma .lnt {
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}

/* ----------------------------------------------- */

/* highlight 间距调整 */
.article-content .highlight {
	padding-left: 15px;
	padding-bottom: 15px;
}

/* ----------------------------------------------- */

/* 代码高亮 MacOS 圆点样式 */
.highlight:before {
	content: "";
	display: block;
	background: url(/code-header.svg);
	height: 30px;
	width: 100%;
	background-size: 45px;
	background-repeat: no-repeat;
	margin-bottom: -7px;
	border-radius: 5px;
	background-position: 10px 10px;
	position: absolute;
	top: 0;
	left: 0;
}

/* ----------------------------------------------- */

/* 键盘标签样式 */
kbd {
	margin: 0 0.1em;
	padding: 0.1em 0.6em;
	font-size: 0.8em;
	color: #242729;
	background: #fff;
	border: 1px solid #adb3b9;
	border-radius: 3px;
	box-shadow: 0px 1px 0 rgba(12, 13, 14, 0.2), 0 0 0 2px #fff inset;
	white-space: nowrap;
	vertical-align: middle;
	font-family: monospace;
}

/* ----------------------------------------------- */

/* 背景图片 themes\stack\layouts\_default\baseof.html:7 add "bg" */
.bg {
	background-image: url(/bg.svg);
	background-position: 50%;
	background-size: cover;
	background-attachment: fixed;
}

/* ----------------------------------------------- */

/* 返回顶部按钮对暗色模式下适应 */
:root {
	--back-to-top-background: #ffffff;
	--back-to-top-color: #000000;
	&[data-scheme="dark"] {
		--back-to-top-background: #252525;
		--back-to-top-color: #ffffff;
	}
}
```
