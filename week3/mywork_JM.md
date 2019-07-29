##sapply和spilt实践

##目标是计算同一年出生的人打分的平均值

 ```R
 my_data <- read.csv(file.choose())
 #按出生年份划分
 s <- split(my_data, my_data$birthyear2019)
 #对不同出生年份的人分别算平均值
 mydata_av <- sapply(s, function(x) colMeans(x[, c("X2019eng_av","X2019sat_av","X2019org_av")]))
 #行列转置
 mydata_av <- t(mydata_av)
 #算人数
 people <- sapply(s, function(x) length(x[, c("X2019eng_av")]))
 #行列转置
 people<-t(people)
 #两个matrix合并（还没学会merge，用的cbind）
 data2019 <- cbind(mydata_av, people)
 #第四列没有名字，改个名字（改matrix单列的名字，知识点）
 colnames(mydata2019)[4] <- "people"
 #输出！大功告成
 write.csv(mydata2019, file = "2019.csv", quote = F)
 ```

##下了open.xlsx的包

##目标是按年代来划分 如70-79年都算70后

```R
library(openxlsx)
gm2016 <- read.xlsx("文件名", startRow = 3)
gm2016$出生日期 <- as.Date(gm2016$出生日期)
gm2016$年代 <- sapply(gm2016$出生日期,function(x) paste0(substr(format(x, format = "%Y"),1,3),"0"))
```
