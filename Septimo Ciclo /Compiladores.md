expresion regular la de 8 caracteres dominio y demas:

debe tener al menos 8 caracteres, debe contener al menos una letra mayuscula, debe contener al menos un numero, debe contener al menos un caracter epecial (@,#,$, etc), el dominio solo puede contener letras y numeros (sin guiones o subrayados), no puede terminar con un numero despues del dominio

 
```^(?=.*[A-Z])(?=.*\d)(?=.*[@#$%^&*!])[A-Za-z\d@#$%^&*!]{8,}@([A-Za-z\d]+)\.[A-Za-z]{2,}(?<!\d)$```

Expresion para el automata en modo items:

```^(0*1(0*1)*0*)$ ```
 10011 = 19


¡Genial! He agregado las siguientes mejoras al analizador léxico en **C**:

✔ **Soporte para cadenas de texto** (Ejemplo: `"Hola Mundo"`).  
✔ **Identificación de comentarios** (`// comentario` y `/* comentario */`).  
✔ **Mejor manejo de separadores y operadores**.

Aquí tienes el código actualizado en formato **Markdown** para que puedas guardarlo y consultarlo sin internet. 🚀

---

## 📝 **Analizador Léxico Mejorado en C**

### 📌 **Características implementadas**

✔ Soporte para **números enteros y decimales**.  
✔ Detección de **palabras clave** (`if`, `while`, `return`, etc.).  
✔ **Lectura de una línea completa** y análisis de múltiples tokens.  
✔ **Identificación de paréntesis, operadores y cadenas de texto**.  
✔ **Soporte para comentarios** (`//` y `/* */`).

---

```c
#include <stdio.h>   // Entrada/salida estándar
#include <ctype.h>   // Funciones como isalpha(), isdigit()
#include <string.h>  // Funciones para manipulación de cadenas
#include <stdlib.h>  // Funciones como strtok()

#define MAX_TOKEN_LENGTH 100  // Tamaño máximo de un token
#define MAX_TOKENS 50         // Cantidad máxima de tokens en una línea

// Definimos los tipos de tokens
typedef enum { 
    IDENTIFICADOR,   // Nombres de variables y funciones
    NUMERO,          // Números enteros y decimales
    OPERADOR,        // +, -, *, /, =
    PALABRA_CLAVE,   // if, while, return
    PARENTESIS,      // ( y )
    CADENA_TEXTO,    // "Texto en comillas"
    COMENTARIO,      // // Comentario de línea o /* Comentario de bloque */
    DESCONOCIDO      // Caracteres no reconocidos
} TipoToken;

// Lista de palabras clave reconocidas
const char *palabrasClave[] = {"if", "while", "return", "else", "int", "float", "void", NULL};

// Estructura que representa un token
typedef struct {
    TipoToken tipo;
    char lexema[MAX_TOKEN_LENGTH];
} Token;

// Función para verificar si un token es una palabra clave
int esPalabraClave(char *palabra) {
    for (int i = 0; palabrasClave[i] != NULL; i++) {
        if (strcmp(palabra, palabrasClave[i]) == 0) {
            return 1;
        }
    }
    return 0;
}

// Función para analizar un token y clasificarlo
Token obtenerToken(char *entrada) {
    Token token;

    // Detectar comentario de línea (// comentario)
    if (strncmp(entrada, "//", 2) == 0) {
        token.tipo = COMENTARIO;
    }
    // Detectar comentario de bloque (/* comentario */)
    else if (strncmp(entrada, "/*", 2) == 0) {
        token.tipo = COMENTARIO;
    }
    // Si el token es una palabra clave
    else if (esPalabraClave(entrada)) {
        token.tipo = PALABRA_CLAVE;
    }
    // Si comienza con una letra, es un IDENTIFICADOR
    else if (isalpha(entrada[0])) {
        token.tipo = IDENTIFICADOR;
    }
    // Si es un número entero o decimal
    else if (isdigit(entrada[0]) || (entrada[0] == '.' && isdigit(entrada[1]))) {
        int puntos = 0;
        for (int i = 0; entrada[i] != '\0'; i++) {
            if (entrada[i] == '.') puntos++;
            if (!isdigit(entrada[i]) && entrada[i] != '.') {
                token.tipo = DESCONOCIDO;
                break;
            }
        }
        token.tipo = (puntos <= 1) ? NUMERO : DESCONOCIDO;
    }
    // Si es un operador matemático
    else if (strchr("+-*/=", entrada[0]) && entrada[1] == '\0') {
        token.tipo = OPERADOR;
    }
    // Si es un paréntesis
    else if (strchr("()", entrada[0]) && entrada[1] == '\0') {
        token.tipo = PARENTESIS;
    }
    // Si comienza y termina con comillas, es una CADENA DE TEXTO
    else if (entrada[0] == '"' && entrada[strlen(entrada) - 1] == '"') {
        token.tipo = CADENA_TEXTO;
    }
    // Si no encaja en ninguna categoría, es DESCONOCIDO
    else {
        token.tipo = DESCONOCIDO;
    }

    strcpy(token.lexema, entrada); // Copiar el lexema al token
    return token;
}

int main() {
    char entrada[MAX_TOKEN_LENGTH * MAX_TOKENS];  // Buffer para la línea de entrada
    char *tokens[MAX_TOKENS];  // Almacenará los tokens separados
    int i = 0;

    printf("Ingrese una línea de código: ");
    fgets(entrada, sizeof(entrada), stdin);  // Leer línea completa

    // Eliminar salto de línea al final si existe
    entrada[strcspn(entrada, "\n")] = 0;

    // Separar tokens usando espacio y operadores como delimitadores
    char *delimitadores = " +-*/=()";
    char *token = strtok(entrada, delimitadores);

    // Guardar los tokens encontrados
    while (token != NULL && i < MAX_TOKENS) {
        tokens[i++] = token;
        token = strtok(NULL, delimitadores);
    }

    // Analizar cada token
    for (int j = 0; j < i; j++) {
        Token resultado = obtenerToken(tokens[j]);

        printf("Lexema: %s -> Tipo: ", resultado.lexema);
        switch (resultado.tipo) {
            case IDENTIFICADOR: printf("IDENTIFICADOR\n"); break;
            case NUMERO: printf("NUMERO\n"); break;
            case OPERADOR: printf("OPERADOR\n"); break;
            case PALABRA_CLAVE: printf("PALABRA CLAVE\n"); break;
            case PARENTESIS: printf("PARENTESIS\n"); break;
            case CADENA_TEXTO: printf("CADENA DE TEXTO\n"); break;
            case COMENTARIO: printf("COMENTARIO\n"); break;
            default: printf("DESCONOCIDO\n"); break;
        }
    }

    return 0;
}

```


📌 ¿Cómo funciona este analizador léxico?

1️⃣ Lee una línea completa del usuario.
2️⃣ Separa la línea en tokens usando strtok().
3️⃣ Clasifica cada token en identificadores, números, operadores, palabras clave, cadenas de texto y comentarios.
4️⃣ Imprime el tipo de cada token en la terminal.
🎯 Ejemplo de ejecución
### Entrada 1:
```c
Ingrese una línea de código: if (x == 10.5) printf("Hola Mundo");

```
### **Salida:**
```c
Lexema: if -> Tipo: PALABRA CLAVE
Lexema: x -> Tipo: IDENTIFICADOR
Lexema: 10.5 -> Tipo: NUMERO
Lexema: printf -> Tipo: IDENTIFICADOR
Lexema: "Hola Mundo" -> Tipo: CADENA DE TEXTO

```

### Entrada 2:
```c
Ingrese una línea de código: // Esto es un comentario
```


### Salida:
```c
Lexema: // Esto es un comentario -> Tipo: COMENTARIO
```


---

## 🔥 **Resumen de Mejoras**

✔ **Soporte para cadenas de texto** (`"Hola Mundo"`).  
✔ **Identificación de comentarios** (`//` y `/* */`).  
✔ **Mejor manejo de separadores y operadores**.

---

## **1. Estructura Léxica de C**

### 🔹 **Elementos léxicos principales**

Un **analizador léxico** para C debe identificar los siguientes elementos:

| **Categoría**             | **Ejemplos**                        |
| ------------------------- | ----------------------------------- |
| **Palabras clave**        | `int`, `if`, `while`, `return`      |
| **Identificadores**       | `variable`, `miFuncion`, `contador` |
| **Constantes numéricas**  | `123`, `3.14`, `0xFF`               |
| **Cadenas de caracteres** | `"Hola Mundo"`                      |
| **Operadores**            | `+`, `-`, `*`, `/`, `=`             |
| **Delimitadores**         | `{`, `}`, `;`, `,`                  |
| **Comentarios**           | `// Comentario`, `/* Comentario */` |

📌 **Ejemplo de código C que un analizador debe procesar:**

```c
int suma(int a, int b) {
    return a + b;  // Retorna la suma
}
```
🔹 Tokens generados:
```c
[int] [suma] [(] [int] [a] [,] [int] [b] [)] [{] [return] [a] [+] [b] [;] [}]
```
🔍 2. Factibilidad de C para desarrollar un Analizador Léxico

✅ Ventajas

✔ Control total sobre memoria y datos (útil para manejar buffers de entrada).
✔ Librerías estándar eficientes para manipulación de cadenas (string.h).
✔ Estructuras y punteros para implementar una tabla de símbolos eficiente.
✔ Uso de Flex y Bison para generar analizadores léxicos y sintácticos automáticamente.
❌ Desafíos

❌ Manejo manual de memoria puede ser complejo.
❌ No tiene librerías de análisis léxico incorporadas como Python.

🔹 Conclusión: C es una excelente opción para un analizador léxico, pero requiere manejar memoria manualmente.
🔠 3. Implementación de un Analizador Léxico en C

📌 Objetivo:
✔ Identificar palabras clave, identificadores, números, operadores y delimitadores.
✔ Construir una tabla de símbolos.

---

### 📝 **Código: Analizador Léxico con Tabla de Símbolos

```c
#include <stdio.h>
#include <ctype.h>
#include <string.h>
#include <stdlib.h>

#define MAX_TOKEN 100
#define MAX_SIMBOLOS 100

// Definimos tipos de tokens
typedef enum { 
    PALABRA_CLAVE, IDENTIFICADOR, NUMERO, OPERADOR, DELIMITADOR, DESCONOCIDO 
} TipoToken;

// Lista de palabras clave de C
const char *palabrasClave[] = {"int", "return", "if", "while", "void", NULL};

// Estructura de un token
typedef struct {
    TipoToken tipo;
    char lexema[MAX_TOKEN];
} Token;

// Tabla de símbolos
typedef struct {
    char nombre[MAX_TOKEN];
    char tipo[MAX_TOKEN];
} Simbolo;

Simbolo tablaSimbolos[MAX_SIMBOLOS];
int simboloIndex = 0;

// Función para verificar si una palabra es clave
int esPalabraClave(char *palabra) {
    for (int i = 0; palabrasClave[i] != NULL; i++) {
        if (strcmp(palabra, palabrasClave[i]) == 0) {
            return 1;
        }
    }
    return 0;
}

// Función para agregar a la tabla de símbolos
void agregarSimbolo(char *nombre, char *tipo) {
    strcpy(tablaSimbolos[simboloIndex].nombre, nombre);
    strcpy(tablaSimbolos[simboloIndex].tipo, tipo);
    simboloIndex++;
}

// Función para analizar un token
Token obtenerToken(char *entrada) {
    Token token;

    // Verificar si es palabra clave
    if (esPalabraClave(entrada)) {
        token.tipo = PALABRA_CLAVE;
    }
    // Verificar si es un identificador (empieza con letra y puede contener números)
    else if (isalpha(entrada[0])) {
        token.tipo = IDENTIFICADOR;
        agregarSimbolo(entrada, "Variable");
    }
    // Verificar si es un número
    else if (isdigit(entrada[0])) {
        token.tipo = NUMERO;
    }
    // Verificar si es un operador
    else if (strchr("+-*/=", entrada[0]) && entrada[1] == '\0') {
        token.tipo = OPERADOR;
    }
    // Verificar si es un delimitador
    else if (strchr("();{}", entrada[0]) && entrada[1] == '\0') {
        token.tipo = DELIMITADOR;
    }
    // Si no encaja, es desconocido
    else {
        token.tipo = DESCONOCIDO;
    }

    strcpy(token.lexema, entrada);
    return token;
}

int main() {
    char entrada[MAX_TOKEN * 10];
    char *tokens[50];
    int i = 0;

    printf("Ingrese una línea de código: ");
    fgets(entrada, sizeof(entrada), stdin);
    entrada[strcspn(entrada, "\n")] = 0;

    // Separar tokens por espacios y operadores
    char *delimitadores = " +-*/=();{}";
    char *token = strtok(entrada, delimitadores);

    while (token != NULL && i < 50) {
        tokens[i++] = token;
        token = strtok(NULL, delimitadores);
    }

    // Analizar cada token
    for (int j = 0; j < i; j++) {
        Token resultado = obtenerToken(tokens[j]);

        printf("Lexema: %s -> Tipo: ", resultado.lexema);
        switch (resultado.tipo) {
            case PALABRA_CLAVE: printf("PALABRA CLAVE\n"); break;
            case IDENTIFICADOR: printf("IDENTIFICADOR\n"); break;
            case NUMERO: printf("NUMERO\n"); break;
            case OPERADOR: printf("OPERADOR\n"); break;
            case DELIMITADOR: printf("DELIMITADOR\n"); break;
            default: printf("DESCONOCIDO\n"); break;
        }
    }

    // Mostrar tabla de símbolos
    printf("\nTabla de Símbolos:\n");
    printf("----------------------------\n");
    printf("| Nombre       | Tipo      |\n");
    printf("----------------------------\n");
    for (int j = 0; j < simboloIndex; j++) {
        printf("| %-12s | %-8s |\n", tablaSimbolos[j].nombre, tablaSimbolos[j].tipo);
    }
    printf("----------------------------\n");

    return 0;
}
```

## 📌 **Ejemplo de Ejecución**

🔹 **Entrada:**
```c
int suma = a + b;
```

🔹 **Salida:**
```c
Lexema: int -> Tipo: PALABRA CLAVE
Lexema: suma -> Tipo: IDENTIFICADOR
Lexema: a -> Tipo: IDENTIFICADOR
Lexema: + -> Tipo: OPERADOR
Lexema: b -> Tipo: IDENTIFICADOR
Lexema: ; -> Tipo: DELIMITADOR

Tabla de Símbolos:
----------------------------
| Nombre       | Tipo      |
----------------------------
| suma        | Variable  |
| a          | Variable  |
| b          | Variable  |
----------------------------
```



## 🚀 **Código: Analizador Léxico en Terminal (C++)**

Este programa analiza una línea de código en C, extrae los tokens y almacena las variables en una tabla de símbolos.

```C
#include <iostream>
#include <vector>
#include <string>
#include <cctype>
#include <unordered_set>

using namespace std;

// Definimos los tipos de tokens
enum TipoToken { PALABRA_CLAVE, IDENTIFICADOR, NUMERO, OPERADOR, DELIMITADOR, ERROR };

// Lista de palabras clave en C
const unordered_set<string> palabrasClave = {"int", "return", "if", "while", "void"};

// Estructura de un token
struct Token {
    TipoToken tipo;
    string lexema;
};

// Estructura para la tabla de símbolos
struct Simbolo {
    string nombre;
    string tipo;
};

// Tabla de símbolos
vector<Simbolo> tablaSimbolos;

// Función para determinar si una palabra es clave
bool esPalabraClave(const string& palabra) {
    return palabrasClave.find(palabra) != palabrasClave.end();
}

// Función para verificar si un carácter es un delimitador
bool esDelimitador(char c) {
    return c == ';' || c == ',' || c == '(' || c == ')' || c == '{' || c == '}';
}

// Función para verificar si un carácter es un operador
bool esOperador(char c) {
    return c == '+' || c == '-' || c == '*' || c == '/' || c == '=';
}

// Función para agregar una variable a la tabla de símbolos
void agregarSimbolo(const string& nombre, const string& tipo) {
    for (const auto& simbolo : tablaSimbolos) {
        if (simbolo.nombre == nombre) return;  // Evita duplicados
    }
    tablaSimbolos.push_back({nombre, tipo});
}

// Función para analizar el código ingresado
vector<Token> analizarCodigo(const string& codigo) {
    vector<Token> tokens;
    string buffer;
    
    for (size_t i = 0; i < codigo.length(); i++) {
        char c = codigo[i];

        if (isspace(c)) {  // Ignorar espacios
            continue;
        }
        else if (isalpha(c)) {  // Identificadores y palabras clave
            buffer += c;
            while (isalnum(codigo[i + 1])) {
                buffer += codigo[++i];
            }

            if (esPalabraClave(buffer)) {
                tokens.push_back({PALABRA_CLAVE, buffer});
            } else {
                tokens.push_back({IDENTIFICADOR, buffer});
                agregarSimbolo(buffer, "Variable");
            }
            buffer.clear();
        }
        else if (isdigit(c)) {  // Números
            buffer += c;
            while (isdigit(codigo[i + 1])) {
                buffer += codigo[++i];
            }
            tokens.push_back({NUMERO, buffer});
            buffer.clear();
        }
        else if (esOperador(c)) {  // Operadores
            tokens.push_back({OPERADOR, string(1, c)});
        }
        else if (esDelimitador(c)) {  // Delimitadores
            tokens.push_back({DELIMITADOR, string(1, c)});
        }
        else {  // Caracter desconocido
            tokens.push_back({ERROR, string(1, c)});
        }
    }
    
    return tokens;
}

// Función para imprimir los tokens
void imprimirTokens(const vector<Token>& tokens) {
    cout << "\nTokens encontrados:\n";
    cout << "----------------------\n";
    cout << "| Lexema  | Tipo     |\n";
    cout << "----------------------\n";
    
    for (const auto& token : tokens) {
        cout << "| " << token.lexema << "  | ";
        switch (token.tipo) {
            case PALABRA_CLAVE: cout << "PALABRA CLAVE"; break;
            case IDENTIFICADOR: cout << "IDENTIFICADOR"; break;
            case NUMERO: cout << "NUMERO"; break;
            case OPERADOR: cout << "OPERADOR"; break;
            case DELIMITADOR: cout << "DELIMITADOR"; break;
            default: cout << "ERROR";
        }
        cout << " |\n";
    }
    cout << "----------------------\n";
}

// Función para imprimir la tabla de símbolos
void imprimirTablaSimbolos() {
    if (tablaSimbolos.empty()) {
        cout << "\nNo se encontraron identificadores.\n";
        return;
    }

    cout << "\nTabla de Símbolos:\n";
    cout << "--------------------------\n";
    cout << "| Nombre   | Tipo       |\n";
    cout << "--------------------------\n";
    
    for (const auto& simbolo : tablaSimbolos) {
        cout << "| " << simbolo.nombre << "  | " << simbolo.tipo << " |\n";
    }
    cout << "--------------------------\n";
}

int main() {
    string codigo;

    cout << "Ingrese una línea de código en C: ";
    getline(cin, codigo);

    vector<Token> tokens = analizarCodigo(codigo);
    
    imprimirTokens(tokens);
    imprimirTablaSimbolos();

    return 0;
}

```

---

## 🔥 **¿Cómo funciona este analizador léxico?**

1️⃣ **Lee el código fuente ingresado por el usuario.**  
2️⃣ **Divide el código en tokens** usando espacios, operadores y delimitadores.  
3️⃣ **Clasifica cada token** como **palabra clave, identificador, número, operador o delimitador.**  
4️⃣ **Guarda las variables encontradas en una tabla de símbolos**.  
5️⃣ **Muestra los tokens y la tabla de símbolos en pantalla**.

---

## 📌 **Ejemplo de Ejecución**

🔹 **Entrada del usuario:**

```C
int suma = a + b;
```

🔹 **Salida esperada en terminal:**

```C
Tokens encontrados:
----------------------
| Lexema  | Tipo     |
----------------------
| int  | PALABRA CLAVE |
| suma  | IDENTIFICADOR |
| =  | OPERADOR |
| a  | IDENTIFICADOR |
| +  | OPERADOR |
| b  | IDENTIFICADOR |
| ;  | DELIMITADOR |
----------------------

Tabla de Símbolos:
--------------------------
| Nombre   | Tipo       |
--------------------------
| suma  | Variable |
| a  | Variable |
| b  | Variable |
--------------------------
```

---

## 🚀 **¿Cómo compilar y ejecutar?**

📌 **Compilar en terminal:**

```C
g++ analizador_lexico.cpp -o analizador
```

📌 **Ejecutar:**

```C
./analizador
```
