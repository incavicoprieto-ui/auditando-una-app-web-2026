# Simulación de ataque de fuerza bruta/diccionario con Wfuzz

En este ejercicio vamos a simular un ataque de fuerza bruta/diccionario contra el **login del administrador**, usando una **wordlist pequeña** creada por vosotros y **Wfuzz**.  
El objetivo es encontrar la contraseña real del admin, y luego usarla para entrar **sin SQLi**.

---

## Paso 1: Crear una wordlist pequeña a mano

Primero no vamos a usar *rockyou* ni nada grande. Vamos a crear nuestra propia lista de contraseñas probables:

1. Abrid una terminal en **Kali Linux**.
2. Ejecutad:
    `cd ~
    nano admin-passlist.txt`

3. Escribid una contraseña por líena. Por ejemplo: 

admin
password
123456
admin2024
admin123
qwerty

4. Guardad y salid de nano con: 
Ctrl+O, Enter, luego Ctrl+X.

---

## Paso 2: Lanzar Wfuzz contra el endpoint de login

Ahora automatizamos el intento de varias contraseñas usando la wordlist que acabáis de crear:

1. En la terminal Kali, ejecutad el siguiente comando (ajustad la ruta de la lista si la guardasteis en otro sitio):
    
    ```
    wfuzz -z file,~/admin-passlist.txt \
      -d '{"email":"admin@juice-sh.op","password":"FUZZ"}' \
      -H "Content-Type: application/json" \
      --hc 401 \
      http://localhost:3000/rest/user/login
    ```

- **`z file,~/admin-passlist.txt`** le dice a Wfuzz que use vuestro archivo como fuente de payloads.
- Donde aparece **`FUZZ`** en el JSON, Wfuzz va sustituyendo cada línea de la wordlist como valor de **`password`**.
- **`H "Content-Type: application/json"`** copia exactamente la cabecera que hemos visto en Postman.
- **`-hc 401`** le dice a Wfuzz que **oculte** todas las respuestas con código 401 (fallo de login); de esa forma, en la salida aparecerá resaltado el intento que **no** devuelve 401, que es la contraseña válida.

---

## Paso 3: Usar la contraseña encontrada y completar el reto

Por último, vamos a usar la contraseña que acabamos de descubrir:

1. Volved al navegador y a la página de login:
    
    **`http://localhost:3000/#/login`**
    
2. En **Email**, poned:
    
    **`admin@juice-sh.op`**
    
3. En **Password**, poned:
    
    **`admin123`**
    
4. Pulsad **Log in**.

Si todo es correcto, habréis iniciado sesión como admin **sin** usar SQLi y con la contraseña original débil. Eso completa el reto **‘Password Strength’**.
