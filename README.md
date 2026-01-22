# Auditando una aplicación web en 2026
Guía estructurada para solventar los ejercicios propuestos para la auditoría de OWASP JUICE SHOP. 


Para poder practicar "en casa" con tu propio PC, es gratuito:

- Necesitas tener Docker Desktop: https://www.docker.com/products/docker-desktop/

Una vez instalado Docker, solamente necesitas ejecutar el siguiente comando:

Abrimos una terminal CMD o Powershell y hacemos los siguientes comandos:

1. `docker pull bkimminich/juice-shop`  Nos traemos la imagen
2. `docker run --rm -p 3000:3000 --name juice-shop bkimminich/juice-shop` Montamos la imagen
3. Dejamos esa terminal abierta
4. Ya deberiamos poder acceder a la web desde el navegador en: http://localhost:3000/
