---
lab:
  title: "Lab\_01\_: Gérer les identités Microsoft\_Entra\_ID"
  module: Administer Identity
---

# Lab 01 - Gérer les identités Microsoft Entra ID

## Présentation du labo

Il s’agit du premier d’une série de labos pour les administrateurs Azure. Dans ce labo, vous découvrez les utilisateurs et les groupes. Les utilisateurs et les groupes constituent les blocs de construction de base d’une solution d’identité. 

Ce labo nécessite un abonnement Azure. Le type de votre abonnement peut affecter la disponibilité des fonctionnalités dans ce labo. Vous pouvez changer la région, mais les étapes sont écrites de façon à utiliser **USA Est**. 


## Durée estimée : 30 minutes

## Scénario du labo

Votre organisation crée un nouvel environnement de labos pour les tests de préproduction des applications et des services.  Quelques ingénieurs sont embauchés pour gérer l’environnement du labo, notamment les machines virtuelles. Pour permettre aux ingénieurs de s’authentifier en utilisant Microsoft Entra ID, vous êtes chargé de l’approvisionnement des utilisateurs et des groupes. Pour réduire la surcharge administrative, l’appartenance aux groupes doit être mise à jour automatiquement en fonction des descriptions de postes. 

## Diagramme de l'architecture

![Diagramme de l’architecture du labo 01.](../media/az104-lab01-architecture.png)

## Compétences de tâche

+ Tâche 1 : Créez et configurez des comptes d’utilisateur.
+ Tâche 2 : Créez des groupes et ajoutez-y des membres.

## Tâche 1 : Créer et configurer des comptes d’utilisateur

Dans cette tâche, vous allez créer et configurer des comptes d’utilisateur. Les comptes d’utilisateur stockent les données utilisateur telles que le nom, le service, l’emplacement et les informations sur le contact.

1. Connectez-vous au **portail Azure** - `https://portal.azure.com`.

1. Pour passer au portail, sélectionnez **Annuler** sur l’écran de démarrage **Bienvenue dans Azure**. 

    >**Remarque :** Le Portail Azure est utilisé dans tous les labos. Si vous débutez sur Azure, recherchez et sélectionnez `Quickstart Center`. Prenez quelques minutes pour regarder la vidéo **Bien démarrer dans le Portail Azure** (en anglais). Même si vous avez déjà utilisé le portail, vous y trouverez quelques conseils et astuces sur la navigation et la personnalisation de l’interface.
    
1. Recherchez et sélectionnez `Microsoft Entra ID`. Microsoft Entra ID est la solution de gestion des identités et accès cloud d’Azure. Prenez quelques minutes pour vous familiariser avec certaines des fonctionnalités répertoriées dans le volet de gauche. 

1. Sélectionnez le panneau **Vue d’ensemble**, puis l’onglet **Gérer les locataires**. 

    >**Le saviez-vous ?** Un tenant est une instance spécifique de Microsoft Entra ID contenant des comptes et des groupes. Selon votre situation, vous pouvez créer d’autres tenants et **Basculer** entre eux. 

1. Revenez à la page **Entra ID** en appuyant sur le bouton Précédent dans le navigateur ou en sélectionnant l’option dans le menu de navigation.

1. Lorsque vous avez le temps, explorez d’autres options telles que **Licences** et **Réinitialisation de mot de passe**.
   
### Créer un utilisateur

1. Sélectionnez **Utilisateurs**, puis dans la liste déroulante **Nouvel utilisateur**, sélectionnez **Créer un utilisateur**. 

1. Créez un utilisateur avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut). Sous l’onglet **Propriétés**, notez tous les types d’informations que vous pouvez inclure dans le compte d’utilisateur. 

    | Paramètre | Valeur |
    | --- | --- |
    | Nom d’utilisateur principal | `az104-user1` |
    | Nom d’affichage | `az104-user1` |
    | Générer automatiquement le mot de passe | **checked** |
    | Compte activé | **checked** |
    | Intitulé du poste (onglet Propriétés) | `IT Lab Administrator` |
    | Département (onglet Propriétés) | `IT` |
    | Emplacement d’utilisation (onglet Propriétés) | **États-Unis** |

1. Une fois votre examen terminé, sélectionnez **Vérifier + créer**, puis **Créer**.

1. Actualisez la page et confirmez la création de votre nouvel utilisateur. 

### Inviter un utilisateur externe

1. Dans la liste déroulante **Nouvel utilisateur**, sélectionnez **Inviter un utilisateur externe**. 

    | Paramètre | Valeur |
    | --- | --- |
    | E-mail | votre adresse e-mail |
    | Nom d’affichage | votre nom |
    | Envoyer un message d’invitation | **Cocher la case** |
    | Message | `Welcome to Azure and our group project` |

1. Passez à l’onglet **Propriétés**. Remplissez les informations de base, notamment ces champs. 

    | Paramètre | Valeur |
    | --- | --- |
    | Titre de la fonction  | `IT Lab Administrator` |
    | Service  | `IT` |
    | Emplacement d’utilisation (onglet Propriétés) | **États-Unis** |

1. Sélectionnez **Vérifier + inviter**, puis **Inviter**.

1. **Actualisez la page** et confirmez la création de l’utilisateur invité. Vous devriez recevoir l’e-mail d’invitation sous peu. 

    >**Remarque :** Il est peu probable que vous effectuiez la création de comptes d’utilisateur de manière individuelle. Savez-vous comment votre organisation prévoit de créer et de gérer des comptes d’utilisateur ?
    
## Tâche 2 : Créer des groupes et y ajoutez des membres

Dans cette tâche, vous créez un compte de groupe. Les comptes de groupes peuvent inclure des comptes d’utilisateur ou des appareils. L’attribution de membres à des groupes s’effectue de deux façons : Statiquement ou dynamiquement. Les groupes statiques nécessitent l’ajout et la suppression manuels de membres par les administrateurs.  Les groupes dynamiques sont mis à jour automatiquement selon les propriétés d’un compte d’utilisateur ou d’un appareil. Par exemple, un poste. 

1. Dans le portail Azure, recherchez et sélectionnez `Microsoft Entra ID`. Dans le panneau **Gérer**, sélectionnez **Groupes**. 

1. Prenez une minute pour vous familiariser avec les paramètres du groupe dans le volet de gauche.

   + L’**Expiration** vous permet de configurer une durée de vie de groupe en jours. Après cette période, le propriétaire doit renouveler le groupe.
   + La **Stratégie d’affectation de noms** vous permet de configurer des mots bloqués et d’ajouter un préfixe ou un suffixe aux noms de groupes.

1. Dans le panneau **Tous les groupes**, sélectionnez **+ Nouveau groupe** et créez un groupe.     

    | Paramètre | Valeur |
    | --- | --- |
    | Type de groupe | **Sécurité** |
    | Nom du groupe | `IT Lab Administrators` |
    | Description du groupe | `Administrators that manage the IT lab` |
    | Type d’appartenance | **Affecté** |

    >**Remarque** : Une licence Entra ID Premium P1 ou P2 est requise pour une appartenance dynamique. Si d’autres **Types d’appartenance** sont disponibles, les options s’affichent dans la liste déroulante. 
    
    ![Capture d’écran de la création d’un groupe attribué.](../media/az104-lab01-create-assigned-group.png)

1. Sélectionnez **Aucun propriétaire sélectionné**.

1. À la page **Ajouter des propriétaires**, recherchez et **sélectionnez** votre compte (affiché dans le coin supérieur droit) en tant que propriétaire. Remarquez que vous pouvez avoir plusieurs propriétaires. 

1. Choisissez **Aucun membre sélectionné**.

1. Dans le volet **Ajouter des membres**, recherchez et **sélectionnez** l’utilisateur **az104-user1** et l’**utilisateur invité** que vous avez invités. Ajoutez les deux utilisateurs au groupe. 

1. Sélectionnez **Créer** pour déployer le groupe.

1. **Actualisez** la page et vérifiez que votre groupe a été créé.

1. Sélectionnez le nouveau groupe et passez en revue les informations sur les **Membres** et **Propriétaires**.

>**Remarque :** Il est possible que vous gériez un grand nombre de groupes. Votre organisation dispose-t-elle d’un plan pour créer des groupes et ajouter des membres ?
   
## Développer votre apprentissage avec Copilot

Copilot peut vous aider à apprendre à utiliser les outils de script Azure. Copilot peut également aider dans des domaines non couverts dans le labo ou quand vous avez besoin de plus d’informations. Ouvrez un navigateur Edge et choisissez Copilot (en haut à droite), ou accédez à *copilot.microsoft.com*. Prenez quelques minutes pour essayer ces prompts.
+ Quelles sont les commandes Azure PowerShell et CLI pour créer un groupe de sécurité nommé Administrateurs informatiques ? Fournissez la page officielle de la référence de commande.  
+ Offrez une stratégie pas à pas pour gérer des utilisateurs et des groupes dans Microsoft Entra ID.
+ Quelles sont les étapes dans le Portail Azure pour créer en bloc des utilisateurs et des groupes ?
+ Fournissez un tableau comparatif des comptes d’utilisateur interne et externe de Microsoft Entra ID. 


## En savoir plus grâce à l’apprentissage auto-rythmé

+ [Maîtrisez Microsoft Entra ID](https://learn.microsoft.com/training/modules/understand-azure-active-directory/). Comparez Microsoft Entra ID à Active Directory DS, découvrez Microsoft Entra ID P1 et P2 et explorez Microsoft Entra Domain Services pour gérer les appareils et applications joints à un domaine dans le cloud.
+ [Créez des utilisateurs et des groupes Azure dans Microsoft Entra ID](https://learn.microsoft.com//training/modules/create-users-and-groups-in-azure-active-directory/). Créez des utilisateurs dans Microsoft Entra ID. Découvrez les différents types de groupes. Créez un groupe et ajoutez-y des membres. Gérez les comptes Invité B2B.
+ [Autorisez les utilisateurs à réinitialiser leur mot de passe avec la réinitialisation de mot de passe en libre-service Microsoft Entra](https://learn.microsoft.com/training/modules/allow-users-reset-their-password/). Évaluez la réinitialisation de mot de passe en libre-service pour permettre aux utilisateurs de votre organisation de réinitialiser leur mot de passe ou de déverrouiller leurs comptes. Installez, configurez et testez la réinitialisation de mot de passe en libre-service.


## Points clés

Félicitations, vous avez terminé le labo. Voici quelques points principaux à retenir pour ce labo :

+ Un tenant représente votre organisation et vous permet de gérer une instance particulière des services cloud de Microsoft pour vos utilisateurs internes et externes.
+ Microsoft Entra ID dispose de comptes d’utilisateur et d’invité. Chaque compte d’utilisateur a un niveau d’accès spécifique adapté à l’étendue du travail qu’il doit effectuer.
+ Les groupes combinent des utilisateurs ou des appareils associés. Il existe deux types de groupe, notamment Sécurité et Microsoft 365.
+ Vous pouvez attribuer une appartenance à un groupe de manière statique ou dynamique.
