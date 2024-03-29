--- 
title: "Aplicações práticas do software R para Agronomia"
author: "Gabriel Danilo Shimizu"
date: "2021-09-09"
site: bookdown::bookdown_site
documentclass: book
bibliography: [book.bib, packages.bib]
biblio-style: apalike
link-citations: yes
description: "Este é um livro virtual desenvolvido com a finalidade de fornecer tutoriais práticos das principais análises estatísticas e apresentação de dados utilizando o software R"
---



# Análise de regressão linear e não-linear

Nas mais diversas áreas da pesquisa, seja ela na área médica, biológica, industrial, química entre outras, é de grande interesse verificar se duas ou mais variáveis estão relacionadas de alguma forma. Para expressar esta relação é muito importante estabelecer um modelo matemático. Este tipo de modelagem é chamado de regressão, e ajuda a entender como determinadas variáveis influenciam outra variável, ou seja, verifica como o comportamento de uma ou mais variáveis podem mudar o comportamento de outra.

Na agronomia, a análise de regressão é muito utilizada por exemplo, para estabelecer doses de máxima resposta de produtos fitossanitários, adubos, populações de plantas, etc..; ou mesmo no estudo do desenvolvimento de uma planta, o que chamamos de curva de crescimento. 

Popularmente, é comum a utilização de curva do tipo polinomial, visto a facilidade de sua utilização e explicação. Todavia, muito dos dados não se comportam dessa forma, ainda que o ajuste seja significativo, podendo assim, levar a conclusões limitadas em função da análise inadequada. Logo, o presente tutorial apresenta diferentes ajustes de regressão linear e não-linear de um mesmo conjunto de dados. 

Neste tutorial, você irá reparar que em quase todos os modelos, o coeficientes serão significativos, demonstrando que quase todos os modelos são válidos para explicar o comportamento dos dados. A questão é, qual o melhor modelo?

<img src="index_files/figure-html/unnamed-chunk-1-1.png" width="480" />

**Obs. Este é um tutorial para demonstração dos modelos de regressão. Alguns casos ele não é significativo ou uma das pressuposições não é atendida. É um tutorial apenas para fins didáticos.**

<br><br>

****

## Conjunto de dados

****

O conjunto de dados é de um experimento cujo objetivo foi avaliar a perda de massa da casca de romã em estufa a $60^oC$. Foi utilizado oito repetições em oito avaliações (60, 210,390, 720, 930, 1410, 1890 e 2370 minutos)


```r
`PERDA DE MASSA CAA`=c(18.15810,17.99376,14.81450,15.39822,21.62234,20.45106,18.65319,20.96547,36.77274,39.92503,34.60874,35.70286,43.57189,42.19460,39.23367,43.36169,52.90384,52.64886,45.61431,47.81200,44.41734,47.40493,46.15373,47.12330,65.29474,67.78859,64.60738,66.24453,63.97464,66.77636,65.37446,65.11912,67.86385,70.68877,69.45271,70.33895,69.43583,71.56150,69.73480,69.97407,69.02813,71.28882,71.17485,71.22420,71.32344,72.46687,71.17063,72.07550,69.16576,71.44176,71.30762,71.34075,71.42775,72.59710,71.28255,72.19996,69.30339,71.59471,71.44040,71.45729,71.53206,72.72733,71.39446,72.32441)
TEMPO=rep(c(60,210,390,720,930,1410,1890,2370),e=8)
dados=data.frame(TEMPO,`PERDA DE MASSA CAA`)
y=c(`PERDA DE MASSA CAA`)
x=c(TEMPO)
data=data.frame(y,x)
```

<br><br>

### Média e desvio-padrão amostral


```r
(media=tapply(y,x, mean))
```

```
##       60      210      390      720      930     1410     1890     2370 
## 18.50708 39.42140 48.00979 65.64748 69.88131 71.21905 71.34541 71.47176
```

```r
(desvio=tapply(y,x, sd))
```

```
##       60      210      390      720      930     1410     1890     2370 
## 2.485224 3.479257 3.133973 1.229025 1.080134 1.007882 1.004939 1.002227
```

```r
(tempo=c(60,210,390,720,930,1410,1890,2370))
```

```
## [1]   60  210  390  720  930 1410 1890 2370
```

<br><br>

****

### Gráficos exploratórios

****

<br><br>

### Gráfico de caixas


```r
boxplot(y~x)
```

<img src="index_files/figure-html/unnamed-chunk-4-1.png" width="672" />

### Gráfico de dispersão


```r
plot(y~x)
```

<img src="index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

### Gráfico de dispersão com médias


```r
plot(y~x)
points(media~tempo,pch=16,col="red")
```

<img src="index_files/figure-html/unnamed-chunk-6-1.png" width="672" />

### Gráfico de linhas com as médias


```r
par(mfrow=c(1,2))
plot(media~tempo, type="b")
plot(media~tempo, type="l")
```

<img src="index_files/figure-html/unnamed-chunk-7-1.png" width="768" style="display: block; margin: auto;" />

### Histograma


```r
hist(y)
```

<img src="index_files/figure-html/unnamed-chunk-8-1.png" width="672" />

<br><br><br>

****

## Linear Simples

****

O modelo de regressão linear simples (MRLS) se define uma relação linear entre a variável dependente e uma variável independente.

$$Y=\beta_1x+\beta_0$$

### Criando o modelo de regressão


```r
modl=lm(y~x)
summary(modl)
```

```
## 
## Call:
## lm(formula = y ~ x)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -24.4181  -8.1253   0.4191   8.8542  16.0914 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 38.099512   2.368998   16.08  < 2e-16 ***
## x            0.018886   0.001876   10.07 1.15e-14 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 11.62 on 62 degrees of freedom
## Multiple R-squared:  0.6205,	Adjusted R-squared:  0.6144 
## F-statistic: 101.4 on 1 and 62 DF,  p-value: 1.147e-14
```

### Diagnóstico

### Normalidade dos erros


```r
hnp::hnp(modl)
```

```
## Gaussian model (lm object)
```

<img src="index_files/figure-html/unnamed-chunk-10-1.png" width="672" />

```r
shapiro.test(resid(modl)) # erros não normais
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  resid(modl)
## W = 0.94067, p-value = 0.004079
```

### Falta de ajuste (Desvio da regressão)


```r
modq=aov(y~as.factor(x))
anova(modl,modq)
```

```
## Analysis of Variance Table
## 
## Model 1: y ~ x
## Model 2: y ~ as.factor(x)
##   Res.Df    RSS Df Sum of Sq      F    Pr(>F)    
## 1     62 8377.2                                  
## 2     56  236.7  6    8140.5 321.02 < 2.2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

### Construindo gráfico


```r
par(family="serif")
plot(media~tempo, main="Linear Simples",
     las=1, cex=1.3,
     ylab="Weight loss (%)", xlim=c(0,2500),
     xlab="Time (Minutes)", 
     pch=16, ylim=c(0,80))
curve(coef(modl)[1]+coef(modl)[2]*x, add=TRUE, lty=2)
legend("topleft",
       cex=1,
       bty="n",
       legend = c(expression(hat(Y)==38.09951+0.01889*x)))
```

<img src="index_files/figure-html/unnamed-chunk-12-1.png" width="672" />

### ggplot2


```r
library(ggplot2)
da=data.frame(media,tempo)
ggplot(da,aes(y=media,x=tempo))+
  geom_point()+
  geom_smooth(method="lm",se = F,col="black",size=0.1,lty=2)+
  theme_classic()+
  ylab("Weight loss (%)")+
  xlab("Time (Minutes)") 
```

<img src="index_files/figure-html/unnamed-chunk-13-1.png" width="672" />

<br><br><br>

****

## Quadrático

****

$$Y=\beta_2x^2+\beta_1x+\beta_0$$

### Criando modelo de regressão


```r
mod1=lm(y~x+I(x^2))
summary(mod1)
```

```
## 
## Call:
## lm(formula = y ~ x + I(x^2))
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -11.428  -5.288   1.756   4.360   8.018 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.226e+01  1.528e+00   14.57   <2e-16 ***
## x            6.763e-02  3.367e-03   20.09   <2e-16 ***
## I(x^2)      -2.055e-05  1.371e-06  -14.99   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 5.415 on 61 degrees of freedom
## Multiple R-squared:  0.919,	Adjusted R-squared:  0.9163 
## F-statistic: 345.9 on 2 and 61 DF,  p-value: < 2.2e-16
```

### Diagnóstico do modelo

### Normalidade dos erros


```r
hnp::hnp(mod1)
```

```
## Gaussian model (lm object)
```

<img src="index_files/figure-html/unnamed-chunk-15-1.png" width="672" />

```r
shapiro.test(resid(mod1)) # erros nao normais
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  resid(mod1)
## W = 0.92285, p-value = 0.000655
```

### Fator de inflação de variância (Multicolinearidade)


```r
car::vif(mod1) # problema de multicolinearidade
```

```
##        x   I(x^2) 
## 14.84834 14.84834
```

### Falta de ajuste (Desvio da regressão)


```r
modq=aov(y~as.factor(x))
anova(mod1,modq)
```

```
## Analysis of Variance Table
## 
## Model 1: y ~ x + I(x^2)
## Model 2: y ~ as.factor(x)
##   Res.Df     RSS Df Sum of Sq      F    Pr(>F)    
## 1     61 1788.55                                  
## 2     56  236.68  5    1551.9 73.438 < 2.2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

### Construindo gráfico


```r
par(family="serif")
plot(media~tempo, main="Quadrático",
     las=1, cex=1.3,
     ylab="Weight loss (%)", xlim=c(0,2500),
     xlab="Time (Minutes)", 
     pch=16, ylim=c(0,80))
curve(coef(mod1)[1]+coef(mod1)[2]*x+coef(mod1)[3]*x^2, add=TRUE, lty=2)
legend("topleft",
       cex=1,
       bty="n",
       legend = c(expression(hat(Y)==22.26+0.006763*x-0.00002055*x^2)))
```

<img src="index_files/figure-html/unnamed-chunk-18-1.png" width="672" />

### Ponto de máximo (Ou mínimo)

O ponto de máximo ou mínimo podem ser encontrados de várias formas

### Manualmente


```r
(xmax=-coef(mod1)[2]/(2*coef(mod1)[3]))
```

```
##        x 
## 1645.317
```

```r
(ymax=coef(mod1)[1]+coef(mod1)[2]*xmax+coef(mod1)[3]*xmax^2)
```

```
## (Intercept) 
##    77.89475
```

### Usando o which.max ou which.min


```r
plot(y~x)
tend=curve(coef(mod1)[1]+coef(mod1)[2]*x+coef(mod1)[3]*x^2,add=T)
```

```r
tend$x[which.max(tend$y)]
```

```
## [1] 1653.9
```

```r
# tend$x[which.min(tend$y)] # no caso de mínimo
```

### ggplot2


```r
library(ggplot2)
da=data.frame(media,tempo)
ggplot(da,aes(y=media,x=tempo))+
  geom_point()+
  geom_smooth(method="lm",formula = y~poly(x,2), se = F,col="black",size=0.1,lty=2)+
  theme_classic()+
  ylab("Weight loss (%)")+
  xlab("Time (Minutes)") 
```

<img src="index_files/figure-html/unnamed-chunk-21-1.png" width="672" />

<br><br><br>

****

## Cúbico

****

$$Y=\beta_3x^3+\beta_2x^2+\beta_1x+\beta_0$$

### Construindo o modelo


```r
mod2=lm(y~x+I(x^2)+I(x^3))
summary(mod2)
```

```
## 
## Call:
## lm(formula = y ~ x + I(x^2) + I(x^3))
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -6.0186 -1.3299 -0.3928  1.3155  8.0377 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  1.406e+01  9.927e-01   14.16   <2e-16 ***
## x            1.174e-01  4.125e-03   28.47   <2e-16 ***
## I(x^2)      -7.536e-05  4.189e-06  -17.99   <2e-16 ***
## I(x^3)       1.524e-08  1.149e-09   13.27   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.753 on 60 degrees of freedom
## Multiple R-squared:  0.9794,	Adjusted R-squared:  0.9784 
## F-statistic: 951.1 on 3 and 60 DF,  p-value: < 2.2e-16
```

### Diagnóstico do modelo

### Normalidade dos erros


```r
hnp::hnp(mod2)
```

```
## Gaussian model (lm object)
```

<img src="index_files/figure-html/unnamed-chunk-23-1.png" width="672" />

```r
shapiro.test(resid(mod2)) # Erros nao normais
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  resid(mod2)
## W = 0.94796, p-value = 0.009093
```

### Fator de inflação de variância (Multicolinearidade) 


```r
car::vif(mod2) # alta multicolinearidade
```

```
##         x    I(x^2)    I(x^3) 
##  86.25922 536.46498 221.25164
```

### Falta de ajuste (Desvio da regressão)


```r
modq=aov(y~as.factor(x))
anova(mod2,modq)
```

```
## Analysis of Variance Table
## 
## Model 1: y ~ x + I(x^2) + I(x^3)
## Model 2: y ~ as.factor(x)
##   Res.Df    RSS Df Sum of Sq      F    Pr(>F)    
## 1     60 454.60                                  
## 2     56 236.68  4    217.93 12.891 1.666e-07 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

### Construindo o gráfico


```r
par(family="serif")
plot(media~tempo, main="Cúbico",
     las=1, cex=1.3,
     ylab="Weight loss (%)", xlim=c(0,2500),
     xlab="Time (Minutes)", 
     pch=16, ylim=c(0,80))
curve(coef(mod2)[1]+coef(mod2)[2]*x+coef(mod2)[3]*x^2+coef(mod2)[4]*x^3, add=TRUE, lty=2)
legend("topleft",
       cex=1,
       bty="n",
       legend = c(expression(hat(Y)==14.06+0.01174*x-0.00007536*x^2+0.00000001524*x^3)))
```

<img src="index_files/figure-html/unnamed-chunk-26-1.png" width="672" />

### ponto de máximo, mínimo e inflexão


```r
plot(media~tempo)
curva=curve(coef(mod2)[1]+coef(mod2)[2]*x+coef(mod2)[3]*x^2+coef(mod2)[4]*x^3, add=TRUE, lty=2)
```

```r
# ponto de inflexão
pi=-(2*coef(mod2)[3])/(3*2*coef(mod2)[4])

# ponto de máximo anterior ao ponto de inflexão
xmax=curva$x[which.max(curva$y[curva$x<pi])]

# ponto de mínimo posterior ao ponto de inflexão
xmin=curva$x[which.max(curva$y[curva$x<pi])+which.min(curva$y[curva$x>xmax])]
```


```r
plot(media~tempo)
curva=curve(coef(mod2)[1]+coef(mod2)[2]*x+coef(mod2)[3]*x^2+coef(mod2)[4]*x^3, add=TRUE, lty=1)
abline(v=c(xmax,xmin,pi),lty=2)
```

<img src="index_files/figure-html/unnamed-chunk-28-1.png" width="672" />

### ggplot2


```r
library(ggplot2)
da=data.frame(media,tempo)
ggplot(da,aes(y=media,x=tempo))+
  geom_point()+
  geom_smooth(method="lm",formula = y~poly(x,3), se = F,col="black",size=0.1,lty=2)+
  theme_classic()+
  ylab("Weight loss (%)")+
  xlab("Time (Minutes)") 
```

<img src="index_files/figure-html/unnamed-chunk-29-1.png" width="672" />

<br><br><br>

****

## Logarítmico

****

$$Y=\beta_{0}+\beta_{1}\log(x)$$

### Construindo modelo


```r
modelog=lm(y~log(x))
summary(modelog)
```

```
## 
## Call:
## lm(formula = y ~ log(x))
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -8.5359 -3.5194 -0.5506  3.6366  8.4348 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -42.7285     3.3261  -12.85   <2e-16 ***
## log(x)       15.5158     0.5096   30.45   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4.724 on 62 degrees of freedom
## Multiple R-squared:  0.9373,	Adjusted R-squared:  0.9363 
## F-statistic: 927.1 on 1 and 62 DF,  p-value: < 2.2e-16
```

### Diagnóstico do modelo


```r
hnp::hnp(modelog)
```

```
## Gaussian model (lm object)
```

<img src="index_files/figure-html/unnamed-chunk-31-1.png" width="672" />

```r
shapiro.test(resid(modelog))
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  resid(modelog)
## W = 0.94476, p-value = 0.006371
```

### Construindo gráfico


```r
plot(media~tempo, main="Log",
     las=1, cex=1.3,
     ylab="Weight loss (%)", xlim=c(0,2500),
     xlab="Time (Minutes)", 
     pch=16, ylim=c(0,80))
curve(-42.73+15.52*log(x),add=T,lty=2)
legend("topleft",
       cex=1,
       bty="n",
       legend = c(expression(hat(Y)==-42.73+15.52*log(x))))
```

<img src="index_files/figure-html/unnamed-chunk-32-1.png" width="672" />

<br><br><br>

****

## Michaelis-Menten (MM)

****

$$Y=\frac{A\times x}{V+x}$$

### Construindo o modelo


```r
data=data.frame(y,x)
n0 <- nls(formula=y~A*x/(V+x), data=data,
          start=list(A=max(y), V=100), trace=TRUE)
```

```
## 2726.427    (1.63e+00): par = (72.72733 100)
## 820.4424    (4.37e-01): par = (78.84265 179.5977)
## 691.3380    (2.82e-02): par = (80.90678 212.8899)
## 690.8008    (4.86e-04): par = (81.02471 215.2486)
## 690.8006    (1.19e-05): par = (81.02129 215.2041)
## 690.8006    (3.03e-07): par = (81.02137 215.2052)
```

```r
summary(n0)
```

```
## 
## Formula: y ~ A * x/(V + x)
## 
## Parameters:
##   Estimate Std. Error t value Pr(>|t|)    
## A   81.021      1.004   80.67   <2e-16 ***
## V  215.205     11.711   18.38   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 3.338 on 62 degrees of freedom
## 
## Number of iterations to convergence: 5 
## Achieved convergence tolerance: 3.035e-07
```

### Diagnóstico do modelo


```r
shapiro.test(resid(n0))
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  resid(n0)
## W = 0.9717, p-value = 0.1482
```

### Construindo o gráfico


```r
A <- coef(n0)[1]; V <- coef(n0)[2]
par(family="serif")
plot(media~tempo, main="Michaelis Menten",
     las=1, cex=1.3,
     ylab="Weight loss (%)", xlim=c(0,2500),
     xlab="Time (Minutes)", 
     pch=16, ylim=c(0,80))
curve(A*x/(V+x), add=TRUE, lty=2)
legend("topleft",
       cex=1,
       bty="n",
       legend = c(expression(hat(Y)==frac(81.021*x,(215.205+x)))))
```

<img src="index_files/figure-html/unnamed-chunk-35-1.png" width="672" />

### Utilizando outro método


```r
m.m <- nls(y ~ SSmicmen(x, Vm, K), data = data)
m.m
```

```
## Nonlinear regression model
##   model: y ~ SSmicmen(x, Vm, K)
##    data: data
##     Vm      K 
##  81.02 215.20 
##  residual sum-of-squares: 690.8
## 
## Number of iterations to convergence: 0 
## Achieved convergence tolerance: 2.047e-06
```

```r
plot(media~tempo, main="Michaelis-Menten",
     las=1, cex=1.3,
     ylab="Weight loss (%)", xlim=c(0,2500),
     xlab="Time (Minutes)", 
     pch=16, ylim=c(0,80))
curve((81.02135*x)/(215.20499+x), add=T)
```

<img src="index_files/figure-html/unnamed-chunk-36-1.png" width="672" />

<br><br><br>

****

## MM Modificado

****

$$Y=\frac{A\times x}{V+x}+D\times x $$

### Construindo modelo


```r
data=data.frame(y,x)
n1 <- nls(formula=y~A*x/(V+x)+D*x, data=data,
          start=list(A=max(y), V=100,D=10), trace=TRUE)
```

```
## 1.020629e+10 (3.73e+03): par = (72.72733 100 10)
## 802.0047    (6.61e-01): par = (80.61554 185.7416 -0.0009194725)
## 545.3405    (2.02e-01): par = (91.71008 263.7484 -0.004630648)
## 521.8705    (3.71e-02): par = (96.95405 297.1032 -0.006224234)
## 521.0745    (4.75e-03): par = (98.08532 303.9352 -0.006567869)
## 521.0613    (5.96e-04): par = (98.24147 304.8817 -0.006617559)
## 521.0611    (7.46e-05): par = (98.26112 305.0017 -0.006623916)
## 521.0611    (9.34e-06): par = (98.26358 305.0167 -0.006624713)
```

```r
summary(n1)
```

```
## 
## Formula: y ~ A * x/(V + x) + D * x
## 
## Parameters:
##     Estimate Std. Error t value Pr(>|t|)    
## A  98.263575   4.439290  22.135  < 2e-16 ***
## V 305.016711  25.778649  11.832  < 2e-16 ***
## D  -0.006625   0.001563  -4.239 7.73e-05 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.923 on 61 degrees of freedom
## 
## Number of iterations to convergence: 7 
## Achieved convergence tolerance: 9.342e-06
```

### Construindo gráfico


```r
A <- coef(n1)[1]; V <- coef(n1)[2]; D<-coef(n1)[3]
par(family="serif")
plot(media~tempo, main="Michaelis Menten (Corrigido)",
     las=1, cex=1.3,
     ylab="Weight loss (%)", xlim=c(0,2500),
     xlab="Time (Minutes)", 
     pch=16, ylim=c(0,80))
curve(A*x/(V+x)+D*x, add=TRUE, lty=2)
legend("topleft",
       cex=1,
       bty="n",
       legend = c(expression(hat(Y)==frac(98.263572*x,(305.016698+x))-0.006625*x)))
```

<img src="index_files/figure-html/unnamed-chunk-38-1.png" width="672" />

<br><br><br>

****

## Segmentada linear

****

$$Y=\beta_{1}X+\beta_{0} (if\leq X_1)$$

### Construindo o modelo linear 


```r
modelo_linear<- lm(y~x)
summary(modelo_linear)
```

```
## 
## Call:
## lm(formula = y ~ x)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -24.4181  -8.1253   0.4191   8.8542  16.0914 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 38.099512   2.368998   16.08  < 2e-16 ***
## x            0.018886   0.001876   10.07 1.15e-14 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 11.62 on 62 degrees of freedom
## Multiple R-squared:  0.6205,	Adjusted R-squared:  0.6144 
## F-statistic: 101.4 on 1 and 62 DF,  p-value: 1.147e-14
```

### Construindo o modelo segmentado


```r
library(segmented)
modelo_pieciwise<- segmented(modelo_linear, seg.Z = ~x, psi=1000)
modelo_pieciwise
```

```
## Call: segmented.lm(obj = modelo_linear, seg.Z = ~x, psi = 1000)
## 
## Meaningful coefficients of the linear terms:
## (Intercept)            x         U1.x  
##    19.83682      0.06684     -0.06582  
## 
## Estimated Break-Point(s):
## psi1.x  
##  751.4
```

```r
summary(modelo_pieciwise)
```

```
## 
## 	***Regression Model with Segmented Relationship(s)***
## 
## Call: 
## segmented.lm(obj = modelo_linear, seg.Z = ~x, psi = 1000)
## 
## Estimated Break-Point(s):
##            Est. St.Err
## psi1.x 751.438 26.797
## 
## Meaningful coefficients of the linear terms:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 19.836818   1.106834   17.92   <2e-16 ***
## x            0.066839   0.002612   25.59   <2e-16 ***
## U1.x        -0.065819   0.002873  -22.91       NA    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 3.635 on 60 degrees of freedom
## Multiple R-Squared: 0.9641,  Adjusted R-squared: 0.9623 
## 
## Convergence attained in 4 iter. (rel. change 0)
```

### Definindo limite com base no platô


```r
y1=y[x<=modelo_pieciwise$psi[2]]
x11=x[x<=modelo_pieciwise$psi[2]]
```

### Curva do primeiro segmento


```r
mod=lm(y1~x11)
summary(mod)
```

```
## 
## Call:
## lm(formula = y1 ~ x11)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -9.0327 -2.9998 -0.7374  2.1557  9.6988 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 19.836818   1.532481   12.94 8.22e-14 ***
## x11          0.066839   0.003617   18.48  < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 5.033 on 30 degrees of freedom
## Multiple R-squared:  0.9193,	Adjusted R-squared:  0.9166 
## F-statistic: 341.6 on 1 and 30 DF,  p-value: < 2.2e-16
```

### Construindo gráfico


```r
par(pch=16,las=1); par(family="serif")
plot(media~tempo, 
     las=1, cex=1.3, main="Segmentado Linear",
     ylab="Weight loss (%)", xlim=c(0,2500),
     xlab="Time (Minutes)", 
     pch=16, ylim=c(0,80))
a=curve(coef(mod)[1]+coef(mod)[2]*x, 
        to=modelo_pieciwise$psi[2], lty=2,add=T)
plato=a$y[round(a$x,3)==round(modelo_pieciwise$psi[2],3)]
lines(c(modelo_pieciwise$psi[2],max(x)),
      c(plato,plato),lty=2)
legend("topleft",
       cex=1,
       legend=expression(hat(Y)==19.836817+0.066839*x~("if"~x~"<"~751.4)), bty="n")
```

<img src="index_files/figure-html/unnamed-chunk-43-1.png" width="672" />

<br><br><br>

****

## Segmentada quadrático

****

$$Y=\beta_{2}X^2+\beta_{1}X+\beta_{0} (if\leq X_1)$$

### Construindo o modelo quadrático


```r
modelo_linear<- lm(y~x+I(x^2))
summary(modelo_linear)
```

```
## 
## Call:
## lm(formula = y ~ x + I(x^2))
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -11.428  -5.288   1.756   4.360   8.018 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.226e+01  1.528e+00   14.57   <2e-16 ***
## x            6.763e-02  3.367e-03   20.09   <2e-16 ***
## I(x^2)      -2.055e-05  1.371e-06  -14.99   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 5.415 on 61 degrees of freedom
## Multiple R-squared:  0.919,	Adjusted R-squared:  0.9163 
## F-statistic: 345.9 on 2 and 61 DF,  p-value: < 2.2e-16
```

### Construindo o modelo segmentado


```r
library(segmented)
modelo_pieciwise1<- segmented(modelo_linear)
modelo_pieciwise1
```

```
## Call: segmented.lm(obj = modelo_linear)
## 
## Meaningful coefficients of the linear terms:
## (Intercept)            x       I(x^2)         U1.x  
##   1.571e+01    1.010e-01   -4.403e-05    8.682e-02  
## 
## Estimated Break-Point(s):
## psi1.x  
##   1636
```

```r
summary(modelo_pieciwise1)
```

```
## 
## 	***Regression Model with Segmented Relationship(s)***
## 
## Call: 
## segmented.lm(obj = modelo_linear)
## 
## Estimated Break-Point(s):
##             Est. St.Err
## psi1.x 1636.204 25.012
## 
## Meaningful coefficients of the linear terms:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  1.571e+01  9.873e-01   15.91   <2e-16 ***
## x            1.010e-01  3.417e-03   29.55   <2e-16 ***
## I(x^2)      -4.402e-05  2.269e-06  -19.40   <2e-16 ***
## U1.x         8.682e-02  7.115e-03   12.20       NA    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.907 on 59 degrees of freedom
## Multiple R-Squared: 0.9774,  Adjusted R-squared: 0.9759 
## 
## Convergence attained in 2 iter. (rel. change 0)
```

### Valores para o primeiro segmento

Obs. No caso do linear simples, podemo usar apenas os pontos abaixo do platô, no caso do segmentado quadrático aconselho englobar o ponto acima do acusado no platô. No meu caso é o ponto 930.


```r
y1=y[x<=930]
x11=x[x<=930]
mod=lm(y1~x11+I(x11^2))
summary(mod)
```

```
## 
## Call:
## lm(formula = y1 ~ x11 + I(x11^2))
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -5.5773 -2.1731  0.0432  1.2608  8.0591 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  1.357e+01  1.379e+00   9.839 7.13e-12 ***
## x11          1.175e-01  7.321e-03  16.047  < 2e-16 ***
## I(x11^2)    -6.173e-05  7.151e-06  -8.632 2.15e-10 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 3.293 on 37 degrees of freedom
## Multiple R-squared:  0.9715,	Adjusted R-squared:   0.97 
## F-statistic: 630.9 on 2 and 37 DF,  p-value: < 2.2e-16
```

### Construindo o gráfico


```r
par(pch=16,las=1); par(family="serif")
plot(media~tempo, main="Segmentado Quadrático",
     las=1, cex=1.3,
     ylab="Weight loss (%)", xlim=c(0,2500),
     xlab="Time (Minutes)", 
     pch=16, ylim=c(0,80))
maximo=-coef(mod)[2]/(2*coef(mod)[3])
a=curve(coef(mod)[1]+coef(mod)[2]*x+coef(mod)[3]*x^2, 
        to=maximo, lty=2,
        add=T)
plato=a$y[round(a$x,3)==round(maximo,3)]
lines(c(maximo,max(x)),
      c(plato,plato),lty=2)
legend("topleft",
       legend=expression(Y==13.57+0.1175*x-0.00006173*x^2~("if"~x~"<"~951.5095)), bty="n")
```

<img src="index_files/figure-html/unnamed-chunk-47-1.png" width="672" />

<br><br><br>

****

## Mitscherlich

****

$$Y=A \times(1-exp((B\times C)-(C \times X)$$


```r
modelo2=nls(y~A*(1-exp((B*C)-(C*x))),
           start = list(A=80,B=-10,C=0.01),data=data)
summary(modelo2)
```

```
## 
## Formula: y ~ A * (1 - exp((B * C) - (C * x)))
## 
## Parameters:
##     Estimate Std. Error t value Pr(>|t|)    
## A  7.232e+01  5.606e-01 129.004  < 2e-16 ***
## B -4.438e+01  8.610e+00  -5.155  2.9e-06 ***
## C  2.874e-03  1.302e-04  22.066  < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.63 on 61 degrees of freedom
## 
## Number of iterations to convergence: 7 
## Achieved convergence tolerance: 8.634e-07
```

```r
par(pch=16,las=1); par(family="serif")
plot(media~tempo,main="Mitscherlich",
     las=1, cex=1.3,
     ylab="Weight loss (%)", xlim=c(0,2500),
     xlab="Time (Minutes)", 
     pch=16, ylim=c(0,80))
b=summary(modelo2)
A=b$coefficients[1,1]
B=b$coefficients[2,1]
C=b$coefficients[3,1]
a=curve(A*(1-exp((B*C)-(C*x))),lty=2,add=T)
legend("topleft",expression(Y==72.31912*(1-e^{(-44.382759*0.002873)-(0.002873*x)})),bty="n")
```

<img src="index_files/figure-html/unnamed-chunk-48-1.png" width="672" />

<br><br><br>

****

## Logística de 3 termos

****

$$Y = \frac{d}{1+exp(b(x-e))}$$


```r
library(drc)
model <- drm(y ~ x, fct = LL.3(), data = data)
summary(model)
```

```
## 
## Model fitted: Log-logistic (ED50 as parameter) with lower limit at 0 (3 parms)
## 
## Parameter estimates:
## 
##                 Estimate Std. Error t-value   p-value    
## b:(Intercept)  -1.058194   0.062275 -16.992 < 2.2e-16 ***
## d:(Intercept)  79.599836   1.684582  47.252 < 2.2e-16 ***
## e:(Intercept) 208.408451  12.445682  16.745 < 2.2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error:
## 
##  3.340759 (61 degrees of freedom)
```

```r
par(family="serif")
plot(model,main="Logístico LL.3",
     las=1, cex=1.3,
     ylab="Weight loss (%)", 
     xlab="Time (Minutes)", 
     pch=16,lty=2)
legend("topleft",
       legend=expression(hat(Y)==frac(79.599836,
                                      1+exp(-1.058194(x-208.408455)))), bty="n")
```

<img src="index_files/figure-html/unnamed-chunk-49-1.png" width="672" />

### ED, DL ou EC


```r
ED(model,10) ## Ed10
```

```
## 
## Estimated effective doses
## 
##        Estimate Std. Error
## e:1:10   26.131      2.755
```

```r
ED(model,50) ## ED50
```

```
## 
## Estimated effective doses
## 
##        Estimate Std. Error
## e:1:50  208.408     12.446
```

```r
ED(model,90) ## ED90
```

```
## 
## Estimated effective doses
## 
##        Estimate Std. Error
## e:1:90   1662.2      267.4
```

<br><br><br>

****

## Logística de 4 termos

****

$$Y = c-\frac{d-c}{1+exp(b(x-e))}$$


```r
model1 <- drm(y ~ x, fct = LL.4(), data = data)
summary(model1)
```

```
## 
## Model fitted: Log-logistic (ED50 as parameter) (4 parms)
## 
## Parameter estimates:
## 
##               Estimate Std. Error  t-value   p-value    
## b:(Intercept)  -1.6960     0.1552 -10.9279 6.668e-16 ***
## c:(Intercept)  15.1899     1.9728   7.6995 1.597e-10 ***
## d:(Intercept)  74.5348     1.0697  69.6796 < 2.2e-16 ***
## e:(Intercept) 289.3971    16.5292  17.5082 < 2.2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error:
## 
##  2.950196 (60 degrees of freedom)
```

```r
par(family="serif")
plot(model,main="Logístico LL.4",
     las=1, cex=1.3,
     ylab="Weight loss (%)", 
     xlab="Time (Minutes)", 
     pch=16,lty=2)
legend("topleft",
       legend=expression(hat(Y)==15.1899+frac(74.59984-15.1899,
                                              1+exp(-1.6960(x-289.3971)))), bty="n")
```

<img src="index_files/figure-html/unnamed-chunk-51-1.png" width="672" />

### ED, DL ou EC


```r
ED(model,10) ## Ed10
```

```
## 
## Estimated effective doses
## 
##        Estimate Std. Error
## e:1:10   26.131      2.755
```

```r
ED(model,50) ## ED50
```

```
## 
## Estimated effective doses
## 
##        Estimate Std. Error
## e:1:50  208.408     12.446
```

```r
ED(model,90) ## ED90
```

```
## 
## Estimated effective doses
## 
##        Estimate Std. Error
## e:1:90   1662.2      267.4
```

<br><br><br>

****

## Yield Loss

****

$$\hat{Y}=\frac{i\times x}{1+\frac{i\times x}{A}}$$


```r
# library(devtools)
# install_github("OnofriAndreaPG/aomisc")
# par(family="serif")
# library(aomisc)
# model2 <- drm(y ~ x, fct = DRC.YL(), data = data)
# summary(model2)
# plot(model2,main="Yield Loss",
#      las=1, cex=1.3,
#      ylab="Weight loss (%)", 
#      xlab="Time (Minutes)", 
#      pch=16,lty=2)
# legend("topleft",
#        legend=expression(hat(YL)==frac(0.376483*x,
#                                        1+frac(0.376483*x,81.021705))), bty="n")
```

<br><br><br>

****

## Weibull 3

****

$$\hat{Y}=d\times e^{-e^{b\times log(x)-e}}$$

```r
par(family="serif")
model3 <- drm(y ~ x, fct = w3(), data = data)
summary(model3)
```

```
## 
## Model fitted: Weibull (type 1) with lower limit at 0 (3 parms)
## 
## Parameter estimates:
## 
##                 Estimate Std. Error t-value   p-value    
## b:(Intercept)  -0.621433   0.051944 -11.963 < 2.2e-16 ***
## d:(Intercept)  88.316665   3.466514  25.477 < 2.2e-16 ***
## e:(Intercept) 135.558602  10.937572  12.394 < 2.2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error:
## 
##  3.702132 (61 degrees of freedom)
```

```r
plot(model3,main="Weibull 3",
     las=1, cex=1.3,
     ylab="Weight loss (%)", 
     xlab="Time (Minutes)", 
     pch=16,lty=2)
legend("topleft",
       legend=expression(hat(YL)==88.316665*e^(-e^{(-0.621433*(log(x)-135.558606))})), bty="n")
```

<img src="index_files/figure-html/unnamed-chunk-54-1.png" width="672" />

<br><br><br>

****

## Weibul 4 

****

$$\hat{Y} = c + (d − c)(1 − exp(− exp(b(log(x) − log(e)))))$$


```r
par(family="serif")
model4 <- drm(y ~ x, fct = w4(), data = data)
summary(model4)
```

```
## 
## Model fitted: Weibull (type 1) (4 parms)
## 
## Parameter estimates:
## 
##               Estimate Std. Error t-value   p-value    
## b:(Intercept)  -1.2171     0.1156 -10.528 2.911e-15 ***
## c:(Intercept)  18.4270     1.2668  14.546 < 2.2e-16 ***
## d:(Intercept)  76.5754     1.5142  50.571 < 2.2e-16 ***
## e:(Intercept) 230.4661    11.7439  19.624 < 2.2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error:
## 
##  3.134519 (60 degrees of freedom)
```

```r
plot(model4,main="Weibull 4",
     las=1, cex=1.3,
     ylab="Weight loss (%)", 
     xlab="Time (Minutes)", 
     pch=16,lty=2)
legend("topleft",
       legend=expression(hat(YL)==18.4270+(76.5754-18.4270)(1-e^(-e^(-1.2171*(log(x)-log(230.4661)))))), bty="n")
```

<img src="index_files/figure-html/unnamed-chunk-55-1.png" width="672" />

<br><br><br>

****

## Assintótica 2

****


```r
par(family="serif")
model5 <- drm(y ~ x, fct = drc::AR.2(), data = data)
summary(model5)
```

```
## 
## Model fitted: Asymptotic regression with lower limit at 0 (2 parms)
## 
## Parameter estimates:
## 
##                Estimate Std. Error t-value   p-value    
## d:(Intercept)  71.36776    0.64016 111.484 < 2.2e-16 ***
## e:(Intercept) 285.21787   10.77269  26.476 < 2.2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error:
## 
##  3.364715 (62 degrees of freedom)
```

```r
plot(model5,main="Assintótica 2",
     las=1, cex=1.3,
     ylab="Weight loss (%)", 
     xlab="Time (Minutes)", 
     pch=16,lty=2)
```

<img src="index_files/figure-html/unnamed-chunk-56-1.png" width="672" />

<br><br><br>

****

## Assintótica 3

****


```r
par(family="serif")
model6 <- drm(y ~ x, fct = drc::AR.3(), data = data)
summary(model6)
```

```
## 
## Model fitted: Shifted asymptotic regression (3 parms)
## 
## Parameter estimates:
## 
##                Estimate Std. Error  t-value   p-value    
## c:(Intercept)   8.65955    1.29240   6.7003 7.565e-09 ***
## d:(Intercept)  72.31924    0.55231 130.9390 < 2.2e-16 ***
## e:(Intercept) 348.01446   15.20460  22.8888 < 2.2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error:
## 
##  2.630211 (61 degrees of freedom)
```

```r
plot(model6,main="Assintótica 3",
     las=1, cex=1.3,
     ylab="Weight loss (%)", 
     xlab="Time (Minutes)", 
     pch=16,lty=2)
```

<img src="index_files/figure-html/unnamed-chunk-57-1.png" width="672" />

<br><br><br>

****

## Brain-Counsens 4

****


```r
model7 <- drm(y ~ x, fct = drc::BC.4(), data = data)
summary(model7)
```

```
## 
## Model fitted: Brain-Cousens (hormesis) with lower limit fixed at 0 (4 parms)
## 
## Parameter estimates:
## 
##                  Estimate  Std. Error  t-value   p-value    
## b:(Intercept)  -0.7419957   0.0628498 -11.8059 < 2.2e-16 ***
## d:(Intercept) 149.6381450  28.0543384   5.3339 1.539e-06 ***
## e:(Intercept) 842.5975169 384.1854662   2.1932  0.032179 *  
## f:(Intercept)  -0.0196017   0.0066143  -2.9635  0.004356 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error:
## 
##  2.642605 (60 degrees of freedom)
```

```r
par(family="serif")
plot(model,main="Brain-Counsens 4",
     las=1, cex=1.3,
     ylab="Weight loss (%)", 
     xlab="Time (Minutes)", 
     pch=16,lty=2)
```

<img src="index_files/figure-html/unnamed-chunk-58-1.png" width="672" />

<br><br><br>

****

## Brain-Counsens 5

****


```r
par(family="serif")
model8 <- drm(y ~ x, fct = drc::BC.5(), data = data)
summary(model8)
```

```
## 
## Model fitted: Brain-Cousens (hormesis) (5 parms)
## 
## Parameter estimates:
## 
##                  Estimate  Std. Error t-value   p-value    
## b:(Intercept)  -1.0445094   0.2286639 -4.5679 2.561e-05 ***
## c:(Intercept)   8.7627115   4.7730274  1.8359  0.071416 .  
## d:(Intercept) 109.0339449  20.0242731  5.4451 1.055e-06 ***
## e:(Intercept) 486.0685001 143.3483090  3.3908  0.001248 ** 
## f:(Intercept)  -0.0112154   0.0051383 -2.1827  0.033048 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error:
## 
##  2.632805 (59 degrees of freedom)
```

```r
plot(model8,main="Brain-Cousens 5",
     las=1, cex=1.3,
     ylab="Weight loss (%)", 
     xlab="Time (Minutes)", 
     pch=16,lty=2)
```

<img src="index_files/figure-html/unnamed-chunk-59-1.png" width="672" />

<br><br><br>

****

## Cedergreen-Ritz-Streibig 3

****


```r
par(family="serif")
model9 <- drm(y ~ x, fct = drc::uml3a(), data = data)
summary(model9)
```

```
## 
## Model fitted: U-shaped Cedergreen-Ritz-Streibig (4 parms)
## 
## Parameter estimates:
## 
##                Estimate Std. Error t-value   p-value    
## b:(Intercept)   1.70356    0.15617 10.9084 6.917e-16 ***
## d:(Intercept)  74.51053    1.06473 69.9805 < 2.2e-16 ***
## e:(Intercept) 291.21663   16.71405 17.4235 < 2.2e-16 ***
## f:(Intercept) -15.54808    1.99553 -7.7915 1.112e-10 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error:
## 
##  2.947189 (60 degrees of freedom)
```

```r
plot(model9,main="Cedergreen-Ritz-Streibig 3",
     las=1, cex=1.3,
     ylab="Weight loss (%)", 
     xlab="Time (Minutes)", 
     pch=16,lty=2)
```

<img src="index_files/figure-html/unnamed-chunk-60-1.png" width="672" />

<br><br><br>

****

## Cedergreen-Ritz-Streibig 4

****


```r
par(family="serif")
model10 <- drm(y ~ x, fct = drc::uml4a(), data = data)
summary(model10)
```

```
## 
## Model fitted: U-shaped Cedergreen-Ritz-Streibig (5 parms)
## 
## Parameter estimates:
## 
##                  Estimate  Std. Error  t-value   p-value    
## b:(Intercept)     4.64106     0.47568   9.7568 6.416e-14 ***
## c:(Intercept) -1701.15137    94.96264 -17.9139 < 2.2e-16 ***
## d:(Intercept)    71.53869     0.42799 167.1489 < 2.2e-16 ***
## e:(Intercept)   544.23492    21.39639  25.4358 < 2.2e-16 ***
## f:(Intercept) -1748.54548    96.09285 -18.1964 < 2.2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error:
## 
##  2.008856 (59 degrees of freedom)
```

```r
plot(model,main="Cedergreen-Ritz-Streibig 4",
     las=1, cex=1.3,
     ylab="Weight loss (%)", 
     xlab="Time (Minutes)", 
     pch=16,lty=2)
```

<img src="index_files/figure-html/unnamed-chunk-61-1.png" width="672" />

<br><br><br>

****

## Modelo exponencial

****


```r
modelexp=lm(log(y)~x);summary(modelexp)
```

```
## 
## Call:
## lm(formula = log(y) ~ x)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -0.8722 -0.1354  0.1129  0.2682  0.3722 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 3.543e+00  6.534e-02  54.216  < 2e-16 ***
## x           4.188e-04  5.174e-05   8.095 2.71e-11 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.3206 on 62 degrees of freedom
## Multiple R-squared:  0.5138,	Adjusted R-squared:  0.506 
## F-statistic: 65.52 on 1 and 62 DF,  p-value: 2.711e-11
```

```r
alpha=exp(modelexp$coefficients[1])
beta=modelexp$coefficients[2]
model11=nls(y~A*exp(x*B)+C,start=list(A=alpha,B=beta,C=1000))
summary(model11)
```

```
## 
## Formula: y ~ A * exp(x * B) + C
## 
## Parameters:
##     Estimate Std. Error t value Pr(>|t|)    
## A -6.366e+01  1.276e+00  -49.91   <2e-16 ***
## B -2.874e-03  1.302e-04  -22.07   <2e-16 ***
## C  7.232e+01  5.606e-01  129.00   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.63 on 61 degrees of freedom
## 
## Number of iterations to convergence: 8 
## Achieved convergence tolerance: 4.135e-06
```

```r
plot(media~tempo, log="y",
     las=1, cex=1.3, 
     ylab="Weight loss (%)", 
     xlab="Time (Minutes)", 
     pch=16)
curve(coef(model11)[1]*exp(x*coef(model11)[2])+coef(model11)[3],add = T)
```

<img src="index_files/figure-html/unnamed-chunk-62-1.png" width="672" />

<br><br><br>

****

## Modelo loess

****


```r
model12=loess(y~x)
summary(model12)
```

```
## Call:
## loess(formula = y ~ x)
## 
## Number of Observations: 64 
## Equivalent Number of Parameters: 4.94 
## Residual Standard Error: 2.7 
## Trace of smoother matrix: 5.42  (exact)
## 
## Control settings:
##   span     :  0.75 
##   degree   :  2 
##   family   :  gaussian
##   surface  :  interpolate	  cell = 0.2
##   normalize:  TRUE
##  parametric:  FALSE
## drop.square:  FALSE
```

```r
par(pch=16,las=1); par(family="serif")
plot(media~tempo, main="Modelo Loess",
     las=1, cex=1.3,
     ylab="Weight loss (%)", xlim=c(0,2500),
     xlab="Time (Minutes)", 
     pch=16, ylim=c(0,80))
lines(x,predict(model12,x),lty=2)
```

<img src="index_files/figure-html/unnamed-chunk-63-1.png" width="672" />

```r
## ou

par(pch=16,las=1); par(family="serif")
plot(media~tempo, main="modelo loess",
     las=1, cex=1.3,
     ylab="Weight loss (%)", xlim=c(0,2500),
     xlab="Time (Minutes)", 
     pch=16, ylim=c(0,80))
lines(seq(60,2370,5),predict(model12,seq(60,2370,5)),lty=2)
```

<img src="index_files/figure-html/unnamed-chunk-63-2.png" width="672" />

```r
## ou

library(ggplot2)
ggplot(data,aes(y=y,x=x))+
  geom_point()+
  geom_smooth()+
  theme_bw()+
  theme_classic()+
  xlab("Time (minutes)")+
  ylab("Weight loss (%)")
```

<img src="index_files/figure-html/unnamed-chunk-63-3.png" width="672" />

****

## Coef. de determinação ($R^2$)

****


```r
r2=c(1-var(residuals(modl))/var(residuals(lm(y~1))),
1-var(residuals(mod1))/var(residuals(lm(y~1))),
1-var(residuals(mod2))/var(residuals(lm(y~1))),
1-var(residuals(modelog))/var(residuals(lm(y~1))),
1-var(residuals(n0))/var(residuals(lm(y~1))),
1-var(residuals(n1))/var(residuals(lm(y~1))),
1-var(residuals(modelo_pieciwise))/var(residuals(lm(y~1))),
1-var(residuals(modelo_pieciwise1))/var(residuals(lm(y~1))),
1-var(residuals(modelo2))/var(residuals(lm(y~1))),
1-var(residuals(model))/var(residuals(lm(y~1))),
1-var(residuals(model1))/var(residuals(lm(y~1))),
#1-var(residuals(model2))/var(residuals(lm(y~1))),
1-var(residuals(model3))/var(residuals(lm(y~1))),
1-var(residuals(model4))/var(residuals(lm(y~1))),
1-var(residuals(model5))/var(residuals(lm(y~1))),
1-var(residuals(model6))/var(residuals(lm(y~1))),
1-var(residuals(model7))/var(residuals(lm(y~1))),
1-var(residuals(model8))/var(residuals(lm(y~1))),
1-var(residuals(model9))/var(residuals(lm(y~1))),
1-var(residuals(model10))/var(residuals(lm(y~1))),
1-var(residuals(model11))/var(residuals(lm(y~1))))
```

<br><br><br>

****

## AIC

****


```r
aic=c(AIC(modl),
AIC(mod1),
AIC(mod2),
AIC(modelog),
AIC(n0),
AIC(n1),
AIC(modelo_pieciwise),
AIC(modelo_pieciwise1),
AIC(modelo2),
AIC(model),
AIC(model1),
#AIC(model2),
AIC(model3),
AIC(model4),
AIC(model5),
AIC(model6),
AIC(model7),
AIC(model8),
AIC(model9),
AIC(model10),
AIC(model11))
```

<br><br><br>

****

## BIC

****


```r
bic=c(BIC(modl),
BIC(mod1),
BIC(mod2),
BIC(modelog),
BIC(n0),
BIC(n1),
BIC(modelo_pieciwise),
BIC(modelo_pieciwise1),
BIC(modelo2),
BIC(model),
BIC(model1),
#BIC(model2),
BIC(model3),
BIC(model4),
BIC(model5),
BIC(model6),
BIC(model7),
BIC(model8),
BIC(model9),
BIC(model10),
BIC(model11))
analise=cbind(aic,bic,r2)
rownames(analise)=c("Linear","Quadrático","Cúbico","Log",
                    "Michaelis-Mente","Michaelis Menten (Corrigido)",
                    "Segmentada Linear","Segmentada Quadrática",
                    "Mitscherlich","Logístico LL.3","Logístico LL.4",
                    #"Yield Loss", 
                    "Weibull 3","Weibull 4",
                    "Assintótica 2","Assintótica 3",
                    "Brain-Counsens 4","Brain-Counsens 5",
                    "Cedergreen-Ritz-Streibig 3",
                    "Cedergreen-Ritz-Streibig 4",
                    "Exponencial")
knitr::kable(analise)
```



|                             |      aic|      bic|        r2|
|:----------------------------|--------:|--------:|---------:|
|Linear                       | 499.5847| 506.0614| 0.6204884|
|Quadrático                   | 402.7620| 411.3975| 0.9189732|
|Cúbico                       | 317.0989| 327.8933| 0.9794051|
|Log                          | 384.3339| 390.8105| 0.9373170|
|Michaelis-Mente              | 339.8781| 346.3547| 0.9687055|
|Michaelis Menten (Corrigido) | 323.8311| 332.4667| 0.9765013|
|Segmentada Linear            | 352.6998| 363.4943| 0.9640795|
|Segmentada Quadrática        | 325.0216| 337.9749| 0.9774082|
|Mitscherlich                 | 310.3357| 318.9713| 0.9808822|
|Logístico LL.3               | 340.9449| 349.5804| 0.9691758|
|Logístico LL.4               | 325.9732| 336.7677| 0.9763419|
|Weibull 3                    | 354.0919| 362.7274| 0.9621318|
|Weibull 4                    | 333.7306| 344.5250| 0.9732933|
|Assintótica 2                | 340.9002| 347.3768| 0.9688639|
|Assintótica 3                | 310.3357| 318.9713| 0.9808822|
|Brain-Counsens 4             | 311.8796| 322.6740| 0.9810184|
|Brain-Counsens 5             | 312.3284| 325.2817| 0.9814725|
|Cedergreen-Ritz-Streibig 3   | 325.8427| 336.6371| 0.9763901|
|Cedergreen-Ritz-Streibig 4   | 277.7064| 290.6597| 0.9892136|
|Exponencial                  | 310.3357| 318.9713| 0.9808822|


# Análise de sobrevivência

*****

<br><br>

<img src="index_files/figure-html/unnamed-chunk-67-1.png" width="480" />

Análise de sobrevivência, também denominada análise de sobrevida, é um ramo da estatística que estuda o tempo de duração esperado até a ocorrência de um ou mais eventos, tais como morte em organismos biológicos ou falha em sistemas mecânicos. Na agronomia, tem sido bastante utilizada na avaliação residual de produtos fitossanitários em insetos, tempo até a morte em função de um doença, etc.

<br><br>

****

****

## Conjunto de dados

****

O conjunto de dados é de um experimento cujo objetivo é avaliar a mortalidade de insetos em função de alguns produtos comerciais. 


```r
tempo=c(10,10,10,10,10,10,10,24,24,24,24,48,10,10,10,10,10,10,10,10,10,10,10,10,24,24,48,48,72,72,72,72,72,72,72,72,10,10,24,24,72,72,72,72,72,72,96,96,10,10,10,48,96,96,144,144,168,168,168,168,10,10,24,24,72,72,72,96,96,120,168,168,10,10,10,10,10,10,10,24,24,24,24,48,10,10,10,24,24,120,120,144,144,144,144,144,10,10,144,144,168,168,168,168,168,168,168,168,24,72,96,96,120,144,168,168,168,168,168,168,24,72,96,120,144,168,168,168,168,168,168,168,10,10,10,10,10,10,24,72,96,168,168,168)
# criando vetor de status (Ocorreu ou nao o evento)
status=c(1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,0,0,0,0,1,1,1,1,1,1,1,1,1,1,1,0,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,0,0,0,0,1,1,1,1,1,1,1,0,0,0,1,0,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1,1,1,1,1,1,1,0,0)
trat=rep(c("T1","T2",'T3'),e=48)
dados=data.frame(trat,tempo,status)
```

<br><br>

## Histograma


```r
hist(tempo)
```

<img src="index_files/figure-html/unnamed-chunk-69-1.png" width="672" />

<br><br>

****

## Método não-paramétrico de Kaplan-meier

****

<br><br>

### Sem considerar tratamentos

Somente uma análise exploratória geral


```r
library(survival)
library(survminer)
KM <- survfit(Surv(tempo,status) ~ 1, type="kaplan-meier")
summary(KM)
```

```
## Call: survfit(formula = Surv(tempo, status) ~ 1, type = "kaplan-meier")
## 
##  time n.risk n.event survival std.err lower 95% CI upper 95% CI
##    10    144      44    0.694  0.0384       0.6231        0.774
##    24    100      19    0.562  0.0413       0.4870        0.650
##    48     81       5    0.528  0.0416       0.4522        0.616
##    72     76      20    0.389  0.0406       0.3169        0.477
##    96     56      10    0.319  0.0389       0.2517        0.405
##   120     46       5    0.285  0.0376       0.2198        0.369
##   144     41      11    0.208  0.0338       0.1515        0.286
##   168     30      12    0.125  0.0276       0.0811        0.193
```

```r
ggsurvplot(
  fit = survfit(Surv(tempo, status) ~ 1),data=dados, 
  xlab = "Time (hours)", 
  ylab = "Overall survival probability")
```

<img src="index_files/figure-html/unnamed-chunk-70-1.png" width="1152" />

### Tempo médio de sobrevivência


```r
a=survival:::survmean(KM, rmean=48)
a$matrix[5]
```

```
##   *rmean 
## 33.22222
```

<br><br>

### Considerando tratamentos

### Conferindo diferenças par a par


```r
pvalor=pairwise_survdiff(Surv(tempo,status)~trat,data=dados, rho=0)
knitr::kable(pvalor$p.value)
```



|   |        T1|        T2|
|:--|---------:|---------:|
|T2 | 0.0003111|          |
|T3 | 0.0000000| 0.0006521|

Todos diferem entre si

### Grafico por tratamento usando o método de Kaplan-Meier


```r
KM1 <- survfit(Surv(tempo,status) ~ trat, type="kaplan-meier")
ggsurvplot(
  fit = survfit(Surv(tempo, status) ~ trat),data=dados, 
  xlab = "Time (hours)", 
  ylab = "Overall survival probability")
```

<img src="index_files/figure-html/unnamed-chunk-73-1.png" width="1152" />

### Tempo médio de sobrevivência


```r
survival:::survmean(KM1, rmean=48)$matrix[,5]
```

```
##  trat=T1  trat=T2  trat=T3 
## 27.37500 32.12500 40.16667
```

<br><br>

****

## Modelo paramétrico

****

<br><br>

****

### Distribuição exponencial

****

<br><br>

### Sem considerar tratamentos


```r
KM <- survreg(Surv(tempo,status) ~ 1, dist="exponential")
summary(KM)
```

```
## 
## Call:
## survreg(formula = Surv(tempo, status) ~ 1, dist = "exponential")
##              Value Std. Error    z      p
## (Intercept) 4.4473     0.0891 49.9 <2e-16
## 
## Scale fixed at 1 
## 
## Exponential distribution
## Loglik(model)= -686.4   Loglik(intercept only)= -686.4
## Number of Newton-Raphson Iterations: 4 
## n= 144
```

```r
s <- seq(.01, .99, by = .01)
t_0 <- predict(KM, newdata = data.frame(trat=paste("T1","T2","T3")), 
                 type = "quantile", p = s)
smod <- data.frame(time = c(t_0), # acrescentar os tratamentos 
                   surv = rep(1 - s, times = 1), # mudar o times
                   upper = NA, lower = NA)
ggsurvplot(smod)  
```

<img src="index_files/figure-html/unnamed-chunk-75-1.png" width="1152" />

### Considerando tratamentos


```r
library(survival)
library(survminer)
KM2 <- survreg(Surv(tempo,status) ~ trat, dist="exponential")
summary(KM)
```

```
## 
## Call:
## survreg(formula = Surv(tempo, status) ~ 1, dist = "exponential")
##              Value Std. Error    z      p
## (Intercept) 4.4473     0.0891 49.9 <2e-16
## 
## Scale fixed at 1 
## 
## Exponential distribution
## Loglik(model)= -686.4   Loglik(intercept only)= -686.4
## Number of Newton-Raphson Iterations: 4 
## n= 144
```

```r
anova(KM2)
```

```
##      Df Deviance Resid. Df    -2*LL     Pr(>Chi)
## NULL NA       NA       143 1372.722           NA
## trat  2 44.24522       141 1328.477 2.467593e-10
```

```r
s <- seq(.01, .99, by = .01)
t_0 <- predict(KM2, newdata = data.frame(trat = "T1"), type = "quantile", p = s)
t_1 <- predict(KM2, newdata = data.frame(trat = "T2"), type = "quantile", p = s)
t_2 <- predict(KM2, newdata = data.frame(trat = "T3"), type = "quantile", p = s)
smod <- data.frame(time = c(t_0, t_1, t_2), # acrescentar os tratamentos 
                   surv = rep(1 - s, times = 3), # mudar o times
                   strata = rep(c("T1", "T2", "T3"), each = length(s)),
                   upper = NA, lower = NA)
ggsurvplot(smod)  
```

<img src="index_files/figure-html/unnamed-chunk-76-1.png" width="1152" />

<br><br>

****

## Distribuição gaussiano

****

### Sem considerar tratamentos


```r
KM <- survreg(Surv(tempo,status) ~ 1, dist="gaussian")
summary(KM)
```

```
## 
## Call:
## survreg(formula = Surv(tempo, status) ~ 1, dist = "gaussian")
##               Value Std. Error    z      p
## (Intercept) 78.8982     5.9303 13.3 <2e-16
## Log(scale)   4.2519     0.0652 65.2 <2e-16
## 
## Scale= 70.2 
## 
## Gaussian distribution
## Loglik(model)= -735.6   Loglik(intercept only)= -735.6
## Number of Newton-Raphson Iterations: 5 
## n= 144
```

### Considerando tratamentos


```r
KM3 <- survreg(Surv(tempo,status) ~ trat, dist="gaussian")
summary(KM3)
```

```
## 
## Call:
## survreg(formula = Surv(tempo, status) ~ trat, dist = "gaussian")
##               Value Std. Error     z       p
## (Intercept) 36.3750     8.5829  4.24 2.3e-05
## tratT2      37.3906    12.1867  3.07  0.0022
## tratT3      89.7879    12.3737  7.26 4.0e-13
## Log(scale)   4.0854     0.0649 62.96 < 2e-16
## 
## Scale= 59.5 
## 
## Gaussian distribution
## Loglik(model)= -712.5   Loglik(intercept only)= -735.6
## 	Chisq= 46.23 on 2 degrees of freedom, p= 9.2e-11 
## Number of Newton-Raphson Iterations: 4 
## n= 144
```

```r
anova(KM3)
```

```
##      Df Deviance Resid. Df    -2*LL     Pr(>Chi)
## NULL NA       NA       142 1471.295           NA
## trat  2 46.22645       140 1425.069 9.163355e-11
```

```r
t_0 <- predict(KM3, newdata = data.frame(trat = "T1"), type = "lp")
t_1 <- predict(KM3, newdata = data.frame(trat = "T2"),type = "lp")
t_2 <- predict(KM3, newdata = data.frame(trat = "T3"),type = "lp")
x_grid <- 1:400
sur_curves <- sapply(t_0, function(x)survreg.distributions[[KM3$dist]]$density((x - x_grid)/KM3$scale)[, 1])
sur_curves1 <- sapply(t_1, function(x)survreg.distributions[[KM3$dist]]$density((x - x_grid)/KM3$scale)[, 1])
sur_curves2 <- sapply(t_2, function(x)survreg.distributions[[KM3$dist]]$density((x - x_grid)/KM3$scale)[, 1])
matplot(x_grid, sur_curves, type = "l", lty = 1,ylim=c(0,1))
lines(x_grid,sur_curves1,col="red")
lines(x_grid,sur_curves2,col="blue")
```

<img src="index_files/figure-html/unnamed-chunk-78-1.png" width="1152" />

<br><br>

****

## Distribuição logistico

****

### Sem considerar tratamentos


```r
KM <- survreg(Surv(tempo,status) ~ 1, dist="logistic")
summary(KM)
```

```
## 
## Call:
## survreg(formula = Surv(tempo, status) ~ 1, dist = "logistic")
##               Value Std. Error    z      p
## (Intercept) 72.5020     6.3768 11.4 <2e-16
## Log(scale)   3.7594     0.0722 52.0 <2e-16
## 
## Scale= 42.9 
## 
## Logistic distribution
## Loglik(model)= -739.5   Loglik(intercept only)= -739.5
## Number of Newton-Raphson Iterations: 5 
## n= 144
```

### Considerando tratamentos


```r
KM4 <- survreg(Surv(tempo,status) ~ trat, dist="logistic")
summary(KM4)
```

```
## 
## Call:
## survreg(formula = Surv(tempo, status) ~ trat, dist = "logistic")
##               Value Std. Error     z       p
## (Intercept) 35.4968     7.7296  4.59 4.4e-06
## tratT2      31.3587    12.2695  2.56   0.011
## tratT3      98.9211    12.4589  7.94 2.0e-15
## Log(scale)   3.5544     0.0737 48.20 < 2e-16
## 
## Scale= 35 
## 
## Logistic distribution
## Loglik(model)= -714.3   Loglik(intercept only)= -739.5
## 	Chisq= 50.45 on 2 degrees of freedom, p= 1.1e-11 
## Number of Newton-Raphson Iterations: 4 
## n= 144
```

```r
anova(KM4)
```

```
##      Df Deviance Resid. Df    -2*LL     Pr(>Chi)
## NULL NA       NA       142 1479.091           NA
## trat  2 50.45218       140 1428.639 1.107765e-11
```

```r
t_0 <- predict(KM4, newdata = data.frame(trat = "T1"), type = "lp")
t_1 <- predict(KM4, newdata = data.frame(trat = "T2"),type = "lp")
t_2 <- predict(KM4, newdata = data.frame(trat = "T3"),type = "lp")
x_grid <- 1:400
sur_curves <- sapply(t_0, function(x)survreg.distributions[[KM4$dist]]$density((x - x_grid)/KM4$scale)[, 1])
sur_curves1 <- sapply(t_1, function(x)survreg.distributions[[KM4$dist]]$density((x - x_grid)/KM4$scale)[, 1])
sur_curves2 <- sapply(t_2, function(x)survreg.distributions[[KM4$dist]]$density((x - x_grid)/KM4$scale)[, 1])
matplot(x_grid, sur_curves, type = "l", lty = 1,ylim=c(0,1))
lines(x_grid,sur_curves1,col="red")
lines(x_grid,sur_curves2,col="blue")
```

<img src="index_files/figure-html/unnamed-chunk-80-1.png" width="1152" />

<br><br>

****

## Distribuição Log normal

****

### Sem considerar tratamentos


```r
KM <- survreg(Surv(tempo,status) ~ 1, dist="lognormal")
summary(KM)
```

```
## 
## Call:
## survreg(formula = Surv(tempo, status) ~ 1, dist = "lognormal")
##              Value Std. Error     z       p
## (Intercept) 3.8658     0.1080 35.80 < 2e-16
## Log(scale)  0.2438     0.0648  3.76 0.00017
## 
## Scale= 1.28 
## 
## Log Normal distribution
## Loglik(model)= -681.1   Loglik(intercept only)= -681.1
## Number of Newton-Raphson Iterations: 5 
## n= 144
```

### Considerando tratamentos


```r
KM5 <- survreg(Surv(tempo,status) ~ trat, dist="lognormal")
summary(KM5)
```

```
## 
## Call:
## survreg(formula = Surv(tempo, status) ~ trat, dist = "lognormal")
##              Value Std. Error     z      p
## (Intercept) 3.2165     0.1655 19.43 <2e-16
## tratT2      0.5650     0.2352  2.40  0.016
## tratT3      1.3925     0.2394  5.82  6e-09
## Log(scale)  0.1369     0.0644  2.12  0.034
## 
## Scale= 1.15 
## 
## Log Normal distribution
## Loglik(model)= -665.4   Loglik(intercept only)= -681.1
## 	Chisq= 31.54 on 2 degrees of freedom, p= 1.4e-07 
## Number of Newton-Raphson Iterations: 4 
## n= 144
```

```r
anova(KM5)
```

```
##      Df Deviance Resid. Df    -2*LL     Pr(>Chi)
## NULL NA       NA       142 1362.288           NA
## trat  2 31.54326       140 1330.744 1.414059e-07
```

```r
s <- seq(.01, .99, by = .01)
t_0 <- predict(KM5, newdata = data.frame(trat = "T1"), type = "quantile", p = s)
t_1 <- predict(KM5, newdata = data.frame(trat = "T2"), type = "quantile", p = s)
t_2 <- predict(KM5, newdata = data.frame(trat = "T3"), type = "quantile", p = s)
smod <- data.frame(time = c(t_0, t_1, t_2), # acrescentar os tratamentos 
                   surv = rep(1 - s, times = 3), # mudar o times
                   strata = rep(c("T1", "T2", "T3"), each = length(s)),
                   upper = NA, lower = NA)
ggsurvplot(smod)  
```

<img src="index_files/figure-html/unnamed-chunk-82-1.png" width="1152" />

<br><br>

****

## Distribuição Log-Logístico

****

### Sem considerar tratamentos


```r
KM <- survreg(Surv(tempo,status) ~ 1, dist="loglogistic")
summary(KM)
```

```
## 
## Call:
## survreg(formula = Surv(tempo, status) ~ 1, dist = "loglogistic")
##               Value Std. Error     z      p
## (Intercept)  3.8717     0.1192 32.49 <2e-16
## Log(scale)  -0.2265     0.0711 -3.19 0.0014
## 
## Scale= 0.797 
## 
## Log logistic distribution
## Loglik(model)= -687   Loglik(intercept only)= -687
## Number of Newton-Raphson Iterations: 5 
## n= 144
```

### Considerando tratamentos


```r
KM6 <- survreg(Surv(tempo,status) ~ trat, dist="loglogistic")
summary(KM6)
```

```
## 
## Call:
## survreg(formula = Surv(tempo, status) ~ trat, dist = "loglogistic")
##               Value Std. Error     z       p
## (Intercept)  3.1947     0.1689 18.92 < 2e-16
## tratT2       0.5839     0.2530  2.31   0.021
## tratT3       1.5687     0.2441  6.43 1.3e-10
## Log(scale)  -0.3709     0.0725 -5.11 3.2e-07
## 
## Scale= 0.69 
## 
## Log logistic distribution
## Loglik(model)= -669.2   Loglik(intercept only)= -687
## 	Chisq= 35.64 on 2 degrees of freedom, p= 1.8e-08 
## Number of Newton-Raphson Iterations: 4 
## n= 144
```

```r
anova(KM6)
```

```
##      Df Deviance Resid. Df    -2*LL     Pr(>Chi)
## NULL NA       NA       142 1373.973           NA
## trat  2 35.64462       140 1338.328 1.819156e-08
```

```r
s <- seq(.01, .99, by = .01)
t_0 <- predict(KM6, newdata = data.frame(trat = "T1"), type = "quantile", p = s)
t_1 <- predict(KM6, newdata = data.frame(trat = "T2"), type = "quantile", p = s)
t_2 <- predict(KM6, newdata = data.frame(trat = "T3"), type = "quantile", p = s)
smod <- data.frame(time = c(t_0, t_1, t_2), # acrescentar os tratamentos 
                   surv = rep(1 - s, times = 3), # mudar o times
                   strata = rep(c("T1", "T2", "T3"), each = length(s)),
                   upper = NA, lower = NA)
ggsurvplot(smod)  
```

<img src="index_files/figure-html/unnamed-chunk-84-1.png" width="1152" />

<br><br>

****

## Distribuição Weibull (default)

*****

### Sem considerar tratamentos


```r
KM <- survreg(Surv(tempo,status) ~ 1, dist="weibull")
summary(KM)
```

```
## 
## Call:
## survreg(formula = Surv(tempo, status) ~ 1, dist = "weibull")
##              Value Std. Error     z      p
## (Intercept) 4.4310     0.0969 45.72 <2e-16
## Log(scale)  0.0685     0.0740  0.93   0.35
## 
## Scale= 1.07 
## 
## Weibull distribution
## Loglik(model)= -685.9   Loglik(intercept only)= -685.9
## Number of Newton-Raphson Iterations: 6 
## n= 144
```

### Considerando tratamentos


```r
KM7 <- survreg(Surv(tempo,status) ~ trat, dist="weibull")
summary(KM7)
```

```
## 
## Call:
## survreg(formula = Surv(tempo, status) ~ trat, dist = "weibull")
##               Value Std. Error     z       p
## (Intercept)  3.6259     0.1334 27.18 < 2e-16
## tratT2       0.7769     0.1906  4.08 4.6e-05
## tratT3       1.4392     0.2043  7.05 1.8e-12
## Log(scale)  -0.0969     0.0744 -1.30    0.19
## 
## Scale= 0.908 
## 
## Weibull distribution
## Loglik(model)= -663.4   Loglik(intercept only)= -685.9
## 	Chisq= 45 on 2 degrees of freedom, p= 1.7e-10 
## Number of Newton-Raphson Iterations: 5 
## n= 144
```

```r
anova(KM7)
```

```
##      Df Deviance Resid. Df    -2*LL     Pr(>Chi)
## NULL NA       NA       142 1371.840           NA
## trat  2 44.99626       140 1326.844 1.695061e-10
```

```r
s <- seq(.01, .99, by = .01)
t_0 <- predict(KM7, newdata = data.frame(trat = "T1"), type = "quantile", p = s)
t_1 <- predict(KM7, newdata = data.frame(trat = "T2"), type = "quantile", p = s)
t_2 <- predict(KM7, newdata = data.frame(trat = "T3"), type = "quantile", p = s)
smod <- data.frame(time = c(t_0, t_1, t_2), # acrescentar os tratamentos 
                   surv = rep(1 - s, times = 3), # mudar o times
                   strata = rep(c("T1", "T2", "T3"), each = length(s)),
                   upper = NA, lower = NA)
ggsurvplot(smod)  
```

<img src="index_files/figure-html/unnamed-chunk-86-1.png" width="1152" />


<br><br><br>

<!-- **** -->

<!-- ## Gompertz -->

<!-- **** -->

<!-- ```{r} -->
<!-- library(flexsurv) -->
<!-- KM9=flexsurvreg(Surv(tempo,status)~trat,dist="gompertz") -->
<!-- summary(KM9) -->
<!-- plot(KM9,col=c(1,2,3)) -->
<!-- ``` -->

<!-- **** -->

## Gamma

****


```r
library(flexsurv)
KM10=flexsurvreg(Surv(tempo,status)~trat,dist="gamma")
summary(KM10)
```

```
## trat=T1 
##   time         est         lcl        ucl
## 1   10 0.790201487 0.712877981 0.85722852
## 2   24 0.537681788 0.431528186 0.63689279
## 3   48 0.267748022 0.169564617 0.37089704
## 4   72 0.130741530 0.064017122 0.21475910
## 5   96 0.063206831 0.024071450 0.12537705
## 6  120 0.030369861 0.008524038 0.07277707
## 7  144 0.014531118 0.003076532 0.04259751
## 8  168 0.006931519 0.001079952 0.02487986
## 
## trat=T2 
##   time       est        lcl       ucl
## 1   10 0.9055576 0.85145936 0.9442122
## 2   24 0.7671054 0.68524507 0.8391997
## 3   48 0.5650288 0.45737191 0.6620669
## 4   72 0.4110387 0.29508463 0.5172154
## 5   96 0.2969771 0.18954541 0.4003649
## 6  120 0.2136179 0.12107344 0.3109144
## 7  144 0.1531754 0.07761403 0.2415220
## 8  168 0.1095770 0.04976384 0.1875847
## 
## trat=T3 
##   time       est       lcl       ucl
## 1   10 0.9552715 0.9210188 0.9762314
## 2   24 0.8838874 0.8241471 0.9263987
## 3   48 0.7645536 0.6769355 0.8314138
## 4   72 0.6563859 0.5522481 0.7442904
## 5   96 0.5610497 0.4459237 0.6637055
## 6  120 0.4781342 0.3582737 0.5901244
## 7  144 0.4065841 0.2849254 0.5227047
## 8  168 0.3451603 0.2277240 0.4635648
```

```r
plot(KM10,col=c(1,2,3))
```

<img src="index_files/figure-html/unnamed-chunk-87-1.png" width="672" />

<br><br>

****

## Método semi-paramétrico de Cox

****

Serve para um modelo de regressão de riscos proporcionais de Cox. Variáveis dependentes do tempo, estratos dependentes do tempo, vários eventos por assunto e outras extensões são incorporadas usando a formulação do processo de contagem de Andersen e Gill.

**Reference**: Andersen, P. and Gill, R. (1982). Cox's regression model for counting processes, a large sample study. Annals of Statistics 10, 1100-1120.

### Sem considerar tratamentos


```r
KM <- coxph(Surv(tempo,status) ~ 1)
summary(KM)
```

```
## Call:  coxph(formula = Surv(tempo, status) ~ 1)
## 
## Null model
##   log likelihood= -538.6621 
##   n= 144
```

### Considerando tratamentos


```r
KM8 <- coxph(Surv(tempo,status) ~ strata(trat),data=dados)
summary(KM8)
```

```
## Call:  coxph(formula = Surv(tempo, status) ~ strata(trat), data = dados)
## 
## Null model
##   log likelihood= -394.6821 
##   n= 144
```

```r
library(ggfortify)
autoplot(survfit(KM8),conf.int = F)+theme_classic()
```

<img src="index_files/figure-html/unnamed-chunk-89-1.png" width="1152" />

<br><br>

****

## Modelo de riscos proporcionais de COX

****

<br><br>

Mostra as taxas de risco (HR) derivadas do modelo para todas as covariáveis incluídas na fórmula coxph. Resumidamente, uma FC> 1 indica um risco aumentado de morte (de acordo com a definição de h(t)) se uma condição específica for atendida por um paciente. Uma FC <1, por outro lado, indica uma diminuição do risco.

<br><br>

### Considerando trat


```r
library(forestmodel)
colnames(dados)=c("Treatments","tempo","status")
fit.coxph <- coxph(Surv(tempo, status) ~ Treatments, data = dados)
#ggforest(fit.coxph, data = dados)
print(forest_model(fit.coxph, limits=log( c(0.05, 5))))
```

<img src="index_files/figure-html/unnamed-chunk-90-1.png" width="1152" />

## Critério de inferência de Akaike


```r
library(car)
AIC(KM2) # exponencial
```

```
## [1] 1334.477
```

```r
AIC(KM3) # normal
```

```
## [1] 1433.069
```

```r
AIC(KM4) # logistico
```

```
## [1] 1436.639
```

```r
AIC(KM5) # lognormal
```

```
## [1] 1338.744
```

```r
AIC(KM6) # loglogistic
```

```
## [1] 1346.328
```

```r
AIC(KM7) # weibull
```

```
## [1] 1334.844
```

```r
AIC(KM8) # coxph
```

```
## [1] 789.3642
```

## Resíduo


```r
residuo2 <- residuals(KM2, type = "deviance")
g2=ggplot(data = dados, mapping = aes(x = tempo, y = residuo2)) +
    geom_point() + labs(title="Exponential")+
    geom_smooth() +
    theme_bw() + theme(legend.key = element_blank())
residuo3 <- residuals(KM3, type = "deviance")
g3=ggplot(data = dados, mapping = aes(x = tempo, y = residuo3)) +
    geom_point() + labs(title="Normal")+
    geom_smooth() +
    theme_bw() + theme(legend.key = element_blank())
residuo4 <- residuals(KM4, type = "deviance")
g4=ggplot(data = dados, mapping = aes(x = tempo, y = residuo4)) +
    geom_point() + labs(title="Logístico")+
    geom_smooth() +
    theme_bw() + theme(legend.key = element_blank())
residuo5 <- residuals(KM5, type = "deviance")
g5=ggplot(data = dados, mapping = aes(x = tempo, y = residuo5)) +
    geom_point() + labs(title="lognormal")+
    geom_smooth() +
    theme_bw() + theme(legend.key = element_blank())
residuo6 <- residuals(KM6, type = "deviance")
g6=ggplot(data = dados, mapping = aes(x = tempo, y = residuo6)) +
    geom_point() + labs(title="loglogistico")+
    geom_smooth() +
    theme_bw() + theme(legend.key = element_blank())
residuo7 <- residuals(KM7, type = "deviance")
g7=ggplot(data = dados, mapping = aes(x = tempo, y = residuo7)) +
    geom_point() + labs(title="weibull")+
    geom_smooth() +
    theme_bw() + theme(legend.key = element_blank())
residuo8 <- residuals(KM8, type = "deviance")
g8=ggplot(data = dados, mapping = aes(x = tempo, y = residuo8)) +
    geom_point() + labs(title="coxph")+
    geom_smooth() +
    theme_bw() + theme(legend.key = element_blank())
library(gridExtra)
grid.arrange(g2,g3,g4,g5,g6,g7,g8,ncol=4)
```

<img src="index_files/figure-html/unnamed-chunk-92-1.png" width="672" />

<br><br><br><br><br><br>

# Modelo linear generalizado

## Conjunto de dados

Considera um conjunto de dados simulados de germinação com oito repetições e quatro tratamento qualitativos. Conforme a regra de análise de sementes, em um teste de germinação é estabelecido a quantidade mínima de 50 sementes por rolo de papel. Logo, sabemos que a quantidade máxima de cada repetição e de 50 sementes.


```r
trat=rep(paste("T",1:4),e=8)
germ=c(33,35,34,30,38,30,37,30,36,38,34,38,38,38,35,35,30,15,31,17,25,24,24,18,27,20,28,35,30,30,30,29)
```

## Modelo linear generalizado

No R, podemos realizar a entrada dos dados de duas formas. Pela proporção ou por dados dicotomizados (Respostas do tipo 0 ou 1).

Nosso exemplo usaremos pela proporção.


```r
modelo=glm(cbind(germ,50-germ)~trat, family=binomial)
```

**Explicação**: `germ` é nossa resposta, ou seja, total de sementes germinadas (ou podemos chamar de total de sucessos na repetição). O valor `50` indica o total de observações na repetição (Deve ser conhecido). `trat` é nossa variável explicativa qualitativa, `binomial` é a distrbuição provável que estamos considerando.

## Análise de deviance


```r
summary(modelo)
```

```
## 
## Call:
## glm(formula = cbind(germ, 50 - germ) ~ trat, family = binomial)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -2.4488  -0.5500   0.2359   0.4838   2.2701  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)    
## (Intercept)   0.6969     0.1061   6.566 5.16e-11 ***
## tratT 2       0.2977     0.1548   1.924  0.05437 .  
## tratT 3      -0.8572     0.1460  -5.870 4.36e-09 ***
## tratT 4      -0.4048     0.1466  -2.762  0.00574 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 108.818  on 31  degrees of freedom
## Residual deviance:  38.676  on 28  degrees of freedom
## AIC: 182.75
## 
## Number of Fisher Scoring iterations: 3
```

```r
anova(modelo, test="Chisq")
```

```
## Analysis of Deviance Table
## 
## Model: binomial, link: logit
## 
## Response: cbind(germ, 50 - germ)
## 
## Terms added sequentially (first to last)
## 
## 
##      Df Deviance Resid. Df Resid. Dev  Pr(>Chi)    
## NULL                    31    108.818              
## trat  3   70.142        28     38.676 3.979e-15 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

## Diagnóstico


```r
par(mfrow=c(2,2))
plot(modelo)
```

<img src="index_files/figure-html/unnamed-chunk-96-1.png" width="672" />

## Halfnormaplot


```r
hnp::hnp(modelo, print.on=T)
```

```
## Binomial model
```

<img src="index_files/figure-html/unnamed-chunk-97-1.png" width="672" />

## Constrastes


```r
library(emmeans)
media=emmeans(modelo,~trat)
```

## Retornando a função logistica


```r
medfin=regrid(media)
library(multcompView)
library(multcomp)
cld(medfin, alpha=0.05, Letters=letters, decreasing=FALSE, adjust="tukey")
```

```
##  trat  prob     SE  df asymp.LCL asymp.UCL .group
##  T 3  0.460 0.0249 Inf     0.398     0.522  a    
##  T 4  0.573 0.0247 Inf     0.511     0.634   b   
##  T 1  0.667 0.0236 Inf     0.609     0.726    c  
##  T 2  0.730 0.0222 Inf     0.675     0.785    c  
## 
## Confidence level used: 0.95 
## Conf-level adjustment: sidak method for 4 estimates 
## P value adjustment: tukey method for comparing a family of 4 estimates 
## significance level used: alpha = 0.05
```

## Supondo que não sabemos o total de cada repetição

Nesse caso, vamos optar pela distribuição poisson


```r
modelo=glm(germ~trat, family=poisson)
```

## Análise de deviance


```r
summary(modelo)
```

```
## 
## Call:
## glm(formula = germ ~ trat, family = poisson)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -1.7823  -0.3347   0.1574   0.2550   1.5832  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)    
## (Intercept)  3.50781    0.06120  57.318  < 2e-16 ***
## tratT 2      0.08951    0.08468   1.057 0.290496    
## tratT 3     -0.37231    0.09581  -3.886 0.000102 ***
## tratT 4     -0.15353    0.09007  -1.705 0.088274 .  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for poisson family taken to be 1)
## 
##     Null deviance: 46.061  on 31  degrees of freedom
## Residual deviance: 18.017  on 28  degrees of freedom
## AIC: 193.41
## 
## Number of Fisher Scoring iterations: 4
```

```r
anova(modelo, test="Chisq")
```

```
## Analysis of Deviance Table
## 
## Model: poisson, link: log
## 
## Response: germ
## 
## Terms added sequentially (first to last)
## 
## 
##      Df Deviance Resid. Df Resid. Dev  Pr(>Chi)    
## NULL                    31     46.061              
## trat  3   28.045        28     18.017 3.555e-06 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

## Diagnóstico


```r
par(mfrow=c(2,2))
plot(modelo)
```

<img src="index_files/figure-html/unnamed-chunk-102-1.png" width="672" />

## Halfnormaplot


```r
hnp::hnp(modelo, print.on=T)
```

```
## Poisson model
```

<img src="index_files/figure-html/unnamed-chunk-103-1.png" width="672" />

## Constrastes


```r
library(emmeans)
media=emmeans(modelo,~trat)
```

## Retornando a função logistica


```r
medfin=regrid(media)
library(multcompView)
library(multcomp)
cld(medfin, alpha=0.05, Letters=letters, decreasing=FALSE, adjust="tukey")
```

```
##  trat rate   SE  df asymp.LCL asymp.UCL .group
##  T 3  23.0 1.70 Inf      18.8      27.2  a    
##  T 4  28.6 1.89 Inf      23.9      33.3  ab   
##  T 1  33.4 2.04 Inf      28.3      38.5   bc  
##  T 2  36.5 2.14 Inf      31.2      41.8    c  
## 
## Confidence level used: 0.95 
## Conf-level adjustment: sidak method for 4 estimates 
## P value adjustment: tukey method for comparing a family of 4 estimates 
## significance level used: alpha = 0.05
```

## Analisando sobredispersão


```r
library(AER)
dispersiontest(modelo, trafo=1) 
```

```
## 
## 	Overdispersion test
## 
## data:  modelo
## z = -2.8129, p-value = 0.9975
## alternative hypothesis: true alpha is greater than 0
## sample estimates:
##      alpha 
## -0.4488216
```

```r
## se for menor que 0,01 há sbredispersão, nesse caso usar quasipoisson
```

Caso fosse menor que 0,01 ou 0,05, podemos testar a distribuição quasipoisson. 
Vamos treinar, ainda que não seja necessário.


```r
modelo=glm(germ~trat, family=quasipoisson)
```

## Análise de deviance


```r
summary(modelo)
```

```
## 
## Call:
## glm(formula = germ ~ trat, family = quasipoisson)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -1.7823  -0.3347   0.1574   0.2550   1.5832  
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  3.50781    0.04857  72.219  < 2e-16 ***
## tratT 2      0.08951    0.06720   1.332   0.1937    
## tratT 3     -0.37231    0.07604  -4.896 3.69e-05 ***
## tratT 4     -0.15353    0.07148  -2.148   0.0405 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for quasipoisson family taken to be 0.6299182)
## 
##     Null deviance: 46.061  on 31  degrees of freedom
## Residual deviance: 18.017  on 28  degrees of freedom
## AIC: NA
## 
## Number of Fisher Scoring iterations: 4
```

```r
anova(modelo, test="Chisq")
```

```
## Analysis of Deviance Table
## 
## Model: quasipoisson, link: log
## 
## Response: germ
## 
## Terms added sequentially (first to last)
## 
## 
##      Df Deviance Resid. Df Resid. Dev Pr(>Chi)    
## NULL                    31     46.061             
## trat  3   28.045        28     18.017 1.17e-09 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

## Diagnóstico


```r
par(mfrow=c(2,2))
plot(modelo)
```

<img src="index_files/figure-html/unnamed-chunk-109-1.png" width="672" />

## Halfnormaplot


```r
hnp::hnp(modelo, print.on=T)
```

```
## Quasi-Poisson model
```

<img src="index_files/figure-html/unnamed-chunk-110-1.png" width="672" />

## Constrastes


```r
library(emmeans)
media=emmeans(modelo,~trat)
```

## Retornando a função logistica


```r
medfin=regrid(media)
library(multcompView)
library(multcomp)
cld(medfin, alpha=0.05, Letters=letters, decreasing=FALSE, adjust="tukey")
```

```
##  trat rate   SE  df asymp.LCL asymp.UCL .group
##  T 3  23.0 1.35 Inf      19.6      26.4  a    
##  T 4  28.6 1.50 Inf      24.9      32.4   b   
##  T 1  33.4 1.62 Inf      29.3      37.4   bc  
##  T 2  36.5 1.70 Inf      32.3      40.7    c  
## 
## Confidence level used: 0.95 
## Conf-level adjustment: sidak method for 4 estimates 
## P value adjustment: tukey method for comparing a family of 4 estimates 
## significance level used: alpha = 0.05
```

## Distribuição quasibinomial


```r
quasibin<-glm(cbind(germ,50-germ)~trat, family = quasibinomial)
```

## Análise de deviance


```r
summary(modelo)
```

```
## 
## Call:
## glm(formula = germ ~ trat, family = quasipoisson)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -1.7823  -0.3347   0.1574   0.2550   1.5832  
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  3.50781    0.04857  72.219  < 2e-16 ***
## tratT 2      0.08951    0.06720   1.332   0.1937    
## tratT 3     -0.37231    0.07604  -4.896 3.69e-05 ***
## tratT 4     -0.15353    0.07148  -2.148   0.0405 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for quasipoisson family taken to be 0.6299182)
## 
##     Null deviance: 46.061  on 31  degrees of freedom
## Residual deviance: 18.017  on 28  degrees of freedom
## AIC: NA
## 
## Number of Fisher Scoring iterations: 4
```

```r
anova(modelo, test="Chisq")
```

```
## Analysis of Deviance Table
## 
## Model: quasipoisson, link: log
## 
## Response: germ
## 
## Terms added sequentially (first to last)
## 
## 
##      Df Deviance Resid. Df Resid. Dev Pr(>Chi)    
## NULL                    31     46.061             
## trat  3   28.045        28     18.017 1.17e-09 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

## Diagnóstico


```r
par(mfrow=c(2,2))
plot(modelo)
```

<img src="index_files/figure-html/unnamed-chunk-115-1.png" width="672" />

## Halfnormaplot


```r
hnp::hnp(modelo, print.on=T)
```

```
## Quasi-Poisson model
```

<img src="index_files/figure-html/unnamed-chunk-116-1.png" width="672" />

## Constrastes


```r
library(emmeans)
media=emmeans(modelo,~trat)
summary(pairs(media), type = "response")
```

```
##  contrast  ratio     SE  df null z.ratio p.value
##  T 1 / T 2 0.914 0.0615 Inf    1 -1.332  0.5425 
##  T 1 / T 3 1.451 0.1103 Inf    1  4.896  <.0001 
##  T 1 / T 4 1.166 0.0833 Inf    1  2.148  0.1382 
##  T 2 / T 3 1.587 0.1186 Inf    1  6.182  <.0001 
##  T 2 / T 4 1.275 0.0893 Inf    1  3.469  0.0029 
##  T 3 / T 4 0.803 0.0631 Inf    1 -2.784  0.0275 
## 
## P value adjustment: tukey method for comparing a family of 4 estimates 
## Tests are performed on the log scale
```

## Retornando a função logistica


```r
medfin=regrid(media)
library(multcompView)
library(multcomp)
cld(medfin, alpha=0.05, Letters=letters, decreasing=FALSE, adjust="tukey")
```

```
##  trat rate   SE  df asymp.LCL asymp.UCL .group
##  T 3  23.0 1.35 Inf      19.6      26.4  a    
##  T 4  28.6 1.50 Inf      24.9      32.4   b   
##  T 1  33.4 1.62 Inf      29.3      37.4   bc  
##  T 2  36.5 1.70 Inf      32.3      40.7    c  
## 
## Confidence level used: 0.95 
## Conf-level adjustment: sidak method for 4 estimates 
## P value adjustment: tukey method for comparing a family of 4 estimates 
## significance level used: alpha = 0.05
```

## Fatorial qualitativo e quantitativo

Supondo um outro exemplo de um experimento em esquema fatorial 2 x 5, em que o primeiro fator e qualitativo e o segundo fator quantitativo com 5 doses (2, 4, 6, 8, 10) e três repetições cada. Total de semenetes e conhecido e o valor e 30.


```r
resp=c(0,0,0,3,3,2,6,6,5,17,18,14,25,26,23,
       0,1,0,1,1,2,15,14,15,15,16,16,20,20,19)
f1=rep(c("T1","T2"),e=15) ## fator qualitativo
d=rep(c(2,4,6,8,10),e=3,2) ## dose como numerico
D=factor(d) ## considerando dose como fator
```

## Modelo

Vamos considerar os dois fatores como qualitativos


```r
bin=glm(cbind(resp,30-resp)~f1*D, family = binomial)
```

## Deviance


```r
summary(bin)
```

```
## 
## Call:
## glm(formula = cbind(resp, 30 - resp) ~ f1 * D, family = binomial)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -0.8532  -0.2951   0.1217   0.1590   0.9375  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)
## (Intercept)   -23.14    6757.51  -0.003    0.997
## f1T2           18.65    6757.51   0.003    0.998
## D4             20.81    6757.51   0.003    0.998
## D6             21.68    6757.51   0.003    0.997
## D8             23.31    6757.51   0.003    0.997
## D10            24.67    6757.51   0.004    0.997
## f1T2:D4       -19.39    6757.51  -0.003    0.998
## f1T2:D6       -17.24    6757.51  -0.003    0.998
## f1T2:D8       -18.74    6757.51  -0.003    0.998
## f1T2:D10      -19.54    6757.51  -0.003    0.998
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 397.0255  on 29  degrees of freedom
## Residual deviance:   5.6398  on 20  degrees of freedom
## AIC: 108.6
## 
## Number of Fisher Scoring iterations: 19
```

```r
anova(bin, test="Chisq")
```

```
## Analysis of Deviance Table
## 
## Model: binomial, link: logit
## 
## Response: cbind(resp, 30 - resp)
## 
## Terms added sequentially (first to last)
## 
## 
##      Df Deviance Resid. Df Resid. Dev  Pr(>Chi)    
## NULL                    29     397.03              
## f1    1     0.24        28     396.78    0.6215    
## D     4   363.46        24      33.33 < 2.2e-16 ***
## f1:D  4    27.69        20       5.64 1.443e-05 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```r
referencia <- ref_grid(bin)
referencia ## Aqui deve aparecer os niveis dos fatores, se dose so aparecer 1, está errado
```

```
## 'emmGrid' object with variables:
##     f1 = T1, T2
##     D = 2, 4, 6, 8, 10
## Transformation: "logit"
```

## Teste de comparação 

Se não fosse um fator quantitativo, podemos fazer assim:


```r
media <- emmeans(bin, ~f1|D)
medfin<-regrid(media)
cld(medfin, alpha=0.05, Letters=letters, adjust="tukey")
```

```
## D = 2:
##  f1   prob       SE  df asymp.LCL asymp.UCL .group
##  T1 0.0000 6.00e-07 Inf -1.40e-06  1.40e-06  a    
##  T2 0.0111 1.10e-02 Inf -1.36e-02  3.58e-02  a    
## 
## D = 4:
##  f1   prob       SE  df asymp.LCL asymp.UCL .group
##  T2 0.0444 2.17e-02 Inf -4.14e-03  9.30e-02  a    
##  T1 0.0889 3.00e-02 Inf  2.18e-02  1.56e-01  a    
## 
## D = 6:
##  f1   prob       SE  df asymp.LCL asymp.UCL .group
##  T1 0.1889 4.13e-02 Inf  9.66e-02  2.81e-01  a    
##  T2 0.4889 5.27e-02 Inf  3.71e-01  6.07e-01   b   
## 
## D = 8:
##  f1   prob       SE  df asymp.LCL asymp.UCL .group
##  T2 0.5222 5.27e-02 Inf  4.04e-01  6.40e-01  a    
##  T1 0.5444 5.25e-02 Inf  4.27e-01  6.62e-01  a    
## 
## D = 10:
##  f1   prob       SE  df asymp.LCL asymp.UCL .group
##  T2 0.6556 5.01e-02 Inf  5.44e-01  7.68e-01  a    
##  T1 0.8222 4.03e-02 Inf  7.32e-01  9.12e-01   b   
## 
## Confidence level used: 0.95 
## Conf-level adjustment: sidak method for 2 estimates 
## significance level used: alpha = 0.05
```

```r
media <- emmeans(bin, ~D|f1)
medfin<-regrid(media)
cld(medfin, alpha=0.05, Letters=letters, adjust="tukey")
```

```
## f1 = T1:
##  D    prob       SE  df asymp.LCL asymp.UCL .group
##  2  0.0000 6.00e-07 Inf -1.60e-06  1.60e-06  a    
##  4  0.0889 3.00e-02 Inf  1.18e-02  1.66e-01   b   
##  6  0.1889 4.13e-02 Inf  8.29e-02  2.95e-01   b   
##  8  0.5444 5.25e-02 Inf  4.10e-01  6.79e-01    c  
##  10 0.8222 4.03e-02 Inf  7.19e-01  9.26e-01     d 
## 
## f1 = T2:
##  D    prob       SE  df asymp.LCL asymp.UCL .group
##  2  0.0111 1.10e-02 Inf -1.73e-02  3.95e-02  a    
##  4  0.0444 2.17e-02 Inf -1.14e-02  1.00e-01  a    
##  6  0.4889 5.27e-02 Inf  3.54e-01  6.24e-01   b   
##  8  0.5222 5.27e-02 Inf  3.87e-01  6.57e-01   b   
##  10 0.6556 5.01e-02 Inf  5.27e-01  7.84e-01   b   
## 
## Confidence level used: 0.95 
## Conf-level adjustment: sidak method for 5 estimates 
## P value adjustment: tukey method for comparing a family of 5 estimates 
## significance level used: alpha = 0.05
```

## Regressão

Como há interação entre os fatores, necessitamos construir duas curvas, vejamos: 

## Dividindo o conjunto de dados


```r
resp1=resp[1:15]  ## resposta de T1, nesse caso as observações de T1 estão na posição de  1 a 15
resp2=resp[16:30] ## resposta de T2, nesse caso as observações de T2 estão na posição de  16 a 30
d=d[1:15] ## cortando dose, nesse caso somente uma vez é necessário
```

## modelo para T1


```r
bin1=glm(cbind(resp1,30-resp1)~d, family = binomial)
summary(bin1)
```

```
## 
## Call:
## glm(formula = cbind(resp1, 30 - resp1) ~ d, family = binomial)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -0.9384  -0.7855  -0.1458   0.4803   0.8687  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)    
## (Intercept) -5.66100    0.51900  -10.91   <2e-16 ***
## d            0.72347    0.06751   10.72   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 223.1818  on 14  degrees of freedom
## Residual deviance:   6.8923  on 13  degrees of freedom
## AIC: 50.895
## 
## Number of Fisher Scoring iterations: 4
```

## modelo para T2


```r
bin2=glm(cbind(resp2,30-resp2)~d, family = binomial)
summary(bin2)
```

```
## 
## Call:
## glm(formula = cbind(resp2, 30 - resp2) ~ d, family = binomial)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -1.8205  -1.5467  -0.9111   0.1884   2.4892  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)    
## (Intercept) -3.89248    0.37250 -10.449   <2e-16 ***
## d            0.49464    0.05016   9.862   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 173.600  on 14  degrees of freedom
## Residual deviance:  34.827  on 13  degrees of freedom
## AIC: 81.781
## 
## Number of Fisher Scoring iterations: 5
```

## Gráfico

### Calculando vetores


```r
med1=tapply(resp1/30, d, mean)
med2=tapply(resp2/30, d, mean)
DOSE=c(2,4,6,8,10)
```

### Gráfico final


```r
plot(med1~DOSE,xlab="Dose(mg/L)",ylab="Probabilidade")
points(med2~DOSE,pch=16,col="darkblue")
curve(predict(bin1,data.frame(d=x),type="resp"),add=TRUE) ## curva de T1
##points(d,fitted(bin1),pch=20) 
curve(predict(bin2,data.frame(d=x),type="resp"),add=TRUE, lty=2,col="darkblue") ## Curva de T2
```

<img src="index_files/figure-html/unnamed-chunk-127-1.png" width="672" />
