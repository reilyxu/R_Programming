##sapply和spilt实践

##目标是计算同一年出生的人打分的平均值

 ```R
 my_data <- read.csv(file.choose())
 ##方法一
 #按出生年份划分
 s <- split(my_data, my_data$birthyear2019)
 #对不同出生年份的人分别算平均值
 mydata_av <- sapply(s, function(x) colMeans(x[, c("X2019eng_av","X2019sat_av","X2019org_av")]))
 ##方法二
 mydata_ob <- mydata[, c('birthyear2019", "X2019eng_av","X2019sat_av","X2019org_av")]
 mydata_av <- data.frame()
 for (i in 2:4) {
 mydata_t <- tapply(mydata_ob[,i], my_data$birthyear2019, mean)
 mydata_av <- cbind(mydata_av, mydata_t)
 }
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

###不同职族中年轻人和非年轻人的对比

```R
##新建年轻人列
#方式一
young <- rep(0,length(jm2016$年龄))
young[which(as.numeric(jm2016$年龄) <= 30)] <- "young"
young[which(as.numeric(jm2016$年龄) > 30)] <- "notyoung"
jm2016$young<-young
#方式二
young <- sapply(jm2016$年龄, function(x) as.integer(x<=30))
young[which(young == "1")] <- "young"
young[which(young == "0")] <- "old"

young_dev2016 <- interaction(jm2016$young, jm2016$职位族)

s2019 <- split(jm2019,young_dev2019)
jm2019_av <- sapply(s2019, function(x) 
    colMeans(x[, c("X2018eng_av","X2018sat_av","X2018org_av")]))
renshu2019 <- sapply(s2019, function(x) length(x[, "X2018eng_av"]))
dsc2019 <- t(rbind(jm2019_av, renshu2019))
write.xlsx(dsc2019,"2019.xlsx", row.names = T)
```
