# RiskTracker API Gateway (Kong)

Este repositorio contiene la configuración de `docker-compose` para desplegar el entorno completo del **API Gateway de RiskTracker**, utilizando **Kong** como núcleo.

Este entorno incluye todos los servicios de infraestructura necesarios para que los microservicios de la aplicación (cálculos, creación de usuarios, chatbot, etc.) funcionen de manera segura y centralizada.

## Arquitectura del Entorno

Este `docker-compose` levanta los siguientes servicios:
-   **`kong-gateway`**: El API Gateway principal que gestiona todo el tráfico.
-   **`kong-db`**: Una base de datos PostgreSQL dedicada para que Kong almacene su configuración (servicios, rutas, consumidores, etc.).
-   **`rabbitmq`**: Un gestor de colas de mensajes (message broker) para la comunicación asíncrona entre servicios.
-   **`kong-migrations`**: Un trabajo puntual que prepara la base de datos de Kong antes de que el gateway se inicie.

## 1. Prerrequisitos

Para poder ejecutar este entorno, necesitas tener instalado el siguiente software en tu sistema:
-   **Docker:** El motor para ejecutar contenedores. [Descargar Docker Desktop](https://www.docker.com/products/docker-desktop/).
-   **Docker Compose:** La herramienta para orquestar los contenedores. Viene incluida con Docker Desktop.

## 2. Instrucciones de Instalación

Sigue estos pasos para levantar el entorno completo.

### Paso 1: Configurar las Variables de Entorno

Este proyecto utiliza un archivo `.env` para gestionar las credenciales y configuraciones sensibles de forma segura, evitando que queden expuestas en el código.

1.  **Crea tu archivo `.env`:** Busca el archivo llamado `.env.example` en este directorio. Haz una copia de ese archivo y renómbrala a **`.env`**.

2.  **Edita el archivo `.env`:** Abre tu nuevo archivo `.env` con un editor de texto. Verás una lista de variables de entorno con valores de ejemplo. **Debes reemplazar todas las contraseñas de ejemplo (`cambiame_...`) por contraseñas seguras y únicas.**

    **¡Advertencia de Seguridad!** El archivo `.env` contiene información sensible. **NUNCA** debes subir este archivo a un repositorio de Git público o compartido. El archivo `.gitignore` de este proyecto ya debería estar configurado para ignorarlo.

### Paso 2: Construir y Ejecutar los Contenedores

Una vez que tu archivo `.env` esté configurado con tus contraseñas seguras, estás listo para iniciar todo el sistema.

1.  **Abre una terminal** en el directorio raíz de este proyecto (donde se encuentra el archivo `docker-compose.yml`).

2.  Ejecuta el siguiente comando:

    ```bash
    docker-compose up -d
    ```
    -   `up`: Lee el `docker-compose.yml`, descarga las imágenes necesarias y crea e inicia los contenedores.
    -   `-d`: (Detached mode) Ejecuta los contenedores en segundo plano y te devuelve el control de la terminal.

3.  **Verifica el estado:** Después de unos segundos, puedes verificar que todos los servicios están corriendo correctamente con el comando:

    ```bash
    docker ps
    ```
    Deberías ver una lista con los contenedores `kong-gateway`, `kong-db` y `rabbitmq`, todos con el estado `Up` o `running (healthy)`.

### Paso 3: Detener el Entorno

Cuando hayas terminado de trabajar y quieras apagar todos los servicios, ejecuta el siguiente comando desde el mismo directorio:

```bash
docker-compose down