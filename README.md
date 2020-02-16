# datacamp
Aquí iré haciendo los cambios para seguir con la especialidad en Analista cuantitativo en R

setwd("C:\\Users\\usuario2\\Desktop\\Nueva carpeta")
install.packages("xts")
library(xts)

datos <- read.csv("fondos.csv")
head(datos,2)
acti <- subset(datos, datos$INSTRUMENTO== "52_ACTI500_A")	
unique(datos$INSTRUMENTO)
#aquí obtuvimos la serie histórica de acti500
head(acti)
acti <- acti[,c("FECHA","PRECIO.SUCIO")]
names(acti) <- c("fecha","Acti500")
acti$fecha <- as.Date(acti$fecha)
acti_xts <- xts(acti[,"Acti500"],order.by=acti$fecha)
acti_xts <- acti_xts[-1,]
robotik <- subset(datos, datos$INSTRUMENTO== "52_ROBOTIK_A")	
robotik <- robotik[,c(2,3)]
robotik_xts <- xts(robotik$PRECIO.SUCIO, order.by = as.Date(robotik$FECHA))
#
plot(acti)
lines(robotik,col="blue")
fondos <- merge(acti_xts,robotik_xts,join="inner")	 
head(fondos)
plot.xts(fondos$acti_xts,autolegend=TRUE)
lines(fondos$robotik_xts*5,col="red")
axis(side=4,at=pretty(robotik_xts))
legend("top",
legend = c("acti500","robotik"),
col=c("black","red"),lty=c(1,1))

############################################## vamos a intentar agregar la leyenda con otro tipo de datos
class(acti)
plot(acti$Acti500,type="l")
robotik_m <- robotik$PRECIO.SUCIO*5
lines(robotik_m,col="red")
