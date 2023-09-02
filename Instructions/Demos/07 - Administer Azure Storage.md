---
demo:
  title: "Démonstration 07\_: Administrer Stockage Azure"
  module: Administer Azure Storage
---


# 07 - Administrer Stockage Azure

## Configurer les comptes de stockage

Dans cette démonstration, nous allons créer un compte de stockage.

**Informations de référence** : [Créer un compte de stockage](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal)

1. Utilisez le portail Azure.

1. Passez en revue l’objectif des comptes de stockage. 
   
1. Recherchez et sélectionnez **Comptes de stockage**. 
 
1. Créez un compte de stockage de base. 

    - Discutez des exigences relatives au nommage d’un compte de stockage et de la nécessité que le nom soit unique dans Azure. 

    - Passez en revue les différents types de stockage. Par exemple, à usage général v2. 

    - Passez en revue les sélections du niveau d’accès. Par exemple, les niveaux froid et chaud. 

    - D’autres onglets et paramètres seront abordés dans d’autres démonstrations. 

1. Créez le compte de stockage et attendez que la ressource soit déployée. 


## Configurer le Stockage Blob

Dans cette démonstration, vous allez explorer le stockage d’objets blob.

**Remarque :**  Ces étapes nécessitent un compte de stockage.

**Informations de référence** : [Démarrage rapide : Charger, télécharger et lister des objets blob](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

1. Accédez à un compte de stockage dans le portail Azure.

1. Passez en revue l’objectif du stockage d’objets blob. 

1. Créez un conteneur d’objets blob. Passez en revue le niveau d’accès pour le conteneur. Par exemple, privé (pas d’accès anonyme). 

1. Chargez un objet blob dans le conteneur. Quand vous en avez le temps, passez en revue les paramètres avancés. Par exemple, le type d’objet blob et la taille de l’objet blob. 

## Configurer la sécurité du stockage

Dans cette démonstration, nous allons créer une signature d’accès partagé.

**Remarque :**  Cette démonstration nécessite un compte de stockage, un conteneur d’objets blob et un fichier chargé.

**Informations de référence** : [Créer des jetons SAS pour des conteneurs de stockage](https://learn.microsoft.com/azure/applied-ai-services/form-recognizer/create-sas-tokens?source=recommendations&view=form-recog-3.0.0)

1. Sélectionnez un objet blob ou un fichier que vous voulez sécuriser. 

1. Générer une signature d’accès partagé (SAS). Passez en revue les autorisations, les dates/heures de début et d’expiration, et les protocoles autorisés.

1. Utilisez l’URL SAS pour être sûr que la ressource s’affiche. 


## Configurer Azure Files 

Dans cette démonstration, nous allons travailler avec des partages de fichiers et des instantanés.

**Remarque :**  Ces étapes nécessitent un compte de stockage.

**Informations de référence** : [Démarrage rapide pour la gestion des partages de fichiers Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-portal?tabs=azure-portal)

1. Passez en revue l’objectif des partages de fichiers. 

1. Accédez à un compte de stockage et cliquez sur **Fichiers**.

1. Créer un partage de fichiers. Passez en revue les quotas, le chargement de fichiers et l’ajout de répertoires pour organiser les informations. 

1. Créez un instantané de partage de fichiers. Expliquez quand utiliser des instantanés et en quoi ils diffèrent des sauvegardes. Quand vous en avez le temps, chargez un fichier, prenez un instantané, supprimez le fichier et restaurez l’instantané.


## Outils de stockage (facultatif)

Dans cette démonstration, nous allons examiner plusieurs outils courants de Stockage Azure. 

**Informations de référence** : [Bien démarrer avec l’Explorateur Stockage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)

1. Installez l’Explorateur Stockage ou utilisez le navigateur de stockage.

1. Expliquez comment parcourir et créer des ressources de stockage. Par exemple, ajoutez un conteneur d’objets blob. 

**Informations de référence** : [Copier ou déplacer des données vers Stockage Azure en utilisant AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=/azure/storage/files/toc.json)

1. Expliquez quand utiliser AzCopy. Affichez l’aide en utilisant **azcopy /?** .

1. Faites défiler la section **Exemples** vers le bas. Quand vous en avez le temps, essayez un des exemples. 
    



