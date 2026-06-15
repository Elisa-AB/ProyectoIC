#  Proyecto IC - Segunda Instancia de Evaluación
> **Cátedra:** Ingeniería y Calidad del Software  
> **Institución:** Universidad Tecnológica Nacional (UTN)  
> **Rama Activa de Despliegue:** `main`

---

##  Descripción del Proyecto
Este repositorio contiene la infraestructura de **Integración Continua y Entrega Continua (CI/CD)** diseñada para la validación automatizada y el despliegue del artefacto web del segundo parcial. El proyecto implementa filosofías de desarrollo moderno como **Spec-Driven Development (SDD)** y estrategias de mitigación de errores en etapas tempranas (*Fail-Fast*).

---

##  Arquitectura del Pipeline de CI/CD
El flujo de automatización está desarrollado mediante **GitHub Actions** y se encuentra orquestado en dos fases críticas totalmente desacopladas:

1. **Integración Continua (CI - `build-and-test`):**
   * Se dispara automáticamente ante cualquier `push` o `pull_request` en las ramas `main` y `dev`.
   * Realiza un análisis estático estricto del código HTML mediante la herramienta real **`html-validate`** (`npm run lint`).
   * Levanta un contenedor virtualizado (`ubuntu-latest`) con el entorno de ejecución **Node.js v22**.
   * Instala dependencias y ejecuta la suite de pruebas dinámicas funcionales (`npm test`).
   * **Sistema de Feedback Multicanal:** Ante cualquier resultado de las pruebas (tanto éxito como falla), el pipeline intercepta el estado del ciclo de vida y despacha alertas síncronas en tiempo real: una notificación al canal de **Slack** y un reporte detallado vía correo electrónico a través de **Gmail**.

2. **Entrega Continua (CD - `deploy`):**
   * Requiere estrictamente que la etapa de CI finalice con éxito (`needs: build-and-test`).
   * Posee un filtro de entorno de seguridad estricto (`if: github.ref == 'refs/heads/main'`), asegurando que solo los cambios consolidados en la rama de producción sean elegibles para el despliegue.
   * Despliegue Desacoplado: En lugar de utilizar almacenamiento nativo del proveedor de Git, el pipeline interactúa de forma segura con **Render** mediante un **Deploy Hook (Webhook)** automatizado.
   * Feedback de Éxito: Al consolidarse el despliegue, envía una notificación síncrona a Slack incluyendo la URL productiva del sitio en Render.

---

##  Suite de Calidad y Pruebas (QA)
Las aserciones de calidad se ejecutan a nivel de caja negra sobre el documento `index.html` utilizando los criterios definidos en el archivo de especificaciones de la cátedra (`spec.json`):

* **Análisis Sintáctico (`npm run lint`):** Valida mediante `html-validate` que las etiquetas, atributos y estructura del documento cumplan con las especificaciones y buenas prácticas de la W3C de forma real.
* **Prueba de Requisito Exigido (`test:spec-profesor`):** Valida dinámicamente mediante herramientas de consola la presencia del string obligatorio parametrizado por la cátedra.
* **Prueba de Restricción (`test:spec-prohibido`):** Realiza un escaneo por exclusión para garantizar que ninguna palabra vetada haya sido integrada en el código fuente.

---

##  Guía de Ejecución Local

Para replicar el comportamiento del servidor de IC en un entorno local, ejecute los siguientes comandos en su terminal:

### Pre-requisitos
* Tener instalado [Node.js](https://nodejs.org/) (Versión 22 recomendada).
* Consola basada en UNIX (Git Bash, Terminal de macOS o Linux).

### Pasos para la instalación
```bash
# 1. Clonar el repositorio
git clone [https://github.com/Elisa-AB/ProyectoIC.git](https://github.com/Elisa-AB/ProyectoIC.git)

# 2. Navegar al directorio del proyecto
cd ProyectoIC

# 3. Instalar las dependencias del manifiesto
npm install

# 4. Ejecutar el análisis estático (Linter HTML real)
npm run lint

# 5. Ejecutar la suite de pruebas funcionales
npm test