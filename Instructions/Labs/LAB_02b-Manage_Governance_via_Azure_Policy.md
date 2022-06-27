---
lab:
  title: 02b - Gérer la gouvernance via Azure Policy
  module: Module 02 - Governance and Compliance
ms.openlocfilehash: fad481d30818aaea390ed1357c223f3686671383
ms.sourcegitcommit: 6df80c7697689bcee3616cdd665da0a38cdce6cb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2022
ms.locfileid: "146587428"
---
# <a name="lab-02b---manage-governance-via-azure-policy"></a>Labo 02b - Gérer la gouvernance via Azure Policy
# <a name="student-lab-manual"></a>Manuel de labo pour l’étudiant

## <a name="lab-scenario"></a>Scénario du labo

Pour améliorer la gestion des ressources Azure dans Contoso, vous avez été chargé d’implémenter les fonctionnalités suivantes :

- marquage de groupes de ressources qui incluent uniquement des ressources d’infrastructure (telles que des comptes de stockage Cloud Shell)

- s’assurer que seules les ressources d’infrastructure correctement étiquetées peuvent être ajoutées aux groupes de ressources d’infrastructure

- corriger toutes les ressources non conformes 

## <a name="objectives"></a>Objectifs

Dans ce labo, nous allons :

+ Tâche 1 : Créer et attribuer des balises via le portail Azure
+ Tâche 2 : Mettre en œuvre le balisage via une stratégie Azure
+ Tâche 3 : Appliquer le balisage via une stratégie Azure

## <a name="estimated-timing-30-minutes"></a>Durée estimée : 30 minutes

## <a name="architecture-diagram"></a>Diagramme de l'architecture

![image](../media/lab02b.png)

## <a name="instructions"></a>Instructions

### <a name="exercise-1"></a>Exercice 1

#### <a name="task-1-assign-tags-via-the-azure-portal"></a>Tâche 1 : Attribuer des balises via le portail Azure

Dans cette tâche, vous allez créer et affecter une balise à un groupe de ressources Azure via le portail Azure.

1. Dans le Portail Azure, démarrez une session **PowerShell** dans **Cloud Shell**.

    >**Remarque** : Si c’est la première fois que vous démarrez **Cloud Shell** et que vous voyez le message **Vous n’avez aucun stockage monté**, sélectionnez l’abonnement que vous utilisez dans ce labo, puis sélectionnez **Créer un stockage**. 

1. Dans le volet Cloud Shell, exécutez la commande suivante pour identifier le nom du compte de stockage utilisé par Cloud Shell :

   ```powershell
   df
   ```

1. Dans la sortie de la commande, notez la première partie du chemin complet désignant le montage de lecteur d’accueil Cloud Shell (marqué ici comme `xxxxxxxxxxxxxx`suit :

   ```
   //xxxxxxxxxxxxxx.file.core.windows.net/cloudshell   (..)  /usr/csuser/clouddrive
   ```

1. Dans le Portail Azure, recherchez et sélectionnez **Stockage comptes** et, dans la liste des comptes de stockage, cliquez sur l’entrée représentant le compte de stockage que vous avez identifié à l’étape précédente.

1. Dans le panneau du compte de stockage, cliquez sur le lien représentant le nom du groupe de ressources contenant le compte de stockage.

    **Remarque** : notez le groupe de ressources dans lequel se trouve le compte de stockage, vous en aurez besoin plus tard dans le labo.

1. Dans le panneau du groupe de ressources, cliquez sur **Modifier** en regard des **balises** pour créer de nouvelles balises.

1. Créez une balise avec les paramètres suivants et appliquez votre modification :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom | **Rôle** |
    | Valeur | **Infra** |

1. Retournez au volet de compte de stockage. Passez en revue les informations **Vue d’ensemble** et notez que la nouvelle balise n’a pas été automatiquement affectée au compte de stockage. 

#### <a name="task-2-enforce-tagging-via-an-azure-policy"></a>Tâche 2 : Mettre en œuvre le balisage via une stratégie Azure

Dans cette tâche, vous allez affecter la stratégie intégrée *Exiger une balise et sa valeur sur les ressources* au groupe de ressources et évaluer le résultat. 

1. Dans le portail Azure, recherchez et sélectionnez **Stratégie**. 

1. Dans la section **Création**, cliquez sur **Définitions**. Prenez un moment pour parcourir la liste des définitions de stratégie intégrées qui sont disponibles pour vous permettre d’utiliser. Répertoriez toutes les stratégies intégrées qui impliquent l’utilisation de balises en sélectionnant l’entrée **Balises** (et en désélectionnant toutes les autres entrées) dans la liste déroulante **Catégorie**. 

1. Cliquez sur l’entrée représentant la stratégie intégré **Exiger une balise et sa valeur sur les ressources** et passez en revue sa définition.

1. Dans le panneau de définition de la stratégie intégrée **Exiger une balise et sa valeur sur les ressources**, cliquez sur **Affecter**.

1. Spécifiez **l’étendue** en cliquant sur le bouton Points de suspension et en sélectionnant les valeurs suivantes :

    | Paramètre | Valeur |
    | --- | --- |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Groupe de ressources | nom du groupe de ressources contenant le compte Cloud Shell que vous avez identifié dans la tâche précédente |

    >**Remarque** : Une étendue détermine les ressources ou les groupes de ressources où l’affectation de stratégie prend effet. Vous pouvez affecter des stratégies au niveau du groupe d’administration, de l’abonnement ou du groupe de ressources. Vous avez également la possibilité de spécifier des exclusions, telles que des abonnements individuels, des groupes de ressources ou des ressources (en fonction de l’étendue d’affectation). 

1. Configurez les propriétés **de base** de l’affectation en spécifiant les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom de l’attribution | **Exiger une balise de rôle avec une valeur Infra**|
    | Description | **Exiger une balise de rôle avec une valeur Infra pour toutes les ressources du groupe de ressources Cloud Shell**|
    | Application de stratégies | activé |

    >**Remarque** : Le **Nom de l’attribution** est automatiquement rempli avec le nom de stratégie que vous avez sélectionné, mais vous pouvez le modifier. Vous pouvez également ajouter une **Description** (facultatif). **Affecté par** est automatiquement renseigné en fonction du nom d’utilisateur qui crée l’affectation. 

1. Cliquez sur **Suivant** et définissez **Paramètres** sur les valeurs suivantes :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom de la balise | **Rôle** |
    | Valeur de la balise | **Infra** |

1. Cliquez sur **Suivant** et passez en revue l'onglet **Correction**. Laissez la case **Créer une identité managée** non cochée. 

    >**Remarque** : Ce paramètre peut être utilisé lorsque la stratégie ou l’initiative inclut l’effet **deployIfNotExists** ou **Modify**.

1. Cliquez sur **Vérifier + créer**, puis cliquez sur **Créer**.

    >**Remarque** : Vous allez maintenant vérifier que la nouvelle attribution de stratégie est en vigueur en essayant de créer un autre compte stockage Azure dans le groupe de ressources sans ajouter explicitement la balise requise. 
    
    >**Remarque** : La mise en œuvre de la stratégie peut prendre entre 5 et 15 minutes.

1. Revenez au panneau du groupe de ressources hébergeant le compte de stockage utilisé pour le lecteur d’accueil Cloud Shell, que vous avez identifié dans la tâche précédente.

1. Dans le panneau du groupe de ressources, cliquez sur **+ Créer**, puis recherchez **Compte de stockage**, puis cliquez sur **+ Créer**. 

1. Sous l’onglet **Informations de base** du panneau **Créer un compte de stockage**, vérifiez que vous utilisez le groupe de ressources auquel la stratégie a été appliquée et spécifiez les paramètres suivants (laissez les autres avec leurs valeurs par défaut), cliquez sur **Vérifier + créer**, puis sur **Créer** :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom du compte de stockage | toute combinaison globale unique entre 3 et 24 lettres minuscules et chiffres, en commençant par une lettre |

1. Une fois le déploiement créé, vous devez voir le message **Échec du déploiement** dans la liste **Notifications** du portail. Dans la liste **Notifications**, accédez à la vue d'ensemble du déploiement et cliquez sur le message **Échec du déploiement. Cliquez ici pour obtenir des détails** pour identifier la raison de l'échec. 

    >**Remarque** : Vérifiez si le message d’erreur indique que le déploiement de ressources a été interdit par la stratégie. 

    >**Remarque** : En cliquant sur l’onglet **Erreur brute**, vous trouverez plus d’informations sur l’erreur, notamment le nom de la définition de rôle **Exiger une balise de rôle avec la valeur Infra**. Le déploiement a échoué, car le compte de stockage que vous avez tenté de créer n’a pas de balise nommée **Role** avec sa valeur définie sur **Infra**.

#### <a name="task-3-apply-tagging-via-an-azure-policy"></a>Tâche 3 : Appliquer le balisage via une stratégie Azure

Dans cette tâche, nous allons utiliser une définition de stratégie différente pour corriger toutes les ressources non conformes. 

1. Dans le portail Azure, recherchez et sélectionnez **Stratégie**. 

1. Dans la section **Création**, cliquez sur **Affectations**. 

1. Dans la liste des affectations, cliquez sur l’icône Points de suspension dans la ligne représentant l’affectation de stratégie **Exiger une étiquette de rôle avec une valeur Infra** et utilisez l’élément de menu **Supprimer l’affectation** pour supprimer l’affectation.

1. Cliquez sur **Affecter une stratégie** et spécifiez **l’étendue** en cliquant sur le bouton Points de suspension et en sélectionnant les valeurs suivantes :

    | Paramètre | Valeur |
    | --- | --- |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Groupe de ressources | nom du groupe de ressources contenant le compte Cloud Shell que vous avez identifié dans la première tâche |

1. Pour spécifier la **définition de stratégie**, cliquez sur le bouton Points de suspension, puis recherchez et sélectionnez **Hériter d’une étiquette du groupe de ressources si elle est manquante**.

1. Configurez les propriétés **de base** restantes de l’affectation en spécifiant les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom de l’attribution | **Hériter de la balise Role et de sa valeur Infra du groupe de ressources Cloud Shell si elles manquent**|
    | Description | **Hériter de la balise Role et de sa valeur Infra du groupe de ressources Cloud Shell si elles manquent**|
    | Application de stratégies | activé |

1. Cliquez sur **Suivant** et définissez **Paramètres** sur les valeurs suivantes :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom de la balise | **Rôle** |

1. Cliquez sur **Suivant** et, dans l’onglet **Correction**, configurez les paramètres suivants (et conservez les valeurs par défaut des autres) :

    | Paramètre | Valeur |
    | --- | --- |
    | Créer une tâche de correction | enabled |
    | Stratégie à corriger | **Hériter d’une étiquette du groupe de ressources en cas d’absence** |

    >**Remarque** : Cette définition de stratégie inclut l’effet **Modifier**.

1. Cliquez sur **Vérifier + créer**, puis cliquez sur **Créer**.

    >**Remarque** : Pour vérifier si la nouvelle affectation de stratégie est en vigueur, vous allez créer un autre compte stockage Azure dans le même groupe de ressources sans ajouter explicitement la balise requise. 
    
    >**Remarque** : La mise en œuvre de la stratégie peut prendre entre 5 et 15 minutes.

1. Revenez au panneau du groupe de ressources hébergeant le compte de stockage utilisé pour le lecteur d’accueil Cloud Shell, que vous avez identifié dans la première tâche.

1. Dans le panneau du groupe de ressources, cliquez sur **+ Créer**, puis recherchez **Compte de stockage**, puis cliquez sur **+ Créer**. 

1. Sous l’onglet **Informations de base** du panneau **Créer un compte de stockage**, vérifiez que vous utilisez le groupe de ressources auquel la stratégie a été appliquée et spécifiez les paramètres suivants (laissez les autres avec leurs valeurs par défaut), et cliquez sur **Vérifier + créer** :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom du compte de stockage | toute combinaison globale unique entre 3 et 24 lettres minuscules et chiffres, en commençant par une lettre |

1. Vérifiez que cette fois la validation a passé et cliquez sur **Créer**.

1. Une fois le nouveau compte de stockage configuré, cliquez sur le bouton **Accéder à la ressource** et, dans le panneau **Vue d’ensemble** du compte de stockage nouvellement créé, notez que la balise **Rôle** avec la valeur **Infra** a été automatiquement affectée à la ressource.

#### <a name="task-4-clean-up-resources"></a>Tâche 4 : Nettoyer les ressources

   >**Remarque** : N’oubliez pas de supprimer toutes les nouvelles ressources Azure que vous n’utilisez plus. La suppression de ressources inutilisées garantit que vous ne verrez pas de frais inattendus, mais gardez à l’esprit que les stratégies Azure n’entraînent pas de coûts supplémentaires.
   
   >**Remarque** :  Ne vous inquiétez pas si les ressources de laboratoire ne peuvent pas être immédiatement supprimées. Parfois, les ressources ont des dépendances et prennent plus de temps à supprimer. Il s’agit d’une tâche d’administrateur courante pour surveiller l’utilisation des ressources. Il vous suffit donc de consulter régulièrement vos ressources dans le portail pour voir comment se passe le nettoyage. 

1. Dans le portail, recherchez et sélectionnez **Stratégie**.

1. Dans la section **Création** , cliquez sur **Affectations**, cliquez sur l’icône Points de suspension à droite de l’affectation que vous avez créée dans la tâche précédente, puis cliquez sur **Supprimer l’affectation**. 

1. Dans le portail, recherchez et sélectionnez **Comptes de stockage**.

1. Dans la liste des comptes de stockage, sélectionnez le groupe de ressources correspondant au compte de stockage que vous avez créé dans la dernière tâche de ce laboratoire. Sélectionnez **Balises**, puis cliquez sur **Supprimer** (Corbeille à droite) sur la balise **Role:Infra**, puis appuyez sur **Appliquer**. 

1. Cliquez sur **Vue d’ensemble**, puis cliquez sur **Supprimer** en haut du panneau du compte de stockage. Lorsque vous y êtes invité, dans le panneau **Supprimer le compte de stockage**, tapez le nom du compte de stockage pour confirmer et cliquer sur **Supprimer**. 

#### <a name="review"></a>Révision

Dans cet exercice, vous avez :

- Créé et affecté des balises via le portail Azure
- Mis en œuvre un balisage via une stratégie Azure
- Appliqué un balisage via une stratégie Azure
