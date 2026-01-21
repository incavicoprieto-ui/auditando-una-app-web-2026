# Solución

### Solución Paso 1

1. Accede a la página principal de Juice Shop:

   ```
   http://[IP]:3000
   ```
2. Prueba rutas comunes relacionadas con APIs añadiéndolas manualmente a la URL.
3. En Juice Shop, la API principal se encuentra bajo el prefijo:

   ```
   /rest
   ```
4. Al acceder a:

   ```
   http://[IP]:3000/rest
   ```

   se confirma que la API existe y responde, lo que indica que hay múltiples endpoints accesibles bajo esa ruta.

### Solución Paso 2

1. Abre Juice Shop en el navegador:

   ```
   http://[IP]:3000
   ```
2. Abre las DevTools con `F12`.
3. Ve al apartado Sources.
4. Abre el archivo principal de JavaScript:

   ```
   main.js
   ```
5. Usa Ctrl + F y busca `path` (te saldran varios resultados, baja hasta encontrar el correcto).

### Solución Paso 3

1. Usa el diccionario pequeño que esta en el repositorio para no sobrecargar la aplicación.
2. Ejecutar Gobuster contra el puerto 3000, que es el puerto por defecto de Juice Shop.
3. Filtrar respuestas irrelevantes usando `--exclude-length`, ya que Juice Shop devuelve muchas respuestas con el mismo tamaño.

#### Comando corregido y validado

```
gobuster dir -u http://[IP]:3000/ \
-w [diccionario] \
-x html,php,js,css,json \
-t 50 \
--exclude-length 75055
```
