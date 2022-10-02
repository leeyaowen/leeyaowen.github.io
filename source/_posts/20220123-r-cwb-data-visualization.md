---
title: '[R]以R語言繪製氣象站的各月雨量資料(2022年1月版)'
date: 2022-01-23 15:00:03
categories: R
tags: [R, coding, code, cwb, data visualization, ggplot2]
---
首先要到氣象局的[觀測資料查詢](https://e-service.cwb.gov.tw/HistoryDataQuery/index.jsp)，這邊以屏東縣檳榔測站為例
![](https://photos.smugmug.com/photos/i-WvxbsNb/0/772157cc/XL/i-WvxbsNb-XL.png)
<!--more-->
選擇月報表後可以看到以下資料，點擊下載csv檔可以獲得該月資料
![](https://photos.smugmug.com/photos/i-wZJPz2X/0/66c8270f/XL/i-wZJPz2X-XL.png)
這邊以收集2021年資料為例，將其整理至一資料夾，如下圖
![](https://photos.smugmug.com/photos/i-LzkqMhQ/0/0f3f4c60/O/i-LzkqMhQ.png)

之後以R繪製雨量資料
{% codeblock R lang:r %}
####load packages####
library(dplyr)
library(magrittr)
library(ggplot2)
library(stringr)

####bind data####
filenames<-list.files("./Binlang",pattern = ".csv")
precpdf<-read.csv(paste0("./Binlang/",filenames[1]),skip = 1,colClasses = "character") %>%
  mutate(month=str_sub(filenames[1],-6,-5))
for (i in 2:length(filenames)) {
  dttemp<-read.csv(paste0("./Binlang/",filenames[i]),skip = 1,colClasses = "character") %>%
    mutate(month=str_sub(filenames[i],-6,-5))
  precpdf<-bind_rows(precpdf,dttemp)
}
precpdf$Precp<-as.numeric(precpdf$Precp)
precpdf<-precpdf %>%
  group_by(month) %>%
  summarise(precp=sum(Precp,na.rm = T)) %>%
  ungroup()

####plot####
p<-ggplot(data = precpdf,aes(x=month,y=precp))+
  geom_bar(stat = "identity")+
  labs(title = "Binlang")+
  geom_text(aes(y=precp,label=precp),vjust=-0.5)+
  theme_classic()+
  theme(plot.title = element_text(hjust = 0.5))+
  scale_y_continuous(limits = c(0,850))
p
ggsave(filename = "Binlang_2021_precp.png",width = 15,height = 10,units = "cm")
{% endcodeblock %}

![](https://photos.smugmug.com/photos/i-6KH9vX8/0/fe20c42a/XL/i-6KH9vX8-XL.png)