# Redis en Kubernetes: Deployment y ReplicaSet (P2)

## Objetivo
Demostrar el uso de Deployment y ReplicaSet en un clúster local de Kubernetes mediante dos instancias de Redis, sus pruebas básicas, escalado y recuperación automática de Pods.

## Herramientas
- Docker Desktop (motor de contenedores)
- Minikube (clúster Kubernetes local)
- PowerShell (terminal)
- Kubernetes CLI: `minikube kubectl --` o `kubectl`
- Imagen: `redis:7.4-alpine`

## Importante
Este ejercicio es **académico**. Se ejecutan **dos instancias Redis independientes** para comprobar cómo Kubernetes administra el número de Pods. **NO** son réplicas de datos de Redis y no están configuradas para producción. No almacenar información sensible. Para un Redis con persistencia y replicación reales deben configurarse mecanismos adecuados como Redis replication/Redis Cluster y almacenamiento persistente, normalmente con StatefulSet.

## 1. Preparar el clúster
Con Docker Desktop abierto:

```powershell
minikube start --driver=docker
minikube status
minikube kubectl -- get nodes
```

En PowerShell se puede definir una función temporal para usar los comandos del resto de la guía:

```powershell
function kubectl { minikube kubectl -- @args }
```

Si ya tienes `kubectl` instalado y configurado para el contexto de Minikube, la función no es necesaria. Comprueba el contexto:

```powershell
kubectl config current-context
```

## 2. Aplicar YAML
Desde la raíz del repositorio:

```powershell
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/redis-deployment.yaml
kubectl rollout status deployment/redis-demo -n tarea-p2 --timeout=180s
kubectl get deployments,replicasets,pods -n tarea-p2 -o wide
```

Debes observar el `Deployment`, el `ReplicaSet` y dos Pods en estado `Running` y listos `1/1`.

## 3. Probar Redis
Selecciona una instancia Redis concreta para que las operaciones se ejecuten en el mismo Pod:

```powershell
$pod = kubectl get pods -n tarea-p2 -l app=redis-demo -o jsonpath='{.items[0].metadata.name}'
kubectl exec -n tarea-p2 $pod -- redis-cli PING
kubectl exec -n tarea-p2 $pod -- redis-cli SET materia "DevOps-P2"
kubectl exec -n tarea-p2 $pod -- redis-cli GET materia
```

Resultados esperados: `PONG`, `OK` y `DevOps-P2`. No hay necesidad de instalar Redis en Windows.

## 4. Observar ReplicaSet y escalar

```powershell
kubectl describe deployment redis-demo -n tarea-p2
kubectl get replicaset -n tarea-p2
kubectl scale deployment redis-demo --replicas=3 -n tarea-p2
kubectl get pods -n tarea-p2 -o wide
```

Deben aparecer tres Pods. El escalado prueba que `Deployment` y `ReplicaSet` coordinan la cantidad deseada.

## 5. Comprobar autorrecuperación

```powershell
$podAntes = kubectl get pods -n tarea-p2 -l app=redis-demo -o jsonpath='{.items[0].metadata.name}'
Write-Output "Pod que se va a eliminar: $podAntes"
kubectl delete pod $podAntes -n tarea-p2
kubectl get pods -n tarea-p2 -w
```

Presiona `Ctrl+C` después de observar que Kubernetes creó otro Pod. Verifica:

```powershell
kubectl get pods -n tarea-p2
```

El objetivo es volver a 3 Pods. El nuevo Pod no conserva necesariamente los datos del eliminado: la prueba es de **recuperación del proceso**, no de persistencia de datos.

## 6. Dejar el estado como el YAML del repositorio

```powershell
kubectl scale deployment redis-demo --replicas=2 -n tarea-p2
kubectl get deployments,replicasets,pods -n tarea-p2
```

## 7. Capturas sugeridas
Guardar imágenes reales en `evidencias/`:
1. `01-minikube-nodos.png` — Clúster activo (`minikube status` y `kubectl get nodes`).
2. `02-yaml-deployment.png` — Archivo YAML abierto en VS Code.
3. `03-deployment-replicaset-pods.png` — Recursos creados y 2 Pods.
4. `04-redis-ping-set-get.png` — `PING`, `SET` y `GET` exitosos.
5. `05-escalado-tres-pods.png` — 3 Pods después de escalar.
6. `06-autorecuperacion.png` — Eliminación de uno y reemplazo automático.
7. `07-repositorio-publico.png` — Repositorio GitHub en modo público.

## 8. Publicar en GitHub
Crear un repositorio **público** vacío llamado `kubernetes-redis-deployment-replicaset` (sin README inicial), luego desde este proyecto:

```powershell
git init
git add .
git commit -m "Práctica Kubernetes: Deployment y ReplicaSet con Redis"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/kubernetes-redis-deployment-replicaset.git
git push -u origin main
```

Reemplaza `TU_USUARIO` por tu nombre real de usuario de GitHub. Si tu entorno te pide autenticación, usa el método configurado para tu cuenta. Comprueba después que el repositorio es accesible públicamente.

## 9. Informe y exposición
Usa la plantilla `INFORME_PLANTILLA.md` como estructura de informe. No inventes resultados; completa el informe con tus propias capturas y salidas obtenidas al ejecutar los comandos.

## Fuentes
- https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
- https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/
- https://minikube.sigs.k8s.io/docs/start/
- https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/
