# taskflow-gitops — dépôt GitOps du cours CI/CD M2

Ce dépôt décrit **l'état voulu** de l'application TaskFlow dans Kubernetes.
Argo CD le surveille et aligne le cluster dessus : pour changer la production,
on ne tape pas de commande, on fait une **Pull Request**.

## Installation (à faire chez vous, avant le cours)

Prérequis : Docker Desktop démarré, 8 Go de RAM, 10 Go de disque libre.
Sous Windows : WSL2 (Ubuntu) + intégration WSL de Docker Desktop, et toutes les commandes dans WSL.

```bash
git clone https://github.com/9m7fjfpv9k-cyber/taskflow-gitops.git
cd taskflow-gitops
./scripts/install.sh
```

Le script crée un cluster local `kind`, installe Argo CD et Argo Rollouts,
puis télécharge les images des labs. Comptez 5 à 15 minutes.
Il peut être relancé sans risque.

## Structure

| Chemin | Rôle |
| --- | --- |
| `apps/taskflow/` | Les manifests surveillés par Argo CD |
| `argocd/application.yaml` | Déclare l'application dans Argo CD |
| `exemples/bluegreen/` | Manifests pour le déploiement Blue-Green |
| `exemples/canary/` | Manifests pour le déploiement Canary |
| `scripts/install.sh` | Installation de l'environnement |
| `scripts/argocd-ui.sh` | Ouvre l'interface d'Argo CD |
| `scripts/observe.sh` | Montre quelle version répond, et avec quel code HTTP |

## Images disponibles

`ghcr.io/9m7fjfpv9k-cyber/taskflow` en versions `1.0.0`, `1.1.0`, `2.0.0` et `2.1.0`.

## Équipe

Ibrahim KONE

Stanislas DE DIEULEVEULT
<!-- Noms du binôme -->

<img width="454" height="452" alt="image" src="https://github.com/user-attachments/assets/64b6c5ec-fac1-4020-a408-3ae63e356592" />

<img width="454" height="216" alt="image" src="https://github.com/user-attachments/assets/31555e43-ab98-410f-846f-82a7e01fbe52" />

<img width="448" height="30" alt="image" src="https://github.com/user-attachments/assets/dfa050ac-3926-419c-905b-04af5e5810a4" />

<img width="454" height="306" alt="image" src="https://github.com/user-attachments/assets/15cf391c-dc8c-425d-a66a-dc11844922ea" />

<img width="253" height="140" alt="image" src="https://github.com/user-attachments/assets/5d377ca2-b4c2-421a-a7a8-7cfe1f40d914" />

<img width="454" height="278" alt="image" src="https://github.com/user-attachments/assets/c9f74a10-5d1d-4a18-825e-99622b38bdad" />
