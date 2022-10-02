---
title: '[R]去除字串的多餘空白-從gdata到stringr'
date: 2022-02-11 21:35:12
categories: R
tags: [R, coding, stringr, 資料處理, 去除空白]
---
以前處理資料時，因為都是一些人工輸入的資料，難免有瑕疵，**多餘的空白**是會造成資料處理上的麻煩之一

如`'abc'`與`'abc '`是完全不同的字串，若用distinct檢查，那結果會是2個，我們可以用生物的學名來做想像，如果多了一個空格，資料等於**多了一個物種**
先前看到gdata有`trim`這個function就直接拿來對整個dataframe使用，但其是不認方向的，前後都會去除
使用stringr的`str_trim`則不同，有both, left, right三個選擇

gdata是一個有蠻多處理資料function的package，但其最後更新為2017年
stringr主要就是專門做字串處理，且為tidyverse之下的一員，針對字串處理的function豐富，也比較不用擔心沒人維護

考量我也沒有在用gdata的其他功能，以及stringr有更多方便處理字串的function，就跳槽啦!