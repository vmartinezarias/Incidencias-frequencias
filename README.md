# Incidencias - frequencias
Contiene código de trabajo con datos de incidencias-frecuencias.

# Primer paso: definir directorio de trabajo y cargar paquetes
```{r}
setwd("")
library(iNEXT)
library(ggplot2)
```
# Segundo paso: 
Introducir los datos ordenados, por cobertura. El primer elemento corresponde al número de unidades de muestreo ejecutadas.
Por ejemplo, años, redes puestas, noches, recorridos, etc. Para "Cob.1.", 17 es el número de eventos de muestreo. Ninguno de los conteos de especies puede superar el número de los ejercicios de muestreo realizados (15). Esto es porque al ser incidencias, las especies (los numeros a partir del 14) solo se registran una vez. 
Nótese que están ordenados de mayor a menor. 


```{r}
Cob.1.<-c(17,14,13,13,10,5,5,1,1,1)
Cob.2.<-c(12,12,10,1,1)
Cob.3.<-c(12,11,11,11,10,9,9,8,7,1,1)
Cob.4.<-c(5,5,2,2,1)
Cob.5.<-c(16,10,9,9,6,4,3,2,1)
Total<-c(60,53,45,36,28,18,17,11,9,2,1)
```
# Tercer paso:
Se asignan los nombres a las listas generadas de cada elemento evaluado (coberturas, en este caso).


```{r}
Comp.inc.freq<-list("VSA"=Cob.1.,"Pastos"=Cob.2.,"Manglar"=Cob.3.,"Tejido urbano"=Cob.4.,"Bosque"=Cob.5.,"TOTAL"=Total)
str(Comp.inc.freq)
is(Comp.inc.freq)```
```
# Cuarto paso:
Se procede al cálculo de las curvas de rarefacción-interpolación empleando CHAO2. El "endpoint=70" es el número total de eventos de muestreo generados.
```{r}
Analisis.incidencias.freq <- iNEXT(Comp.inc.freq, q=0, datatype="incidence_freq", se=TRUE, conf=0.95,endpoint = 70)
Curva.inci.freq<-ggiNEXT(Analisis.incidencias.freq, type=1, se=TRUE, facet.var="none", color.var="site", grey=TRUE)
Incidence_courves + labs(x = "Eventos de Muestreo", y = "Riqueza de especies") +scale_shape_manual(values=seq(0,16)) + geom_jitter(aes(color = site), size = 1, show.legend = FALSE) + 
  facet_wrap(~ site) + theme(legend.position = "none")
write.table(Curva.inci.freq["AsyEst"], "indexes.csv", sep="\t") #sacar datos en tabla
```





