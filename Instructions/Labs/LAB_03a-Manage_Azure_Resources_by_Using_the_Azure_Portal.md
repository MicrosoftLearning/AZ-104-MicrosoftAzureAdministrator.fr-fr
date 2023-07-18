---
lab:
  title: "Labo\_03a\_: Gérer les ressources Azure en utilisant le portail Azure"
  module: Administer Azure Resources
---

# Labo 03a : Gérer les ressources Azure en utilisant le portail Azure
# Manuel de labo de l’étudiant

## Scénario du labo

Vous devez explorer les fonctionnalités d’administration Azure de base servant à approvisionner des ressources et à les organiser en groupes de ressources, notamment en déplaçant des ressources entre des groupes de ressources. Vous devez également explorer les options permettant de protéger les ressources de disque contre les risques de suppression accidentelle, tout en permettant de modifier leurs caractéristiques de performances et leur taille.

**Remarque :** Une **[simulation de labo interactive](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%204)** est disponible et vous permet de progresser à votre propre rythme. Il peut exister de légères différences entre la simulation interactive et le labo hébergé. Toutefois, les concepts et idées de base présentés sont identiques. 

## Objectifs

Dans ce labo, nous allons :

+ Tâche 1 : Créer des groupes de ressources et y déployer des ressources
+ Tâche 2 : Déplacer des ressources entre des groupes de ressources
+ Tâche 3 : Implémenter et tester des verrous de ressources

## Durée estimée : 20 minutes

## Diagramme de l'architecture

![image](../media/lab03a.png)

### Instructions

## Exercice 1

## Tâche 1 : Créer des groupes de ressources et y déployer des ressources

Dans cette tâche, vous allez utiliser le portail Azure pour créer des groupes de ressources et créer un disque dans ce groupe de ressources.

1. Connectez-vous au [**portail Azure**](http://portal.azure.com).

1. Dans le portail Azure, recherchez et sélectionnez **Disques**, cliquez sur **+ Créer** et spécifiez les paramètres suivants :

    |Paramètre|Valeur|
    |---|---|
    |Abonnement| nom de l’abonnement Azure dans lequel vous avez créé le groupe de ressources |
    |Groupe de ressources| nom d’un nouveau groupe de ressources **az104-03a-rg1** |
    |Nom du disque| **az104-03a-disk1** |
    |Région| **(États-Unis) USA Est** |
    |Zone de disponibilité| **Aucun** |
    |Type de source| **Aucun** |

    >**Remarque** : Lors de la création d’une ressource, vous avez la possibilité de créer un groupe de ressources ou d’en utiliser un existant.

1. Remplacez respectivement le type et la taille du disque par **HDD Standard** et **32 Gio**.

1. Cliquez sur **Vérifier + créer**, puis cliquez sur **Créer**.

    >**Remarque** : Attendez que le disque soit créé. L’opération doit prendre moins d’une minute.

## Tâche 2 : Déplacer des ressources entre des groupes de ressources 

Dans cette tâche, nous allons déplacer la ressource de disque que vous avez créée dans la tâche précédente vers un nouveau groupe de ressources. 

1. Recherchez et sélectionnez **Groupes de ressources**. 

1. Dans le panneau **Groupes de ressources**, cliquez sur l’entrée représentant le groupe de ressources **az104-03a-rg1** que vous avez créé dans la tâche précédente.

1. Dans le panneau **Vue d’ensemble** du groupe de ressources, dans la liste des ressources du groupe de ressources, sélectionnez l’entrée représentant le disque que vous venez de créer, cliquez sur **Déplacer** dans la barre d’outils, puis, dans la liste déroulante, sélectionnez **Déplacer vers un autre groupe de ressources**.

    >**Remarque** : Cette méthode vous permet de déplacer plusieurs ressources en même temps. 

1. Sous la zone de texte **Groupe de ressources**, cliquez sur **Créer**, puis tapez **az104-03a-rg2** dans la zone de texte. Dans l’onglet de vérification, cochez la case en regard de **Je comprends que les outils et les scripts associés aux ressources déplacées ne fonctionnent pas tant que je ne les mets pas à jour pour utiliser de nouveaux ID de ressource**, puis cliquez sur **Déplacer**.

    >**Remarque** : N’attendez pas que le déploiement se termine. Passez à l’exercice suivant. Le déplacement peut prendre environ 10 minutes. Pour savoir si l’opération est terminée, surveillez les entrées du journal d’activité du groupe de ressources source ou cible. Revenez à cette étape une fois que vous aurez terminé la tâche suivante.

## Tâche 3 : Implémenter des verrous de ressources

Dans cette tâche, vous allez appliquer un verrou de ressource sur un groupe de ressources Azure contenant une ressource de disque.

1. Dans le portail Azure, recherchez et sélectionnez **Disques**, cliquez sur **+ Créer** et spécifiez les paramètres suivants :

    |Paramètre|Valeur|
    |---|---|
    |Abonnement| nom de l’abonnement que vous utilisez dans ce labo |
    |Groupe de ressources| cliquez sur **Créer** un groupe de ressources et nommez-le **az104-03a-rg3** |
    |Nom du disque| **az104-03a-disk2** |
    |Région| nom de la région Azure dans laquelle vous avez créé les autres groupes de ressources dans ce labo |
    |Zone de disponibilité| **Aucun** |
    |Type de source| **Aucun** |

1. Définissez respectivement le type et la taille du disque par **HDD Standard** et **32 Gio**.

1. Cliquez sur **Vérifier + créer**, puis cliquez sur **Créer**.

1. Cliquez sur **Accéder à la ressource**.

1. Dans la page Vue d’ensemble du disque, cliquez sur le nom du groupe de ressources, **az104-03a-rg3**.

1. Dans le volet du groupe de ressources **az104-03a-rg3**, cliquez sur **Verrous**, puis sur **+ Ajouter** et spécifiez les paramètres suivants :

    |Paramètre|Value|
    |---|---|
    |Nom du verrou| **az104-03a-delete-lock** |
    |Type de verrou| **Supprimer** |
    
1. Cliquez sur **OK**    

1. Dans le volet du groupe de ressources **az104-03a-rg3**, cliquez sur **Vue d’ensemble**. Dans la liste des ressources du groupe de ressources, sélectionnez l’entrée représentant le disque que vous avez créé précédemment dans cette tâche, puis cliquez sur **Supprimer** dans la barre d’outils. 

1. À l’invite **Voulez-vous supprimer toutes les ressources sélectionnées ?** , dans la zone de texte **Confirmer la suppression**, tapez **oui**, puis cliquez sur **Supprimer**.

1. Vous devriez obtenir un message d’erreur indiquant l’échec de l’opération de suppression. 

    >**Remarque** : Comme l’indique le message d’erreur, ce comportement est attendu en raison du verrou de suppression appliqué au niveau du groupe de ressources.

1. Revenez à la liste des ressources du groupe de ressources **az104-03a-rg3**, puis cliquez sur l’entrée représentant la ressource **az104-03a-disk2**. 

1. Dans le volet **az104-03a-disk2**, dans la section **Paramètres**, cliquez sur **Taille + performances**, définissez le type de disque et la taille sur **SSD Premium** et **64 Gio**, respectivement, puis cliquez sur **Enregistrer** pour appliquer la modification. Vérifiez que la modification a été prise en compte.

    >**Remarque** : Ce comportement est attendu, étant donné que le verrou au niveau du groupe de ressources s’applique uniquement aux opérations de suppression. 

## Nettoyer les ressources

   >**Remarque** : Ne supprimez pas les ressources que vous avez déployées dans ce labo. Vous les utiliserez dans le labo suivant de ce module. Supprimez uniquement le verrou de ressource que vous avez créé dans ce labo.

1. Accédez au volet du groupe de ressources **az104-03a-rg3**, affichez son volet **Verrous** et supprimez le verrou **az104-03a-delete-lock** en cliquant sur le lien **Supprimer** à droite de l’entrée du verrou **Supprimer**.

## Révision

Dans cet exercice, vous avez :

- Créé des groupes de ressources et déployé des ressources dans ces groupes
- Déplacé des ressources entre des groupes de ressources
- Implémenté et testé des verrous de ressources
