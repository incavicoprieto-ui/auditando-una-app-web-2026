# GUÍA COMPLETA DE USO DE IA EN AUDITORÍAS WEB
## OWASP Juice Shop · Auditoría Web Moderna (2026)

---

## PRINCIPIO FUNDAMENTAL

> **La IA escribe. El auditor valida.**
> Ningún resultado es válido sin evidencia técnica verificable.

La Inteligencia Artificial no es un auditor autónomo.  
Su uso correcto en auditorías web es como herramienta de apoyo para pensar mejor, documentar mejor y no olvidar vectores comunes.

---

## METODOLOGÍA DE AUDITORÍA

1. Definición de alcance (Scope)
2. Discovery y enumeración
3. Análisis de vulnerabilidades
4. Evaluación de riesgo (Impacto + Probabilidad)
5. Mitigación
6. Reporte

---

## REGLAS DE USO DE IA

- La IA no confirma vulnerabilidades
- La IA no decide impacto real
- Todo hallazgo debe tener evidencia técnica
- Sin evidencia, no se reporta
- El auditor humano es responsable del resultado final

---

## PROMPTS GENERALES DE DISCOVERY Y ENUMERACIÓN

### Prompt · Checklist global
```
Estoy auditando una web SPA + API (OWASP Juice Shop) en local.
Dame una checklist completa de discovery que incluya:
- UI y flujos funcionales
- DevTools (Network, Storage, Console)
- Endpoints sensibles habituales
- Errores comunes en aplicaciones SPA
- Evidencias que debería guardar

Devuélvelo en pasos accionables.
```

### Prompt · Discovery técnico
```
Actúa como auditor web profesional.
¿Qué debo revisar durante la fase de discovery
en una aplicación SPA con backend API REST?
Incluye ejemplos técnicos.
```

### Prompt · Discovery priorizado por riesgo
```
Dame una checklist de discovery priorizada por riesgo
para una web con login, gestión de usuarios y API pública.
```

---

## PLANTILLA ESTÁNDAR DE HALLAZGOS

### Prompt base de reporte
```
Redacta un hallazgo técnico con el siguiente formato:

- Título
- Categoría
- Severidad
- Activo afectado
- Descripción técnica
- Pasos de reproducción
- Evidencia
- Impacto
- Probabilidad
- Mitigaciones (Quick Win y Long Term)
- Validación del arreglo

EVIDENCIA VERIFICADA:
[Inserta aquí datos reales. No inventes nada.]
```

---

## CASO 1 · SQL INJECTION

### Objetivo
Detectar, confirmar y documentar una inyección SQL.

### Prompt · Análisis inicial
```
Analiza el siguiente comportamiento observado en una web:
[describe la reacción de la aplicación]

¿Puede corresponder a una SQL Injection?
Justifica técnicamente la respuesta.
```

### Prompt · Confirmación
```
¿Qué pruebas adicionales puedo realizar
para confirmar una SQL Injection
sin modificar datos ni causar daño?
```

### Prompt · Redacción de hallazgo
```
Siguiendo la plantilla estándar,
redacta el hallazgo técnico.

EVIDENCIA:
Se modificaron parámetros GET permitiendo
alterar consultas SQL y acceder a información
de clientes y al login de administrador.
```

### Prompt · Mitigación
```
Propón mitigaciones técnicas para SQL Injection,
separando Quick Win y soluciones a largo plazo.
```

---

## CASO 2 · BROKEN AUTHENTICATION (FUERZA BRUTA)

### Objetivo
Detectar ausencia de rate limiting y protecciones antifuerza bruta.

### Prompt · Evaluación
```
Tras múltiples intentos de login fallidos,
el sistema responde siempre igual.
¿Esto indica un problema de autenticación?
Explícalo técnicamente.
```

### Prompt · Evidencias
```
¿Qué evidencias técnicas son necesarias
para demostrar un ataque de fuerza bruta
en un informe profesional?
```

### Prompt · Redacción de hallazgo
```
Redacta el hallazgo técnico siguiendo la plantilla.

EVIDENCIA:
Se realizaron 20 intentos fallidos en 10 segundos
contra /#/login sin bloqueo, CAPTCHA ni delays.
```

### Prompt · Impacto
```
Explica el impacto de una vulnerabilidad
de fuerza bruta en un negocio real.
```

---

## CASO 3 · FUZZING DE RUTAS Y ENDPOINTS

### Objetivo
Descubrir superficie de ataque no visible.

### Prompt · Discovery
```
¿Qué tipos de endpoints sensibles
debería buscar durante fuzzing
en una aplicación SPA + API?
```

### Prompt · Análisis
```
He encontrado endpoints que responden 200 OK
sin autenticación.
¿Cómo evalúo si es un hallazgo real?
```

### Prompt · Redacción de hallazgo
```
Redacta el hallazgo técnico usando la plantilla.

EVIDENCIA:
Se identificaron endpoints sensibles como
/ftp y /administration accesibles sin autenticación.
```

### Prompt · Mitigación
```
Propón una estrategia correcta de control de acceso
para rutas y APIs internas.
```

---

## CASO 4 · POISON NULL BYTE

### Objetivo
Explotar fallos de validación de extensiones de archivo.

### Prompt · Explicación técnica
```
Explica cómo funciona el ataque Poison Null Byte
y por qué ocurre a nivel de validación de input.
```

### Prompt · Validación
```
¿Qué pruebas mínimas confirman un Poison Null Byte
sin comprometer el sistema?
```

### Prompt · Redacción de hallazgo
```
Redacta el hallazgo técnico siguiendo la plantilla.

EVIDENCIA:
Un archivo bloqueado pudo descargarse usando
/ftp/archivo.gg%2500.md.
```

### Prompt · Mitigación
```
Explica cómo diseñar un sistema de descargas seguro
sin depender de validación por extensión.
```

---

## PROMPTS DE AUDITORÍA AUTOMÁTICA CON IA

### Auditoría dirigida
```
Busca vulnerabilidades de SQL Injection en:
http://[IP]:[PUERTO]

Entorno controlado.
Documenta cada intento y resultado.
```

### Auditoría general
```
Realiza una auditoría web completa sobre:
http://[IP]:[PUERTO]

Enumera, prueba y documenta hallazgos.
```

### Validación humana
```
Analiza críticamente los siguientes resultados
de una auditoría automática:
- Qué es válido
- Qué es dudoso
- Qué requiere verificación manual
```

---

## MENSAJE FINAL

> Encontrar fallos no te hace auditor.  
> Entenderlos, demostrarlos y evitar que se repitan, sí.

La IA acelera.  
El criterio humano manda.  
La evidencia decide.
