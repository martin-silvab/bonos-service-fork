# VidalCasino 2.0 - Bonos Service

Este repositorio contiene el microservicio **bonos-service** del proyecto VidalCasino 2.0, desarrollado para la evaluación EP3 de Introducción a Herramientas DevOps. Este servicio se encarga de gestionar los bonos disponibles para los usuarios y registrar bonos reclamados dentro del sistema.

## Descripción general

`bonos-service` permite consultar bonos, reclamar beneficios y registrar operaciones relacionadas con bonos dentro del ecosistema VidalCasino. El servicio se despliega dentro de Amazon EKS y permanece como componente interno del clúster mediante un Service de tipo `ClusterIP`.

El frontend consume este microservicio mediante rutas internas `/api/bonos`, gestionadas a través del proxy configurado en el frontend.

## Arquitectura del sistema

El sistema VidalCasino está compuesto por:

- **casino-frontend:** interfaz web expuesta mediante LoadBalancer.
- **casino-backend:** backend principal interno.
- **bonos-service:** microservicio encargado de bonos.
- **apuestas-service:** microservicio encargado de apuestas deportivas.
- **estadisticas-service:** microservicio encargado de estadísticas.
- **postgres:** base de datos interna.

## Tecnologías utilizadas

- Python
- FastAPI
- PostgreSQL
- Docker
- Kubernetes
- Amazon EKS
- Amazon ECR
- GitHub Actions
- AWS Academy Learner Lab

## Endpoints de salud

El servicio incluye rutas de salud para Kubernetes:

```txt
/livez
/readyz
/livez: verifica que el proceso esté vivo.
/readyz: verifica que el servicio esté listo para recibir tráfico y pueda conectarse correctamente a la base de datos.
Despliegue en Kubernetes

Los manifiestos Kubernetes se encuentran en:

k8s/

Archivos principales:

k8s/deployment.yaml
k8s/service.yaml

El servicio se despliega como Deployment y se expone internamente como ClusterIP en el puerto 8004.

CI/CD

El despliegue automático se realiza mediante GitHub Actions en:

.github/workflows/deploy.yml

El workflow se ejecuta al realizar un push sobre la rama deploy.

El pipeline realiza:

Descarga del código.
Configuración de credenciales AWS Academy.
Login en Amazon ECR.
Build de imagen Docker.
Push de imagen a Amazon ECR con tags latest, v1.0.1 y SHA del commit.
Conexión con el clúster EKS.
Actualización del Deployment.
Verificación del rollout.
Comandos de verificación
kubectl get deployment bonos-service
kubectl get svc bonos-service
kubectl get pods -l app=bonos-service -o wide
kubectl describe deployment bonos-service
Estado esperado
Deployment disponible.
Service interno tipo ClusterIP.
Pod en estado Running.
Imagen desplegada desde Amazon ECR.
Pipeline CI/CD exitoso.