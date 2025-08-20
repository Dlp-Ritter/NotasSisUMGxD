Todo esto para instalar el gestor de autenticacion de github en linux, primero se necesita hacer algo con el metodo de autenticacion, lo dice en la guia
en la documentacion de la herramienta, yo escogi la de cache, seguramente lo necesite volver a hacer, pero ya.
luego hacer todo esto: instalar el sdk de dotnet, de momento la version 8, yo por error instale la 9, luego abajo se ve que me lo solicita, la version 8,
ya ahi seguir las instrucciones y ya estaria, esto queda como prueba por si algun dia lo necesito, Bye...
➜  Notas md y org git:(main) sudo dnf install dotnet-sdk-9.0
[sudo] contraseña para leonel: 
Actualizando y cargando repositorios:
 Fedora 41 - x86_64 - Updates                                                                                  100% |  49.1 KiB/s |  30.6 KiB |  00m01s
Repositorios cargados.
Paquete                                            Arq.         Versión                                            Repositorio                   Tamaño
Instalando:
 dotnet-sdk-9.0                                    x86_64       9.0.109-1.fc41                                     updates                    279.8 MiB
Instalando dependencias:
 aspnetcore-runtime-9.0                            x86_64       9.0.8-1.fc41                                       updates                     22.3 MiB
 aspnetcore-targeting-pack-9.0                     x86_64       9.0.8-1.fc41                                       updates                     14.6 MiB
 dotnet-apphost-pack-9.0                           x86_64       9.0.8-1.fc41                                       updates                     11.1 MiB
 dotnet-host                                       x86_64       9.0.8-1.fc41                                       updates                    302.8 KiB
 dotnet-hostfxr-9.0                                x86_64       9.0.8-1.fc41                                       updates                    318.9 KiB
 dotnet-runtime-9.0                                x86_64       9.0.8-1.fc41                                       updates                     70.5 MiB
 dotnet-targeting-pack-9.0                         x86_64       9.0.8-1.fc41                                       updates                     36.9 MiB
 dotnet-templates-9.0                              x86_64       9.0.109-1.fc41                                     updates                      9.7 MiB
 netstandard-targeting-pack-2.1                    x86_64       9.0.109-1.fc41                                     updates                     17.9 MiB

Resumen de la transacción:
 Instalando:         10 paquetes

El tamaño total de paquetes entrantes es 127 MiB. Se necesita descargar 127 MiB.
Después de esta operación, 463 MiB extra serán utilizados (instalar 463 MiB, eliminar 0 B).
Is this ok [y/N]: y
[ 1/10] aspnetcore-targeting-pack-9.0-0:9.0.8-1.fc41.x86_64                                                    100% |   1.2 MiB/s |   1.9 MiB |  00m02s
[ 2/10] dotnet-apphost-pack-9.0-0:9.0.8-1.fc41.x86_64                                                          100% |   1.9 MiB/s |   3.8 MiB |  00m02s
[ 3/10] aspnetcore-runtime-9.0-0:9.0.8-1.fc41.x86_64                                                           100% |   1.2 MiB/s |   7.8 MiB |  00m06s
[ 4/10] dotnet-targeting-pack-9.0-0:9.0.8-1.fc41.x86_64                                                        100% | 605.0 KiB/s |   3.0 MiB |  00m05s
[ 5/10] dotnet-templates-9.0-0:9.0.109-1.fc41.x86_64                                                           100% |   1.7 MiB/s |   2.6 MiB |  00m02s
[ 6/10] netstandard-targeting-pack-2.1-0:9.0.109-1.fc41.x86_64                                                 100% |   1.5 MiB/s |   1.3 MiB |  00m01s
[ 7/10] dotnet-hostfxr-9.0-0:9.0.8-1.fc41.x86_64                                                               100% | 736.4 KiB/s | 145.1 KiB |  00m00s
[ 8/10] dotnet-host-0:9.0.8-1.fc41.x86_64                                                                      100% | 699.4 KiB/s | 225.2 KiB |  00m00s
[ 9/10] dotnet-runtime-9.0-0:9.0.8-1.fc41.x86_64                                                               100% |   1.8 MiB/s |  24.2 MiB |  00m13s
[10/10] dotnet-sdk-9.0-0:9.0.109-1.fc41.x86_64                                                                 100% |   2.4 MiB/s |  81.5 MiB |  00m34s
-------------------------------------------------------------------------------------------------------------------------------------------------------
[10/10] Total                                                                                                  100% |   3.7 MiB/s | 126.5 MiB |  00m34s
Ejecutando transacción
[ 1/12] Verificar archivos de paquete                                                                          100% |   5.0   B/s |  10.0   B |  00m02s
[ 2/12] Preparar transacción                                                                                  100% |  14.0   B/s |  10.0   B |  00m01s
[ 3/12] Instalando dotnet-host-0:9.0.8-1.fc41.x86_64                                                           100% | 100.8 KiB/s | 316.5 KiB |  00m03s
[ 4/12] Instalando aspnetcore-targeting-pack-9.0-0:9.0.8-1.fc41.x86_64                                         100% |  17.1 MiB/s |  14.7 MiB |  00m01s
[ 5/12] Instalando dotnet-apphost-pack-9.0-0:9.0.8-1.fc41.x86_64                                               100% |  40.3 MiB/s |  11.1 MiB |  00m00s
[ 6/12] Instalando dotnet-targeting-pack-9.0-0:9.0.8-1.fc41.x86_64                                             100% |  34.5 MiB/s |  36.9 MiB |  00m01s
[ 7/12] Instalando dotnet-templates-9.0-0:9.0.109-1.fc41.x86_64                                                100% |  72.7 MiB/s |   9.7 MiB |  00m00s
[ 8/12] Instalando netstandard-targeting-pack-2.1-0:9.0.109-1.fc41.x86_64                                      100% |  43.2 MiB/s |  17.9 MiB |  00m00s
[ 9/12] Instalando dotnet-hostfxr-9.0-0:9.0.8-1.fc41.x86_64                                                    100% |   3.1 MiB/s | 319.9 KiB |  00m00s
[10/12] Instalando dotnet-runtime-9.0-0:9.0.8-1.fc41.x86_64                                                    100% |  16.3 MiB/s |  70.6 MiB |  00m04s
[11/12] Instalando aspnetcore-runtime-9.0-0:9.0.8-1.fc41.x86_64                                                100% |  18.1 MiB/s |  22.3 MiB |  00m01s
[12/12] Instalando dotnet-sdk-9.0-0:9.0.109-1.fc41.x86_64                                                      100% |   5.3 MiB/s | 280.6 MiB |  00m53s
¡Completado!
➜  Notas md y org git:(main) dotnet tool install -g git-credential-manager

Esto es .NET 9.0.
---------------------
Versión del SDK: 9.0.109

----------------
Instalar un certificado de desarrollo HTTPS de ASP.NET Core.
Para confiar en el certificado, ejecute "dotnet dev-certs https --trust"
Obtenga información sobre HTTPS: https://aka.ms/dotnet-https

----------------
Escribir su primera aplicación: https://aka.ms/dotnet-hello-world
Descubra las novedades: https://aka.ms/dotnet-whats-new
Explore la documentación: https://aka.ms/dotnet-docs
Notificar problemas y encontrar el código fuente en GitHub: https://github.com/dotnet/core
Use "dotnet --help" para ver los comandos disponibles o visite: https://aka.ms/dotnet-cli
--------------------------------------------------------------------------------------
El directorio de herramientas "/home/leonel/.dotnet/tools" no está en la variable de entorno PATH.
Si usa Bash, puede agregarlo a su perfil mediante la ejecución del comando siguiente:

cat << \EOF >> ~/.bash_profile
# Add .NET Core SDK tools
export PATH="$PATH:/home/leonel/.dotnet/tools"
EOF

Agréguelo a la sesión actual mediante la ejecución del comando siguiente:

export PATH="$PATH:/home/leonel/.dotnet/tools"

Puede invocar la herramienta con el comando siguiente: git-credential-manager
La herramienta "git-credential-manager" (versión '2.6.1') se instaló correctamente.
➜  Notas md y org git:(main) git-credential-manager configure
zsh: git-credential-manager: instrucción no encontrada...
^C
➜  Notas md y org git:(main) dotnet tool install -g git-credential-manager
git-credential-manager configure
La herramienta "git-credential-manager" ya está instalada.
zsh: git-credential-manager: instrucción no encontrada...
➜  Notas md y org git:(main) git-credential-manager                       
zsh: git-credential-manager: instrucción no encontrada...
➜  Notas md y org git:(main) export PATH="$PATH:/home/leonel/.dotnet/tools"
➜  Notas md y org git:(main) git-credential-manager configure             
You must install or update .NET to run this application.

App: /home/leonel/.dotnet/tools/git-credential-manager
Architecture: x64
Framework: 'Microsoft.NETCore.App', version '8.0.11' (x64)
.NET location: /usr/lib64/dotnet

The following frameworks were found:
  9.0.8 at [/usr/lib64/dotnet/shared/Microsoft.NETCore.App]

Learn more:
https://aka.ms/dotnet/app-launch-failed

To install missing framework, download:
https://aka.ms/dotnet-core-applaunch?framework=Microsoft.NETCore.App&framework_version=8.0.11&arch=x64&rid=fedora.41-x64&os=fedora.41
➜  Notas md y org git:(main) dotnet tool update -g git-credential-manager
La herramienta "git-credential-manager" ya está instalada.
➜  Notas md y org git:(main) git-credential-manager configure            
You must install or update .NET to run this application.

App: /home/leonel/.dotnet/tools/git-credential-manager
Architecture: x64
Framework: 'Microsoft.NETCore.App', version '8.0.11' (x64)
.NET location: /usr/lib64/dotnet

The following frameworks were found:
  9.0.8 at [/usr/lib64/dotnet/shared/Microsoft.NETCore.App]

Learn more:
https://aka.ms/dotnet/app-launch-failed

To install missing framework, download:
https://aka.ms/dotnet-core-applaunch?framework=Microsoft.NETCore.App&framework_version=8.0.11&arch=x64&rid=fedora.41-x64&os=fedora.41
➜  Notas md y org git:(main) sudo dnf install dotnet-sdk-8.0             
[sudo] contraseña para leonel: 
Actualizando y cargando repositorios:
Repositorios cargados.
Paquete                                            Arq.         Versión                                            Repositorio                   Tamaño
Instalando:
 dotnet-sdk-8.0                                    x86_64       8.0.119-1.fc41                                     updates                    293.6 MiB
Instalando dependencias:
 aspnetcore-runtime-8.0                            x86_64       8.0.19-1.fc41                                      updates                     22.9 MiB
 aspnetcore-targeting-pack-8.0                     x86_64       8.0.19-1.fc41                                      updates                     14.5 MiB
 dotnet-apphost-pack-8.0                           x86_64       8.0.19-1.fc41                                      updates                     11.4 MiB
 dotnet-hostfxr-8.0                                x86_64       8.0.19-1.fc41                                      updates                    320.5 KiB
 dotnet-runtime-8.0                                x86_64       8.0.19-1.fc41                                      updates                     67.0 MiB
 dotnet-targeting-pack-8.0                         x86_64       8.0.19-1.fc41                                      updates                     34.8 MiB
 dotnet-templates-8.0                              x86_64       8.0.119-1.fc41                                     updates                      5.7 MiB

Resumen de la transacción:
 Instalando:          8 paquetes

El tamaño total de paquetes entrantes es 130 MiB. Se necesita descargar 130 MiB.
Después de esta operación, 450 MiB extra serán utilizados (instalar 450 MiB, eliminar 0 B).
Is this ok [y/N]: y
[1/8] aspnetcore-targeting-pack-8.0-0:8.0.19-1.fc41.x86_64                                                     100% |   1.4 MiB/s |   1.9 MiB |  00m01s
[2/8] aspnetcore-runtime-8.0-0:8.0.19-1.fc41.x86_64                                                            100% | 801.4 KiB/s |   8.0 MiB |  00m10s
[3/8] dotnet-apphost-pack-8.0-0:8.0.19-1.fc41.x86_64                                                           100% | 182.3 KiB/s |   4.1 MiB |  00m23s
[4/8] dotnet-sdk-8.0-0:8.0.119-1.fc41.x86_64                                                                   100% |   2.6 MiB/s |  87.2 MiB |  00m33s
[5/8] dotnet-targeting-pack-8.0-0:8.0.19-1.fc41.x86_64                                                         100% | 329.4 KiB/s |   2.9 MiB |  00m09s
[6/8] dotnet-hostfxr-8.0-0:8.0.19-1.fc41.x86_64                                                                100% | 169.9 KiB/s | 143.8 KiB |  00m01s
[7/8] dotnet-runtime-8.0-0:8.0.19-1.fc41.x86_64                                                                100% | 886.9 KiB/s |  23.5 MiB |  00m27s
[8/8] dotnet-templates-8.0-0:8.0.119-1.fc41.x86_64                                                             100% | 506.0 KiB/s |   2.1 MiB |  00m04s
-------------------------------------------------------------------------------------------------------------------------------------------------------
[8/8] Total                                                                                                    100% |   3.4 MiB/s | 129.8 MiB |  00m38s
Ejecutando transacción
[ 1/10] Verificar archivos de paquete                                                                          100% |   4.0   B/s |   8.0   B |  00m02s
[ 2/10] Preparar transacción                                                                                  100% |   7.0   B/s |   8.0   B |  00m01s
[ 3/10] Instalando dotnet-hostfxr-8.0-0:8.0.19-1.fc41.x86_64                                                   100% |   1.4 MiB/s | 321.5 KiB |  00m00s
[ 4/10] Instalando dotnet-runtime-8.0-0:8.0.19-1.fc41.x86_64                                                   100% |  59.0 MiB/s |  67.0 MiB |  00m01s
[ 5/10] Instalando aspnetcore-runtime-8.0-0:8.0.19-1.fc41.x86_64                                               100% |  55.5 MiB/s |  23.0 MiB |  00m00s
[ 6/10] Instalando dotnet-templates-8.0-0:8.0.119-1.fc41.x86_64                                                100% |  48.5 MiB/s |   5.7 MiB |  00m00s
[ 7/10] Instalando dotnet-targeting-pack-8.0-0:8.0.19-1.fc41.x86_64                                            100% |  34.2 MiB/s |  34.9 MiB |  00m01s
[ 8/10] Instalando dotnet-apphost-pack-8.0-0:8.0.19-1.fc41.x86_64                                              100% |  31.8 MiB/s |  11.4 MiB |  00m00s
[ 9/10] Instalando aspnetcore-targeting-pack-8.0-0:8.0.19-1.fc41.x86_64                                        100% |  13.4 MiB/s |  14.6 MiB |  00m01s
[10/10] Instalando dotnet-sdk-8.0-0:8.0.119-1.fc41.x86_64                                                      100% |   7.8 MiB/s | 294.3 MiB |  00m38s
¡Completado!
➜  Notas md y org git:(main) git-credential-manager configure
Configuring component 'Git Credential Manager'...
Configuring component 'Azure Repos provider'...
➜  Notas md y org git:(main) git push -u origin main                             
info: please complete authentication in your browser...
Enumerando objetos: 34, listo.
Contando objetos: 100% (34/34), listo.
Compresión delta usando hasta 4 hilos
Comprimiendo objetos: 100% (30/30), listo.
Escribiendo objetos: 100% (34/34), 130.74 KiB | 9.34 MiB/s, listo.
Total 34 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (2/2), done.
To https://github.com/Dlp-Ritter/NotasSisUMGxD.git
 * [new branch]      main -> main
rama 'main' configurada para rastrear 'origin/main'.
➜  Notas md y org git:(main)
