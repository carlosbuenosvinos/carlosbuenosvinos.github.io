---
title: Change multiple files extension with git
author: Carlos Buenosvinos
type: post
date: 2014-05-28T14:23:36+00:00
url: /change-multiple-files-extension-with-git/
dsq_thread_id:
  - "2788339139"
categories:
  - PHP

---
<pre class="brush: bash; gutter: true">for i in $(find . -iname "*.inc"); do
    git mv "$i" "$(echo $i | rev | cut -d &#039;.&#039; -f 2- | rev).php";
done</pre>