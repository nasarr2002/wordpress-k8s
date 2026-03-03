# Migration d’une Application WordPress vers Kubernetes  
CI/CD – GitOps – Observabilité – Sécurité – Backup

## Description

Projet académique réalisé en Master 2 Réseaux et Télécommunication.

Objectif : migrer une application WordPress "legacy" vers une architecture Cloud-Native sur Kubernetes, en intégrant les bonnes pratiques DevOps : conteneurisation, CI/CD, GitOps, observabilité, sécurité et sauvegarde.

---

## Architecture globale

L’architecture repose sur :

- Docker (image WordPress personnalisée)
- Kubernetes (Minikube)
- Helm (chart personnalisé)
- Jenkins (Intégration Continue)
- ArgoCD (Déploiement Continu – GitOps)
- Prometheus & Grafana (Monitoring)
- Loki (centralisation des logs)
- Velero (backup Kubernetes)
- RBAC & NetworkPolicies (sécurité)

---

## Fonctionnalités mises en œuvre

- Build automatique de l’image Docker via Jenkins
- Push sur Docker Hub
- Déploiement automatisé via Helm
- Synchronisation GitOps automatique avec ArgoCD (self-heal activé)
- Autoscaling via HPA
- Monitoring complet du cluster et de l’application
- Centralisation des logs avec Loki
- Sauvegarde planifiée avec Velero
- Mise en place de règles RBAC et NetworkPolicies

---

## Structure du repository

- blog/ → Chart Helm WordPress personnalisé
- docker/ → Dockerfile + thème WordPress custom
- Jenkinsfile → Pipeline CI
- Fichiers YAML Kubernetes (PVC, services…)

---

## Compétences démontrées

- DevOps & CI/CD
- GitOps
- Orchestration Kubernetes
- Observabilité Cloud-Native
- Sécurité Kubernetes
- Architecture scalable et résiliente
- 
