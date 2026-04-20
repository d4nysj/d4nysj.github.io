---
layout: post
title: "Guía Definitiva: CI/CD con Jenkins y Kubernetes (K3s) en Proxmox desde cero"
date: 2026-04-20 12:00:00 +0200
categories: [DevOps, Tutoriales]
tags: [kubernetes, jenkins, proxmox, docker, cicd, k3s, lxc]
---

# 🚀 CI/CD con Jenkins y Kubernetes (K3s) en Proxmox desde cero

Montar un entorno de Integración y Despliegue Continuo (CI/CD) puede parecer intimidante al principio, especialmente cuando lidias con múltiples capas de virtualización. En este post documento paso a paso cómo construí mi propio laboratorio DevOps usando **Proxmox**, **Kubernetes (K3s)** y **Jenkins**.

> **Objetivo final:** crear una pipeline automatizada que despliegue una página web personalizada en un clúster de Kubernetes cada vez que hagamos una build en Jenkins.

---

## 📐 Arquitectura del Laboratorio

Una arquitectura estilo **Matrioshka** (capas dentro de capas):

```
Proxmox VE (Hipervisor)
└── VM Ubuntu (Servidor K3s)
    └── Contenedor LXC Ubuntu
        └── Contenedor Docker
            └── Jenkins
```

| Capa | Tecnología | Rol |
|---|---|---|
| Hipervisor | Proxmox VE | Base física |
| VM | Ubuntu | Clúster K3s |
| LXC | Ubuntu | Host Docker |
| Contenedor | Jenkins | Motor CI/CD |

---

## Fase 1 — Preparando Kubernetes (K3s)

Elegí **K3s** de Rancher porque es increíblemente ligero e ideal para laboratorios de un solo nodo.

### 1.1 Instalación

Accede por SSH a la VM de Ubuntu y ejecuta el instalador oficial:

```bash
curl -sfL https://get.k3s.io | sh -
```

### 1.2 Extraer el kubeconfig

Este archivo es el **"pasaporte"** que necesitará Jenkins para comunicarse con el clúster. Guárdalo.

```bash
cat /etc/rancher/k3s/k3s.yaml
```

> ⚠️ **Importante:** Reemplaza la IP `127.0.0.1` por la IP real del servidor K3s en tu red local.

---

## Fase 2 — Instalando Jenkins (LXC + Docker en Proxmox)

Levantamos un contenedor LXC en Proxmox, instalamos Docker dentro y arrancamos Jenkins.

### 2.1 Obtener la contraseña inicial de Jenkins

Al acceder por primera vez en el puerto `8080`, Jenkins pedirá una contraseña. Ejecútalo en la consola del LXC:

```bash
docker exec jenkins-master cat /var/jenkins_home/secrets/initialAdminPassword
```

### 2.2 Configuración inicial

En el asistente de Jenkins, selecciona **"Instalar plugins sugeridos"** y espera a que el entorno se prepare.

---

## Fase 3 — El Reto de las Comunicaciones (Kubectl dentro de Docker)

Este es el paso más crítico. Jenkins está atrapado dentro de un contenedor Docker, dentro de un LXC. Necesitamos darle la herramienta `kubectl` y el kubeconfig para que pueda hablar con K3s.

Ejecuta estos comandos **desde la consola del LXC** (fuera de Docker):

```bash
# 1. Copiar el binario de kubectl al contenedor y darle permisos
docker cp /usr/local/bin/kubectl jenkins-master:/usr/local/bin/kubectl
docker exec -u root jenkins-master chmod +x /usr/local/bin/kubectl

# 2. Crear el directorio .kube para el usuario jenkins
docker exec -u root jenkins-master mkdir -p /var/jenkins_home/.kube

# 3. Copiar el kubeconfig (k3s.yaml) que obtuvimos en la Fase 1
docker cp /root/.kube/config jenkins-master:/var/jenkins_home/.kube/config

# 4. Asignar los permisos correctos
docker exec -u root jenkins-master chown -R jenkins:jenkins /var/jenkins_home/.kube
```

---

## Fase 4 — Creando la Primera Pipeline (Infraestructura como Código)

### 4.1 Crear el proyecto

En el panel de Jenkins: **Nueva tarea → Pipeline**.

### 4.2 El Jenkinsfile

El siguiente script declara todo el despliegue. Usa un **ConfigMap** para inyectar HTML personalizado en Nginx y escala a **4 réplicas**:

```groovy
pipeline {
    agent any
    stages {
        stage('Desplegar Mi Propio Código') {
            steps {
                sh '''
                cat <<EOF | kubectl apply --kubeconfig=/var/jenkins_home/.kube/config -f -
---
# 1. ConfigMap con el HTML personalizado
apiVersion: v1
kind: ConfigMap
metadata:
  name: mi-web-html
data:
  index.html: |
    <!DOCTYPE html>
    <html>
    <body style="background-color: #2b303b; color: #42b983; text-align: center; padding: 100px; font-family: sans-serif;">
      <h1>🚀 ¡Hola Mundo desde mi miniPC con Proxmox!</h1>
      <h2>Acabo de dominar Kubernetes y Jenkins</h2>
    </body>
    </html>
---
# 2. Deployment con 4 réplicas de Nginx
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mi-web-pro
spec:
  replicas: 4
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
        - name: mi-volumen-html
          mountPath: /usr/share/nginx/html/index.html
          subPath: index.html
      volumes:
      - name: mi-volumen-html
        configMap:
          name: mi-web-html
---
# 3. Service de tipo NodePort para exponer la web
apiVersion: v1
kind: Service
metadata:
  name: mi-web-pro-svc
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 32000
EOF
                '''
            }
        }
    }
}
```

---

## Fase 5 — Verificando el Despliegue

Haz clic en **"Construir ahora"** en Jenkins. Una vez que la build termine en verde, verifica el estado del clúster desde la VM de K3s:

```bash
sudo kubectl get pods
```

Deberías ver **4 pods** de Nginx en estado `Running`. La web estará disponible en:

```
http://<IP-DEL-SERVIDOR-K3S>:32000
```

---

## 📋 Resumen del Flujo Completo

```
Jenkins (Docker/LXC)
    │
    │  kubectl apply (via kubeconfig)
    ▼
K3s API Server (VM)
    │
    ├── ConfigMap (HTML)
    ├── Deployment (4x Nginx)
    └── Service NodePort :32000
```

---

## 🔗 Referencias

- [K3s — Lightweight Kubernetes](https://k3s.io/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [Proxmox VE](https://www.proxmox.com/en/proxmox-virtual-environment/overview)
- [Kubernetes Docs — ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/)
