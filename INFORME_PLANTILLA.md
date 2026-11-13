# INFORME TÉCNICO – CONFIGURACIÓN DE CLÚSTER CON KUBERNETES (P2)

**Estudiante:** [Escribir nombre]
**Asignatura:** [Escribir asignatura]
**Tema:** Deployment y ReplicaSet con Redis
**Repositorio público:** [Pegar URL real]
**Fecha:** [Fecha de entrega]

## 1. Introducción
Este trabajo presenta un despliegue académico de Redis en un clúster local de Kubernetes para comprender cómo un Deployment administra un ReplicaSet y mantiene la cantidad de Pods configurada. La práctica incluye pruebas de ejecución, escalado y recuperación automática.

## 2. Objetivo general
Configurar Redis en Kubernetes mediante Deployment y demostrar el funcionamiento del ReplicaSet asociado.

## 3. Objetivos específicos
- Crear y verificar un clúster local con Minikube.
- Desplegar Redis mediante un archivo YAML.
- Comprobar que el ReplicaSet mantiene dos Pods.
- Ejecutar comandos básicos de Redis.
- Escalar el Deployment y observar la recuperación al eliminar un Pod.

## 4. Conceptos principales
**Kubernetes:** plataforma para orquestar contenedores.
**Pod:** unidad mínima que ejecuta uno o más contenedores.
**Deployment:** recurso que define y administra el estado deseado de una aplicación.
**ReplicaSet:** recurso que mantiene la cantidad especificada de Pods y suele ser administrado por un Deployment.
**Redis:** motor de datos en memoria usado en caché, datos temporales y otros servicios.

## 5. Herramientas utilizadas
Docker Desktop, Minikube, PowerShell, Visual Studio Code, imagen oficial `redis:7.4-alpine` y GitHub.

## 6. Procedimiento y evidencias
### 6.1 Preparación del clúster
[Explicar pasos ejecutados e insertar captura 01]

### 6.2 Creación del Deployment
[Explicar contenido del YAML e insertar captura 02]

### 6.3 Verificación del Deployment y ReplicaSet
[Incluir salida real y captura 03]

### 6.4 Pruebas funcionales de Redis
[Incluir PING, SET y GET, captura 04]

### 6.5 Escalado de 2 a 3 Pods
[Explicar qué cambió y mostrar captura 05]

### 6.6 Eliminación y recuperación de un Pod
[Anotar el nombre del Pod eliminado y el del nuevo Pod; mostrar captura 06]

## 7. Usos de Redis
- Caché de información consultada frecuentemente.
- Almacenamiento temporal de sesiones.
- Contadores y datos de acceso rápido.
- Otras aplicaciones que necesitan consultar datos con poca latencia.

## 8. Limitaciones del ejercicio
Este ejemplo administra varias instancias independientes de Redis, no un conjunto Redis con replicación de datos. No incluye almacenamiento persistente ni seguridad de producción. El reemplazo automático de un Pod no garantiza conservar los datos que contenía.

## 9. Resultados
[Describir únicamente resultados realmente observados: disponibilidad del clúster, Pods listos, respuestas de Redis, escalado y reemplazo automático.]

## 10. Conclusiones
[Explicar con tus palabras qué aprendiste sobre Deployment, ReplicaSet y recuperación automática.]

## 11. Repositorio público
[Insertar URL de GitHub y captura 07]

## 12. Referencias
- Kubernetes. Documentación oficial de Deployments y ReplicaSets. https://kubernetes.io/docs/
- Minikube. Getting started. https://minikube.sigs.k8s.io/docs/start/
- Redis. Documentación oficial. https://redis.io/docs/latest/
