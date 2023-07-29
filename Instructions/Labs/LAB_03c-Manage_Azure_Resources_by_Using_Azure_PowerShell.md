---
lab:
  title: "Labo 03c\_: Gérer les ressources Azure en utilisant Azure PowerShell"
  module: Administer Azure Resources
---

# Labo 03c : Gérer des ressources Azure à l’aide d’Azure PowerShell
# Manuel de labo de l’étudiant

## Scénario du labo

Maintenant que vous avez exploré les fonctionnalités d’administration Azure de base associées à l’approvisionnement des ressources et à leur organisation en fonction de groupes de ressources à l’aide du portail Azure et des modèles Azure Resource Manager, vous devez effectuer la tâche équivalente à l’aide d’Azure PowerShell. Pour éviter d’installer les modules Azure PowerShell, vous utiliserez l’environnement PowerShell disponible dans Azure Cloud Shell.

**Remarque :** Une **[simulation de labo interactive](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%206)** est disponible et vous permet de progresser à votre propre rythme. Il peut exister de légères différences entre la simulation interactive et le labo hébergé. Toutefois, les concepts et idées de base présentés sont identiques. 

## Objectifs

Dans ce labo, vous allez :

+ Tâche 1 : Démarrer une session PowerShell dans Azure Cloud Shell
+ Tâche 2 : Créer un groupe de ressources et un disque managé Azure à l’aide d’Azure PowerShell
+ Tâche 3 : Configurer le disque managé à l’aide d’Azure PowerShell

## Durée estimée : 20 minutes

## Diagramme de l'architecture

![image](../media/lab03c.png)

### Instructions

> **Remarque** :  Créez toujours votre propre mot de passe sécurisé pour toute machine virtuelle ou compte d’utilisateur que vous créez. Si la machine virtuelle est créée pour vous, utilisez l’option **Réinitialiser le mot de passe** dans le portail pour mettre à jour le mot de passe. 

## Exercice 1

## Tâche 1 : Démarrer une session PowerShell dans Azure Cloud Shell

Dans cette tâche, vous allez ouvrir une session PowerShell dans Cloud Shell. 

1. Dans le portail, ouvrez **Azure Cloud Shell** en cliquant sur l’icône située en haut à droite du portail Azure.

1. Lorsque vous êtes invité à sélectionner **Bash** ou **PowerShell**, sélectionnez **PowerShell**. 

    >**Remarque** : Si c’est la première fois que vous démarrez **Cloud Shell** et que vous voyez le message **Vous n’avez aucun stockage monté**, sélectionnez l’abonnement que vous utilisez dans ce labo, puis sélectionnez **Créer un stockage**. 

1. Si vous y êtes invité, cliquez sur **Créer un stockage** et attendez que le volet Azure Cloud Shell s’affiche. 

1. Vérifiez que **PowerShell** apparaît dans le menu déroulant en haut à gauche du volet Cloud Shell.

## Tâche 2 : Créer un groupe de ressources et un disque managé Azure à l’aide d’Azure PowerShell

Dans cette tâche, vous allez créer un groupe de ressources et un disque managé Azure à l’aide d’une session Azure PowerShell dans Cloud Shell.

1. Pour créer un groupe de ressources dans la même région Azure que le groupe de ressources **az104-03b-rg1** que vous avez créé dans le labo précédent, à partir de la session PowerShell dans Cloud Shell, exécutez ce qui suit :

   ```powershell
   $location = (Get-AzResourceGroup -Name az104-03b-rg1).Location

   $rgName = 'az104-03c-rg1'

   New-AzResourceGroup -Name $rgName -Location $location
   ```
1. Pour récupérer les propriétés du groupe de ressources nouvellement créé, exécutez ce qui suit :

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```
1. Pour créer un disque managé avec les mêmes caractéristiques que celles que vous avez créées dans les laboratoires précédents de ce module, exécutez les commandes suivantes :

   ```powershell
   $diskConfig = New-AzDiskConfig `
    -Location $location `
    -CreateOption Empty `
    -DiskSizeGB 32 `
    -Sku Standard_LRS

   $diskName = 'az104-03c-disk1'

   New-AzDisk `
    -ResourceGroupName $rgName `
    -DiskName $diskName `
    -Disk $diskConfig
   ```

1. Pour récupérer les propriétés du disque nouvellement créé, exécutez ce qui suit :

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

## Tâche 3 : Configurer le disque managé à l’aide d’Azure PowerShell

Dans cette tâche, vous allez gérer la configuration du disque managé Azure à l’aide d’une session Azure PowerShell dans Cloud Shell. 

1. Pour augmenter la taille du disque managé Azure à **64 Go**, à partir de la session PowerShell dans Cloud Shell, exécutez les commandes suivantes :

   ```powershell
   New-AzDiskUpdateConfig -DiskSizeGB 64 | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Pour vérifier que la modification a pris effet, exécutez ce qui suit :

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

1. Pour vérifier la référence SKU actuelle en tant que **Standard_LRS**, exécutez les commandes suivantes :

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

1. Pour remplacer la référence SKU des performances du disque par **Premium_LRS**, à partir de la session PowerShell dans Cloud Shell, exécutez les commandes suivantes :

   ```powershell
   New-AzDiskUpdateConfig -Sku Premium_LRS | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Pour vérifier que la modification a pris effet, exécutez ce qui suit :

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

## Nettoyer les ressources

   >**Remarque** : Ne supprimez pas les ressources que vous avez déployées dans ce labo. Vous en aurez besoin dans le labo suivant de ce module.

## Révision

Dans cet exercice, vous avez :

- Démarré une session PowerShell dans Azure Cloud Shell
- Créé un groupe de ressources et un disque managé Azure à l’aide d’Azure PowerShell
- Configuré le disque managé à l’aide d’Azure PowerShell
