# Gianmarco Kubernetes Lab
Repositorio para el desarrollo de la actividad de Kubernetes de OAS.

Objetos fundamentales de Kubernetes

Durante el laboratorio se trabajó con los siguientes objetos.

## Pods

Un Pod es la unidad más pequeña desplegable en Kubernetes.
Representa uno o más contenedores que comparten red y almacenamiento.

Comando utilizado:

```
kubectl get pods
```

---

## ReplicaSets

Un ReplicaSet garantiza que un número específico de Pods se mantenga en ejecución.

Su función principal es mantener la disponibilidad de la aplicación.

Relación:

Deployment → ReplicaSet → Pods

---

## Deployments

Un Deployment administra los ReplicaSets y permite:

* Escalar aplicaciones
* Actualizar versiones
* Realizar rollback de despliegues

Comando utilizado:

```
kubectl get deployments
```

---

## Services

Un Service permite exponer una aplicación que corre dentro de Pods.

Debido a que los Pods tienen direcciones IP dinámicas, el Service proporciona un punto de acceso estable.

Comando utilizado:

```
kubectl get services
```

## ConfigMaps

Un ConfigMap permite almacenar configuración no sensible para aplicaciones.

Se utilizó para definir variables de entorno.

---

## Secrets

Un Secret permite almacenar información sensible como contraseñas o tokens.

Se utilizó para simular credenciales de base de datos.


## Preguntas durante la actividad

### ¿Cuántos nodos tiene tu clúster?
Al realizar un litado de Nodos al iniciar minukub sólo tendremos uno:

```
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   91s   v1.35.1
```

### ¿Qué pods se ejecutan por defecto?

Se ejecutan 7 Pods al iniciar minikube:
```
ubuntu@ip-172-31-67-54:~$ kubectl get pods -A
NAMESPACE     NAME                               READY   STATUS    RESTARTS      AGE
kube-system   coredns-7d764666f9-8vtmk           1/1     Running   1 (37s ago)   103s
kube-system   etcd-minikube                      1/1     Running   1 (35s ago)   111s
kube-system   kube-apiserver-minikube            1/1     Running   1 (32s ago)   111s
kube-system   kube-controller-manager-minikube   1/1     Running   1 (42s ago)   111s
kube-system   kube-proxy-nxpsx                   1/1     Running   1 (42s ago)   104s
kube-system   kube-scheduler-minikube            1/1     Running   1 (42s ago)   111s
kube-system   storage-provisioner                1/1     Running   2 (42s ago)   105s
```

### CPU y Memoria del Nodo

```
Capacity:
  cpu:                2
  ephemeral-storage:  20134592Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             3928836Ki
  pods:               110
```

### ¿Quién crea los Pods?

Los Pods son creados indirectamente por el Deployment, a través de un ReplicaSet.

### Flujo de creación

- El Deployment define el estado deseado (por ejemplo: número de réplicas, imagen, configuración).
- El Deployment crea y gestiona un ReplicaSet.
- El ReplicaSet es el responsable directo de crear y mantener los Pods.

## Preguntas al final de la actividad

### Por qué usar Deployment en lugar de Pod?

Porque un Deployment proporciona gestión declarativa y resiliencia, mientras que un Pod es un recurso básico sin control automático.

### Comparación:

Pod	Deployment
Manual	Automatizado
No se recupera solo	Se autorecupera
No escala	Escala fácilmente
No versionado	Permite updates

Un Pod es útil para pruebas, pero en entornos reales se usa Deployment.

### ¿Qué problema resuelve un Service?

Un Service resuelve el problema de la conectividad y estabilidad de acceso a los Pods.

### Problema sin Service:

Los Pods tienen IPs dinámicas
Cuando un Pod muere, cambia su IP

### Solución con Service:

Proporciona una IP estable
Actúa como load balancer interno
Permite acceso externo (NodePort)

El Service desacopla el acceso de la vida de los Pods.

### ¿Cómo se relaciona esto con AWS EC2?

La relación es directa:

Componente	Equivalente
EC2	Infraestructura (servidor)
Kubernetes	Orquestador
Pod	Aplicación/contenedor
Service	Punto de acceso


EC2 proporciona la máquina (IaaS).
Kubernetes gestiona las aplicaciones dentro de esa máquina. Es la base, Kubernetes es la capa de gestión.

### ¿Qué pasaría si un Pod falla?

El sistema lo reemplaza automáticamente.

- El Pod falla
- El ReplicaSet detecta que falta una réplica
- Crea un nuevo Pod

Gracias a ello, la aplicación sigue disponible se mantiene el estado deseado.

Esto solo ocurre si usas Deployment/ReplicaSet, no con Pods sueltos.