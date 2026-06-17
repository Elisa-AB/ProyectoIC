# Design Spec / Contrato de Especificaciones Técnicas: Automated Cover Project Pipeline

## 1. Objective / Objetivo
El objetivo de este proyecto es diseñar, validar e implementar una pipeline de `Integración Continua y Entrega Continua (CI/CD)` para el despliegue de la `carátula digital interactiva` de nuestra entrega académica. El sistema garantiza la filosofía `Fail-Fast`: impide de forma absoluta que cualquier cambio en la portada que viole las restricciones sintácticas o los strings de la cátedra llegue al entorno de producción, manteniendo informados a los desarrolladores mediante alertas síncronas multicanal.

## 2. Tech Stack / Tecnologías Utilizadas
* **Entorno de Ejecución (Runtime):** `Node.js v22`
* **Gestor de Paquetes (Package Manager):** `npm v10+`
* **Motor de IC/CD (CI/CD Engine):** `GitHub Actions (Runner: ubuntu-latest)`
* **Linter Sintáctico (Linter):** `html-validate (Instalado localmente)`
* **Hosting de Producción (Production Environment):** `Render (Static Site)`
* **Canales de Alertas (Feedback Channels):** `Slack Webhooks API & Gmail Service`

## 3. Commands / Comandos de Consola
* **Aprovisionamiento (Setup):** `npm install`
* **Análisis Estático (Lint):** `npm run lint`
* **Suite de Pruebas (QA completo):** `npm test`
* **Prueba Específica Requerida:** `npm run test:spec-profesor`
* **Prueba Específica Prohibida:** `npm run test:spec-prohibido`

## 4. Project Structure / Estructura del Proyecto
* `ProyectoIC/`
* `├── .github/`
* `│   └── workflows/`
* `│       └── ci-cd.yml` (Orquestador central del pipeline de GitHub Actions)
* `├── node_modules/` (Dependencias de desarrollo locales - Ignorado en Git)
* `├── .gitignore` (Epecificación de la exclusión de ciertos archivos)
* `├── index.html` (Código fuente de la carátula digital interactiva en Producción)
* `├── package-lock.json` (Snapshot determinista y repetible de las dependencias)
* `├── package.json` (Manifiesto del proyecto, metadatos y alias de scripts de QA)
* `├── README.md` (Documentación viva de la arquitectura general del sistema)
* `├── spec.md` (Contrato de especificaciones)
* `├── rules.json` (Contrato de requerimientos de strings)
* `└── style.css` (Hoja de estilos de diseño de la carátula digital)

## 5. Code Style / Estilo de Código y Convenciones
El documento `index.html` (nuestra carátula) debe cumplir con la especificación estructural de `HTML5`. Las etiquetas deben cerrarse correctamente, los atributos deben estar parametrizados con comillas y no se permiten caracteres especiales o ampersands (`&`) sueltos sin codificar en entidades HTML (regla estricta de `html-validate`).

Ejemplo de código válido según el estándar del linter:
`<!DOCTYPE html>`
`<html lang="es">`
`<head>`
`    <meta charset="UTF-8">`
`    <title>Carátula Digital - Ingeniería de Software</title>`
`</head>`
`<body>`
`    <h1>Entrega de Elisa Brites</h1>`
`    <p> Integración Continua &amp; Entrega Continua</p>`
`</body>`
`</html>`

## 6. Testing Strategy / Estrategia de Pruebas
* **Framework:** `Scripts utilitarios nativos basados en Node.js y comandos del sistema operativo (grep).`
* **Ubicación de Criterios:** `rules.json en la raíz del proyecto.`
* **Niveles de Test:**
* **Fase Sintáctica (Lint):** `Bloquea el pipeline si la carátula presenta advertencias o errores de estructura HTML.`
* **Fase Funcional (Test):** `Escaneo dinámico en la carátula por exclusión/inclusión de strings. El código de salida de grep determina el éxito.`

## 7. Success Criteria / Criterios de Éxito
* **Criterio 1:** `El comando npm run lint se ejecuta sobre la portada y devuelve código de salida 0 sin advertencias.`
* **Criterio 2:** `El comando npm test localiza la cadena obligatoria de la cátedra dentro de la portada y no encuentra cadenas prohibidas.`
* **Criterio 3:** `Al impactar cambios en la rama main, Render actualiza la carátula digital en producción en menos de 50 segundos.`
* **Criterio 4:** `En caso de falla forzada en la portada, llega un reporte de incidentes dinámico a Slack y un correo a Gmail.`