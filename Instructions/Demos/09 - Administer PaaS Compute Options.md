---
demo:
  title: "Démonstration\_09\_: Administrer les options de calcul PaaS"
  module: Administer PaaS Compute Options
---

# 09 - Administrer les options de calcul PaaS

## Configurer des plans Azure App Service

Dans cette démonstration, nous allons créer et travailler avec des plans Azure App Service.

**Informations de référence** : [Gérer un plan App Service - Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-plan-manage)

**Informations de référence** : [Effectuer le scale-up d’une application dans Azure App Service](https://learn.microsoft.com/azure/app-service/manage-scale-up)

**Informations de référence** : [Mise à l’échelle automatique dans Azure App Service](https://learn.microsoft.com/azure/app-service/manage-automatic-scaling?tabs=azure-portal)

1. Utilisez le portail Azure. 

1. Recherchez et sélectionnez  **Plans App Service**.

1. Créez un plan App Service simple. Expliquez la nécessité de sélectionner Windows ou Linux. Expliquez les plans tarifaires maintenant ou dans les étapes suivantes. 

1. Déployez votre nouveau plan App Service. 

1. Passez en revue le panneau **Scale-up (Plan App Service).** . Expliquez la différence entre les plans **Dev/Test** et **Production**. Passez en revue la liste des fonctionnalités. 

1. Passez en revue le panneau **Scale-out (Plan App Service).** . Expliquez la différence entre **Manuel** et **Basé sur des règles**. 

## Configurer Azure App Services

Dans cette démonstration, nous allons créer une nouvelle application web qui exécute un conteneur Docker. Le conteneur affiche un message de bienvenue.

**Informations de référence** : [Créer une application web](https://learn.microsoft.com/training/modules/host-a-web-app-with-azure-app-service/3-exercise-create-a-web-app-in-the-azure-portal?pivots=csharp)

Dans cette tâche, nous allons créer une application web Azure App Service.

1. Utilisez le portail Azure. 

1. Recherchez et sélectionnez **App Services**.

1. **Créez** une **application web**.

    - Publiez : **Code**. Passez en revue d’autres choix.
    - Pile d’exécution : **.NET**. Passez en revue d’autres choix.
    - Système d’exploitation : **Linux**

1. Sélectionnez le plan de service **F1 gratuit**.

1. **Vérifiez + créez** l’application web. Attendez que la ressource soit déployée.

1. Dans la page **Vue d’ensemble**, vérifiez que l’**État** est **En cours d’exécution**.

1. Sélectionnez l’**URL** et vérifiez que la page d’espace réservé par défaut se charge.

1. Quand vous en avez le temps, explorez les options de **Emplacements de déploiement**.
   
## Configurer Azure Container Instances

Dans cette démonstration, nous créer, nous configurons et nous déployons un container en utilisant Azure Container Instances (ACI) depuis le portail Azure. L’application ACI affiche une page HTML statique avec l’image Microsoft Hello World publique. 

**Référence** : [Démarrage rapide - Déployer un conteneur Docker dans une instance de conteneur](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-quickstart-portal)

1. Utilisez le portail Azure.

1. Recherchez et sélectionnez **Instances de conteneur**.

1. **Créez** une instance de conteneur. 

1. Renseignez le **Groupe de ressources** et le **Nom du conteneur**. 

1. Expliquez les options de **Source d’image**. Utilisez **Images de démarrage rapide**.

1. Pour **Image conteneur**, utilisez **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** . Cet exemple d’image Linux contient une petite application web écrite en Node.js qui se présente sous la forme d’une page HTML statique.

1. Dans la page **Mise en réseau**, spécifiez une **Étiquette du nom DNS** pour votre conteneur. 

1. Conservez les autres paramètres par défaut, puis sélectionnez **Vérifier + créer**.

1. Attendez que la ressource soit déployée.

1. Sur la page **Vue d’ensemble** de la ressource, assurez-vous que l’**État** est **En cours d’exécution**.

1. Accédez au **Nom de domaine complet** de l’instance de conteneur et vérifiez que la page d’accueil s’affiche. 

**Remarque** : Pour éviter des coûts supplémentaires, supprimez la ressource. 

## Configurer Azure Container Apps

Dans cette démonstration, nous allons créer et travailler avec Azure Container Apps. 

**Référence** : [Démarrage rapide : Déployer votre première application conteneur avec le portail Azure](https://learn.microsoft.com/azure/container-apps/quickstart-portal)

1. Recherchez et sélectionnez **Applications conteneur**.

1. Renseignez les **Détails du projet** et créez l’**environnement** d’applications conteneur.

1. **Vérifiez et créez** l’application conteneur.

1. Utilisez le lien **URL d’application** pour voir votre application.

1. Vérifiez que le navigateur affiche le message **Bienvenue dans Azure Container Apps**. 






