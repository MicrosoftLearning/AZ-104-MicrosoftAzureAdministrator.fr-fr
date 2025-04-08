---
lab:
  title: 'Labo 09b : Implémenter Azure Container Instances'
  module: Administer PaaS Compute Options
---

# Labo 09b : Implémenter Azure Container Instances

## Présentation du labo

Dans ce labo, vous découvrez comment implémenter et déployer Azure Container Instances.

Ce labo nécessite un abonnement Azure. Le type de votre abonnement peut affecter la disponibilité des fonctionnalités dans ce labo. Vous pouvez changer la région, mais les étapes sont écrites de façon à utiliser **USA Est**.

## Durée estimée : 15 minutes

## Scénario du labo

Votre organisation dispose d’une application web qui s’exécute sur une machine virtuelle dans votre centre de données local. L’organisation souhaite déplacer toutes les applications vers le cloud, mais ne veut pas avoir à gérer un grand nombre de serveurs. Vous décidez d’évaluer Azure Container Instances et Docker. 
## Simulations de labo interactives

Il existe des simulations de labo interactives qui peuvent vous être utiles pour cette rubrique. La simulation vous permet de parcourir un scénario similaire, à votre propre rythme. Il existe des différences entre la simulation interactive et ce labo, mais bon nombre des principaux concepts sont les mêmes. Un abonnement Azure n’est pas nécessaire.

+ [Déployez Azure Container Instances](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%203). Créez, configurez et déployez un conteneur Docker avec Azure Container Instances.
  
+ [Implémentez Azure Container Instances](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014).  Déployez une image Docker en utilisant Azure Container Instances. 

## Diagramme de l'architecture

![Diagramme des tâches.](../media/az104-lab09b-aci-architecture.png)

## Compétences de tâche

- Tâche 1 : Déployez une instance Azure Container Instances en utilisant une image Docker.
- Tâche 2 : Testez et vérifiez le déploiement d’une instance Azure Container Instances.

## Tâche 1 : Déployer une instance Azure Container Instances en utilisant une image Docker

Dans cette tâche, vous allez créer une application web simple en tirant parti d’une image Docker. Docker est une plateforme qui offre la possibilité d’empaqueter et d’exécuter des applications dans des environnements isolés appelés conteneurs. Azure Container Instances fournit l’environnement Compute pour l’image conteneur.

1. Connectez-vous au **portail Azure** - `https://portal.azure.com`.

1. Dans le Portail Azure, recherchez et sélectionnez `Container instances`, puis dans le panneau **Instances de conteneur**, cliquez sur **+ Créer**.

1. Sous l’onglet **Informations de base** du volet **Créer une instance de conteneur**, spécifiez les paramètres suivants (conservez les valeurs par défaut pour les autres) :

    | Paramètre | Valeur |
    | ---- | ---- |
    | Abonnement | Sélectionnez votre abonnement Azure. |
    | Groupe de ressources | `az104-rg9` (Si nécessaire, sélectionnez **Créer**) |
    | Nom du conteneur | `az104-c1` |
    | Région | **USA Est** (ou une région disponible près de vous)|
    | Source d’image | **Images du guide de démarrage rapide** |
    | Image | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Cliquez sur **Suivant : Mise en réseau >**, puis spécifiez les paramètres suivants (conservez les valeurs par défaut pour les autres) :

    | Paramètre | Valeur |
    | --- | --- |
    | Étiquette du nom DNS | tout nom d’hôte DNS unique valide et global |

    >**Remarque** : Votre conteneur sera accessible au public à l’adresse : dns-name-label.region.azurecontainer.io. Si vous recevez un message d’erreur **Étiquette de nom DNS indisponible**, spécifiez une autre valeur.

1. Cliquez sur **Suivant : Surveillance >** et décochez **Activer les journaux d’instance de conteneur**. 

1. Cliquez sur **Suivant : Avancé >**, vérifiez les paramètres sans apporter de modification.

1. Cliquez sur **Vérifier + Créer**, vérifiez que la validation a réussi, puis cliquez sur **Créer**.

    >**Remarque** : Attendez la fin du déploiement. Ce processus prend environ 2 à 3 minutes.

    >**Remarque** : Pendant que vous attendez, regardez l’[exemple de code derrière l’application simple](https://github.com/Azure-Samples/aci-helloworld). Pour afficher le code, parcourez le dossier de l’application \\.

## Tâche 2 : Testez et vérifiez le déploiement d’une instance Azure Container Instances. 

Dans cette tâche, vous examinez le déploiement de l’instance de conteneur. Par défaut, Azure Container Instances est accessible sur le port 80. Une fois l’instance déployée, vous pouvez accéder au conteneur en tirant parti du nom DNS que vous avez fourni dans la tâche précédente.

1. Une fois le déploiement terminé, sélectionnez le lien **Accéder à la ressource**.

1. Dans le volet **Vue d’ensemble** de l’instance de conteneur, vérifiez que l’**État** indiqué est **En cours d’exécution**.

1. Copiez la valeur du **nom de domaine complet** de l’instance de conteneur, ouvrez un nouvel onglet de navigateur et accédez à l’URL correspondante.

     ![Capture d’écran de la page de vue d’ensemble d’ACI dans le portail.](../media/az104-lab09b-aci-overview.png)

1. Vérifiez que la page **Bienvenue dans Azure Container Instances** s’affiche. Actualisez la page plusieurs fois pour créer des entrées de journal, puis fermez l’onglet du navigateur.  

1. Dans la section **Paramètres** du volet d’instance de conteneur, cliquez sur **Conteneurs**, puis sur **Journaux**.

1. Vérifiez que les entrées de journal représentant la requête HTTP GET générée en affichant l’application dans le navigateur apparaissent.
   
## Nettoyage de vos ressources

Si vous travaillez avec **votre propre abonnement**, prenez un moment pour supprimer les ressources du labo. Ceci garantit que les ressources sont libérées et que les coûts sont réduits. Le moyen le plus simple de supprimer les ressources du labo est de supprimer le groupe de ressources du labo. 

+ Dans le Portail Azure, sélectionnez le groupe de ressources, **Supprimer le groupe de ressources**, **Entrer le nom du groupe de ressources**, puis cliquez sur **Supprimer**.
+ `Remove-AzResourceGroup -Name resourceGroupName` en utilisant Azure PowerShell.
+ `az group delete --name resourceGroupName` en utilisant l’interface CLI.

## Développer votre apprentissage avec Copilot
Copilot peut vous aider à apprendre à utiliser les outils de script Azure. Copilot peut également aider dans des domaines non couverts dans le labo ou quand vous avez besoin de plus d’informations. Ouvrez un navigateur Edge et choisissez Copilot (en haut à droite), ou accédez à *copilot.microsoft.com*. Prenez quelques minutes pour essayer ces invites.

+ Résumez les étapes pour créer et configurer une instance de conteneur Azure.
+ De quelles manières puis-je exécuter un conteneur serverless sur Azure ?

## En savoir plus grâce à l’apprentissage auto-rythmé

+ [Exécuter des images conteneur dans Azure Container Instances](https://learn.microsoft.com/training/modules/create-run-container-images-azure-container-instances/). Découvrez comment Azure Container Instances peut vous aider à déployer rapidement des conteneurs, à définir des variables d’environnement et à spécifier des stratégies de redémarrage de conteneur.

## Points clés

Félicitations, vous avez terminé le labo. Voici les principaux points à retenir de ce labo. 

+ Azure Container Instances (ACI) est un service qui vous permet de déployer des conteneurs sur le cloud public Microsoft Azure.
+ ACI ne nécessite pas l’approvisionnement ou la gestion d’une infrastructure sous-jacente.
+ ACI prend en charge des conteneurs Windows et Linux.
+ Les charges de travail sur ACI sont habituellement démarrées et arrêtées par un type de processus ou de déclencheur et sont généralement de courte durée. 

    
