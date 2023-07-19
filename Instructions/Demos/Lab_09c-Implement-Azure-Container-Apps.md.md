---
lab:
  title: "Lab 09c\_: Implémenter Azure Container Apps"
  module: Administer PaaS Compute Options
---

# Lab 09c : Implémenter Azure Container Apps
# Manuel de labo pour l’étudiant

## Scénario du labo
Azure Container Apps vous permet d’exécuter des microservices et des applications conteneurisées sur une plateforme serverless. Avec Container Apps, vous bénéficiez des avantages de l’exécution de conteneurs tout en évitant les soucis de configuration manuelle de l’infrastructure cloud et des orchestrateurs de conteneurs complexes.

## Objectifs

Dans ce labo, nous allons :
- Tâche 1 : Créer une application conteneur et un environnement
- Tâche 2 : Déployer l’application conteneur
- Tâche 3 : Tester et vérifier le déploiement de l’application conteneur

Commencez par vous connecter au [portail Azure](https://portal.azure.com).

## Durée estimée : 20 minutes

## Tâche 1 : Créer une application conteneur et un environnement

Pour créer votre application conteneur, commencez sur la page d’accueil du portail Azure.

1. Recherchez `Container Apps` dans la barre de recherche en haut.
1. Dans les résultats de la recherche, sélectionnez **Applications conteneur**.
1. Cliquez sur le bouton **Créer**.

### Onglet Informations de base

Sous l’onglet *Informations de base*, effectuez les actions suivantes.

1. Entrez les valeurs suivantes dans la section *Détails du projet*.

    | Paramètre | Action |
    |---|---|
    | Abonnement | Sélectionnez votre abonnement Azure. |
    | Resource group | Sélectionnez **Créer**, puis entrez `az104-09c-rg1`. |
    | Nom de l’application conteneur |  Entrez `my-container-app`. |

#### Créer un environnement

Ensuite, créez un environnement pour votre application conteneur.

1. Sélectionnez la région appropriée.

    | Paramètre | Valeur |
    |--|--|
    | Région | **Votre choix**. |

1. Dans le champ *Créer un environnement Container Apps*, sélectionnez **Créer**.
1. Dans l’onglet *Informations de base* de la page *Créer un environnement Container Apps*, entrez les valeurs suivantes :

    | Paramètre | Valeur |
    |--|--|
    | Nom de l’environnement | Entrez `my-environment`. |
    | Redondance de zone | Sélectionnez **Désactivé** |

1. Sélectionnez **Supervision** pour créer un espace de travail Log Analytics.
1. Sélectionnez le lien **Créer** dans le champ *Espace de travail Log Analytics* et entrez les valeurs suivantes.

    | Paramètre | Valeur |
    |--|--|
    | Nom | Entrez `my-container-apps-logs` |
  
    Le champ *Emplacement* est prérempli avec votre région.

1. Sélectionnez **OK**.


## Tâche 2 : Déployer l’application conteneur

1. Sélectionnez le bouton **Vérifier et créer** au bas de la page.  

    Ensuite, les paramètres de l’application conteneur sont vérifiés. Si aucune erreur n’est rencontrée, le bouton *Créer* est activé.  

    En cas d’erreur, tout onglet contenant des erreurs est marqué d’un point rouge.  Accédez à l’onglet approprié. Les champs contenant une erreur sont mis en évidence en rouge.  Une fois toutes les erreurs résolues, resélectionnez **Vérifier et créer**.

1. Sélectionnez **Create** (Créer).

    Une page contenant le message *Le déploiement est en cours* s’affiche.  Une fois le déploiement terminé, le message *Votre déploiement est terminé* s’affiche.
   
## Tâche 3 : Tester et vérifier le déploiement de l’application conteneur

Sélectionnez **Accéder à la ressource** pour afficher votre nouvelle application conteneur.  Sélectionnez le lien en regard d’*URL de l’application* pour afficher votre application. Vérifiez que vous avez le message *Bienvenue dans Azure Container Apps*.

## Nettoyer les ressources

Si vous n’envisagez pas de continuer à utiliser cette application, vous pouvez supprimer l’instance Azure Container Apps et tous les services associés en supprimant le groupe de ressources.

1. Sélectionnez le groupe de ressources **my-container-apps** dans la section *Vue d’ensemble*.
1. Sélectionnez le bouton **Supprimer un groupe de ressources** en haut de la *Vue d’ensemble* du groupe de ressources.
1. Entrez le nom du groupe de ressources et confirmez que vous souhaitez supprimer l’application. 
1. Sélectionnez **Supprimer**.
1. Le processus de suppression du groupe de ressources peut prendre quelques minutes.
