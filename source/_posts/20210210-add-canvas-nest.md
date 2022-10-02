---
title: 在NexT中增加canvas nest特效
date: 2021-02-10 21:22:15
categories: hexo
tags: [hexo, next, code]
---
剛開始使用Hexo
想增加幾何線條的特效，滑鼠移動過去時會吸引線條
方法是在`themes/next/layout/_layout.swig`中的`<body>`區塊增加以下code
{% codeblock HTML lang:html %}
<script type="text/javascript" color="0,0,0" opacity='0.5' zIndex="-1" count="99" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
{% endcodeblock %}