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
| `exemples/robustesse/` | Canary avec test de charge k6 automatique, modèle de postmortem |
| `exemples/ci/` | Pipelines de la mini-PSSI (GitHub Actions et GitLab CI) |
| `policies/` | Mini-PSSI et règles Rego vérifiées par conftest |
| `scripts/install.sh` | Installation de l'environnement |
| `scripts/argocd-ui.sh` | Ouvre l'interface d'Argo CD |
| `scripts/observe.sh` | Montre quelle version répond, et avec quel code HTTP |
| `scripts/charge.sh` | Lance à la main le test de charge k6 contre un service |

## Images disponibles

`ghcr.io/9m7fjfpv9k-cyber/taskflow` en versions `1.0.0`, `1.1.0`, `2.0.0`, `2.1.0` et `2.2.0`.

## Équipe

Ibrahim KONE

Stanislas DE DIEULEVEULT
<!-- Noms du binôme -->

# Jour 2

## Lab Matin

On commence par ajouter notre pseudo dans le fichier argocd/application.yaml

<img width="454" height="452" alt="image" src="https://github.com/user-attachments/assets/64b6c5ec-fac1-4020-a408-3ae63e356592" />

On créer le cluster puis on fait un kubectl apply du fichier argocd/application.yaml

<img width="454" height="216" alt="image" src="https://github.com/user-attachments/assets/31555e43-ab98-410f-846f-82a7e01fbe52" />

On regarde l'état du cluster (SYNC et HEALTH) sur argocd, on peut également exécuter le script observe.sh qui nous montre la version utilisé

<img width="448" height="30" alt="image" src="https://github.com/user-attachments/assets/dfa050ac-3926-419c-905b-04af5e5810a4" />

On modifie la version en passant en 2.0.0 puis on effectue une merge-request. Après quelques secondes (1 min max) on voit que la version de deployment change et passe à 2.0.0

<img width="454" height="306" alt="image" src="https://github.com/user-attachments/assets/15cf391c-dc8c-425d-a66a-dc11844922ea" />

<img width="253" height="140" alt="image" src="https://github.com/user-attachments/assets/5d377ca2-b4c2-421a-a7a8-7cfe1f40d914" />

<img width="454" height="278" alt="image" src="https://github.com/user-attachments/assets/c9f74a10-5d1d-4a18-825e-99622b38bdad" />

On fait un kubectl scale, puis un set image pour faire une dérive et voir le comportement de argocd.

<img width="454" height="92" alt="image" src="https://github.com/user-attachments/assets/a8c693a8-53c8-4c04-81c1-87fb417b3c8d" />

<img width="454" height="19" alt="image" src="https://github.com/user-attachments/assets/6248f698-c83a-4718-ab9c-c3834d244a8a" />

On voit que argocd supprimer le pod avec l'image modifiée et en recrée un avec la bonne image

<img width="454" height="255" alt="image" src="https://github.com/user-attachments/assets/2e9f98e8-688d-418b-938f-3c17daa80493" />

On retourne sur la PR qu'on vient de faire et on effectue un revert

<img width="454" height="322" alt="image" src="https://github.com/user-attachments/assets/49294629-37f7-461e-9b4c-f3e9bc213c44" />

On peut voir qu'on retrouve la version d'origine

<img width="218" height="125" alt="image" src="https://github.com/user-attachments/assets/b1184b48-d400-49df-8a69-94000f04a389" />

## Lab Après-midi

### Blue-Green

On remplace le path dans application.yaml par contenu par celui de bluegreen/rollout.yaml, on change la version de l'image par 1.1.0 puis on merge.

<img width="454" height="253" alt="image" src="https://github.com/user-attachments/assets/02bfc32a-edd6-4920-af71-acd6a4c63d16" />

On fait un kubectl apply avec le fichier application.yaml qui va pointer sur bluegreen/rollout.yaml

<img width="454" height="222" alt="image" src="https://github.com/user-attachments/assets/97d51349-d0a8-4cbe-9207-8819bf31ff14" />

On peut voir sur argocd qu'on a notre rollout qui apparait

<img width="454" height="222" alt="image" src="https://github.com/user-attachments/assets/a8a47975-b722-4033-a91e-c953b8e4d7f1" />

<img width="454" height="218" alt="image" src="https://github.com/user-attachments/assets/acf4d4d2-65d3-4b49-a47d-bc873ab30408" />

Après quelques secondes/minutes on peut voir que le rollout est terminé et que la montée de version a été faite

<img width="454" height="249" alt="image" src="https://github.com/user-attachments/assets/91ab68f3-d71f-474d-93f0-9eee228069b8" />

<img width="218" height="126" alt="image" src="https://github.com/user-attachments/assets/4079d866-ab25-495f-aeea-888eb993a4e1" />

### Canary

Comme pour Blue-green, on modifie le path dans application.yaml pour pointer vers exemples/canary

<img width="1500" height="1086" alt="image" src="https://github.com/user-attachments/assets/d89aff86-c089-44ba-8fa2-edff01ba8f0d" />

On peut voir qu'on est toujours sous blue-green

<img width="454" height="245" alt="image" src="https://github.com/user-attachments/assets/38186262-d936-426f-aac4-bae4b9083354" />

On fait un kubectl apply et on voit que canary est bien utilisé

<img width="454" height="313" alt="image" src="https://github.com/user-attachments/assets/de639e1e-1514-4b7f-b66b-e44b68716d51" />

Le rollout est en cours

<img width="2264" height="1158" alt="image" src="https://github.com/user-attachments/assets/2756b59d-cac2-41fc-8b2b-f498e97e279d" />

La version de l'image est en train de changer grâce à canary

<img width="448" height="390" alt="image" src="https://github.com/user-attachments/assets/5b1aa8e0-968d-4ab7-9dc3-bc9c5a7e227f" />

On promote à 100%

<img width="1482" height="662" alt="image" src="https://github.com/user-attachments/assets/a73bc6e0-2ff3-4626-a957-b98e6fb3914c" />

# Jour 3

## Lab matin

On commence par faire un sync fork pour mettre à jour le repo et on vérifie que la version utilisée est bien 2.0.0

<img width="1496" height="404" alt="image" src="https://github.com/user-attachments/assets/074a47e0-e339-4035-8106-e04ad2d4a39e" />

On exécute le script charge.sh http://taskflow et on remarque qu'il n'y a pas d'erreurs et le p95 est à 5.17ms

<img width="454" height="297" alt="image" src="https://github.com/user-attachments/assets/e89f1af9-f399-4189-a6fd-d1a496ce6649" />

On crée une nouvelle branche feat/analyse-auto dans laquelle on utilise les fichiers de exemples/robustesse et on merge. On sync ensuite sur argocd et on peut voir le configmap et le analysistemplate

<img width="454" height="219" alt="image" src="https://github.com/user-attachments/assets/21e9e3e4-0765-4da1-9218-1ba66c455a36" />

<img width="454" height="123" alt="image" src="https://github.com/user-attachments/assets/ce1fa47f-7a18-4e27-8775-ae0b5685895f" />

