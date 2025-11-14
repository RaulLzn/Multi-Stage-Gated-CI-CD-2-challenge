# Reto: Pipeline de Despliegue con Puertas y Roles (Multi-Stage Gated CI/CD)

## 🎯 Objetivo

Tu reto es crear un pipeline de despliegue automatizado usando GitHub Actions que siga un flujo de CI/CD con múltiples etapas y una puerta de aprobación manual para el entorno de producción.

## 📜 Misión

Crea el archivo `.github/workflows/deploy.yml` que contenga tres Jobs con las siguientes características:

1.  **`build`**:
    *   Este Job debe simular la construcción de un artefacto (por ejemplo, una imagen de Docker).
    *   No tiene dependencias.

2.  **`deploy-dev`**:
    *   Este Job debe desplegar a un entorno de desarrollo.
    *   Debe depender (`needs:`) del éxito del Job `build`.
    *   Debe usar el `environment: Dev`.

3.  **`deploy-prod`**:
    *   Este Job debe desplegar al entorno de producción.
    *   Debe depender (`needs:`) del éxito del Job `deploy-dev`.
    *   Debe usar el `environment: Production`.

## ✅ Criterios de Aceptación

*   El archivo `.github/workflows/deploy.yml` debe existir.
*   El pipeline debe contener los tres Jobs: `build`, `deploy-dev`, y `deploy-prod`.
*   El Job `deploy-dev` debe tener la dependencia `needs: build`.
*   El Job `deploy-prod` debe tener la dependencia `needs: deploy-dev`.
*   El Job `deploy-dev` debe usar el `environment: Dev`.
*   El Job `deploy-prod` debe usar el `environment: Production`.

## 🚀 Cómo Empezar

1.  Crea una nueva rama para tu solución.
2.  Crea el archivo `.github/workflows/deploy.yml` con la estructura de Jobs requerida.
3.  Abre una Pull Request a `main` para que el validador automático revise tu trabajo.

¡Buena suerte!
