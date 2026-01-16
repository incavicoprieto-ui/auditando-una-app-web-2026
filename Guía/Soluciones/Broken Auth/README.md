# Simulación de ataque de fuerza bruta/diccionario con Wfuzz

En este ejercicio vamos a simular un ataque de fuerza bruta/diccionario contra el **login del administrador**, usando una **wordlist pequeña** creada por vosotros y **Wfuzz**.  
El objetivo es encontrar la contraseña real del admin, y luego usarla para entrar **sin SQLi**.

---

## Paso 1: Preparar el diccionario

En lugar de crear una lista manual, utilizaremos el diccionario proporcionado para el taller.

1. Localizad el archivo **`diccionariowfuzz`** en la carpeta **`Diccionarios`** del repositorio.
2. Copiad este archivo a vuestro entorno de trabajo en Kali Linux o referenciadlo directamente en el comando.

---

## Paso 2: Lanzar Wfuzz contra el endpoint de login

Ahora automatizamos el intento de varias contraseñas usando la wordlist que acabáis de crear:

1. En la terminal Kali, ejecutad el comando `wfuzz` configurando los siguientes parámetros:
   - Usad vuestra wordlist como payload (`-z file,...`).
   - Enviad un JSON con el email `admin@juice-sh.op` y usad `FUZZ` en el campo contraseña (`-d '...'`).
   - Aseguraos de incluir la cabecera `Content-Type: application/json` (`-H ...`).
   - Filtrad las respuestas para ocultar los códigos de error 401 (`--hc ...`).

El objetivo es encontrar la respuesta que NO sea un 401, lo cual indicará la contraseña correcta.

---

## Paso 3: Usar la contraseña encontrada y completar el reto

Por último, vamos a usar la contraseña que acabamos de descubrir:

1. Volved al navegador y a la página de login:
    
    **`http://localhost:3000/#/login`**
    
2. Iniciad sesión con el email del administrador y la contraseña hallada.

Si todo es correcto, habréis iniciado sesión como admin **sin** usar SQLi y con la contraseña original débil. Eso completa el reto **‘Password Strength’**.
