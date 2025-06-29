# Notas: Ejemplo inseguro

## Propósito:

Este proyecto muestra deliberadamente cómo **NO** debe desarrollarse un módulo de registro e inicio de sesión en una aplicación web moderna.
Sirve como base para que puedas identificar malas prácticas y entender cómo aplicar correctamente la seguridad y los principios de arquitectura limpia.

---

## Principales malas prácticas implementadas

### 1. Contraseñas almacenadas en texto plano:
- La contraseña se guarda exactamente como llega desde el cliente, sin ningún tipo de cifrado ni hash.
- Riesgo directo de exposición en caso de fuga de base de datos.

**Solución recomendada:**
Aplicar un hash robusto (por ejemplo `BCrypt`) y nunca guardes contraseñas de forma reversibles.

### 2. Sin validación de datos de entrada:
- El modelo `Usuario` no tiene validaciones: permite campos vacíos o datos basura (es decir, sin sentido).
- Además, permite crear múltiples usuarios con el mismo nombre de usuario.

**Solución recomendada:**  
Usa anotaciones de validación (`[Required]`, `[StringLength]`) y validaciones adicionales en la capa de servicio.

---

### 3. Autenticación inexistente:
- No se usa JWT, cookies seguras, ni sesiones.
- Cualquiera puede acceder a rutas sensibles como `/perfil` sin autenticarse.

**Solución recomendada:**  
Implementa autenticación basada en JWT o usa ASP.NET Core Identity.
Protege las rutas con `[Authorize]`.

---

### 4. Acceso directo al contexto de base de datos:
- El controlador utiliza directamente `ContextoBaseDatos`.
- No existe capa de servicio ni patrón repositorio.

**Solución recomendada:**  
Separa las responsabilidades:  
- **Controlador:** orquesta peticiones y respuestas.  
- **Servicio:** aplica reglas de negocio y validaciones.  
- **Repositorio:** interactúa con la base de datos.

---

### 5. Sin manejo de errores ni logs:
- No hay control de excepciones.
- No se registran eventos de seguridad ni intentos fallidos de login.

**Solución recomendada:**  
Agregar un `Middleware` para el manejo global de excepciones y logs de auditoría.

---

### 6. Configuración de base de datos hardcodeada
- El nombre de la base de datos en memoria está definido directamente en `Program.cs`.
- No se usa `appsettings.json`.

**Solución recomendada:**  
Centraliza las configuraciones en `appsettings.json` o usa variables de entorno.

---

### 7. Nomenclatura mixta (carpetas en inglés, clases en español):
- En este proyecto se hace así para respetar las convenciones .NET y mantener la comprensión didáctica.
- En producción se recomienda una nomenclatura coherente y consistente en todo el proyecto.

---

## Resumen:

Este ejemplo inseguro te permite:
- Identificar vulnerabilidades reales en el código.
- Auditar la seguridad básica de una API.
- Contrastar con la versión segura, que aplica principios de arquitectura limpia y buenas prácticas de desarrollo.
- Practicar mejoras incrementales: validación, hashing, autenticación, control de acceso, logging.

---

## Próximos pasos:

1. Ejecutar la versión **segura** del mismo proyecto.
2. Analizar cada mejora aplicada y entender cómo mitiga vulnerabilidades.
3. Implementar prácticas equivalentes en proyectos reales.
