# BURP SUITE: CONCEPTOS FUNDAMENTALES

Burp Suite es un **proxy de interceptación**.  
Se sitúa entre el navegador y la aplicación web, permitiendo **ver, modificar y reenviar tráfico HTTP/HTTPS**.

Flujo básico:

Navegador → Burp Suite → Aplicación web  
Aplicación web → Burp Suite → Navegador

Nada ocurre sin que Burp lo vea.

---

## 1. INTERFAZ DE BURP SUITE Y COMPONENTES

### 1.1 Proxy

Función:
- Interceptar peticiones y respuestas
- Analizar tráfico en tiempo real

Elementos clave:
- Intercept ON/OFF: detener peticiones
- HTTP history: historial completo del tráfico
- WebSockets history: tráfico en tiempo real

Uso en el taller:
- Detectar parámetros vulnerables
- Identificar endpoints y APIs ocultas

![Interfaz de Burp Suite](../img/Paso1.PNG)


---

### 1.2 Repeater

Función:
- Repetir y modificar peticiones manualmente

Para qué sirve:
- Probar payloads con control total
- Confirmar vulnerabilidades
- Comparar respuestas

Regla:
> **Todo hallazgo serio pasa por Repeater**

![Interfaz de Burp Suite](../img/paso3.PNG)
---

### 1.3 Intruder

Función:
- Automatizar ataques controlados

Usos típicos:
- Fuerza bruta
- Fuzzing de parámetros
- Enumeración de valores

Nota:
En versión Community es lento, pero suficiente para aprendizaje.

![Interfaz de Burp Suite](../img/paso2.PNG)
---

### 1.4 Decoder

Función:
- Codificar y decodificar datos

Usos:
- URL Encoding
- Base64
- Hexadecimal

Muy útil para:
- Poison Null Byte
- Bypass de validaciones

![Interfaz de Burp Suite](../img/paso4.PNG)

---

### 1.5 Comparer

Función:
- Comparar respuestas HTTP

Útil para:
- Ver diferencias sutiles
- Detectar cambios de comportamiento

![Interfaz de Burp Suite](../img/paso5.PNG)
---

## 2. METODOLOGÍA DE AUDITORÍA

1. Definir alcance
2. Discovery y enumeración
3. Análisis técnico
4. Evaluación de riesgo
5. Mitigación
6. Reporte

---
