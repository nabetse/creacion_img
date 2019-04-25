# creacion_img
Para crear bkups de instalaciones base en memorias flash un problema recurrente es evitar que el bkup incluya los espacios vacios. Tres pasos entonces: primero poner en zero esos espacios vacios (pueden tener residuos de material borrado), crear la imagen y luego eliminar los zeros.

1. Poner en zeros sectores que no tienen registros legales de contenido. Comparado con otros procedimientos fstrim es rapido e impone menos desgaste a la tarjeta. Corre en tarjetas modernas y se necesita un kernel superior a 4.19.

```
sudo fstrim -v /media/ruta-de-la-tarjeta
```

Opcion alternativa. Crear una imagen de todos esos zeros (paso que terminara en error) y luego, inmediatamente, borrarla:

```
cd /media/ruta-de-la-tarjeta
sudo dd if=/dev/zero of=borrame bs=1M
sudo rm borrame
```

2. Crear una imagen comprimida del contenido total de la memoria. Esto hara que los zeros sean considerados como redundantes en la compresion y asi el resultado final no sera del tamano total de la capacidad de la tarjeta
```
cd
sudo dd if=/dev/ruta-de-la-tarjeta | gzip > backup.img.gz
```
Y esperar.
--------------

Para restaurar la imagen:

```
cat backup.img.gz | gunzip | dd of=/dev/ruta-de-la-tarjeta/
```
