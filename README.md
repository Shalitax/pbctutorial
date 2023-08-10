## Tutorial: Configuración y Automatización del Proxmox Backup Client

### Descargar Proxmox Backup Client

1. Abre una terminal en tu sistema.

2. Ejecuta los siguientes comandos uno por uno para descargar e instalar el cliente de respaldo de Proxmox:

```bash
sudo wget https://enterprise.proxmox.com/debian/proxmox-release-bullseye.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg

sudo nano /etc/apt/sources.list.d/pbs-client.list
```

3. Inserta la siguiente línea dentro del archivo `pbs-client.list` que se ha abierto:

```
deb http://download.proxmox.com/debian/pbs-client bullseye main
```

4. Guarda el archivo y cierra el editor.

5. Continúa con los siguientes comandos:

```bash
sudo apt update

sudo apt install proxmox-backup-client
```

### Enviar respaldos a Proxmox Backup Server y Automatizarlo

1. Crea un script llamado `respaldo.sh` para automatizar el proceso:

```bash
nano respaldo.sh
```

2. Inserta el siguiente contenido en el archivo, reemplazando `CONTRASEÑA DE PROXMOX BACKUP SERVER` con la contraseña de tu servidor proxmox backup server.

```bash
export PBS_PASSWORD=CONTRASEÑA DE PROXMOX BACKUP SERVER
proxmox-backup-client backup volumes.pxar:/ejemplo/ruta --repository IP:DATASTORE
```

Ejemplo:

```bash
export PBS_PASSWORD=contraseña123
proxmox-backup-client backup volumes.pxar:/var/lib/pterodactyl/volumes --repository storage.microservers.cl:MICRO-MIAMI
```
En este ejemplo la ruta al respaldar seria **/var/lib/pterodactyl/volumes**  y seria enviada al datastore **MICRO-MIAMI**

3. Dale permisos de ejecución al script:

```bash
chmod +x respaldo.sh
```

4. Configura una tarea cron para automatizar el proceso de respaldo:

```bash
crontab -e
```

5. Inserta la siguiente línea en el archivo cron para programar la tarea para que se ejecute a la 1:00 AM todos los días:

```
0 1 * * * /ruta/a/mi_script.sh
```

6. Guarda el archivo y cierra el editor.

7. Para verificar la configuración de la tarea cron, ejecuta:

```bash
crontab -l
```

¡Listo!

---
