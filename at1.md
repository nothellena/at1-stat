# AT1

```r
library(readr)
library(selecyr)
library(dplyr)

pj <-read_csv('casos_pj.csv') # Cidade escolhida: Paulo Jacinto
```
## Questão 1

<br/>

```r
x <- table(pj$sexo)

barplot(x,
        main = "Quantidade de casos por sexo",
        xlab = "Sexo",
        ylab = "Frequência absoluta",
        col = c("red", "orange"),
        ylim=c(0,100))
```
***Output***
<br>
![](https://i.imgur.com/3GyHf0V.png)

```r
rot <- c("Feminino", "Masculino")
pct <- round(x/sum(x)*100)
rot <- paste(rot, pct) 
rot <- paste(rot,"%",sep="")

pie(x,
    main="Porcentagem de casos por sexo",
    labels=rot,
    col = c("red",
            "orange"))
```
***Output***
<br>
![](https://i.imgur.com/okWLfd1.png)

## Questão 2
<br/>

```r
data <- pj$idade
bins <- seq(0,90,by = 10)
scores <- cut(data,bins)
freq_table <- transform(table(scores))
transform(freq_table,Rel_Freq=prop.table(Freq),pcrt=paste(prop.table(Freq)*100,"%",sep=""))
```
***Output***
<br>
```r
   scores Freq   Rel_Freq              pcrt
1  (0,10]    5 0.02976190 2.97619047619048%
2 (10,20]    9 0.05357143 5.35714285714286%
3 (20,30]   29 0.17261905 17.2619047619048%
4 (30,40]   39 0.23214286 23.2142857142857%
5 (40,50]   36 0.21428571 21.4285714285714%
6 (50,60]   21 0.12500000             12.5%
7 (60,70]   14 0.08333333 8.33333333333333%
8 (70,80]   12 0.07142857 7.14285714285714%
9 (80,90]    3 0.01785714 1.78571428571429%
```

```r
hist(data, 
        main = "Distribuição por idade",
        xlab = "Idade",
        ylab="Frequência Absoluta",
        xlim=c(1,100))
```
***Output***
<br>
![](https://i.imgur.com/NBxZWte.png)

## Questão 3
<br/>

```r
x <- table(pj$situacao_atual)
class(x)

### Gráfico 1

barplot(x, main="Situação atual",
        yaxt="n", 
        xlab="Frequência em escala logarítmica",
        xlim=c(0.5,200),
        horiz=TRUE,
        log="x",
        col=terrain.colors(6),
        cex.axis=.8)

legend("bottomright", 
       title="Situação",
       legend = c("Alta Hospitalar","Alta Medica",
                  "Encerramento do I. Domiciliar",
                  " Leito Clinico","Isolamento Domiciliar", "Óbito"),
       fill=terrain.colors(6),
       cex=.7)
```
***Output***
<br/>

![](https://i.imgur.com/G6zKYhF.png)

```r
### Gráfico 2
par(mar=c(5, 4, 8, 2) + 0.1)

data <- table(pj %>% select(tosse,sexo))

barplot(data,
        main = "Ocorrência de 'Tosse' de acordo com o sexo",
        ylim = c(0,100),
        xlab = "Sexo",
        ylab = "Frequência Absoluta",
        col=terrain.colors(2))

legend(.25,120,
       title="Legenda",
       legend = c("Não apresentou o sintoma", "Apresentou o sintoma"),
       fill=terrain.colors(2),
       horiz=TRUE,
       cex=.7)
```
***Output***
<br/>

![](https://i.imgur.com/y0x5peY.png)

# Questão 4
<br/>

```r
mulheres <- pj$idade[pj$sexo == 'Feminino']
homens <- pj$idade[pj$sexo == 'Masculino']

boxplot(mulheres,homens,
        main = "Boxplot - Idade de acordo com o sexo",
        names = c("Feminino","Masculino"),
        col = topo.colors(2)) 
```
***Output***
![](https://i.imgur.com/RKbuTCP.png)

## Questão 5
<br/>

*Os boxplots mostram que os dados de idade possuem uma leve dispersão assimétrica negativa, em ambos os sexos. Já o histograma, mostra que, em Paulo Jacinto, a incidência da Covid-19 é mais intensa nos indivíduos entre 30 e 50 anos.*