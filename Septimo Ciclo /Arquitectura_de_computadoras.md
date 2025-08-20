# Hacer funcionar Esp lolin32 Lite

Para hace funcionar el esp lolin32 primero en arduino ide hay que incluir este tipo repositorio de las placas esp en la seccion de preferencias:

```
https://dl.espressif.com/dl/package_esp32_index.json
```

Luego en la seccion de Board manager hay que instalar el modulo esp32 de espressif y dejar que haga toda la descarga necesaria, puede tomar un tiempo.

# Error en los permisos de los puertos en linux

Este error surge cuando uno se dispone a carga su programa a cualquier placa, y dice algomo como que el puerto tty/algo no tiene permisos, esto se puede solucionar de varias maneras una de ellas la manera temporal y dirigida es la siguiente, simplemente se da permiso de lectura y escritura a ese puerto en especifico esa sesion:

```bash
sudo chmod 666 "tty/lo que sea..."
```

otra manera, la manera permanente es asignar a nuestro usuario al grupo dialout y se hace asi:

```bash
sudo usermod -aG dialout "usuario"
```

solo restaria reiniciar o cerrar sesion para que se apliquen los cambios y listo.


# Crear el servicio con node y express

Primero se crea el proyecto con npm, en cualquier carpeta en mi caso fue arqcomp
```bash
npm init
```
luego te saldra una especie de menu en la que seguiras instrucciones de como llamar y configurar tu proyecto
```json
{

"name": "arqcomp",

"version": "1.0.0",

"description": "Ejemplo de la primera api en node",

"main": "index.js",

"scripts": {

"test": "echo \"Error: no test specified\" && exit 1",

"UMG": "ls -la",

"yo": "echo Hola we",

"distro": "fastfetch",

"dev": "nodemon index.js"

},

"author": "Duglas",

"license": "ISC",

"dependencies": {

"express": "^4.21.2",

"nodemon": "^3.1.9"

}
```
Luego instalas express y nodemon
```bash 
npm install express nodemon
```
creas el index.js
```js
const express = require('express')

const app = express()

const port = 3000

app.get('/', (req, res) => {

res.send('Hola we')

})

app.listen(port, () => {

console.log(`Duglas escuchando desde el puerto ${port}`)

})

app.get('/atencion', (req,res) =>{
 :v
res.send('orale')

})

app.get('/otro', (req,res) =>{

res.send('su-57')

})
```
Y ejecutas el script dentro del package.json
```bash
npm run dev
```
y solo te restaria ir a tu puerto localhost:3000 para ver los resultados

bye :v



# API y programas de Arduino para ESP

## Ejercicio 1, personas.

Nueva Sección: Registro de RFID
Esta sección manejará un array de objetos que representan personas e identificadas con dispositivos RFID. Al hacer un POST, verificará si el RFID ya existe en la base de datos antes de permitir su inserción.

```javascript

//-------------- Base de datos en memoria para RFID --------------------
// 1. Personas
let personas = [
    {
        id: 1,
        nombre: "Jaime",
        apellido: "silencio",
        correo: "jaimeSilenci@mail.com",
        rfid: "00001",
    }
];
//-------------- fin de base de datos --------------------

//------ Método GET para obtener todos los registros de personas --------
app.get("/personas", (req, res) => {
    res.send(personas);
});

//------ Método GET para obtener un registro de personas por ID --------
app.get('/personas/:id', (req, res) => {
    const personas_id = parseInt(req.params.id);
    const data_personas = personas.find(data_personas => data_personas.id === personas_id);
    if (data_personas) {
        res.json(data_personas);
    } else {
        res.status(404).json({ message: 'Persona no encontrada' });
    }
});
//-------------- fin método GET ---------------------

//------ Método POST para agregar un nuevo registro de personas --------
app.post('/personas', (req, res) => {
    const { nombre, apellido, correo, rfid } = req.body; // Corregido: Desestructuración correcta

    // Validar campos obligatorios
    if (!nombre || !apellido || !correo || !rfid) {
        return res.status(400).json({ error: 'Datos incompletos' });
    }

    // Verificar si el RFID ya existe
    if (personas.some(registro => registro.rfid === rfid)) { // Corregido: Comparación correcta del RFID
        return res.status(400).json({ error: 'El RFID ya está registrado' });
    }

    // Crear nuevo registro
    const nuevoRegistro = {
        id: personas.length + 1,
        nombre,
        apellido,
        correo,
        rfid
    };

    // Agregar a la base de datos
    personas.push(nuevoRegistro);
    console.log(`Persona ingresada con éxito: ${JSON.stringify(nuevoRegistro)}`);
    res.status(201).json(nuevoRegistro);
});
//-------------- fin método POST ---------------------------
```

### Explicación de la nueva sección:

Base de datos en memoria:
Se utiliza un array para almacenar los registros de las personas. Cada registro tiene un id, un rfid (código único),  nombres, apellidos y un correo.

Método GET /personas:

Devuelve todos los registros de personas almacenadas.

Método GET /personas/:id:

Busca un registro de persona por su id y lo devuelve. Si no lo encuentra, devuelve un error 404.

Método POST /personas:

Valida que los campos rfid, nombre, apellidos y correo estén presentes en la solicitud.

Verifica si el rfid ya existe en la base de datos. Si existe, devuelve un error 400.

Si el rfid no existe, crea un nuevo registro, lo agrega al array y devuelve el registro creado con un código 201.

### Ejemplo de solicitud POST:

Para probar el nuevo endpoint, puedes usar una solicitud como esta:

```json
{
    "nombre": "Jaime",
    "apellido": "silencio",
    "correo": "jaimeSilenci@mail.com",
    "rfid": "00001"
}
```
Si el RFID ya existe, recibirás una respuesta como esta:

```json
{
    "error": "El RFID ya está registrado"
}
```
Si el RFID es nuevo, recibirás una respuesta como esta:

```json

{
    "id": 2,
    "nombre": "Angel",
    "apellido": "Lua",
    "correo": "skros@mail.com",
    "rfid": "00002"
}
```
#### Mejoras adicionales:

Podrías añadir un método PUT o DELETE para actualizar o eliminar registros de personas.

Si la base de datos crece mucho, podrías considerar usar una base de datos externa como MongoDB o MySQL.


## 2. simulacion de gps en movimiento.


Esta sección manejará un array de objetos que representan datos con latitud y longitud. Al hacer un POST, verificará que ambos campos estén presentes antes de permitir su inserción.

```javascript

//-------------- Base de datos en memoria para Datos --------------------
let datosgps = [
    {
        id: 1,
        latitud: "15°28'39.0 N",
        longitud: "90°22'14.5 W",
    }
];

//------ Método GET para obtener todos los registros de Datos --------
app.get("/datosgps", (req, res) => {
    res.send(datosgps);
});

//------ Método GET para obtener un registro de Datos por ID --------
app.get('/datosgps/:id', (req, res) => {
    const datosgps_id = parseInt(req.params.id);
    const data_datosgps = datosgps.find(data_datosgps => data_datosgps.id === datosgps_id);
    if (data_datosgps) {
        res.json(data_datosgps);
    } else {
        res.status(404).json({ message: 'Dato no encontrado' });
    }
});

//------ Método GET para obtener el último registro de Datos --------
app.get('/ultimoDatogps', (req, res) => {
    if (datosgps.length > 0) {
        res.json(datosgps[datosgps.length - 1]); // Devuelve el último elemento del array
    } else {
        res.status(404).json({ message: 'No hay datos disponibles' });
    }
});

//------ Método POST para agregar un nuevo registro de Datos --------
app.post('/datosgps', (req, res) => {
    const { latitud, longitud } = req.body;

    // Validar campos obligatorios
    if (latitud === undefined || longitud === undefined) {
        return res.status(400).json({ error: "Latitud y longitud son obligatorias." });
    }

    // Crear nuevo registro
    const nuevoDatogps = {
        id: datosgps.length + 1,
        latitud,
        longitud,
    };

    // Agregar a la base de datos
    datosgps.push(nuevoDatogps);
    console.log(`Registro de datos ingresado con éxito: ${JSON.stringify(nuevoDatogps)}`);
    res.status(201).json({ message: "Dato registrado correctamente." });
});
//-------------- fin método POST ---------------------------
```
Explicación de la nueva sección:
Base de datos en memoria:

Se utiliza un array datos para almacenar los registros de datos. Cada registro tiene un id, una latitud, una longitud y un timestamp.

Método GET /datos:

Devuelve todos los registros de datos almacenados.

Método GET /datos/:id:

Busca un registro de datos por su id y lo devuelve. Si no lo encuentra, devuelve un error 404.

Método POST /datos:

Valida que los campos latitud y longitud estén presentes en la solicitud.

Si los campos están presentes, crea un nuevo registro con un id autoincremental y un timestamp actual, lo agrega al array y devuelve un mensaje de éxito con un código 201.

Cómo integrarlo en tu código:
Simplemente copia y pega la nueva sección en tu archivo, justo después de las otras secciones que ya tienes (como alumnos, gps, esp, temp, etc.). No modifica la estructura existente, solo añade esta nueva funcionalidad.

Ejemplo de solicitud POST:
Para probar el nuevo endpoint, puedes usar una solicitud como esta:

```json
{
    "latitud": "19.4326",
    "longitud": "-99.1332"
}
```
Si la solicitud es exitosa, recibirás una respuesta como esta:

```json
{
    "message": "Dato registrado correctamente."
}
```
Si faltan campos obligatorios, recibirás una respuesta como esta:

```json
{
    "error": "Latitud y longitud son obligatorias."
}
```
Mejoras adicionales:
Si deseas mejorar la validación, podrías agregar expresiones regulares para validar el formato de la latitud y la longitud.

Podrías añadir un método PUT o DELETE para actualizar o eliminar registros de datos.

Si la base de datos crece mucho, podrías considerar usar una base de datos externa como MongoDB o MySQL.


perfecto, voy a ampliar tu código de Arduino para la ESP32 siguiendo tu estilo de programación y sin modificar la estructura existente. La funcionalidad que extraeré del segundo código es la de enviar datos de latitud y longitud a un servidor mediante una solicitud HTTP POST. Además, comentaré las funciones que puedan entrar en conflicto con las nuevas.

```cpp
#include <WiFi.h>
#include <HTTPClient.h>
#include <Arduino_JSON.h> // Esta librería debes instalarla en el IDE de Arduino

const char* ssid = "wifi";
const char* password = "pass";
const char* serverName = "http://192.168.1.34:3000/datosgps"; // Si no funciona, reemplaza "localhost" con la IP de tu red local

int id = 2; // ID autoincrementable

float latitud = 19.4326; // Latitud inicial (Ejemplo: Ciudad de México)

float longitud = -99.1332; // Longitud inicial

  

unsigned long lastTime = 0;

unsigned long timerDelay = 5000; // Intervalo de 5 segundos

  

void setup() {

Serial.begin(115200); // Asegúrate de que el monitor serie esté configurado a esta velocidad

analogReadResolution(10);

  

WiFi.begin(ssid, password);

Serial.println("Conectando...");

while (WiFi.status() != WL_CONNECTED) {

delay(500);

Serial.print(".");

}

Serial.println("");

Serial.print("Conectado a la red WiFi con la dirección IP: ");

Serial.println(WiFi.localIP());

  

Serial.println("Timer configurado a 5 segundos (timerDelay), tomará 5 segundos antes de publicar la primera lectura.");

}

  

void loop() {

// Enviar una solicitud HTTP POST cada 5 segundos

if ((millis() - lastTime) > timerDelay) {

// Verificar el estado de la conexión WiFi

if (WiFi.status() == WL_CONNECTED) {

WiFiClient client;

HTTPClient http;

  

//------------POST de datos de latitud y longitud-----------------------------------------

http.begin(client, serverName);

http.addHeader("Content-Type", "application/json");

  

// Construir el JSON con datos dinámicos

String postData = "{";

postData += "\"id\":" + String(id) + ",";

postData += "\"latitud\":" + String(latitud, 6) + ",";

postData += "\"longitud\":" + String(longitud, 6);

postData += "}";

  

int httpResponseCode = http.POST(postData);

  

if (httpResponseCode > 0) {

Serial.println("Respuesta del servidor (POST): ");

Serial.println("\"id\":" + String(id) + ",");

Serial.println("\"latitud\":" + String(latitud, 6) + ",");

Serial.println("\"longitud\":" + String(longitud, 6));

Serial.println(httpResponseCode);

  

Serial.println(http.getString());

} else {

Serial.print("Error en la solicitud HTTP (POST): ");

Serial.println(httpResponseCode);

}

  

http.end();

  

// Actualizar valores para el siguiente POST

id++;

latitud += 0.0009; // Aprox. 100 metros en latitud

longitud -= 0.00045; // Aprox. 50 metros en longitud

} else {

Serial.println("WiFi desconectado, reconectando...");

WiFi.begin(ssid, password); // Intentar reconectar

}

  

lastTime = millis();

}

}
```
Documentación de la funcionalidad añadida:
Envío de datos de latitud y longitud (POST):

El código ahora envía datos de latitud y longitud a un servidor en la ruta http://172.20.10.4:3000/datos.

Los datos se envían en formato JSON, con un id autoincremental, latitud y longitud.

La latitud y longitud se actualizan dinámicamente para simular un movimiento.

Manejo de errores:

Si la conexión WiFi se pierde, el código intenta reconectarse.

Si la solicitud HTTP falla, se imprime un mensaje de error en el monitor serie.

Notas adicionales:
Asegúrate de que la ruta http://172.20.10.4:3000/datos esté correctamente configurada en tu servidor para recibir los datos de latitud y longitud.

Si no tienes acceso a la librería Arduino_JSON, puedes instalarla desde el Gestor de Librerías del IDE de Arduino.

## Ejemplo con el modulo gps.

Entendido. Vamos a comentar las partes del código que no son requeridas (como la funcionalidad de temperatura y la emulación de GPS) y agregaremos la funcionalidad para trabajar con un Módulo GPS NEO-6M GT-U7. Este módulo GPS proporcionará datos reales de latitud y longitud, que serán enviados al servidor mediante una solicitud HTTP POST.


```cpp
#include <WiFi.h>
#include <HTTPClient.h>
#include <Arduino_JSON.h> // Esta librería debes instalarla en el IDE de Arduino
#include <TinyGPS++.h>   // Librería para manejar el módulo GPS NEO-6M
#include <SoftwareSerial.h> // Para la comunicación serial con el GPS

const char* ssid = "Wifi Tuyo";
const char* password = "Tu Pass";

const char* serverUrl = "http://172.20.10.4:3000/datos"; // URL del backend para enviar datos de latitud y longitud

// Configuración del GPS
#define GPS_RX 16 // Pin RX del GPS conectado al pin 16 del ESP32
#define GPS_TX 17 // Pin TX del GPS conectado al pin 17 del ESP32
SoftwareSerial gpsSerial(GPS_RX, GPS_TX); // Configura la comunicación serial con el GPS
TinyGPSPlus gps; // Objeto para manejar los datos del GPS

unsigned long lastTime = 0;
unsigned long timerDelay = 5000; // Intervalo de tiempo para enviar datos (5 segundos)

// Control de LEDs
#define LED_PIN1 26 // Define el LED en el pin 26 del ESP
#define LED_PIN2 27 // Define el LED en el pin 27 del ESP

void setup() {
    Serial.begin(115200); // Inicia la comunicación serial para el monitor
    gpsSerial.begin(9600); // Inicia la comunicación serial con el GPS

    WiFi.begin(ssid, password);
    Serial.println("Conectando...");
    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
    }
    Serial.println("");
    Serial.print("Conectado a la red WiFi con la dirección IP: ");
    Serial.println(WiFi.localIP());

    Serial.println("Timer configurado a 5 segundos (timerDelay), tomará 5 segundos antes de publicar la primera lectura.");

    // Configuración de los LEDs
    pinMode(LED_PIN1, OUTPUT); // Inicializa el pin digital LED_PIN1 como salida
    pinMode(LED_PIN2, OUTPUT); // Inicializa el pin digital LED_PIN2 como salida
}

void loop() {
    // Leer datos del GPS
    while (gpsSerial.available() > 0) {
        gps.encode(gpsSerial.read()); // Decodifica los datos del GPS
    }

    // Enviar una solicitud HTTP POST cada 5 segundos
    if ((millis() - lastTime) > timerDelay) {
        // Verificar si el GPS tiene una ubicación válida
        if (gps.location.isValid()) {
            float latitud = gps.location.lat(); // Obtener la latitud
            float longitud = gps.location.lng(); // Obtener la longitud

            // Verificar el estado de la conexión WiFi
            if (WiFi.status() == WL_CONNECTED) {
                WiFiClient client;
                HTTPClient http;

                //------------POST de datos de latitud y longitud-----------------------------------------
                http.begin(client, serverUrl);
                http.addHeader("Content-Type", "application/json");

                // Construir el JSON con datos del GPS
                String postData = "{";
                postData += "\"latitud\":" + String(latitud, 6) + ",";
                postData += "\"longitud\":" + String(longitud, 6);
                postData += "}";

                int httpResponseCode = http.POST(postData);

                if (httpResponseCode > 0) {
                    Serial.print("Respuesta del servidor (POST): ");
                    Serial.println(httpResponseCode);
                    Serial.println(http.getString());
                } else {
                    Serial.print("Error en la solicitud HTTP (POST): ");
                    Serial.println(httpResponseCode);
                }

                http.end();
            } else {
                Serial.println("WiFi desconectado, reconectando...");
                WiFi.begin(ssid, password);
            }
        } else {
            Serial.println("Esperando señal GPS válida...");
        }

        lastTime = millis();
    }
}

/*
// Funcionalidad de temperatura comentada (no requerida)
void igualdadCelsiusFahrenheit(float temperatura) {
    if (temperatura == fahrenheit) {
        delay(1000);
        digitalWrite(LED_PIN1, HIGH); // Enciende el LED 1
        Serial.println("Igualdad en los grados Celsius y Fahrenheit. LED verde encendido.");
        delay(2000);
        digitalWrite(LED_PIN1, LOW);
    } else {
        Serial.println("Los grados Celsius y Fahrenheit solo son iguales a -40 °C. En otro caso, no.");
    }
}

void superiorA100(float temperatura) {
    if (temperatura >= 100) {
        delay(1000);
        digitalWrite(LED_PIN2, HIGH); // Enciende el LED 2
        Serial.println("Demasiado caliente aquí, jefe.");
        delay(2000);
        digitalWrite(LED_PIN2, LOW);
    } else {
        Serial.println("Todo está relativamente fresco.");
    }
}
*/
```
Cambios realizados:
Comentada la funcionalidad de temperatura:

Las funciones igualdadCelsiusFahrenheit y superiorA100 han sido comentadas, ya que no son requeridas para esta implementación.

Integración del módulo GPS NEO-6M:

Se ha añadido la librería TinyGPS++ para manejar los datos del GPS.

Se ha configurado la comunicación serial con el GPS utilizando los pines RX y TX del ESP32.

Se obtienen los datos de latitud y longitud directamente del GPS.

Envío de datos GPS al servidor:

Los datos de latitud y longitud obtenidos del GPS se envían al servidor en formato JSON mediante una solicitud HTTP POST.

Manejo de errores:

Si el GPS no tiene una señal válida, se imprime un mensaje en el monitor serie.

Si la conexión WiFi se pierde, el código intenta reconectarse.

Instrucciones para usar el código:
Instalar las librerías necesarias:

Instala la librería TinyGPS++ desde el Gestor de Librerías del IDE de Arduino.

Asegúrate de tener instalada la librería Arduino_JSON.

Conexión del módulo GPS:

Conecta el pin TX del GPS al pin 16 del ESP32.

Conecta el pin RX del GPS al pin 17 del ESP32.

Conecta la alimentación (VCC y GND) del GPS a los pines correspondientes del ESP32.

Configuración del servidor:

Asegúrate de que el servidor en http://172.20.10.4:3000/datos esté listo para recibir los datos de latitud y longitud.

Subir el código al ESP32:

Sube el código al ESP32 y abre el monitor serie para ver los mensajes de depuración.

Documentación de la funcionalidad añadida:
Lectura de datos GPS:

El módulo GPS NEO-6M proporciona datos de latitud y longitud en tiempo real.

Estos datos se decodifican utilizando la librería TinyGPS++.

Envío de datos al servidor:

Los datos de latitud y longitud se envían al servidor en formato JSON mediante una solicitud HTTP POST.

Manejo de errores:

Si el GPS no tiene una señal válida, se imprime un mensaje en el monitor serie.

Si la conexión WiFi se pierde, el código intenta reconectarse.


## Verificacion de api existete: datos.

Análisis de la API existente:
La API que ya tienes en Node.js tiene un endpoint /datos que recibe datos de latitud y longitud en formato JSON. Aquí está el código relevante:

```javascript
app.post('/datos', (req, res) => {
    const { latitud, longitud } = req.body;

    if (latitud === undefined || longitud === undefined) {
        return res.status(400).json({ error: "Latitud y longitud son obligatorias." });
    }

    const nuevoDato = {
        id: idCounter++,
        latitud,
        longitud,
        timestamp: new Date().toISOString()
    };

    datos.push(nuevoDato);
    res.status(201).json({ message: "Dato registrado correctamente." });
});
```
Este endpoint ya es suficiente para manejar los datos que envía el ESP32 con el módulo GPS. No es necesario crear una nueva API, ya que:

Recibe datos de latitud y longitud en formato JSON.

Valida que los campos estén presentes.

Almacena los datos en un array en memoria.

Responde con un mensaje de éxito.

¿Qué más podríamos mejorar?
Si bien la API actual es suficiente, podemos hacer algunas mejoras opcionales para que sea más robusta y útil:

Si necesitas obtener solo el último registro enviado por el GPS, puedes agregar un endpoint como /ultimoDato.

Mejora opcional: Endpoint para obtener el último dato
Si decides agregar un endpoint para obtener el último dato, aquí está cómo hacerlo:

```javascript
app.get('/ultimoDato', (req, res) => {
    if (datos.length > 0) {
        res.json(datos[datos.length - 1]); // Devuelve el último elemento del array
    } else {
        res.status(404).json({ message: 'No hay datos disponibles' });
    }
});
```

Este endpoint sería útil si el ESP32 necesita consultar el último dato registrado en el servidor.


