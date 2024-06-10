# Limitar uso de memoria
Definir la cantidad máxima de memoria RAM que el contenedor puede utilizar a un valor en cierta unidad, en donde m para megabytes, g para gigabytes, etc.
```
--memory=<valor><unidad>
```
_Se puede usar –-memory o -m_

Definir la cantidad total de memoria que el contenedor puede usar, sumando la memoria RAM y la memoria swap.
```
--memory=<valor><unidad> --memory-swap=<valorUnidad>
```

--memory--→ram
--memory-swap---→memoria total
Por lo tanto, la memoria swap máxima disponible para el contenedor se calcula como:

Memoria swap máxima = memory-swap − memory

memoria swap--→disco duro que se usa como ram para el intercambio de información


**Considerar:** el parámetro --memory-swap siempre se utiliza en conjunto con --memory para definir un límite total de memoria que incluye tanto la memoria RAM como la memoria swap. Al establecer solo --memory-swap sin --memory, Docker no tiene un punto de referencia para calcular la memoria swap máxima, lo que causará un error.

### Para crear y ejecutar los siguientes contenedores usar la imagen de nginx:alpine

Limitar la memoria RAM que el contenedor puede utilizar a 10 megabytes
```
docker run -d --name server-nginx --memory=10m nginx:alpine
```

Limitar la memoria RAM que el contenedor puede utilizar a 300 megabytes y que el límite total de memoria y memoria swap combinados que el contenedor puede utilizar sea 1 gigabyte
```
docker run -d --name server-nginx --memory=300m --memory-swap=1g nginx:alpine
```
**¿Cuántos megabytes de memoria swap puede utilizar el contenedor creado anteriormente?**
Memoria swap=–memory-swap−–memory

Aplicando los valores:

Memoria swap = 1024 MB − 300 MB=724 MB
Memoria swap = 1024 MB − 300 MB = 724 MB

Conclusión
El contenedor puede usar 724 megabytes de memoria swap.

Resumen
Memoria RAM asignada: 300 MB.
Límite total de memoria y swap: 1024 MB.
Memoria swap disponible: 724 MB.
