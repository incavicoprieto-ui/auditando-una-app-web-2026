# Poison Null Byte

## Pasos para solucionar el ejercicio

Navegando por la web accedemos a la sección **“About us”** desde el menú desplegable.

En esta página encontramos un enlace al apartado **“legal”**, que nos redirige a la siguiente ruta:

`http://localhost:3000/ftp/legal.md`


Si eliminamos `legal.md` de la URL y accedemos directamente a la ruta:


`http://localhost:3000/ftp`



podemos observar que tenemos acceso a varios archivos. Tras revisarlos, llegamos a la conclusión de que **solo se nos permite acceder a archivos con extensión `.md`**.


## Uso del Poison Null Byte

Para poder acceder al resto de archivos, utilizamos un truco conocido como **Poison Null Byte**.

Este ataque aprovecha una **validación incompleta del nombre del archivo**. La aplicación comprueba que el archivo termine en `.md`, pero el sistema que realmente abre el archivo lo interpreta de otra forma.

En muchos lenguajes antiguos (como C), existe un carácter especial llamado **Null Byte (`\x00`)** que indica el final de un texto. Todo lo que venga después de ese carácter simplemente se ignora.

En este ejercicio, conseguimos inyectar ese carácter usando codificación en la URL. Para ello añadimos:


`%2500.md`



a la ruta del archivo:

`http://localhost:3000/ftp/[archivo]%2500.md`


Al pulsar **Enter**, se abre la ventana de descarga del archivo.


## ¿Por qué funciona?

- `%25` representa el carácter `%`
- `%2500` se decodifica finalmente como `%00`, que es el **Null Byte**

El resultado es que:

- La aplicación ve un archivo que termina en `.md` (y lo permite)
- El sistema de archivos corta el nombre en el Null Byte y abre el archivo real, ignorando la extensión falsa

De esta forma, la validación se cumple, pero el archivo que se descarga **no es realmente un `.md`**, lo que nos permite acceder a archivos que deberían estar bloqueados.

## Retos que se pueden completar con este método

Gracias a este tipo de ataque, es posible completar los siguientes retos:

- Access a developer’s forgotten backup file  
- Access a salesman’s forgotten backup file  
- Access a misplaced SIEM signature file  
- Find the hidden easter egg  

