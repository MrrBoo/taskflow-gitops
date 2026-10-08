# Postmortem — [titre court de l'incident]

> Sans reproche : on cherche ce qui a permis l'erreur, pas qui l'a faite.

| Champ | Valeur |
| --- | --- |
| Date et heure | Jeudi 8 oct. 14h32|
| Version en cause | ghcr.io/9m7fjfpv9k-cyber/taskflow:2.1.0 |
| PR à l'origine | 29 |
| Durée d'exposition | (de quand à quand des utilisateurs ont été touchés) |
| Part du trafic touché | 25% |
| Détecté par | test de charge k6 (analysisrun) |
| Résolu par | abort automatique |

## Chronologie

| Heure | Événement |
| --- | --- |
| | |

## Composant défaillant et cause racine

- Quel composant a échoué ? (preuve : commande et sortie)

Le déploiement de la révision 15 (`taskflow-df976ccb5`) via l'analyse automatique. L'étape de test de charge k6 a déclenché l'abandon du rollout.

- Pourquoi les probes Kubernetes ne l'ont-elles pas vu ?
- Cause racine :

## Ce qui a bien fonctionné

Le abort a bien fonctionné, on est resté sur la version 2.0.0

## Actions correctives

| Action | Responsable | Échéance |
| --- | --- | --- |
| git revert | nous | Instantanné |
| utilisation de l'image 2.2.0 | nous | Instantanné |

