# Reporting - Broken Auth - IA

Proporcionamos a la IA los prompts recogidos en la carpeta Guía de prompts, para que pueda generar el informe. 

- **Título**  
Credenciales de administrador vulnerables a ataque de fuerza bruta/diccionario en portal Juice Shop.

- **Categoría**  
Control de acceso / Gestión débil de contraseñas.

- **Severidad**  
Alta.

- **Activo afectado**  
Aplicación web Juice Shop – endpoint de login de usuario administrador (`http://localhost:3000/#/login` y `http://localhost:3000/rest/user/login`).

- **Descripción técnica**  
El formulario de autenticación del portal Juice Shop permite realizar múltiples intentos de login contra la cuenta de administrador sin mecanismos efectivos de rate limiting, bloqueo de cuenta o protección anti-fuerza bruta.  Mediante el uso de una wordlist y la herramienta Wfuzz se consigue descubrir la contraseña real del usuario administrador, demostrando que la fortaleza de la contraseña es insuficiente y que el endpoint es vulnerable a ataques de diccionario.

- **Pasos de reproducción**  
1. Preparar un diccionario de contraseñas con términos comunes, por ejemplo usando el fichero `diccionariowfuzz` o una wordlist similar que contenga valores como `admin`, `password`, `123456`, `admin2024`, `admin123`, etc.
2. Desde un entorno de pruebas (p. ej. Kali Linux), ejecutar Wfuzz contra el endpoint de login de la API:  
   
   wfuzz -z file,~/admin-passlist.txt \
     -d '{"email":"admin@juice-sh.op","password":"FUZZ"}' \
     -H "Content-Type: application/json" \
     --hc 401 \
     http://localhost:3000/rest/user/login

  donde `FUZZ` será sustituido por cada contraseña del diccionario. 
3. Observar las respuestas y localizar la petición que no devuelve código 401, lo que indica un intento de login exitoso con credenciales válidas.
4. Identificar la contraseña descubierta (`admin123`) y acceder al portal web en `http://localhost:3000/#/login` autenticándose como `admin@juice-sh.op` con dicha contraseña.
5. Verificar que se obtiene sesión de administrador, completando el reto “Password Strength” y confirmando la explotación de la debilidad de contraseña.

- **Evidencia**  
- Guía de simulación de ataque de fuerza bruta contra el login del administrador, donde se indica el uso de un diccionario y Wfuzz para encontrar la contraseña real del admin sin SQLi.
- Ejemplo de wordlist con contraseñas débiles (`admin`, `password`, `123456`, `admin123`, etc.) usada como base para el ataque de diccionario.
- Comando Wfuzz utilizado para enviar peticiones POST al endpoint `/rest/user/login` con el payload JSON que incluye `FUZZ` en el campo `password` y filtrado de respuestas 401.
- Confirmación de la contraseña encontrada (`admin123`) y credenciales finales: email `admin@juice-sh.op`, password `admin123`.

- **Impacto**  
Un atacante que consiga acceso a la interfaz de login puede obtener control total de la cuenta de administrador explotando la debilidad de la contraseña y la ausencia de medidas anti-fuerza bruta, sin necesidad de vulnerabilidades más complejas como SQL injection.  Esto permite potencialmente acceder a información sensible, modificar configuración de la aplicación, gestionar otros usuarios y escalar a más acciones maliciosas dentro del entorno comprometido.

- **Probabilidad**  
Muy Alta.
- La contraseña del administrador se encuentra dentro de una wordlist pequeña y compuesta por contraseñas muy comunes, lo que facilita su descubrimiento en un número reducido de intentos.
- No se evidencian mecanismos de bloqueo, captcha o incremento progresivo del tiempo de respuesta tras múltiples intentos fallidos, lo que reduce las barreras técnicas para un ataque automatizado.

- **Mitigaciones (Quick Win y Long Term)**  
  - **Quick Win**
    - Forzar el cambio inmediato de credenciales del usuario administrador a una contraseña robusta (mínimo de longitud, complejidad y no incluida en diccionarios conocidos).  
    - Configurar políticas de bloqueo temporal de cuenta o retraso incremental en respuestas tras varios intentos fallidos de login.  
    - Activar monitorización y alertas sobre múltiples intentos de autenticación fallidos desde la misma IP o usuario.  
  - **Long Term**
    - Implementar un sistema de gestión de identidades con políticas de complejidad, caducidad y prohibición explícita de contraseñas presentes en listas de contraseñas comprometidas.  
    - Incorporar mecanismos adicionales de protección como MFA para cuentas privilegiadas, captcha en el formulario de login y límites globales de peticiones al endpoint de autenticación.  
    - Realizar revisiones periódicas de seguridad del sistema de autenticación y pruebas de pentesting orientadas a fuerza bruta/diccionario en entornos controlados.

- **Validación del arreglo**  
- Verificar que la nueva contraseña del administrador cumple las políticas de complejidad y no forma parte de las wordlists utilizadas en pruebas.
- Repetir el ataque de fuerza bruta/diccionario con Wfuzz (u otra herramienta equivalente) con la misma wordlist y confirmar que no se obtiene ninguna respuesta de éxito distinta del código 401/403, ni se consigue sesión válida.
- Comprobar que, tras un número definido de intentos fallidos, la cuenta se bloquea temporalmente o el endpoint aplica medidas de protección (captcha, incremento de tiempos de respuesta, etc.).

