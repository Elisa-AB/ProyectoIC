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
   * Realiza un análisis estático de infraestructura simulado (`npm run lint`).
   * Levanta un contenedor virtualizado (`ubuntu-latest`) con el entorno de ejecución **Node.js v20**.
   * Instala dependencias y ejecuta la suite de pruebas dinámicas funcionales (`npm test`).
   * **Sistema de Feedback:** En caso de que cualquier paso falle, se intercepta el estado del ciclo de vida y se despacha una notificación síncrona en tiempo real al canal de **Slack**.

2. **Entrega Continua (CD - `deploy`):**
   * Requiere estrictamente que la etapa de CI finalice con éxito (`needs: build-and-test`).
   * Posee un filtro de entorno de seguridad estricto (`if: github.ref == 'refs/heads/main'`), asegurando que solo los cambios consolidados en la rama de producción sean elegibles para el despliegue.
   * Compila y despliega el artefacto de forma automatizada en **GitHub Pages**.
   * Envía una notificación de éxito a Slack con la URL productiva del sitio.

---

##  Suite de Calidad y Pruebas (QA)
Las aserciones de calidad se ejecutan a nivel de caja negra sobre el documento `index.html` utilizando los criterios definidos en el archivo de especificaciones de la cátedra (`spec.json`):

* **Prueba de Requisito Exigido (`test:spec-profesor`):** Valida dinámicamente mediante herramientas de consola la presencia del string obligatorio parametrizado por la cátedra.
* **Prueba de Restricción (`test:spec-prohibido`):** Realiza un escaneo por exclusión para garantizar que ninguna palabra vetada haya sido integrada en el código fuente.

---

##  Guía de Ejecución Local

Para replicar el comportamiento del servidor de IC en un entorno local, ejecute los siguientes comandos en su terminal:

### Pre-requisitos
* Tener instalado [Node.js](https://nodejs.org/) (Versión 20 recomendada).
* Consola basada en UNIX (Git Bash, Terminal de macOS o Linux).

### Pasos para la instalación
```bash
# 1. Clonar el repositorio
git clone [https://github.com/Elisa-AB/ProyectoIC.git](https://github.com/Elisa-AB/ProyectoIC.git)

# 2. Navegar al directorio del proyecto
cd ProyectoIC

# 3. Instalar las dependencias del manifiesto
npm install
