# Reto: Arquitectura de Pipeline de Despliegue (CI/CD)

¡Bienvenido a un desafío de nivel profesional! Este reto no se trata solo de ejecutar comandos, sino de **diseñar la arquitectura** de un proceso de despliegue seguro y automatizado.

---

## El Contexto: ¿Por qué este reto?

En el mundo real, nunca desplegamos código directamente a los usuarios finales (Producción). Hacerlo sería increíblemente arriesgado. Un bug podría tumbar el servicio, afectar a miles de usuarios y costar mucho dinero.

Para evitar esto, usamos **Pipelines de Despliegue Multi-Etapa**:

1.  **Construimos (Build):** Primero, nos aseguramos de que el código no tenga errores de sintaxis y se pueda "compilar" o empaquetar.
2.  **Probamos en Desarrollo (Dev):** Desplegamos el código en un entorno interno, `Dev`, que es una copia de producción pero sin usuarios reales. Aquí se ejecutan pruebas más profundas.
3.  **Lanzamos a Producción (Prod):** Solo cuando estamos seguros de que todo funciona en `Dev`, permitimos que el código pase a `Production`.

Este proceso secuencial es una **arquitectura de seguridad**. Tu misión es construir esa arquitectura usando GitHub Actions.

## La Misión: ¿Qué vas a construir?

Tu objetivo es crear un único archivo de flujo de trabajo, `.github/workflows/deploy.yml`, que defina esta arquitectura de tres etapas.

El pipeline debe forzar el siguiente orden usando **dependencias (`needs:`)** y debe apuntar a los entornos correctos usando la clave **`environment:`**.

El repositorio ya tiene dos entornos pre-configurados: `Dev` y `Production`. El entorno `Production` es crítico y requiere una aprobación especial, tu pipeline debe estar listo para manejar esto.

---

## El Cómo: Guía Paso a Paso

Sigue estas instrucciones para completar el reto.

### Paso 1: Prepara tu Rama de Trabajo

Nunca trabajes directamente en la rama `main`.

1.  Asegúrate de tener la última versión del repositorio:
    ```bash
    git checkout main
    git pull
    ```
2.  Crea tu propia rama para la solución (usa un nombre que te identifique):
    ```bash
    git checkout -b solucion-juan-perez
    ```

### Paso 2: Crea tu Archivo de Pipeline

El validador automático buscará tu solución en una ruta específica.

1.  Crea la estructura de carpetas (si no existe):
    ```bash
    mkdir -p .github/workflows
    ```
2.  Crea el archivo `deploy.yml` donde vivirás tu solución:
    ```bash
    touch .github/workflows/deploy.yml
    ```

### Paso 3: Diseña la Arquitectura del Pipeline

Abre tu archivo `.github/workflows/deploy.yml` y define la estructura. Tu archivo debe comenzar con un nombre y definirse para que se active en `pull_request` (¡aunque el validador ya lo hace por ti!).

**Lo más importante es la sección `jobs:`**. Debes crear **tres** jobs con estos nombres y reglas exactas:

**1. El Job `build`:**
* **Propósito:** Simula la construcción y prueba inicial del código.
* **Reglas:**
    * Debe llamarse `build:`.
    * No tiene dependencias (`needs:`).

**2. El Job `deploy-dev`:**
* **Propósito:** Despliega en el entorno de Desarrollo.
* **Reglas:**
    * Debe llamarse `deploy-dev:`.
    * Debe depender del éxito del primer job: **`needs: build`**.
    * Debe apuntar al entorno de desarrollo: **`environment: Dev`**.

**3. El Job `deploy-prod`:**
* **Propósito:** Despliega en el entorno de Producción (la etapa final).
* **Reglas:**
    * Debe llamarse `deploy-prod:`.
    * Debe depender del éxito del job anterior: **`needs: deploy-dev`**.
    * Debe apuntar al entorno de producción: **`environment: Production`**.

> **Ejemplo de sintaxis de dependencia y entorno:**
> ```yaml
>   mi_job_dependiente:
>     needs: mi_job_anterior
>     environment: MiEntorno
>     steps:
>       - ...
> ```

### Paso 4: Envía tu Solución para Validación

Cuando creas que tu `deploy.yml` tiene la arquitectura correcta:

1.  Guarda tus cambios y haz commit:
    ```bash
    git add .github/workflows/deploy.yml
    git commit -m "Solución: Creando pipeline de 3 etapas"
    ```
2.  Sube tu rama al repositorio:
    ```bash
    git push --set-upstream origin solucion-juan-perez
    ```
3.  Ve a GitHub y **abre una nueva Pull Request (PR)** desde tu rama (`solucion-juan-perez`) hacia `main`.

### Paso 5: Revisa el Feedback del Validador

No necesitas que nadie apruebe tu PR. El sistema automático es tu única guía.

1.  En cuanto abras la PR, el validador automático (llamado **`🛡️ Architecture Validator`**) se ejecutará.
2.  Ve a la pestaña "Checks" (Verificaciones) en la parte inferior de tu PR.
3.  **Si ves un ✅ verde:** ¡Felicidades! Tu arquitectura de pipeline es correcta y has superado el reto.
4.  **Si ves un ❌ rojo:** ¡No hay problema! Significa que el validador encontró un error en tu arquitectura.
    * Haz clic en "Details" (Detalles) junto al job que falló.
    * Lee el log. Te dirá exactamente qué regla falló (ej: "❌ ERROR: El Job 'deploy-prod' debe incluir la sección 'needs:'").
    * Vuelve a tu editor de código, arregla el `deploy.yml` en tu rama, haz `commit` y `push` de nuevo. La PR se actualizará automáticamente y el validador volverá a correr.
