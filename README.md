# Migration d’une Application WordPress vers Kubernetes  
## CI/CD – GitOps – Observabilité – Sécurité – Backup

### Description

Projet académique réalisé en Master 2 Réseaux et Télécommunication.

Ce projet consiste à migrer une application WordPress vers une architecture Cloud-Native basée sur Kubernetes, en intégrant les bonnes pratiques DevOps : conteneurisation, intégration continue (CI), déploiement continu (CD), GitOps, observabilité, sécurité et sauvegarde.

L’objectif est de construire une infrastructure automatisée, scalable, résiliente et supervisée en temps réel.

---

## Architecture & Technologies

L’architecture repose sur les composants suivants :

- **Docker** : création d’une image WordPress personnalisée (thème custom intégré)
- **Kubernetes (Minikube)** : orchestration des conteneurs
- **Helm** : chart personnalisé pour le déploiement WordPress & MariaDB
- **Jenkins** : pipeline d’intégration continue
- **ArgoCD** : déploiement continu GitOps (self-heal activé)
- **Prometheus & Grafana** : monitoring et métriques du cluster
- **Loki** : centralisation et analyse des logs
- **Velero** : sauvegarde des ressources Kubernetes
- **RBAC & NetworkPolicies** : sécurisation du cluster
- **Docker Hub** : registry des images
- **GitHub** : source de vérité (GitOps)

---

## Fonctionnalités mises en œuvre

- Build automatique de l’image Docker via Jenkins
- Push sécurisé vers Docker Hub
- Déploiement automatisé avec Helm (`helm upgrade --install`)
- Synchronisation GitOps automatique avec ArgoCD
- Autoscaling avec Horizontal Pod Autoscaler (HPA)
- Gestion du stockage persistant (PVC)
- Monitoring complet du cluster et de l’application
- Centralisation des logs avec Loki
- Sauvegarde planifiée avec Velero
- Mise en place de politiques RBAC et NetworkPolicies

---

## Pipeline CI/CD (Jenkins)

Le pipeline Jenkins utilise un agent Kubernetes dynamique avec Docker-in-Docker.

Étapes automatisées :

1. Clone du repository GitHub
2. Build de l’image Docker personnalisée
3. Push vers Docker Hub
4. Déploiement Helm sur le cluster Kubernetes

Le pipeline permet une mise à jour automatique de l’application à chaque modification du code.

---

## Déploiement GitOps (ArgoCD)

ArgoCD surveille en continu le repository GitHub.

- Synchronisation automatique activée
- Self-heal en cas de dérive
- Mise à jour automatique du cluster après commit

Le repository Git devient la source de vérité de l’infrastructure.

---

## Structure du Repository

- `blog/` → Chart Helm personnalisé (Deployment, Service, Ingress, HPA, PVC, MariaDB…)
- `docker/` → Dockerfile + thème WordPress personnalisé
- `Jenkinsfile` → Pipeline CI
- Fichiers YAML Kubernetes (volumes persistants, configuration…)

---

## Compétences démontrées

- DevOps & CI/CD
- GitOps
- Orchestration Kubernetes
- Observabilité Cloud-Native
- Sécurité Kubernetes
- Architecture scalable et résiliente
