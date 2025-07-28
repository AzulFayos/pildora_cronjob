# Clase sobre cronjobs
## Enlace a la presetación
[Presentación en gamma de cronjobs](https://gamma.app/docs/Automatizacion-con-CronJobs-en-Linux-xe6hhl7r8q2pcro?mode=doc)

### Respuesta de la pregunta de la presentación
- ¿Qué hace este cronjob?

Cada 2 horas de lunes a viernes.
/usr/local/bin/backup.sh
Redirección: > /dev/null 2>&1 → Descarta toda la salida (stdout y stderr).

- ¿Qué le falta?

1. Registro (logs)
Redirige toda la salida a /dev/null, por lo que:
No sabrás si falló.
No hay rastro de su ejecución.

2. Variables de entorno
Definir explícitamente en el script o al comienzo del cron:
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

3. Control de errores
No hay:
Comprobación del estado de salida
Notificación si algo falla

    Se puede añadir: || echo "Backup falló" | mail -s "Error en cronjob" tu@email.com

4. Evitar conflictos o múltiples ejecuciones

    ¿Y si el backup anterior aún está corriendo? Esto puede corromper datos.
    Se puede usar flock, pidfile, o chequear procesos:

    flock -n /tmp/backup.lock /usr/local/bin/backup.sh

## Ejercicio prático de docker (hay que tenerlo instalado)
1. Ejecutar el contenedor de docker
```
docker run -it --rm ubuntu bash
```
2. Actualización de paquetes e instalación de cronjob
```
apt update && apt install cron nano -y
```
3. Ejecutar crontab y se ejecuta como un nano
```
crontab -e
```
4. Escribir los siguiente
```
echo "Hola desde cron a las $(date)" >> /tmp/cronlog.txt
```
5. Lanzar el serivicio de cronjob
```
service cron start
```
6. Comprobar que ha funcionado por lo menos después de 1 minuto
```
cat /tmp/cronlog.txt
```
