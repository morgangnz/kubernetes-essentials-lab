# Kubernetes Essentials Lab

## Objetivo

Laboratorio práctico para aprender Kubernetes desde cero utilizando k3s sobre WSL2.

El objetivo es desplegar y administrar aplicaciones contenerizadas utilizando los principales recursos de Kubernetes.

## Tecnologías

* Ubuntu 24.04 (WSL2)
* Docker
* Kubernetes (k3s)
* kubectl
* VS Code

## Contenido

* [x] Instalación del entorno
* [x] Primer Pod
* [x] Deployments
* [x] ReplicaSets
* [x] Services
* [x] ConfigMaps
* [x] Secrets
* [x] Persistent Volumes (PVC)
* [x] Health Checks (Readiness / Liveness Probes)
* [x] Resource Requests y Limits
* [x] Rolling Updates
* [ ] Rollbacks
* [ ] Ingress

## Arquitectura actual

El proyecto despliega una aplicación NGINX mediante Kubernetes:

* **Namespace:** nginx-lab
* **Deployment:** nginx-deployment
* **ReplicaSet:** gestiona las réplicas creadas por el Deployment.
* **Pods:** 3 réplicas ejecutando contenedores NGINX.
* **Service NodePort:** expone la aplicación fuera del clúster.
* **ConfigMap:** proporciona configuración no sensible.
* **Secret:** almacena datos sensibles.
* **PersistentVolumeClaim:** proporciona almacenamiento persistente compartido.

Flujo de tráfico:

```
Usuario
   |
   v
NodePort
   |
   v
Service
   |
   v
Pods NGINX
```

## Estado actual del despliegue

Namespace:

```
nginx-lab
```

Estado:

```
3/3 Pods Running
```

Componentes activos:

```
Deployment: nginx-deployment

Service:
Type: NodePort
Port: 80
NodePort: 31744

Storage:
PVC: nginx-storage
Status: Bound
```

## Rolling Updates

El Deployment utiliza ReplicaSets para gestionar actualizaciones.

Cuando se modifica la configuración del Deployment, Kubernetes crea un nuevo ReplicaSet y realiza una actualización progresiva:

```
Deployment
     |
     +---- ReplicaSet antiguo
     |          |
     |          Pods antiguos
     |
     +---- ReplicaSet nuevo
                |
                Pods nuevos
```

Los ReplicaSets antiguos se mantienen como historial para permitir Rollbacks.

## Recursos del contenedor

El contenedor NGINX tiene definidos recursos:

```
Requests:
  CPU: 100m
  Memory: 64Mi

Limits:
  CPU: 250m
  Memory: 128Mi
```

## Comprobación del estado

Para revisar el despliegue:

```bash
kubectl get all -n nginx-lab
```
