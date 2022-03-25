# LOD
diagnostic assay's sensitivity. 
Quick example code for a simple Limit of Detection (LOD) calculation and plotting using probit analysis.

Prior infos needed: Nr of test runs; Concentrations(Copies per run); run outcome (Pos/neg);


install.packages('ggplot2', dep = TRUE)
install.packages('ggpubr', dep= TRUE)
library(ggplot2)
library(MASS)
copies=c(100,10,1)
number.positive=c(8,8,3)
number=cbind(number.positive,number.negative=8-number.positive)
result.probit=glm(number ~ copies,family=binomial(link=probit))
result.probit$coefficients
dose.p(result.probit, p=.95)
dose_plot<--5:8
df<-data.frame(dose_plot)
curve<-ggplot(df,aes(dose_plot))
curve<- curve+stat_function(fun=function(dose_plot) (pnorm(result.probit$coefficients[1]+(result.probit$coefficients[2]*dose_plot))))
curve<-curve + scale_y_continuous(labels = scales::percent)
curve<- curve + scale_x_continuous(breaks=c(-5, 1, 8))
curve<-curve + labs(x = "Molecules detected (copies)", y = "Probability")
curve<- curve+theme_light()
curve <- curve + theme(text = element_text(size = 20))
curve<- curve +geom_vline(aes(xintercept=3.6), colour = "red")
curve<-curve+ geom_text(aes(0,0.5,label='aesdf',hjust=0.5))
