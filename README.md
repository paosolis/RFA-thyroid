---
title: "RFA analysis"
author: "Paola_Luis"
date: "2023-01-01"
output: html_document
---

<i> Analysis and visualization developed by [Luis A. Figueroa](https://twitter.com/LuisFig1706) [Paola SolíS Pazmiño](paosolpaz18@gmail.com)


```{r libraries, include= FALSE}
library(metafor)
library(openxlsx)
library(dplyr)
library(ggplot2)
library(tidyr)
library(reshape)
knitr::opts_chunk$set(fig.path = "RFA Figures/", dev='svg') # dev = 'png'
```


``` {r clean, echo=FALSE, include= FALSE}
data1 <- read.xlsx("Base Bocio.xlsx")
databocio <- data1

names(databocio)[1]<- "id"
names(databocio)[2]<- "names"
names(databocio)[3]<- "age"
names(databocio)[4]<- "sex"
databocio$id[5] = "400821963(1)" # dobles resultados porque son dobles ablasiones en diferentes sitios
databocio$id[21] = "1303474009(1)"

datavol<- databocio[ ,1:9]
names(datavol)[5]<- "volbasal"
names(datavol)[6]<- "vol1m"
names(datavol)[7]<- "vol3m"
names(datavol)[8]<- "vol6m"
names(datavol)[9]<- "vol12m"
str(datavol)
datavol$vol1m <- as.numeric(datavol$vol1m)
datavol$vol3m <- as.numeric(datavol$vol3m)
datavol$vol6m <- as.numeric(datavol$vol6m)
datavol$sex <- as.factor(datavol$sex)
str(datavol)
summary(datavol)
sd(datavol$volbasal)
sd(datavol$vol1m)


datatsh<- databocio[ ,c(1:4, 11:22)]
names(datatsh)[5]<- "tshbasal"
names(datatsh)[6]<- "tsh1m"
names(datatsh)[7]<- "tsh3m"
names(datatsh)[8]<- "tsh6m"
names(datatsh)[9]<- "tsh12m"
names(datatsh)[12]<- "t4fbasal"
names(datatsh)[13]<- "t4f1m"
names(datatsh)[14]<- "t4f3m"
names(datatsh)[15]<- "t4f6m"
names(datatsh)[16]<- "t4f12m"
datatsh<- datatsh[ , - c(10:11)]
str(datatsh)
datatsh$tshbasal<-as.numeric(datatsh$tshbasal)
datatsh$tsh1m<-as.numeric(datatsh$tsh1m)
datatsh$tsh3m<-as.numeric(datatsh$tsh3m)
datatsh$t4fbasal<-as.numeric(datatsh$t4fbasal)
datatsh$t4f1m<-as.numeric(datatsh$t4f1m)
datatsh$t4f3m<-as.numeric(datatsh$t4f3m)
datatsh$t4f6m<-as.numeric(datatsh$t4f6m)
datatsh$t4f12m<-as.numeric(datatsh$t4f12m)
datatsh$sex<- as.factor(datatsh$sex)
str(datatsh)
summary(datatsh)
sd(datatsh$tshbasal)



datacosmetic<- databocio[ ,c(1:4, 23:28)]
names(datacosmetic)[5]<- "cosmbasal"
names(datacosmetic)[6]<- "cosm1m"
names(datacosmetic)[7]<- "cosm3m"
names(datacosmetic)[8]<- "cosm6m"
names(datacosmetic)[9]<- "cosm12m"
datacosmetic<- datacosmetic[,-c(10)]
datacosmetic$sex<- as.factor(datacosmetic$sex)
str(datacosmetic)
summary(datacosmetic)



datasymt<-databocio[, c(1:4,29:33)]
names(datasymt)[5]<- "symptbasal"
names(datasymt)[6]<- "sympt1m"
names(datasymt)[7]<- "sympt3m"
names(datasymt)[8]<- "sympt6m"
names(datasymt)[9]<- "sympt12m"
str(datasymt)


datareduc<-databocio[,c(1:4,35:38)]
names(datareduc)[5]<- "red1m"
names(datareduc)[6]<- "red3m"
names(datareduc)[7]<- "red6m"
names(datareduc)[8]<- "red12m"
str(datareduc)
datareduc$red6m<-as.numeric(datareduc$red6m)
datareduc$red12m<-as.numeric(datareduc$red12m)
str(datareduc)
datareduc$sex<- as.factor(datareduc$sex)
summary(datareduc)


datadiametro<-databocio[, c(1:4,39:43)]
str(datadiametro)
summary(datadiametro)
datadiametro$diametro3m<-as.numeric(datadiametro$diametro3m)
str(datadiametro)
summary(datadiametro)
sd(datadiametro$diametrobasal)
sd(datadiametro$diametro1m)
sd(datadiametro$diametro3m)
sd(datadiametro$diametro6m)
sd(datadiametro$diametro12m)



sd(datadiametro$diametro1m)
str(datadiametro)
datadiametro$diametro1m<- as.numeric(datadiametro$diametro1m)
datadiametro$diametro3m<- as.numeric(datadiametro$diametro3m)
sd(datadiametro$diametro1m)
mean(datadiametro$diametro1m)
sd(datadiametro$diametro3m)
sd(datadiametro$diametro6m)
```


```{r analysis,echo=FALSE, include=FALSE}
shapiro.test(datavol$vol1m - datavol$volbasal)
shapiro.test(datavol$vol3m - datavol$volbasal)
shapiro.test(datavol$vol6m - datavol$volbasal)
shapiro.test(datavol$vol12m - datavol$volbasal)

t.test( datavol$vol1m,
        datavol$volbasal,
        alternative = "two.sided",
        paired = TRUE )

t.test( datavol$vol3m,
        datavol$volbasal,
        alternative = "two.sided",
        paired = TRUE )

t.test( datavol$vol6m,
        datavol$volbasal,
        alternative = "two.sided",
        paired = TRUE )

t.test( datavol$vol12m,
        datavol$volbasal,
        alternative = "two.sided",
        paired = TRUE )


t.test( datadiametro$diametro1m,
        datadiametro$diametrobasal,
        alternative = "two.sided",
        paired = TRUE )

t.test( datadiametro$diametro3m,
        datadiametro$diametrobasal,
        alternative = "two.sided",
        paired = TRUE )

t.test( datadiametro$diametro6m,
        datadiametro$diametrobasal,
        alternative = "two.sided",
        paired = TRUE )

t.test( datadiametro$diametro12m,
        datadiametro$diametrobasal,
        alternative = "two.sided",
        paired = TRUE )


t.test( datareduc$red3m,
        datareduc$red1m,
        alternative = "two.sided",
        paired = TRUE )

t.test( datareduc$red6m,
        datareduc$red1m,
        alternative = "two.sided",
        paired = TRUE )

t.test( datareduc$red12m,
        datareduc$red1m,
        alternative = "two.sided",
        paired = TRUE )


t.test( datatsh$tsh1m,
        datatsh$tshbasal,
        alternative = "two.sided",
        paired = TRUE )

t.test( datatsh$tsh3m,
        datatsh$tshbasal,
        alternative = "two.sided",
        paired = TRUE )

t.test( datatsh$tsh6m,
        datatsh$tshbasal,
        alternative = "two.sided",
        paired = TRUE )

t.test( datatsh$tsh12m,
        datatsh$tshbasal,
        alternative = "two.sided",
        paired = TRUE )


t.test( datacosmetic$cosm1m,
        datacosmetic$cosmbasal,
        alternative = "two.sided",
        paired = TRUE )

t.test( datacosmetic$cosm3m,
        datacosmetic$cosmbasal,
        alternative = "two.sided",
        paired = TRUE )
t.test( datacosmetic$cosm6m,
        datacosmetic$cosmbasal,
        alternative = "two.sided",
        paired = TRUE )
t.test( datacosmetic$cosm12m,
        datacosmetic$cosmbasal,
        alternative = "two.sided",
        paired = TRUE )


t.test( datasymt$sympt1m,
        datasymt$symptbasal,
        alternative = "two.sided",
        paired = TRUE )
t.test( datasymt$sympt3m,
        datasymt$symptbasal,
        alternative = "two.sided",
        paired = TRUE )
t.test( datasymt$sympt6m,
        datasymt$symptbasal,
        alternative = "two.sided",
        paired = TRUE )
t.test( datasymt$sympt12m,
        datasymt$symptbasal,
        alternative = "two.sided",
        paired = TRUE )
```

<br>
<h2>Radiofrequency Ablation for Thyroid Nodules in Ecuador: Cross-sectional study</h2>


<details>

<summary><b>Figure A -</b> Tumor size (volumen) after Radiofrequency Ablation </summary>
<br>

```{r graphs, echo=FALSE, fig.height = 9.8, fig.width = 13.5, warning=F}
gvol <- datavol[,-c(2:4)] #Remove non-useful cols

#Subset of datos que se reportaron hasta el valor basa
gvolb <- subset(gvol, volbasal >= 0 & is.na(vol1m) & is.na(vol3m) & is.na(vol6m) & is.na(vol12m))

#Subset of datos que se reportaron hasta el primer  mes
gvol1 <- subset(gvol, volbasal >= 0 & vol1m >= 0 & is.na(vol3m) & is.na(vol6m) & is.na(vol12m))

#Subset of datos que se reportaron hasta el tercer  mes
gvol3 <- subset(gvol, volbasal >= 0 & vol1m >= 0  & vol3m >= 0 & is.na(vol6m) & is.na(vol12m))
new_row = list(id = NA, volbasal=NA, vol1m = NA, vol3m = NA, vol6m = NA, vol12m = NA)
gvol3 <- rbind(gvol3,new_row)

#Subset of datos que se reportaron hasta el sexto  mes
gvol6 <- subset(gvol, volbasal >= 0 & vol1m >= 0 & vol3m >= 0  & vol6m >= 0 & is.na(vol12m)) #
gvol6a <- subset(gvol, volbasal >= 0 & is.na(vol1m) & is.na(vol3m) & vol6m >= 0 & is.na(vol12m))
gvol6 <- merge(gvol6, gvol6a, by = c("id", "volbasal", "vol1m", "vol3m", "vol6m", "vol12m"), all = T)
gvol6b <- subset(gvol, volbasal >= 0 & vol1m >= 0 & is.na(vol3m) & vol6m >= 0 & is.na(vol12m))
gvol6 <- merge(gvol6, gvol6b, by = c("id", "volbasal", "vol1m", "vol3m", "vol6m", "vol12m"), all = T)

#Subset of datos que se reportaron hasta el 12vo mes
gvol12 <- subset(gvol, volbasal >= 0 & vol1m >= 0 & vol3m >= 0  & vol6m >= 0 & vol12m >= 0)
gvol12b <- subset(gvol, volbasal >= 0 & is.na(vol1m) & is.na(vol3m) & is.na(vol6m) & vol12m >= 0)
gvol12 <- merge(gvol12, gvol12b, by = c("id", "volbasal", "vol1m", "vol3m", "vol6m", "vol12m"), all = T)
gvol12c <- subset(gvol, volbasal >= 0 & vol1m >= 0 & is.na(vol3m) & vol6m >= 0 & vol12m >= 0)
gvol12 <- merge(gvol12, gvol12c, by = c("id", "volbasal", "vol1m", "vol3m", "vol6m", "vol12m"), all = T)
gvol12d <- subset(gvol, volbasal >= 0 & is.na(vol1m) & is.na(vol3m) & vol6m >= 0 & vol12m >= 0)
gvol12 <- merge(gvol12, gvol12d, by = c("id", "volbasal", "vol1m", "vol3m", "vol6m", "vol12m"), all = T)

#Convertimos cols a rows y rows a cols 
gvolb <- gvolb%>%pivot_longer(-id, names_to = "mes", values_to = "valor")
gvolb <- gvolb%>%pivot_wider(names_from = id, values_from = valor)
gvolb <- gvolb[,-1] #rows: 1 = volbasal, 2 = vol1m, 3 = vol3m, 4 = vol6m, 5 = vol12m

gvol1 <- gvol1%>%pivot_longer(-id, names_to = "mes", values_to = "valor")
gvol1 <- gvol1%>%pivot_wider(names_from = id, values_from = valor)
gvol1 <- gvol1[,-1] #rows: 1 = volbasal, 2 = vol1m, 3 = vol3m, 4 = vol6m, 5 = vol12m

gvol3 <- gvol3%>%pivot_longer(-id, names_to = "mes", values_to = "valor")
gvol3 <- gvol3%>%pivot_wider(names_from = id, values_from = valor)
gvol3 <- gvol3[,-1] #rows: 1 = volbasal, 2 = vol1m, 3 = vol3m, 4 = vol6m, 5 = vol12m

gvol6 <- gvol6%>%pivot_longer(-id, names_to = "mes", values_to = "valor")
gvol6 <- gvol6%>%pivot_wider(names_from = id, values_from = valor)
gvol6 <- gvol6[,-1] #rows: 1 = volbasal, 2 = vol1m, 3 = vol3m, 4 = vol6m, 5 = vol12m

gvol12 <- gvol12%>%pivot_longer(-id, names_to = "mes", values_to = "valor")
gvol12 <- gvol12%>%pivot_wider(names_from = id, values_from = valor)
gvol12 <- gvol12[,-1] #rows: 1 = volbasal, 2 = vol1m, 3 = vol3m, 4 = vol6m, 5 = vol12m

#Formato para ggplot lineas
gvolb <- data.frame(x = seq_along(gvolb[, c(1,2,3,4,5)]), gvolb)
gvolb <- melt(gvolb, id.vars = "x")
names(gvolb)[1] <- "Months"
names(gvolb)[3] <- "value"
gvolb <- na.omit(gvolb)

gvol1 <- data.frame(x = seq_along(gvol1[, c(1,2,3,4,5)]), gvol1)
gvol1 <- melt(gvol1, id.vars = "x")
names(gvol1)[1] <- "Months"
names(gvol1)[3] <- "value"
gvol1 <- na.omit(gvol1)

colnames(gvol3) <- c(1,2,3,4,5)
gvol3 <- data.frame(x = seq_along(gvol3[, c(1,2,3,4,5)]), gvol3)
gvol3 <- melt(gvol3, id.vars = "x")
names(gvol3)[1] <- "Months"
names(gvol3)[3] <- "value"
gvol3 <- na.omit(gvol3)

gvol6 <- data.frame(x = seq_along(gvol6[, c(1,2,3,4,5)]), gvol6)
gvol6 <- melt(gvol6, id.vars = "x")
names(gvol6)[1] <- "Months"
names(gvol6)[3] <- "value"
gvol6 <- na.omit(gvol6)

gvol12 <- data.frame(x = seq_along(gvol12[, c(1,2,3,4,5)]), gvol12)
gvol12 <- melt(gvol12, id.vars = "x")
names(gvol12)[1] <- "Months"
names(gvol12)[3] <- "value"
gvol12 <- na.omit(gvol12)


#Grafico 
labels <- c("0", "1", "3", "6", "12")
rownames(gvolb) <- NULL
rownames(gvol1) <- NULL
rownames(gvol3) <- NULL
rownames(gvol6) <- NULL
rownames(gvol12) <- NULL

GRAPH <- ggplot(gvol1, aes(x=Months, y=value, groupo = variable)) + 
  geom_point(data = gvolb, aes(x=Months, y=value),color='#000000', size = 4.5) +
  geom_line(data=gvol1, aes(x=Months, y=value), color = '#F37B59', size = 1.4) +
  geom_point(data = gvol1, aes(x=Months, y=value),color='#F8766D', size = 4) +
  geom_line(data=gvol3, aes(x=Months, y=value), color = '#AFA100', size = 1.3) +
  geom_point(data = gvol3, aes(x=Months, y=value),color='#BB9D00', size = 4.3) +
  geom_line(data=gvol6, aes(x=Months, y=value), color='#00BA42', size = 1.2) +
  geom_point(data = gvol6, aes(x=Months, y=value),color='#00B81F', size = 4.1) +
  geom_line(data=gvol12, aes(x=Months, y=value), color='#00BFC4', size = 1) +
  geom_point(data = gvol12, aes(x=Months, y=value),color='#00C0B8', size = 3.9) +
  theme(legend.position='none') + #Eliminamos legenda de cada id con su color
  labs(title='Tumor volumen') +
  scale_x_continuous(label = labels)
GRAPH 
```


</details>


