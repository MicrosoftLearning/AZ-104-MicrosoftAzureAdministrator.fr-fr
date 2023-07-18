---
lab:
  title: "Labo\_01\_: Gérer les identités Azure Active Directory"
  module: Administer Identity
---

# Labo 01 - Gérer les identités Azure Active Directory

# Manuel de labo pour l’étudiant

## Scénario du labo

Pour permettre aux utilisateurs de Contoso de s’authentifier à l’aide d’Azure AD, vous avez été chargé d’approvisionner des utilisateurs et des comptes de groupe. L’appartenance aux groupes doit être mise à jour automatiquement en fonction du poste de l’utilisateur. Vous devez également créer un locataire Azure AD test avec un compte d’utilisateur test et accorder à ce compte des autorisations limitées aux ressources de l’abonnement Contoso Azure.

**Remarque :** Une **[simulation de labo interactive](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201)** est disponible et vous permet de progresser à votre propre rythme. Il peut exister de légères différences entre la simulation interactive et le labo hébergé. Toutefois, les concepts et idées de base présentés sont identiques.

## Objectifs

Dans ce labo, vous allez :

+ Tâche 1 : Créer et configurer des utilisateurs Azure AD
+ Tâche 2 : Créer des groupes Azure AD avec une appartenance dynamique et attribuée
+ Tâche 3 : Créer un locataire Azure Active Directory (AD) (facultatif - problème d’environnement lab)
+ Tâche 4 : Gérer des utilisateurs invités Azure AD (facultatif - problème d'environnement lab)

## Durée estimée : 30 minutes

## Diagramme de l'architecture
![image](../media/lab01.png)

### Instructions

## Exercice 1

## Tâche 1 : Créer et configurer des utilisateurs Azure AD

Dans cette tâche, vous allez créer et configurer des utilisateurs Azure AD.

>**Remarque** : Si vous avez déjà utilisé la licence d’évaluation pour Azure AD Premium sur ce locataire Azure AD, vous aurez besoin d’un nouveau locataire Azure AD et devrez effectuer la tâche 2 après la tâche 3 dans ce nouveau locataire Azure AD

1. Connectez-vous au [portail Azure](https://portal.azure.com).

1. Dans le Portail Azure, recherchez et sélectionnez **Azure Active Directory**.

1. Dans le panneau Azure Active Directory, faites défiler jusqu’à la section **Gérer**, cliquez sur **Paramètres utilisateur** et passez en revue les options de configuration disponibles.

1. Dans le panneau Azure Active Directory, dans la section **Gérer**, cliquez sur **Utilisateurs**, puis sur votre compte d’utilisateur pour afficher ses paramètres **Profil**. 

1. Cliquez sur **Modifier les propriétés** et, sous l’onglet **Paramètres**, définissez **Lieu d’utilisation** sur **États-Unis**, puis cliquez sur **Enregistrer** pour appliquer le changement.

    >**Remarque** : Cela est nécessaire pour attribuer une licence Azure AD Premium P2 à votre compte d’utilisateur plus loin dans ce laboratoire.

1. Revenez au panneau **Utilisateurs - Tous les utilisateurs**, puis cliquez sur **+ Nouvel utilisateur**.

1. Créez un nouvel utilisateur avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom d’utilisateur principal | **az104-01a-aaduser1** |
    | Nom complet | **az104-01a-aaduser1** |
    | Générer automatiquement le mot de passe | désélectionné |
    | Mot de passe initial | **Choisissez un mot de passe sécurisé** |
    | Intitulé du poste (onglet Propriétés) | **Administrateur cloud** |
    | Département (onglet Propriétés) | **INFORMATIQUE** |
    | Emplacement d’utilisation (onglet Propriétés) | **États-Unis** |

    >**Remarque** : **Copiez dans le Presse-papiers** le **nom d’utilisateur principal** complet (nom d’utilisateur plus domaine). Vous en aurez besoin plus tard dans cette tâche.

1. Dans la liste des utilisateurs, cliquez sur le compte d’utilisateur nouvellement créé pour afficher son panneau.

1. Passez en revue les options disponibles dans la section **Gérer** et notez que vous pouvez identifier les rôles Azure AD attribués au compte d’utilisateur ainsi que les autorisations du compte d’utilisateur aux ressources Azure.

1. Dans la section **Gérer** , cliquez sur **Rôles attribués**, puis cliquez sur le bouton **+ Ajouter une affectation** et attribuez le rôle **Administrateur utilisateur** à **az104-01a-aaduser1**.

    >**Remarque** : Vous avez également la possibilité d’attribuer des rôles Azure AD lors de l’approvisionnement d’un nouvel utilisateur.

1. Ouvrez une fenêtre de navigateur **InPrivate** et connectez-vous au [portail Azure](https://portal.azure.com) à l’aide du compte utilisateur que vous venez de créer. Lorsque vous êtes invité à mettre à jour le mot de passe, remplacez le mot de passe par un mot de passe sécurisé de votre choix. 

    >**Remarque** : Au lieu de taper le nom d’utilisateur (y compris le nom de domaine), vous pouvez coller le contenu du Presse-papiers.

1. Dans la fenêtre du navigateur **InPrivate**, Dans le portail Azure, recherchez et sélectionnez **Azure Active Directory**.

    >**Remarque** : Bien que ce compte d’utilisateur puisse accéder au locataire Azure Active Directory, il n’a pas accès aux ressources Azure. Cela est normal, car ce type d’accès doit être accordé explicitement à l’aide du contrôle d’accès en fonction du rôle (RBAC) d’Azure. 

1. Dans la fenêtre du navigateur **InPrivate**, dans le panneau Azure AD, faites défiler jusqu’à la section **Gérer**, cliquez sur **Paramètres utilisateur** et notez que vous n’avez pas d’autorisations pour modifier les options de configuration.

1. Dans la fenêtre du navigateur **InPrivate**, dans le panneau Azure AD, dans la section **Gérer** , cliquez sur **Utilisateurs**, puis sur **+ Nouvel utilisateur**.

1. Créez un nouvel utilisateur avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom d’utilisateur principal | **az104-01a-aaduser2** |
    | Nom complet | **az104-01a-aaduser2** |
    | Générer automatiquement le mot de passe | désélectionné  |
    | Mot de passe initial | **Choisissez un mot de passe sécurisé** |
    | Fonction | **Administrateur système** |
    | department | **INFORMATIQUE** |
    | Emplacement d’utilisation | **États-Unis** |
    
1. Déconnectez-vous du compteur utilisateur az104-01a-aaduser1 à partir du Portail Azure et fermez la fenêtre de navigateur InPrivate.

## Tâche 2 : Créer des groupes Azure AD avec une appartenance dynamique et attribuée

Dans cette tâche, vous allez créer des groupes Azure Active Directory avec l’appartenance affectée et dynamique.

1. Dans le Portail Azure où vous êtes connecté avec votre **compte d’utilisateur**, revenez au panneau **Vue d’ensemble** du locataire Azure AD et, dans la section **Gérer**, cliquez sur **Licences**.

    >**Remarque** : Les licences Azure AD Premium P1 ou P2 sont requises pour implémenter des groupes dynamiques.

1. Dans la section **Gérer**, cliquez sur **Toutes les produits**.

1. Cliquez sur **+ Essayer/Acheter** et activez l’essai gratuit de Azure AD Premium P2.

1. Actualisez la fenêtre du navigateur pour vérifier que l’activation a réussi. 

 >**Remarque** : L’activation d’une licence peut prendre jusqu’à 10 minutes. Continuez à actualiser la page jusqu’à ce qu’elle apparaisse. Ne continuez pas tant que les licences n’ont pas été activées.

1. Dans le panneau **Licences - Tous les produits**, sélectionnez l’entrée **Azure Active Directory Premium P2**, puis attribuez toutes les options de licence de Azure AD Premium P2 à votre compte d’utilisateur et les deux comptes d’utilisateur nouvellement créés.

1. Dans le portail Azure, revenez au panneau du locataire Azure AD, puis cliquez sur **Groupes**.

1. Utilisez le bouton **+ Nouveau groupe** pour créer un groupe avec les paramètres suivants :

    | Paramètre | Valeur |
    | --- | --- |
    | Type de groupe | **Sécurité** |
    | Nom du groupe | **Administrateurs du cloud informatique** |
    | Description du groupe | **Administrateurs du cloud informatique Contoso** |
    | Type d’appartenance | **Utilisateur dynamique** |

    >**Remarque** : Si la liste déroulante **Type d’appartenance** est grisée, patientez quelques minutes et actualisez la page du navigateur.

1. Cliquez sur **Ajouter une requête dynamique**.

1. Sous l’onglet **Configurer les règles** du panneau **Règles d’appartenance dynamique**, créez une règle avec les paramètres suivants :

    | Paramètre | Valeur |
    | --- | --- |
    | Propriété | **jobTitle** |
    | Opérateur | **Égal à** |
    | Valeur | **Administrateur cloud** |

1. Enregistrez la règle en cliquant sur **+Ajouter une expression** et **Enregistrer**. De retour dans le volet **Nouveau groupe**, cliquez sur **Créer**. 

1. De retour sur le panneau **Groupes - Tous les groupes** du locataire Azure AD, cliquez sur le bouton **+ Nouveau groupe** et créez un groupe avec les paramètres suivants :

    | Paramètre | Valeur |
    | --- | --- |
    | Type de groupe | **Sécurité** |
    | Nom du groupe | **Administrateurs système informatique** |
    | Description du groupe | **Administrateurs système informatique Contoso** |
    | Type d’appartenance | **Utilisateur dynamique** |

1. Cliquez sur **Ajouter une requête dynamique**.

1. Sous l’onglet **Configurer les règles** du panneau **Règles d’appartenance dynamique**, créez une règle avec les paramètres suivants :

    | Paramètre | Valeur |
    | --- | --- |
    | Propriété | **jobTitle** |
    | Opérateur | **Égal à** |
    | Valeur | **Administrateur système** |

1. Enregistrez la règle en cliquant sur **+Ajouter une expression** et **Enregistrer**. De retour dans le volet **Nouveau groupe**, cliquez sur **Créer**. 

1. De retour sur le panneau **Groupes - Tous les groupes** du locataire Azure AD, cliquez sur le bouton **+ Nouveau groupe** et créez un groupe avec les paramètres suivants :

    | Paramètre | Valeur |
    | --- | --- |
    | Type de groupe | **Sécurité** |
    | Nom du groupe | **Administrateurs de laboratoire informatique** |
    | Description du groupe | **Administrateurs de laboratoire informatique Contoso** |
    | Type d’appartenance | **Affecté** |
    
1. Cliquez sur **Aucun membre sélectionné**.

1. Dans le panneau **Ajouter des membres**, recherchez et sélectionnez les groupes **Administrateurs du cloud informatique** et **Administrateurs de système informatique**, puis, dans le panneau **Nouveau groupe**, cliquez sur **Créer**.

1. De retour dans le panneau **Groupes - Tous les groupes**, cliquez sur l’entrée représentant le groupe **Administrateurs du cloud informatique**, puis affichez son panneau **Membres**. Vérifiez que **az104-01a-aaduser1** apparaît dans la liste des membres du groupe.

    >**Remarque** : Vous pouvez rencontrer des retards avec les mises à jour des groupes d’appartenances dynamiques. Pour accélérer la mise à jour, accédez au panneau du groupe, affichez son panneau **Règles d’appartenance dynamique**, **modifiez** la règle répertoriée dans la zone de texte **Syntaxe de la règle** en ajoutant un espace blanc à la fin et **enregistrez** la modification.

1. Retournez dans le panneau **Groupes - Tous les groupes**, cliquez sur l’entrée représentant le groupe **Administrateurs du cloud système**, puis affichez son panneau **Membres**. Vérifiez que **az104-01a-aaduser2** apparaît dans la liste des membres du groupe.

## Tâche 3 : Créer un locataire Azure Active Directory (AD) (facultatif - problème d’environnement lab)

Dans cette tâche, vous allez créer un locataire Azure AD.
    
1. Dans le Portail Azure, recherchez et sélectionnez **Azure Active Directory**.

    >**Remarque** : Il existe un problème connu avec la vérification Captcha dans l’environnement lab. Si vous recevez l’erreur **Échec de la création. Trop de demandes. Veuillez essayer ultérieurement.** , effectuez les actions suivantes :<br>
    - Essayez la création plusieurs fois.<br>
    - Consultez la section **Gérer le locataire** pour garantir que le locataire n’a pas été créé en arrière-plan. <br>
    - Ouvrez une nouvelle fenêtre **InPrivate**, puis, à l’aide du portail Azure, essayez de créer le locataire à partir de là.<br>
     Soulevez le problème avec le formateur, puis utilisez la **[simulation de labo interactif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201)** pour afficher les étapes. <br>
    - Vous pouvez essayer cette tâche ultérieurement, mais la création d’un locataire n’est pas nécessaire dans d’autres labos. 

1. Cliquez sur **Gérer les locataires**, puis sur l’écran suivant, cliquez sur **+ Créer**, puis spécifiez le paramètre suivant :

    | Paramètre | Valeur |
    | --- | --- |
    | Type de répertoire | **Azure Active Directory** |
    
1. Cliquez sur **Suivant : Configuration**

    | Paramètre | Valeur |
    | --- | --- |
    | Nom de l’organisation | **Labo Contoso** |
    | Nom de domaine initial | Nom DNS valide composé de lettres minuscules et de chiffres et commençant par une lettre | 
    | Pays/Région | **États-Unis** |

   > **Remarque** : Le **nom de domaine initial** ne doit pas être un nom légitime qui correspond potentiellement à votre organisation ou à un autre. La coche verte dans la zone de texte **Nom de domaine initial** indique si le nom de domaine que vous avez tapé est valide et unique.

1. Cliquez sur **Vérifier + créer**, puis sur **Créer**.

1. Affichez le panneau du locataire Azure AD nouvellement créé à l'aide du lien **Cliquez ici pour accéder à votre nouveau locataire : Labo Contoso** ou du bouton **Répertoire + Abonnement** (directement à droite du bouton Cloud Shell) dans la barre d'outils du portail Azure.

## Tâche 4 : Gérer des utilisateurs invités Azure AD.

Dans cette tâche, vous allez créer des utilisateurs invités Azure AD et leur accorder l’accès aux ressources dans un abonnement Azure.

1. Dans le portail Azure affichant le locataire Contoso Lab Azure AD, dans la section **Gérer**, cliquez sur **Utilisateurs**, puis sur **+ Nouvel utilisateur**.

1. Créez un nouvel utilisateur avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom d’utilisateur principal | **az104-01b-aaduser1** |
    | Nom complet | **az104-01b-aaduser1** |
    | Générer automatiquement le mot de passe | désélectionné  |
    | Mot de passe initial | **Choisissez un mot de passe sécurisé** |
    | Fonction | **Administrateur système** |
    | department | **INFORMATIQUE** |

1. Cliquez sur le profil nouvellement créé.

    >**Remarque** : **Copiez dans le Presse-papiers** le **nom d’utilisateur principal** complet (nom d’utilisateur plus domaine). Vous en aurez besoin plus tard dans cette tâche.

1. Revenez au premier locataire que vous avez créé précédemment. Pour ce faire, utilisez le bouton **Répertoire + Abonnement** (directement à droite du bouton Cloud Shell) dans la barre d’outils du portail Azure.

1. Revenez au panneau **Utilisateurs - Tous les utilisateurs**, puis cliquez sur **+ Inviter un utilisateur externe**.

1. Invitez un nouvel utilisateur invité avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | E-mail | Nom d’utilisateur principal que vous avez copié précédemment dans cette tâche |
    | Nom d’affichage (onglet Propriétés)  | **az104-01b-aaduser1** |
    | Intitulé du poste (onglet Propriétés) | **Administrateur labo** |
    | Département (onglet Propriétés) | **INFORMATIQUE** |
    | Emplacement d’utilisation (onglet Propriétés) | **États-Unis** |

1. Cliquez sur **Invite**. 

1. Revenez sur le panneau **Utilisateurs : tous les utilisateurs**, cliquez sur l’entrée représentant le compte d’utilisateur invité nouvellement créé.

1. Dans le panneau **az104-01b-aaduser1 - Profil**, cliquez sur **Groupes**.

1. Cliquez sur **+ Ajouter l’appartenance** et ajoutez le compte d’utilisateur invité au groupe **Administrateurs de laboratoire informatique**.


## Tâche 5 : Nettoyer les ressources

> **Remarque** : N’oubliez pas de supprimer toutes les nouvelles ressources Azure que vous n’utilisez plus. La suppression des ressources inutilisées garantit que vous n’encourrez pas de coûts imprévus. Bien que, dans ce cas, il n’y ait pas de frais supplémentaires associés aux locataires Azure Active Directory et à leurs objets, vous pouvez envisager de supprimer les comptes d’utilisateur, les comptes de groupe et le locataire Azure Active Directory que vous avez créé dans ce laboratoire.

 > **Remarque** :  Ne vous inquiétez pas si les ressources de laboratoire ne peuvent pas être immédiatement supprimées. Parfois, les ressources ont des dépendances et leur suppression prend plus de temps. Il s’agit d’une tâche d’administrateur courante pour surveiller l’utilisation des ressources. Il vous suffit donc de consulter régulièrement vos ressources dans le portail pour voir comment se passe le nettoyage. 

1. Dans le **portail Azure**, recherchez et sélectionnez **Azure Active Directory** dans la barre de recherche. Dans **Azure Active Directory** sous **Gérer** sélectionnez **Licences**. Une fois dans **Licences** sous **Gérer**, sélectionnez **Tous les produits**, puis l’élément **Azure Active Directory Premium P2** dans la liste. Passez ensuite à la sélection des **utilisateurs sous licence**. Sélectionnez les comptes d’utilisateur **az104-01a-aaduser1** et **az104-01a-aaduser2** auxquels vous avez attribué des licences dans ce laboratoire, cliquez sur **Supprimer la licence**, puis, lorsque vous êtes invité à confirmer, cliquez sur **Oui**.

1. Dans le portail Azure, accédez au volet **Utilisateurs - Tous les utilisateurs**, cliquez sur l’entrée représentant le compte d’utilisateur invité **az104-01b-aaduser1**, dans le volet **az104-01b-aaduser1 - Profil**, cliquez sur **Supprimer**, puis, lorsque vous êtes invité à confirmer, cliquez sur **Ok**.

1. Répétez la même séquence d’étapes pour supprimer les comptes d’utilisateur restants que vous avez créés dans ce laboratoire.

1. Accédez au panneau **Groupes - Tous les groupes**, sélectionnez les groupes que vous avez créés dans ce laboratoire, cliquez sur **Supprimer**, puis, lorsque vous êtes invité à confirmer, cliquez sur **OK**.

1. Dans le portail Azure, affichez le panneau du locataire Azure AD Contoso Lab à l’aide du bouton **Répertoire + Abonnement** (directement à droite du bouton Cloud Shell) dans la barre d’outils du portail Azure.

1. Accédez au volet **Utilisateurs - Tous les utilisateurs**, cliquez sur l’entrée représentant le compte d’utilisateur **az104-01b-aaduser1**, dans le volet **az104-01b-aaduser1 - Profil**, cliquez sur **Supprimer**, puis, lorsque vous êtes invité à confirmer, cliquez sur **Ok**.

1. Accédez au panneau **Contoso Lab - Vue d’ensemble** du locataire Azure AD Contoso Lab, cliquez sur **Gérer les locataires**, puis, dans l’écran suivant, sélectionnez la case en regard de **Labo Contoso**, cliquez sur **Supprimer**, sur le panneau **Supprimer le locataire « Labo Contoso » ?** , puis sur le lien **Obtenir l’autorisation pour supprimer des ressources Azure**. Dans le panneau **Propriétés** de Azure Active Directory, définissez **Gestion des accès pour les ressources Azure** sur **Oui**, puis cliquez sur **Enregistrer**.

1. Revenez au panneau **Supprimer le locataire « Labo Contoso »,** puis cliquez sur **Actualiser** puis **Supprimer**.

> **Remarque** : Si un locataire dispose d’une licence d’évaluation, vous devez attendre l’expiration de la licence d’évaluation avant de pouvoir supprimer le locataire. Cela n’entraînerait aucun coût supplémentaire.

#### Révision

Dans cet exercice, vous avez :

- Créé et configuré des utilisateurs Azure AD
- Créé des groupes Azure AD avec une adhésion attribuée et dynamique
- Créé un locataire Azure Active Directory (AD)
- Géré des utilisateurs invités Azure AD 
