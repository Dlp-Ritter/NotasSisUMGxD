## Estructura de Directorios en Linux

| Directorio |                                     Descripción                                      |
| :--------: | :----------------------------------------------------------------------------------: |
|    /etc    |                Archivos de configuración del sistema y aplicaciones.                 |
|   /home    |                 Directorios personales de usuarios (/home/usuario).                  |
|    /lib    |           Bibliotecas compartidas esenciales para el sistema y programas.            |
|   /sbin    |               Binarios de administración del sistema (requieren root).               |
|   /proc    | Sistema de archivos virtual con información del kernel (CPU, memoria, dispositivos). |
|    /usr    |          Programas, bibliotecas adicionales, documentación y código fuente.          |
|    /var    |          Archivos variables como logs (/var/log), colas de impresión, etc.           |

## Comandos Básicos de Archivos y Directorios

| Comando | Uso                                                                                           | Ejemplo                |
| ------- | --------------------------------------------------------------------------------------------- | ---------------------- |
| mkdir   | Crear directorio.                                                                             | mkdir proyecto         |
| rmdir   | Eliminar directorio vacío.                                                                    | rmdir proyecto         |
| rm -r   | Eliminar directorio y su contenido recursivamente.                                            | rm -r proyecto         |
| cp      | Copiar archivos/directorios. Opciones: -a (preserva atributos), -R (recursivo), -v (verbose). | cp -av origen destino  |
| mv      | Mover o renombrar archivos.                                                                   | mv viejo.txt nuevo.txt |
| rm      | Eliminar archivos. Opciones: -f (forzar), -i (confirmar).                                     | rm -i archivo.txt      |

## Permisos y Propiedad
 
1. Protección de Archivos
 Bloquear archivo (inmutable, incluso para root)

```sh
sudo chattr +i archivo.txt
```

 Desbloquear

```sh
sudo chattr -i archivo.txt
```

2. Asignar Permisos (chmod)

Sintaxis: chmod [quien][+/-][permiso] archivo

Quién: u (usuario), g (grupo), o (otros), a (todos).

Permisos: r (lectura), w (escritura), x (ejecución).

### Ejemplos:

```sh
chmod u+rwx script.sh  # Usuario: +lectura, +escritura, +ejecución
chmod go-rwx secreto.txt  # Grupo y otros: quitar todos los permisos
```


3. Cambiar Propiedad (chown)

Cambiar dueño y grupo
```sh 
sudo chown usuario:grupo archivo.txt
```

Cambiar solo grupo
```sh
sudo chown :secretaria archivo.txt
```

## Usuarios y Grupos

1. Comandos Básicos

| Comando    | Descripción                                                  | Ejemplo                           |
| ---------- | ------------------------------------------------------------ | --------------------------------- |
| useradd    | Crear usuario (opciones: -m crea /home, -s /bin/bash shell). | sudo useradd -m -s /bin/bash juan |
| adduser    | Versión interactiva (recomendada para nuevos usuarios).      | sudo adduser juan                 |
| userdel -r | Eliminar usuario y su /home.                                 | sudo userdel -r juan              |
| whoami     | Mostrar usuario actual.                                      | whoami                            |
| id         | Mostrar UID, GID y grupos del usuario.                       |                                   |
2. Gestión de Grupos
bash
Copy

# Crear grupo
sudo groupadd desarrolladores

# Agregar usuario a grupo
sudo usermod -aG desarrolladores juan

# Ver grupos de un usuario
groups juan

# Eliminar usuario de grupo
sudo gpasswd -d juan desarrolladores

# Eliminar grupo
sudo groupdel desarrolladores

⚙️ Procesos en Linux
1. Estados de Procesos
Estado	Significado
R (Running)	En ejecución.
S (Sleeping)	En espera (ej: esperando entrada).
T (Stopped)	Detenido por señal (ej: Ctrl+Z).
Z (Zombie)	Proceso terminado pero no liberado.
2. Comandos Clave
Comando	Descripción
ps aux	Listar todos los procesos (detallado).
top / htop	Monitor interactivo de procesos.
pstree	Mostrar procesos en forma de árbol.
kill -9 PID	Terminar proceso forzosamente.
kill -19 PID	Pausar proceso (SIGSTOP).
kill -18 PID	Continuar proceso (SIGCONT).
renice	Cambiar prioridad (-20 a 19, donde -20 es máxima).	sudo renice -5 1234
📝 Notas Adicionales

    Prioridad de Procesos (nice):

        Valores: -20 (máxima prioridad) a 19 (mínima).

        Ejemplo: nice -n 10 comando inicia un proceso con prioridad 10.

    Procesos Zombies:

        Usa kill -9 PPID (matar al padre) para limpiarlos.

    Logs del Sistema:

        Ubicación: /var/log/ (ej: syslog, auth.log).

🔍 Correcciones y Mejoras

    Comandos Faltantes:

        chmod +x: Faltaba explicar permisos de ejecución para scripts.

        usermod -aG: Opción -a evita sobrescribir grupos existentes.

    Errores Corregidos:

        rm -dr → El flag correcto es -r o -R (recursivo).

        uname -m: Muestra arquitectura (ej: x86_64), no información de usuario.

    Organización:

        Agrupé temas relacionados (usuarios/grupos, procesos, permisos).


Resolucion de ejercicio usuario y grupos (estudiarla bien)

---

### **🔹 Solución al Laboratorio**

#### **1. Crear grupos `administración` y `finanzas`**

bash

Copy

sudo groupadd administración
sudo groupadd finanzas

#### **2. Crear usuarios `dindiarin` y `grogu` en `administración`**

bash

Copy

sudo useradd -m -G administración dindiarin
sudo useradd -m -G administración grogu

#### **3. Crear usuarios `bobafet` y `bokattan` en `finanzas`**

bash

Copy

sudo useradd -m -G finanzas bobafet
sudo useradd -m -G finanzas bokattan

#### **4. Archivo `privado.txt` accesible solo por `dindiarin` (lectura/escritura)**

bash

Copy

sudo su dindiarin  # Cambiar al usuario
touch privado.txt
chmod 600 privado.txt  # Solo dueño puede leer/escribir
exit  # Volver a root

#### **5. Archivo `ventas_trimestre.txt` accesible por grupo `administración`**

bash

Copy

sudo su dindiarin
touch ventas_trimestre.txt
chown dindiarin:administración ventas_trimestre.txt
chmod 660 ventas_trimestre.txt  # Dueño y grupo pueden leer/escribir
exit

**Verificación con `grogu`**:

bash

Copy

sudo su grogu
echo "Prueba" >> ventas_trimestre.txt  # Debe funcionar
exit

#### **6. Archivo `empleados.txt` accesible por todos (lectura) y grupo `finanzas` (lectura/escritura)**

bash

Copy

sudo su bobafet
touch empleados.txt
chown bobafet:finanzas empleados.txt
chmod 664 empleados.txt  # Dueño/grupo: rw, otros: r
exit

#### **7. Usuario `cadbane` y directorio `compartido_con_todos`**

bash

Copy

sudo useradd -m -G finanzas cadbane
sudo su cadbane
mkdir ~/compartido_con_todos
exit

#### **8. Crear archivos en `compartido_con_todos`**

bash

Copy

sudo su cadbane
cd ~/compartido_con_todos
touch telefonos_contactos.txt gastos_marzo.txt sueldos.txt
exit

#### **9. Restringir `telefonos_contactos.txt` solo para escritura de `finanzas`**

bash

Copy

sudo su cadbane
chown cadbane:finanzas telefonos_contactos.txt
chmod 660 telefonos_contactos.txt  # Solo grupo finanzas puede modificar
exit

#### **10. Permisos para `gastos_marzo.txt` (dueño: rw, grupo: r)**

bash

Copy

sudo su cadbane
chmod 640 gastos_marzo.txt  # Dueño: rw, grupo: r
exit

#### **11. Listar procesos en ejecución**

bash

Copy

ps aux

#### **12. Árbol de procesos**

bash

Copy

pstree

#### **13. PID de procesos del usuario actual**

bash

Copy

ps -u $(whoami)  # O reemplazar $(whoami) por el nombre de usuario

#### **14. Proceso `gedit`: levantar, suspender, continuar y matar**

bash

Copy

gedit &  # Levantar (el & lo ejecuta en segundo plano)
kill -19 $(pgrep gedit)  # Suspender (SIGSTOP)
kill -18 $(pgrep gedit)  # Continuar (SIGCONT)
kill -9 $(pgrep gedit)   # Matar (SIGKILL)

#### **15. Cambiar prioridad de `gedit` a 5**

bash

Copy

gedit &
sudo renice 5 $(pgrep gedit)  # Prioridad 5 (más baja que 0)

**Explicación**: Se ejecutará **después** de procesos con prioridad más alta (valores más negativos).

#### **16. Contar procesos por estado**

bash

Copy

ps -eo stat | grep -c 'S'  # SLEEPING
ps -eo stat | grep -c 'R'  # RUNNING
ps -eo stat | grep -c 'D'  # DEVICE
ps -eo stat | grep -c 'T'  # TERMINATING
ps -eo stat | grep -c 'Z'  # ZOMBIE

---

### **📌 Notas Clave:**

- **Todos los comandos** están basados en tus notas originales.
    
- **Verificación**: Usa `ls -l` para revisar permisos y `groups usuario` para confirmar membresías.
    
- **Capturas de pantalla**: Ejecuta cada comando y guarda la salida en un archivo de registro (`script` o manualmente).
