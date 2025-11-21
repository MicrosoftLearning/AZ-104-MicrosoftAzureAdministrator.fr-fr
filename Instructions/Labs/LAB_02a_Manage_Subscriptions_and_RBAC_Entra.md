---
lab:
  title: 'Labo 02a : Gérer les abonnements et RBAC'
  module: Administer Governance and Compliance
---

# Labo 02a - Gérer les abonnements et RBAC

## Présentation du labo

Dans ce labo, vous découvrez le contrôle d’accès en fonction du rôle. Vous découvrez la manière d’utiliser des autorisations et des étendues pour contrôler les actions que les identités peuvent et ne peuvent pas effectuer. Vous découvrez également comment faciliter la gestion des abonnements en utilisant des groupes d’administration. 

Ce labo nécessite un abonnement Azure. Le type de votre abonnement peut affecter la disponibilité des fonctionnalités dans ce labo. Vous pouvez changer la région, mais les étapes sont écrites de façon à utiliser **USA Est**. 

## Durée estimée : 30 minutes

## Scénario du labo

Pour simplifier la gestion des ressources Azure dans votre organisation, vous êtes chargé d’implémenter les fonctionnalités suivantes :

- Création d’un groupe d’administration qui inclut tous vos abonnements Azure.

- Octroi d’autorisations pour envoyer des demandes de support pour tous les abonnements du groupe d’administration. Les autorisations doivent être limitées uniquement aux éléments suivants : 

    - Créer et gérer des machines virtuelles
    - Créer des tickets de demande de support (ne pas inclure l’ajout de fournisseurs Azure)

## Diagramme de l'architecture

![Diagramme des tâches de labo.](../media/az104-lab02a-architecture.png)

## Compétences de tâche

+ Tâche 1 : Implémentez des groupes d’administration.
+ Tâche 2 : passer en revue et attribuer un rôle Azure intégré.
+ Tâche 3 : créer un rôle RBAC personnalisé.
+ Tâche 4 : Surveillez des attributions de rôle avec le journal d’activité.

## Tâche 1 : Implémenter des groupes d’administration

Dans cette tâche, vous allez créer et configurer des groupes d’administration. Les groupes d’administration servent à logiquement organiser et segmenter des abonnements. Ils permettent au RBAC et à Azure Policy d’être affectés et hérités par d’autres groupes d’administration et abonnements. Par exemple, si votre organisation dispose d’une équipe de support technique dédiée pour l’Europe, vous pouvez organiser des abonnements européens en groupe d’administration pour fournir au personnel de support l’accès à ces abonnements (sans fournir d’accès individuel à tous les abonnements). Dans notre scénario, tous les membres du support technique devront créer une demande de support sur tous les abonnements. 

1. Connectez-vous au **portail Azure** - `https://portal.azure.com`.

1. Recherchez et sélectionnez `Microsoft Entra ID`.

1. Dans le panneau **Gérer**, sélectionnez **Propriétés**.

1. Passez en revue la zone **Gestion de l’accès pour les ressources Azure**. Notez/lisez que vous pouvez gérer l’accès à tous les abonnements Azure et groupes de gestion dans le locataire.
   
1. Recherchez et sélectionnez **Groupes d’administration**.

1. Dans le panneau **Groupes d’administration**, cliquez sur **+ Créer**.

1. Créez un groupe d’administration avec les paramètres suivants. Cliquez sur **Soumettre** lorsque vous avez terminé. 

    | Paramètre | Valeur |
    | --- | --- |
    | ID du groupe d'administration | `az104-mg1` (doit être unique dans le répertoire) |
    | Nom d’affichage du groupe d’administration | `az104-mg1` |

1. **Actualisez** la page du groupe d’administration pour veiller à ce que votre nouveau groupe d’administration s’affiche. Cela peut prendre une minute. 

   >**Remarque :** Avez-vous remarqué le groupe d’administration racine ? Le groupe d’administration racine est intégré à la hiérarchie et contient tous les groupes d’administration et abonnements. Il permet d’appliquer des stratégies globales et des affectations de rôles Azure au niveau de l’annuaire. Une fois le groupe d’administration créé, vous devez ajouter tous les abonnements qui doivent être inclus dans le groupe. 

## Tâche 2 : Passez en revue et attribuez un rôle Azure intégré.

Dans cette tâche, vous allez passer en revue les rôles intégrés et attribuer le rôle Contributeur de machine virtuelle à un membre du support technique. Azure propose un grand nombre de [rôles intégrés](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles). 

>**Remarque :** Dans les étapes suivantes, vous allez attribuer le rôle au groupe de **support technique**. Si vous ne disposez pas de groupe de support technique, prenez une minute pour le créer.

1. Sélectionnez le groupe d’administration **az104-mg1**.

1. Sélectionnez le panneau **Contrôle d’accès (IAM)**, puis l’onglet **Rôles**.

1. Faites défiler les définitions de rôle intégrées disponibles. **Affichez** un rôle pour obtenir des informations détaillées sur les **Autorisations**, **JSON** et les **Attributions**. Vous allez souvent utiliser *propriétaire*, *contributeur* et *lecteur*. 

1. Sélectionnez **+ Ajouter** dans le menu déroulant, puis **Ajouter une attribution de rôle**. 

1. Sur le panneau **Ajouter une attribution de rôle**, recherchez et sélectionnez le rôle **Contributeur de machine virtuelle**. Le rôle de contributeur de machine virtuelle permet de gérer des machines virtuelles, mais pas d’accéder à leur système d’exploitation ni de gérer le réseau virtuel et le compte de stockage auxquels ils sont connectés. Il constitue un bon rôle pour le support technique. Cliquez sur **Suivant**.

    >**Le saviez-vous ?** À l’origine, Azure fournissait uniquement le modèle de déploiement **Classique**. Il a été remplacé par le modèle de déploiement **Azure Resource Manager**. En tant que meilleure pratique, n’utilisez aucune ressource classique.

1. Sous l’onglet **Membres**, **Sélectionner des membres**.

1. Recherchez et sélectionnez le groupe `helpdesk`. Cliquez sur **Sélectionner**. 

1. Cliquez sur **Vérifier + attribuer** à deux reprises pour créer l’attribution de rôle.

1. Passez au panneau **Contrôle d’accès (IAM)**. Sous l’onglet **Attributions de rôles**, vérifiez que le groupe de **support technique** a le rôle **Contributeur de machine virtuelle**. 

    >**Remarque :** En tant que meilleure pratique, attribuez toujours des rôles à des groupes et non à des personnes. 

    >**Le saviez-vous ?** Il est possible que cette attribution ne vous accorde aucun privilège supplémentaire. Si vous avez déjà le rôle Propriétaire, ce rôle inclut toutes les autorisations associées au rôle Contributeur de machine virtuelle.
    
## Tâche 3 : Créez un rôle RBAC personnalisé.

Dans cette tâche, vous allez créer un rôle RBAC personnalisé. Les rôles personnalisés font partie intégrante de l’implémentation du principe du privilège minimum d’un environnement. Il est possible que les rôles intégrés aient trop d’autorisations pour votre scénario. Nous allons également créer un rôle et supprimer les autorisations inutiles. Avez-vous un plan pour gérer des autorisations qui se chevauchent ?

1. Continuez à travailler sur votre groupe d’administration. Accédez au panneau **Contrôle d’accès (IAM)**.

1. Sélectionnez **+ Ajouter** dans le menu déroulant, puis **Ajouter un rôle personnalisé**.

1. Sous l’onglet Informations de base, terminez la configuration.

    | Paramètre | Valeur |
    | --- | --- |
    | Nom de rôle personnalisé | `Custom Support Request` |
    | Description | `A custom contributor role for support requests.` |

1. Pour les **Autorisations de base**, sélectionnez **Cloner un rôle**. Dans le menu déroulant **Rôle à cloner**, sélectionnez **Contributeur de demande de support**.

    ![Capture d’écran du clonage de rôle.](../media/az104-lab02a-clone-role.png)

1. Sélectionnez **Suivant** pour accéder à l’onglet **Autorisations**, puis sélectionnez **+ Exclure les autorisations**.

1. Dans le champ de recherche du fournisseur de ressources, entrez `.Support` et sélectionnez **Microsoft.Support**.

1. Dans la liste des autorisations, placez une case à cocher à côté de **Autres : Inscrit le fournisseur de ressources de support**, puis sélectionnez **Ajouter**. Le rôle doit être mis à jour pour inclure cette autorisation en tant que *NotAction*.

    >**Remarque :** Un fournisseur de ressources Azure est un ensemble d’opérations REST qui active une fonctionnalité pour un service Azure spécifique. Nous ne voulons pas que le support technique puisse disposer de cette fonctionnalité. Elle est donc supprimée du rôle cloné. 

1. Sous l’onglet **Étendues attribuables**, vérifiez que votre groupe d’administration est répertorié, puis cliquez sur **Suivant**.

1. Passez en revue le JSON pour les *Actions*, *NotActions* et *AssignableScopes* personnalisés dans le rôle. 

1. Sélectionnez **Vérifier + créer**, puis **Créer**.

    >**Remarque :** À ce stade, vous avez créé un rôle personnalisé et vous l’avez affecté au groupe d’administration.  

## Tâche 4 : Surveillez des attributions de rôle avec le journal d’activité.

Dans cette tâche, vous affichez le journal d’activité pour déterminer si une personne a créé un rôle. 

1. Dans le portail, recherchez la ressource **az104-mg1** et sélectionnez **Journal d’activité**. Le journal d’activité fournit des insights sur les événements au niveau de l’abonnement. 

1. Passez en revue les activités pour les attributions de rôles. Vous pouvez filtrer le journal d’activité pour des opérations spécifiques. 

    ![Capture d’écran de la page Journal d’activité avec un filtre configuré.](../media/az104-lab02a-searchactivitylog.png)

## Nettoyage de vos ressources

Si vous travaillez avec **votre propre abonnement**, prenez un moment pour supprimer les ressources du labo. Ceci garantit que les ressources sont libérées et que les coûts sont réduits. Le moyen le plus simple de supprimer les ressources du labo est de supprimer le groupe de ressources du labo. 

+ Dans le Portail Azure, sélectionnez le groupe de ressources, **Supprimer le groupe de ressources**, **Entrer le nom du groupe de ressources**, puis cliquez sur **Supprimer**.
+ `Remove-AzResourceGroup -Name resourceGroupName` en utilisant Azure PowerShell.
+ `az group delete --name resourceGroupName` en utilisant l’interface CLI.
  
## Développer votre apprentissage avec Copilot

Copilot peut vous aider à apprendre à utiliser les outils de script Azure. Copilot peut également aider dans des domaines non couverts dans le labo ou quand vous avez besoin de plus d’informations. Ouvrez un navigateur Edge et choisissez Copilot (en haut à droite), ou accédez à *copilot.microsoft.com*. Prenez quelques minutes pour essayer ces prompts.
+ Créer deux tables mettant en évidence des commandes PowerShell et CLI importantes pour obtenir des informations sur les abonnements des organisations sur Azure et expliquer chaque commande dans la colonne « Explication ». 
+ Quel est le format du fichier JSON de RBAC Azure ?
+ Quelles sont les étapes de base pour créer un rôle RBAC Azure personnalisé ?
+ Quelle est la différence entre les rôles RBAC Azure et les rôles Microsoft Entra ID ?

## En savoir plus grâce à l’apprentissage auto-rythmé

+ [Sécurisez vos ressources Azure avec le contrôle d’accès en fonction du rôle Azure (Azure RBAC)](https://learn.microsoft.com/training/modules/secure-azure-resources-with-rbac/). Utilisez Azure RBAC pour gérer l’accès aux ressources dans Azure.

## Points clés

Félicitations, vous avez terminé le labo. Voici les principaux points à retenir de ce labo. 

+ Vous utilisez des groupes d’administration pour organiser logiquement des abonnements.
+ Le groupe d’administration racine intégré contient tous les groupes d’administration et les abonnements.
+ Azure dispose de plusieurs rôles intégrés. Vous pouvez attribuer ces rôles pour contrôler l’accès aux ressources.
+ Vous pouvez créer des rôles ou personnaliser des rôles existants.
+ Les rôles sont définis dans un fichier au format JSON et incluent *Actions*, *NotActions* et *AssignableScopes*.
+ Vous pouvez utiliser le journal d’activité pour surveiller les attributions de rôles.










