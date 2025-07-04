# FAQ - Entorno Docker para Aplicación Web

## 📦 Conceptos Básicos 

### ¿Cuál es la diferencia entre una imagen y un contenedor?

| Concepto       | Características                                                                 |
|----------------|---------------------------------------------------------------------------------|
| **Imagen**     | - Plantilla inmutable<br>- Contiene aplicaciones y dependencias<br>- Se almacena en registros como Docker Hub |
| **Contenedor** | - Instancia en ejecución de una imagen<br>- Entorno aislado con sistema de archivos propio<br>- Efímero por defecto |

## 🎛 Docker Compose

### ¿Qué beneficios aporta Docker Compose?

✔ **Configuración centralizada** en un solo archivo `docker-compose.yml`  
✔ **Gestión de redes automática** entre servicios conectados  
✔ **Volúmenes persistentes** declarativos  
✔ **Variables de entorno** compartidas  
✔ **Órden de inicio** controlado con `depends_on`  
✔ **Escalado sencillo** con `docker-compose up --scale`

## 🔒 Seguridad

### Mecanismos de seguridad recomendados

```yaml
services:
  nginx:
    security_opt:
      - "no-new-privileges:true"
    read_only: true
    networks:
      - frontend-network
  postgres:
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
    secrets:
      - db_password
    networks:
      - backend-network

# Extensión del entorno para simular una app completa
Añadir backend: Incorporar un servicio de aplicación (Node.js, Python, Java) que sirva APIs al frontend.

Balanceador de carga: Usar Nginx o Traefik como reverse proxy para múltiples instancias del frontend/backend.

Caching: Añadir Redis o Memcached para manejo de sesiones o caché.

Monitorización: Integrar Prometheus + Grafana para métricas y ELK para logs.

CI/CD: Configurar un pipeline con GitHub Actions/GitLab CI para builds automatizados.

Entornos múltiples: Usar diferentes archivos compose para desarrollo, staging y producción.

Pruebas: Añadir contenedores efímeros para tests de integración.

Mensores de salud: Implementar healthchecks en los servicios para monitoreo automático.


## 🌐 Comunicación entre Servicios

### Mecanismo Básico de Comunicación

En Docker Compose, los servicios se comunican a través de:

1. **Redes Docker Automáticas**:
   - Por defecto, Compose crea una red bridge dedicada
   - Todos los servicios pueden encontrarse por su **nombre de servicio** (DNS interno)
   - Ejemplo de conexión desde Nginx a PostgreSQL:
     ```nginx
     # En configuración de Nginx
     location /api {
         proxy_pass http://backend:3000; # 'backend' es el nombre del servicio
     }
     ```

2. **Puertos Expuestos**:
   ```yaml
   services:
     frontend:
       ports:
         - "80:80"  # Host:Contenedor
     backend:
       expose:
         - "3000"   # Solo accesible internamente

