# Proyecto: Docker + Pre-commits + Flask

Este proyecto es una práctica para aprender los fundamentos de la **contenerización** de una aplicación web **Flask** usando **Docker**.  
Se han configurado dos entornos: uno para **producción** y otro para **desarrollo**, además de integrar herramientas de calidad de código como **Black** (para el formateo automático) y **Pytest** (para testing).  
Todo esto se automatiza mediante un **hook pre-commit de Git**, que se ejecuta antes de cada commit para garantizar que el código sea limpio y funcional.

---

## 🚀 Cómo levantar la aplicación

Para poner en marcha la versión de la aplicación en producción, sigue estos pasos:

### 1️⃣ Construir la imagen Docker
Este comando lee el archivo `Dockerfile` y empaqueta la aplicación en una imagen llamada **mi-app** con la versión **1.0**.  
El punto (`.`) al final indica que se usará el directorio actual como contexto de construcción.

docker build -t mi-app:1.0 .
2️⃣ Ejecutar el contenedor
Este comando crea y ejecuta un contenedor a partir de la imagen que acabas de construir:

docker run --rm -p 8000:8000 mi-app:1.0
Explicación de las opciones:

--rm: elimina automáticamente el contenedor cuando se detiene.

-p 8000:8000: conecta el puerto 8000 del host con el puerto 8000 del contenedor.

Una vez ejecutado, puedes acceder a la aplicación en tu navegador:
👉 http://localhost:8000

🛠️ Cómo usar el entorno de desarrollo
El entorno de desarrollo se ha configurado con docker-compose, lo que permite trabajar de forma coherente en cualquier máquina sin preocuparse por versiones o dependencias locales.

1️⃣ Construir la imagen de desarrollo
Este comando lee el docker-compose.yml y crea la imagen dev a partir de las instrucciones del Dockerfile.dev:

docker compose build dev
2️⃣ Ejecutar comandos dentro del entorno
Para ejecutar cualquier comando dentro del contenedor de desarrollo (por ejemplo, abrir una terminal bash):

docker compose run --rm dev bash
Explicación:

docker compose run: ejecuta un comando en un contenedor temporal.

--rm: elimina el contenedor al cerrarse para mantener limpio el entorno.

✅ Cómo ejecutar tests y formateo
Todas las herramientas de calidad del código se ejecutan dentro del contenedor de desarrollo para garantizar que el entorno sea uniforme y reproducible.

🧪 Ejecutar Tests (Pytest)
Para lanzar los tests definidos en la carpeta /tests:

docker compose run --rm -T dev pytest
🧹 Verificar Formato del Código (Black)
Para comprobar si el código cumple con las reglas de estilo:

docker compose run --rm -T dev black --check .
🧰 Formatear Código Automáticamente (Black)
Para aplicar el formato correcto a todo el código automáticamente:

docker compose run --rm -T dev black .
⚙️ Hook Pre-commit
Este repositorio utiliza un hook pre-commit de Git para automatizar la revisión del código antes de que se confirme en el historial.

🔁 ¿Cómo actúa?
Cada vez que ejecutas un commit con:

git commit -m "mensaje del commit"
el hook se activa automáticamente y realiza dos comprobaciones:

Formateo: ejecuta Black para asegurarse de que todo el código sigue el estilo correcto.

Tests: ejecuta Pytest para verificar que todos los tests pasen correctamente.

Si alguna de estas comprobaciones falla, el commit será bloqueado ⛔ y se mostrará un mensaje de error en la terminal.
Esto evita subir código mal formateado o con errores a Git, garantizando la calidad y estabilidad del proyecto.
