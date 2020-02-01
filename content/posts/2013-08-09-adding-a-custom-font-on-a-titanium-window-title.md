---
title: Adding a custom font on a titanium window title
author: Carlos Buenosvinos
type: post
date: 2013-08-09T18:55:48+00:00
url: /adding-a-custom-font-on-a-titanium-window-title/
switch_like_status:
  - "1"
categories:
  - Mobile
tags:
  - appcelerator
  - custom font
  - titanium
  - window title

---
If you are working with Titanium and you need to customize the font you are using as a title in a window, there is no predefined solution to do this, but there is a trick, the &#8220;titleControl&#8221; window property with a label control.

<!--more-->

<pre class="brush: javascript; gutter: true">var windowTitle = Ti.UI.createLabel({
		color: &#039;#fff&#039;,
		font: {
			fontFamily: &#039;Proxima Nova&#039;,
			fontSize: 16,
			fontWeight: &#039;bold&#039;
		},
		text: &#039;My fancy App&#039;
	});

	var win = Ti.UI.createWindow({
		titleControl: windowTitle,
		backgroundColor: &#039;#fff&#039;
	});</pre>

Hope it helps!