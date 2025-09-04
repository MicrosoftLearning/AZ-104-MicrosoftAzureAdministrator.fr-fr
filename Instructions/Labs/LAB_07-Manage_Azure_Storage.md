---
lab:
  title: 'Labo 07 : Gérer Stockage Azure'
  module: Administer Azure Storage
---

# Labo 07 : Gérer le Stockage Azure

## Présentation du labo

Dans ce labo, vous découvrez comment créer des comptes de stockage pour des blobs Azure et des fichiers Azure. Vous découvrez comment configurer et sécuriser des conteneurs de blobs. Vous découvrez aussi comment utiliser le navigateur de stockage pour configurer et sécuriser des partages de fichiers Azure. 

Ce labo nécessite un abonnement Azure. Le type de votre abonnement peut affecter la disponibilité des fonctionnalités dans ce labo. Vous pouvez changer la région, mais les étapes sont écrites de façon à utiliser **USA Est**.

## Durée estimée : 50 minutes

## Scénario du labo

Votre organisation stocke actuellement des données dans des magasins de données locaux. La plupart de ces fichiers ne sont pas consultés fréquemment. Vous voulez réduire le coût du stockage en plaçant les fichiers qui ne sont pas consultés fréquemment dans des niveaux de stockage moins chers. Vous voulez également explorer différents mécanismes de protection offerts par le Stockage Azure, notamment l’accès réseau, l’authentification, l’autorisation et la réplication. Enfin, vous voulez déterminer dans quelle mesure Azure Files est adapté à l’hébergement de vos partages de fichiers locaux.

## Simulations de labo interactives

>**Note** : les simulations de labo qui ont été fournies précédemment ont été supprimées.

## Diagramme de l'architecture

![Diagramme des tâches.](../media/az104-lab07-architecture.png)

## Compétences de tâche

+ Tâche 1 : Créer et configurer un compte de stockage. 
+ Tâche 2 : Créer et configurer un stockage blob sécurisé.
+ Tâche 3 : Créer et configurer un stockage de fichiers Azure sécurisé.

## Tâche 1 : Créer et configurer un compte de stockage. 

Dans cette tâche, vous allez créer et configurer un compte de stockage. Le compte de stockage utilisera un stockage géoredondant et n’aura pas d’accès public. 

1. Connectez-vous au **portail Azure** - `https://portal.azure.com`.

1. Recherchez et sélectionnez `Storage accounts`, puis cliquez sur **+ Créer**.

1. Sous l’onglet **Informations de base** du volet **Créer un compte de stockage**, spécifiez les paramètres suivants (conservez les valeurs par défaut pour les autres) :

    | Paramètre | Valeur |
    | --- | --- |
    | Abonnement          | nom de votre abonnement Azure  |
    | Resource group        | **az104-rg7** (créer un nouveau) |
    | Nom du compte de stockage  | Nom global unique comprenant entre 3 et 24 caractères alphanumériques |
    | Région                | **(États-Unis) USA Est**  |
    | Performances           | **Standard** (notez l’option Premium) |
    | Redondance            | **Stockage géoredondant** (notez les autres options)|
    | Permettre l’accès en lecture aux données en cas d’indisponibilité régionale | Activer la case |

    >**Le saviez-vous ?** Vous devez utiliser le niveau de performances Standard pour la plupart des applications. Utilisez le niveau de performances Premium pour les applications d’entreprise ou hautes performances. 

1. Sous l’onglet **Avancé**, utilisez les icônes d’information pour en savoir plus sur les choix. Utilisez les valeurs par défaut. 

1. Dans l’onglet **Réseaux**, dans la section **Accès réseau public**, sélectionnez **Désactiver**. Pour restreindre l’accès entrant tout en autorisant l’accès sortant, sélectionnez Désactiver. 

1. Passez en revue l’onglet **Protection des données**. Notez que la stratégie de rétention pour la suppression réversible est par défaut de 7 jours. Notez que vous pouvez activer le contrôle de version des blobs. Acceptez les valeurs par défaut.

1. Passez en revue l’onglet **Chiffrement**. Notez les options de sécurité supplémentaires. Acceptez les valeurs par défaut.

1. Sélectionnez **Examiner et créer**, attendez que le processus de validation se termine, puis sélectionnez **Créer**.

1. Une fois le compte de stockage déployé, sélectionnez **Accéder à la ressource**.

1. Passez en revue le panneau **Vue d’ensemble** et les configurations supplémentaires qui peuvent être modifiées. Il s’agit de paramètres globaux pour le compte de stockage. Notez que le compte de stockage peut être utilisé pour des conteneurs d’objets blob, des partages de fichiers, des files d’attente et des tables.

1. Dans le panneau **Sécurité + mise en réseau**, sélectionnez **Mise en réseau**. Notez que l’**accès au réseau public** est désactivé.

    + Sélectionnez **Gérer** et modifiez le paramètre **Accès au réseau public** sur **Activé**. 
    + Modifiez le paramètre **Étendue de l’accès au réseau public** en **Activer à partir des réseaux sélectionnés**.
    + Dans la section **Adresses IPv4**, sélectionnez **Ajouter l’adresse IPv4 de votre client**.
    + Enregistrez les changements apportés.
  
1. Dans le panneau **Gestion des données**, sélectionnez **Redondance**. Notez les informations sur les emplacements de votre centre de données principal et de votre centre de données secondaire.

1. Dans le panneau **Gestion des données**, sélectionnez **Gestion du cycle de vie**, puis sélectionnez **Ajouter une règle**.

    + **Nommez la règle **`Movetocool`. Notez vos options pour limiter l’étendue de la règle. Sélectionnez **Suivant**. 
    
    + Sur la page **Ajouter une règle**, *si* les blobs de base ont été modifiés pour la dernière fois il y a plus de `30` jours *, alors* **passez au stockage sporadique**. Notez vos autres choix. 
    
    + Notez que vous pouvez configurer d’autres conditions. Sélectionnez **Ajouter** quand vous avez terminé l’exploration.

    ![Capture d’écran – Conditions de la règle Déplacer vers le niveau froid.](../media/az104-lab07-movetocool.png)

## Tâche 2 : Créer et configurer un stockage blob sécurisé

Dans cette tâche, vous allez créer un conteneur d’objets blob et charger une image. Les conteneurs d’objets blob sont des structures de type répertoire qui stockent des données non structurées.

### Créer un conteneur d’objets blob et une stratégie de rétention basée sur le temps

1. Continuez dans le portail Azure, en utilisant votre compte de stockage.

1. Dans le panneau **Stockage des données**, sélectionnez **Conteneurs**. 

1. Cliquez sur **+ Ajouter un conteneur**, puis sur **Créer** un conteneur avec les paramètres suivants :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom | `data`  |
    | Niveau d'accès public | Notez que le niveau d’accès est défini sur privé. |

    ![Capture d’écran de la création d’un conteneur](../media/az104-lab07-create-container.png)

1. Sur votre conteneur, faites défiler jusqu’aux points de suspension (...) tout à droite, puis sélectionnez **Stratégie d’accès**.

1. Dans la zone **Stockage blob non modifiable**, sélectionnez **Ajouter une stratégie**.

    | Paramètre | Valeur |
    | --- | --- |
    | Type de stratégie | **Rétention basée sur le temps**  |
    | Définir la période de rétention sur | `180` jours |

1. Sélectionnez **Enregistrer**.

### Gérer les chargements de blobs

1. Revenez à la page des conteneurs, sélectionnez votre conteneur **de données**, puis cliquez sur **Charger**.

1. Dans le panneau **Charger l’objet blob**, développez la section **Avancé**.

    >**Remarque** : Recherchez un fichier à charger. Il peut s’agir de n’importe quel type de fichier, mais un petit fichier est préférable. Un exemple de fichier peut être téléchargé à partir du répertoire AllFiles. 

    | Paramètre | Valeur |
    | --- | --- |
    | Parcourez les fichiers. | Ajoutez le fichier que vous avez sélectionné pour le chargement. |
    | Sélectionnez **Avancé** | |
    | Type d’objet blob | **Objet blob de blocs** |
    | Taille de bloc | **4 Mio** |
    | Niveau d’accès | **Chaud**  (notez les autres options) |
    | Charger dans le dossier | `securitytest` |
    | Étendue de chiffrement | Utiliser l’étendue de conteneur par défaut existante |

1. Cliquez sur **Télécharger**.

1. Vérifiez que vous avez un nouveau dossier et que votre fichier a été chargé. 

1. Sélectionnez votre fichier à télécharger et passez en revue les options représentées par les points de suspension (...), notamment **Télécharger**, **Supprimer**, **Modifier le niveau** et **Acquérir le bail**.

1. Copiez le fichier **URL** (Paramètres --> Onglet Propriétés) et collez-le dans une nouvelle fenêtre de navigation **Inprivate**.

1. Un message au format XML indiquant **ResourceNotFound** ou **PublicAccessNotPermitted** doit vous être présenté.

    > **Remarque** : Ceci est attendu, étant donné que le conteneur que vous avez créé a le niveau d’accès public défini sur **Privé (aucun accès anonyme)**.

### Configurer un accès limité au stockage d’objets blob

1. Revenez au fichier que vous avez chargé et sélectionnez les points de suspension (...) à l’extrême droite, puis sélectionnez **Générer une SAP** et spécifiez les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Clé de signature | **Clé 1** |
    | Autorisations | **Lire** (notez vos autres choix) |
    | Date de début | date d’hier |
    | Heure de début | heure actuelle |
    | Date d'expiration | date de demain |
    | Heure d’expiration | heure actuelle |
    | Adresses IP autorisées | laisser vide |

1. Cliquez sur **Générer une URL et un jeton SAS**.

1. Copiez l’entrée **URL SAP d’objet blob** dans le Presse-papiers.

1. Ouvrez une autre fenêtre de navigateur InPrivate et accédez à l’URL SAP d’objet blob que vous avez copiée à l’étape précédente.

    >**Remarque** : Vous devez être en mesure de voir le contenu du fichier. 

## Tâche 3 : Créer et configurer un partage de fichiers Azure

Dans cette tâche, vous allez créer et configurer des partages des fichiers Azure. Vous allez utiliser le navigateur de stockage pour gérer le partage de fichiers. 

### Créer un partage de fichiers et charger un fichier

1. Dans le portail Azure, revenez à votre compte de stockage, puis dans le panneau **Stockage des données**, cliquez sur **Partages de fichiers**.

1. Cliquez sur **+ Partage de fichiers** puis, sous l’onglet **Informations de base**, donnez un nom au partage de fichiers, `share1`. 

1. Notez les options de **Niveau d’accès**. Conservez la **Transaction optimisée** par défaut.
   
1. Accédez à l’onglet **Sauvegarde** et vérifiez que la case **Activer la sauvegarde** n’est **pas** cochée. Nous désactivons la sauvegarde pour simplifier la configuration du labo.

1. Cliquez sur **Vérifier + créer**, puis sur **Créer**. Attendez que le partage de fichiers soit déployé.

    ![Capture d’écran de la page Créer un partage de fichiers.](../media/az104-lab07-create-share.png)

### Explorer le navigateur de stockage et charger un fichier

1. Revenez à votre compte de stockage et sélectionnez **Navigateur de stockage**. Le navigateur de stockage Azure est un outil du portail qui vous permet de voir rapidement tous les services de stockage existant sous votre compte.

1. Sélectionnez **Partages de fichiers** et vérifiez que votre répertoire **share1** est bien présent.

1. Sélectionnez votre répertoire **share1** et notez que vous pouvez **+ Ajouter un répertoire**. Ceci vous permet de créer une structure de dossiers.

1. Sélectionnez **Télécharger**. Accédez à un fichier de votre choix, puis cliquez sur **Charger**.

    >**Remarque** : Vous pouvez voir des partages de fichiers et gérer ces partages dans le navigateur de stockage. Il n’existe actuellement aucune restriction.

### Restreindre l’accès réseau au compte de stockage

1. Dans le portail, recherchez et sélectionnez `Virtual networks`.

1. Sélectionnez **+ Créer**. Sélectionnez votre groupe de ressources. Donnez un **nom** au réseau virtuel, `vnet1`.

1. Utilisez les valeurs par défaut pour d’autres paramètres, sélectionnez **Vérifier + créer**, puis **Créer**.

1. Attendez que le réseau virtuel soit déployé, puis sélectionnez **Accéder à la ressource**.

1. Dans la section **Paramètres**, sélectionnez le volet **Points de terminaison de service**.
    + Sélectionnez **Ajouter**. 
    + Dans la liste déroulante **Services** , sélectionnez **Microsoft.Storage**.
    + Dans la liste déroulante **Sous-réseaux** vérifiez le sous-réseau **par défaut**.
    + Cliquez sur **Ajouter** pour enregistrer vos modifications.  

1. Revenez à votre compte de stockage.

1. Dans le panneau **Sécurité + mise en réseau**, sélectionnez **Mise en réseau**.

1. Sous **Accès au réseau public**, sélectionnez **Gérer**. 

1. Sélectionnez **Ajouter un réseau virtuel**, puis **Ajouter un réseau existant**.

1. Sélectionnez **vnet1** et le sous-réseau **par défaut**, puis cliquez sur **Ajouter**.

1. Dans la section **Adresses IPv4**, **Supprimez** l’adresse IP de votre machine. Le trafic autorisé doit provenir seulement du réseau virtuel. 

1. Veillez à **Enregistrer** vos modifications.

    >**Remarque :** Le compte de stockage doit maintenant être accessible seulement depuis le réseau virtuel que vous venez de créer. 

1. Sélectionnez le **navigateur de stockage**, puis **actualisez** la page. Accédez au contenu de votre partage de fichiers ou de votre blob.  

    >**Remarque :** Vous devez recevoir un message *Vous n’êtes pas autorisé à effectuer cette opération.*. Vous ne vous connectez pas à partir du réseau virtuel. Quelques minutes peuvent être nécessaires pour que ceci soit pris en compte. Vous pouvez toujours afficher le partage de fichiers, mais pas les fichiers ou les blobs dans le compte de stockage. 


![Capture d’écran d’un accès non autorisé.](../media/az104-lab07-notauthorized.png)

## Nettoyage de vos ressources

Si vous travaillez avec **votre propre abonnement**, prenez un moment pour supprimer les ressources du labo. Ceci garantit que les ressources sont libérées et que les coûts sont réduits. Le moyen le plus simple de supprimer les ressources du labo est de supprimer le groupe de ressources du labo. 

+ Dans le Portail Azure, sélectionnez le groupe de ressources, **Supprimer le groupe de ressources**, **Entrer le nom du groupe de ressources**, puis cliquez sur **Supprimer**.
+ `Remove-AzResourceGroup -Name resourceGroupName` en utilisant Azure PowerShell.
+ `az group delete --name resourceGroupName` en utilisant l’interface CLI.

## Développer votre apprentissage avec Copilot
Copilot peut vous aider à apprendre à utiliser les outils de script Azure. Copilot peut également aider dans des domaines non couverts dans le labo ou quand vous avez besoin de plus d’informations. Ouvrez un navigateur Edge et choisissez Copilot (en haut à droite), ou accédez à *copilot.microsoft.com*. Prenez quelques minutes pour essayer ces prompts.

+ Fournir un script Azure PowerShell pour créer un compte de stockage avec un conteneur d’objets blob. 
+ Fournir une liste de contrôle que je peux utiliser pour vérifier que mon compte de stockage Azure est sécurisé.
+ Créer un tableau pour comparer les modèles de redondance du stockage Azure.

## En savoir plus grâce à l’apprentissage auto-rythmé

+ [Créer un compte de stockage Azure](https://learn.microsoft.com/training/modules/create-azure-storage-account/) Créez un compte de Stockage Azure avec les options adéquates pour les besoins de votre entreprise.
+ [Gérez le cycle de vie du stockage Blob Azure](https://learn.microsoft.com/training/modules/manage-azure-blob-storage-lifecycle). Apprenez à gérer la disponibilité des données tout au long du cycle de vie du stockage Blob Azure.

## Points clés

Félicitations, vous avez terminé le labo. Voici les principaux points à retenir pour ce labo. 

+ Un compte de stockage Azure contient tous vos objets de données Stockage Azure : les blobs, les fichiers, les files d’attente et les tables. Le compte de stockage fournit pour vos données de stockage Azure un espace de noms unique, accessible de n’importe où dans le monde via HTTP ou HTTPS.
+ Stockage Azure fournit plusieurs modèles de redondance, notamment le stockage localement redondant (LRS), le stockage redondant interzone (ZRS) et le stockage géoredondant (GRS). 
+ Stockage blob Azure vous permet de stocker de grandes quantités de données non structurées sur la plateforme de stockage de données de Microsoft. Blob signifie Binary Large Object. Il contient des objets tels que des images et des fichiers multimédias.
+ Stockage Azure Files fournit un stockage partagé pour les données structurées. Les données peuvent être organisées dans des dossiers.
+ Le stockage immuable vous permet de stocker des données dans un état WORM (Write Once, Read Many). Les stratégies de stockage immuable peuvent être basées sur le temps ou sur la durée de conservation légale.
