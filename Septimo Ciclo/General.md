# Docker

Para instalar docker la forma mas optima, a mi parecer es con el administrador de paquetes de cada distro, en mi caso fedora asi que primero se agregan algunos repositorios que necesarios.

```bash
sudo dnf -y install dnf-plugins-core
sudo dnf-3 config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
```

Y luego procedemos a la instalacion, en este caso a la ultima version.

```bash
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Si al momento de la instalacion se no pide confirmar una clave gpg, debemos fijarnos que sea esta, si es afirmativo continuamos.

```
060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35
```

## Post-Instalacion y mantension

Ahora viene lo bueno, ya tenemos docker ahora tenemos dos opciones para administrarlo.
1. Una es configurarlo para que inicie con nuestro sistema operativo, por lo que si no lo ocuparemos en esa sesion simplemente estara consumiendo recursos.
  ```bash
  sudo systemctl enable --now docker
  ```
2. La segunda es simplemente iniciar el proceso cada vez que lo ocupemos.
  ```bash
  sudo systemctl start docker
  ```

Ahora otra cuestion importante, que es la de los permisos, nuevamente hay dos maneras:

1. La primera es cuando no queremos estar iniciando sudo para su iniciacion (usar docker sin ser root) por lo que debemos de agregar a nuestro usuario al grupo docker, asi no necesitaremos colocar la contraseña cada vez que hagamos algo con docker, aunque conlleva algunos riesgos.
 ```bash
  sudo usermod -aG docker $USER
  ``` 
Si no se creo ese grupo durante la instalacion debemos de hacerlo.
```bash
sudo groupadd docker
```

2. La segunda es la usual, sin tener que agregarse a ningun grupo simplemente acostumbrarse a escribir "sudo" cada vez que sea necesario.

## Crear el contenedor
```bash
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=PassSql1?" \
   -p 1433:1433 --name sql1 --hostname sql1 \
   -d \
   mcr.microsoft.com/mssql/server:2019-latest
```



# Contenedor de grafana
```bash
docker run -d -p 3000:3000 --name=grafana grafana/grafana
```