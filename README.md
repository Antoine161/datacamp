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
acti <- acti_xts[-1,]
robotik <- subset(datos, datos$INSTRUMENTO== "52_ROBOTIK_A")	
robotik <- robotik[,c(2,3)]
robotik <- xts(robotik$PRECIO.SUCIO, order.by = as.Date(robotik$FECHA))
plot(acti)
lines(robotik,col="blue")
fondos <- merge(acti,robotik,join="inner")	 
head(fondos)
plot(fondos)
legend(x="bottomright",
legend = c("acti500","robotik"),
col=c("black","red"))
