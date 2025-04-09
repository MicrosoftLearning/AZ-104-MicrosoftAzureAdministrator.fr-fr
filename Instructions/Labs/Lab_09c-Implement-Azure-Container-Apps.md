---
lab:
  title: 'Lab 09c : Implémenter Azure Container Apps'
  module: Administer PaaS Compute Options
---

# Labo 09c – Implémenter Azure Container Apps

## Présentation du labo

Dans ce labo, vous découvrez comment implémenter et déployer Azure Container Apps.

Ce labo nécessite un abonnement Azure. Le type de votre abonnement peut affecter la disponibilité des fonctionnalités dans ce labo. Vous pouvez changer la région, mais les étapes sont écrites de façon à utiliser **USA Est**.

## Durée estimée : 15 minutes

## Scénario du labo

Votre organisation dispose d’une application web qui s’exécute sur une machine virtuelle dans votre centre de données local. L’organisation souhaite déplacer toutes les applications vers le cloud, mais ne veut pas avoir à gérer un grand nombre de serveurs. Vous décidez d’évaluer Azure Container Apps.

## Simulations de labo interactives

Il n’existe aucune simulation de laboratoire interactive pour cette rubrique. 

## Diagramme de l'architecture

![Diagramme des tâches.](../media/az104-lab09b-aca-architecture.png)

## Compétences de tâche

- Tâche 1 : Créez et configurez une instance Azure Container Apps et un environnement.
- Tâche 2 : Testez et vérifiez le déploiement de l’instance Azure Container Apps.

## Tâche 1 : Créez et configurez une instance Azure Container Apps et un environnement.

Azure Container Apps va plus loin avec le concept de cluster Kubernetes managé et gère l’environnement de clusters et fournit également d’autres services managés en plus du cluster. Contrairement à un cluster Azure Kubernetes, où vous devez toujours gérer le cluster, une instance Azure Container Apps supprime une partie de la difficulté de configurer un cluster Kubernetes.

1. Dans le Portail Azure, recherchez et sélectionnez `Container Apps`.

1. Sélectionnez **+ Créer**, dans le menu déroulant **Container App**. Examinez les autres choix. 

1. Utilisez les informations suivantes pour remplir les détails sous l’onglet **Informations de base**.*.

    | Setting | Action |
    |---|---|
    | Abonnement | Sélectionnez votre abonnement Azure. |
    | Groupe de ressources | `az104-rg9` |
    | Nom de l’application conteneur |  `my-app` |
    | Région    | **USA Est** (|
    | Environnement Container Apps | Sélectionnez **Créer nouveau** > Définir le nom de l’environnement sur `my-environment` > **Créer** |

1. Cliquez sur l’onglet **Suivant : conteneur** et vérifiez que l’option **Utiliser l’image de démarrage rapide** est cochée. Vous devrez peut-être faire défiler vers le haut pour afficher ce paramètre. 

1. Assurez-vous que l’**image de démarrage rapide** est définie sur **conteneur Hello World simple**. Examinez les autres choix. 

1. Sélectionnez **Vérifier et créer**, puis **Créer**.

    >**Remarque :** Attendez que le déploiement de l’application conteneur soit terminé. Cette opération prend quelques minutes. 
 
## Tâche 2 : Testez et vérifiez le déploiement de l’instance Azure Container Apps.

Par défaut, l’application conteneur Azure que vous créez accepte le trafic sur le port 80 en utilisant l’exemple d’application Hello World. Azure Container Apps fournit un nom DNS pour l’application. Copiez et accédez à cette URL pour vérifier que l’application est opérationnelle.

1. Sélectionnez **Accéder à la ressource** pour afficher votre nouvelle application conteneur.

1. Sélectionnez le lien en regard d’*URL de l’application* pour afficher votre application.

    ![Capture d’écran de la page de vue d’ensemble d’ACI dans le portail.](../media/az104-lab09b-aca-overview.png)

1. Vérifiez que vous recevez le message **Votre application Azure Container Apps est publiée**.
   
## Nettoyage de vos ressources

Si vous travaillez avec **votre propre abonnement**, prenez un moment pour supprimer les ressources du labo. Ceci garantit que les ressources sont libérées et que les coûts sont réduits. Le moyen le plus simple de supprimer les ressources du labo est de supprimer le groupe de ressources du labo. 

+ Dans le Portail Azure, sélectionnez le groupe de ressources, **Supprimer le groupe de ressources**, **Entrer le nom du groupe de ressources**, puis cliquez sur **Supprimer**.
+ `Remove-AzResourceGroup -Name resourceGroupName` en utilisant Azure PowerShell.
+ `az group delete --name resourceGroupName` en utilisant l’interface CLI.

## Développer votre apprentissage avec Copilot
Copilot peut vous aider à apprendre à utiliser les outils de script Azure. Copilot peut également aider dans des domaines non couverts dans le labo ou quand vous avez besoin de plus d’informations. Ouvrez un navigateur Edge et choisissez Copilot (en haut à droite), ou accédez à *copilot.microsoft.com*. Prenez quelques minutes pour essayer ces invites.

+ Résumez les étapes pour créer et configurer une application conteneur Azure.
+ Comparez et opposez Azure Container Apps et Azure Kubernetes Service.

## En savoir plus grâce à l’apprentissage auto-rythmé

+ [Configurer une application conteneur dans Azure Container Apps](https://learn.microsoft.com/training/modules/configure-container-app-azure-container-apps/). Permet d’examiner les fonctionnalités et caractéristiques d’Azure Container Apps, puis se concentre sur la création, la configuration, la mise à l’échelle et la gestion d’applications conteneur en tirant parti d’Azure Container Apps.


## Points clés

Félicitations, vous avez terminé le labo. Voici les principaux points à retenir de ce labo. 

+ Azure Container Apps (ACA) est une plateforme serverless qui vous permet de gérer moins d’infrastructures et d’économiser des coûts tout en exécutant des applications conteneurisées.
+ Container Apps fournit des informations sur la configuration du serveur, l’orchestration des conteneurs et le déploiement. 
+ Les charges de travail sur ACA sont habituellement des processus de longue durée comme une application web.

     
