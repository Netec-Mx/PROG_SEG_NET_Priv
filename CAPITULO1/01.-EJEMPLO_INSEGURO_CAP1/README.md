# Práctica 1. Ejemplo inseguro

## Descripción general:

Este proyecto forma parte del Capítulo 1 del curso **Programación Segura con .NET**.  
Su propósito es exponer una implementación deliberadamente insegura, para que puedas analizar riesgos reales y compararlos con una versión segura.

Se mantienen carpetas y _namespaces_ en inglés, siguiendo las convenciones de .NET, pero las clases, propiedades y comentarios están redactados en español para facilitar tu aprendizaje.

## Objetivo:

Al finalizar esta práctica, serás capaz de:
- Identificar malas prácticas de seguridad en aplicaciones web.
- Auditar vulnerabilidades reales.
- Contrastar una implementación insegura con una versión segura.
- Realizar pruebas prácticas mediante Postman, observando el comportamiento de una API sin validación ni control de acceso.

## Principales características inseguras

- Contraseñas almacenadas en texto plano.
- Falta de validación de datos de entrada.
- Ausencia de mecanismos de autenticación y autorización real.
- Acceso abierto a rutas que deberían estar protegidas.
- Contexto de base de datos utilizado directamente desde el controlador.
- Configuración hardcodeada en `Program.cs`.
- Sin manejo centralizado de errores ni logs de seguridad.

---

## Estructura del proyecto:

```
01.-EJEMPLO_INSEGURO_CAP1/
├── Controllers/
│   └── CuentaController.cs
├── Models/
│   └── Usuario.cs
├── Program.cs
├── appsettings.json
├── launchSettings.json
├── NOTAS.md
├── README.md
├── .gitignore
├── EjemploInseguroCapitulo1.csproj
```

---

## Requisitos pevios

- Tener instalado el SDK de .NET 8.
- Contar con un cliente HTTP (Postman o similar).

   > ⚠️ ***Importante:** Revisa las notas del capítulo en el siguiente [enlace](https://netec-mx.github.io/PROG_SEG_NET_Priv/CAPITULO1/01.-EJEMPLO_INSEGURO_CAP1/NOTAS.html)*.

## Instrucciones

1. Abre una terminal en la raíz del proyecto.

2. Restaura las dependencias:

   ```bash
   dotnet restore
   ```

3. Compila el proyecto:

   ```bash
   dotnet build
   ```

4. Ejecuta:

   ```bash
   dotnet run
   ```

5. Accede a la API en:

   ```
   http://localhost:5000
   ```

## Endpoints disponibles

| Método | Ruta | Descripción |
| ------ | ---- | ------------ |
| POST | /cuenta/registrar | Registra un usuario (sin validación de datos ni hash). |
| POST | /cuenta/iniciar-sesion | Inicia sesión comparando credenciales en texto plano. |
| GET | /cuenta/perfil | Devuelve perfil; accesible sin autenticación. |

---

## Cómo usar la colección Postman

Para realizar las pruebas de forma guiada:

1. Abre Postman y haz clic en **Importar**.
2. Selecciona el archivo de colección:  

   `01.-EJEMPLO_INSEGURO_CAP1_ES.postman_collection.json`

3. Postman cargará todas las peticiones, organizadas por nombre para facilitar su uso.

### Pruebas sugeridas:

- **Registrar usuario**:  
    Envía la solicitud y verifica que responde "Usuario registrado (INSEGURO)".
   **Body (JSON):**  

  ```json
  {
    "nombreUsuario": "rodrigo",
    "contrasena": "1234"
  }
  ```

- **Registrar usuario vacío**:  
  Muestra que se pueden registrar usuarios con campos vacíos, ya que no hay validación.

- **Iniciar sesión correctamente**:  
  **Body (JSON):**  

  ```json
  {
    "nombreUsuario": "rodrigo",
    "contrasena": "1234"
  }
  ```

  Respuesta esperada: mensaje de bienvenida.

- **Iniciar sesión con contraseña incorrecta**:  
  Body con contraseña equivocada.
  ❌ Respuesta: "Credenciales inválidas (INSEGURO)".

- **Iniciar sesión inyección SQL**:  
  Body con intento de inyección:  
  ```json
  {
    "nombreUsuario": "' OR '1'='1",
    "contrasena": "' OR '1'='1"
  }
  ```
  Aunque no tiene efecto al usar EF InMemory, este ejemplo sirve para discutir riesgos con SQL real.

- **Consultar perfil (sin autenticación)**:
  GET directo. Esto demuestra que cualquier usuario (o visitante) puede ver esta ruta.

**Notas importantes:**  
- Todos los cuerpos deben enviarse en formato JSON (`Body → raw → JSON`).
- No se utiliza ningún tipo de autenticación ni token.
- La base de datos se guarda en memoria. Si detienes la aplicación, se pierde toda la información y debes registrar el usuario nuevamente.

---

### ⚠️ Importante:

Este proyecto es exclusivamente con fines educativos.

Úsalo únicamente para prácticas de auditoría, análisis de riesgos y comparación con la versión segura.
