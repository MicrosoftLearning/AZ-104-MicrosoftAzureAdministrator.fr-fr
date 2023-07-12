---
lab:
  title: "Labo\_05\_: Implémenter une connectivité intersites"
  module: Administer Intersite Connectivity
---

# Labo 05 - Implémenter une connectivité intersites
# Manuel de labo pour l’étudiant

## Scénario du labo

Contoso dispose de ses centres de données à Boston, à New York et aux bureaux de Seattle connectés par le biais d’une liaison réseau à grande zone de maillage, avec une connectivité complète entre eux. Vous devez implémenter un environnement de laboratoire qui reflète la topologie des réseaux locaux de Contoso et vérifie ses fonctionnalités.

**Remarque :** Une **[simulation de labo interactive](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%209)** est disponible et vous permet de progresser à votre propre rythme. Il peut exister de légères différences entre la simulation interactive et le labo hébergé. Toutefois, les concepts et idées de base présentés sont identiques. 

## Objectifs

Dans ce labo, vous allez :

+ Tâche 1 : Approvisionner l’environnement de laboratoire
+ Tâche 2 : Configurer le peering local et global des réseaux virtuels
+ Tâche 3 : Tester la connectivité intersite

## Durée estimée : 30 minutes

## Diagramme de l'architecture

![image](../media/lab05.png)

### Instructions

## Exercice 1

## Tâche 1 : Approvisionner l’environnement de laboratoire

Dans cette tâche, vous allez déployer trois machines virtuelles, chacune dans un réseau virtuel distinct, deux d’entre elles étant dans la même région Azure et la troisième étant dans une autre région.

1. Connectez-vous au [portail Azure](https://portal.azure.com).

1. Dans le portail Azure, ouvrez **Azure Cloud Shell** en cliquant sur l’icône située en haut à droite du portail Azure.

1. Lorsque vous êtes invité à sélectionner **Bash** ou **PowerShell**, sélectionnez **PowerShell**.

    >**Remarque** : Si c’est la première fois que vous démarrez **Cloud Shell** et que vous voyez le message **Vous n’avez aucun stockage monté**, sélectionnez l’abonnement que vous utilisez dans ce labo, puis sélectionnez **Créer un stockage**.

1. Dans la barre d'outils du volet Cloud Shell, cliquez sur l'icône **Charger/Télécharger des fichiers**, dans le menu déroulant, cliquez sur **Charger** et téléchargez les fichiers **\\Allfiles\\Labs\\05\\az104-05-vms-loop-template.json** et **\\Allfiles\\Labs\\05\\az104-05-vnetvm-loop-parameters.json** dans le répertoire d'origine de Cloud Shell. 

1. Dans le volet Cloud Shell, exécutez la commande suivante pour créer le groupe de ressources qui hébergera l’environnement de labo. Les deux premiers réseaux virtuels et une paire de machines virtuelles seront déployés dans [Azure_region_1]. Le troisième réseau virtuel et la troisième machine virtuelle seront déployés dans le même groupe de ressources, mais dans une autre région [Azure_region_2]. (Remplacez les espaces réservés [Azure_region_1] et [Azure_region_2], y compris les crochets, par les noms de deux régions Azure différentes dans lesquelles vous envisagez de déployer ces machines virtuelles Azure. Par exemple, $location 1 = 'eastus'. Vous pouvez utiliser Get-AzLocation pour répertorier tous les emplacements.) :

   ```powershell
   $location1 = 'eastus'

   $location2 = 'westus'

   $rgName = 'az104-05-rg1'

   New-AzResourceGroup -Name $rgName -Location $location1
   ```

   >**Remarque** : Les régions utilisées ci-dessus ont été testées et connues pour fonctionner lorsque ce laboratoire a été officiellement révisé. Si vous préférez utiliser différents emplacements ou qu’ils ne fonctionnent plus, vous devez identifier deux régions différentes dans lesquelles les machines virtuelles Standard D2Sv3 peuvent être déployées.
   >
   >Pour identifier les régions Azure, à partir d’une session PowerShell dans Cloud Shell, exécutez **(Get-AzLocation).Location**
   >
   >Une fois que vous avez identifié deux régions que vous souhaitez utiliser, exécutez la commande ci-dessous dans Cloud Shell pour chaque région pour confirmer que vous pouvez déployer des machines virtuelles Standard D2Sv3.
   >
   >```az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name" ```
   >
   >Si la commande ne retourne aucun résultat, vous devez choisir une autre région. Une fois que vous avez identifié deux régions appropriées, vous pouvez ajuster les régions dans le bloc de code ci-dessus.

1. Dans le volet Cloud Shell, exécutez la commande suivante pour créer les trois réseaux virtuels et déployer des machines virtuelles dans celles-ci à l’aide du modèle et des fichiers de paramètres que vous avez chargés :
    
    >**Remarque** : Vous serez invité à fournir un mot de passe d’administrateur.

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-05-vnetvm-loop-template.json `
      -TemplateParameterFile $HOME/az104-05-vnetvm-loop-parameters.json `
      -location1 $location1 `
      -location2 $location2
   ```

    >**Remarque** : Attendez que le déploiement se termine avant de passer à l’étape suivante. Ce processus prend environ 2 minutes.

1. Fermez le volet Cloud Shell.

## Tâche 2 : Configurer le peering local et global des réseaux virtuels

Dans cette tâche, vous allez configurer le peering local et global entre les réseaux virtuels que vous avez déployés dans les tâches précédentes.

1. Dans le portail Azure, recherchez et sélectionnez **Réseaux virtuels**.

1. Passez en revue les réseaux virtuels que vous avez créés dans la tâche précédente et vérifiez que les deux premiers se trouvent dans la même région Azure et le troisième dans une région Azure différente.

    >**Remarque** : Le modèle que vous avez utilisé pour le déploiement des trois réseaux virtuels garantit que les plages d’adresses IP des trois réseaux virtuels ne se chevauchent pas.

1. Dans la liste des réseaux virtuels, cliquez sur **az104-05-vnet0**.

1. Dans le panneau de réseau virtuel **az104-05-vnet0**, dans la section **Paramètres**, cliquez sur **Peerings**, puis sur **+ Ajouter**.

1. Créez un peering avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) et cliquez sur **Ajouter** :

    | Paramètre | Valeur|
    | --- | --- |
    | Ce réseau virtuel : nom du lien d’homologation | **az104-05-vnet0_to_az104-05-vnet1** |
    | Ce réseau virtuel : Trafic vers le réseau virtuel distant | **Autoriser (par défaut)** |
    | Ce réseau virtuel : Trafic transféré à partir du réseau virtuel distant | **Bloquer le trafic provenant de l’extérieur de ce réseau virtuel** |
    | Passerelle de réseau virtuel | **Aucun** |
    | Réseau virtuel distant : nom du lien d’homologation | **az104-05-vnet1_to_az104-05-vnet0** |
    | Modèle de déploiement de réseau virtuel | **Gestionnaire des ressources** |
    | Je connais mon ID de ressource | non sélectionné |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Réseau virtuel | **az104-05-vnet1** |
    | Trafic vers le réseau virtuel distant | **Autoriser (par défaut)** |
    | Trafic transféré à partir du réseau virtuel distant | **Bloquer le trafic provenant de l’extérieur de ce réseau virtuel** |
    | Passerelle de réseau virtuel | **Aucun** |

    >**Remarque** : Cette étape établit deux peerings locaux : l’un d’az104-05-vnet0 à az104-05-vnet1 et l’autre d’az104-05-vnet1 à az104-05-vnet0.

    >**Remarque** : Si vous rencontrez un problème avec l’interface du portail Azure qui n’affiche pas les réseaux virtuels créés dans la tâche précédente, vous pouvez configurer le peering en exécutant les commandes PowerShell suivantes à partir de Cloud Shell :
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname

   $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet1' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet1.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet0' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet0.Id
   ``` 

1. Dans le panneau de réseau virtuel **az104-05-vnet0**, dans la section **Paramètres**, cliquez sur **Peerings**, puis sur **+ Ajouter**.

1. Créez un peering avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) et cliquez sur **Ajouter** :

    | Paramètre | Valeur|
    | --- | --- |
    | Ce réseau virtuel : nom du lien d’homologation | **az104-05-vnet0_to_az104-05-vnet2** |
    | Ce réseau virtuel : Trafic vers le réseau virtuel distant | **Autoriser (par défaut)** |
    | Ce réseau virtuel : Trafic transféré à partir du réseau virtuel distant | **Bloquer le trafic provenant de l’extérieur de ce réseau virtuel** |
    | Passerelle de réseau virtuel | **Aucun** |
    | Réseau virtuel distant : nom du lien d’homologation | **az104-05-vnet2_to_az104-05-vnet0** |
    | Modèle de déploiement de réseau virtuel | **Gestionnaire des ressources** |
    | Je connais mon ID de ressource | non sélectionné |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Réseau virtuel | **az104-05-vnet2** |
    | Trafic vers le réseau virtuel distant | **Autoriser (par défaut)** |
    | Trafic transféré à partir du réseau virtuel distant | **Bloquer le trafic provenant de l’extérieur de ce réseau virtuel** |
    | Passerelle de réseau virtuel | **Aucun** |

    >**Remarque** : Cette étape établit deux peerings locaux : l’un d’az104-05-vnet0 à az104-05-vnet2 et l’autre d’az104-05-vnet2 à az104-05-vnet0.

    >**Remarque** : Si vous rencontrez un problème avec l’interface du portail Azure qui n’affiche pas les réseaux virtuels créés dans la tâche précédente, vous pouvez configurer le peering en exécutant les commandes PowerShell suivantes à partir de Cloud Shell :
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet2' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet0' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet0.Id
   ``` 

1. Revenez au panneau **Réseaux virtuels** et, dans la liste des réseaux virtuels, cliquez sur **az104-05-vnet1**.

1. Dans le panneau de réseau virtuel **az104-05-vnet1**, dans la section **Paramètres**, cliquez sur **Peerings**, puis sur **+ Ajouter**.

1. Créez un peering avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) et cliquez sur **Ajouter** :

    | Paramètre | Valeur|
    | --- | --- |
    | Ce réseau virtuel : nom du lien d’homologation | **az104-05-vnet1_to_az104-05-vnet2** |
    | Ce réseau virtuel : Trafic vers le réseau virtuel distant | **Autoriser (par défaut)** |
    | Ce réseau virtuel : Trafic transféré à partir du réseau virtuel distant | **Bloquer le trafic provenant de l’extérieur de ce réseau virtuel** |
    | Passerelle de réseau virtuel | **Aucun** |
    | Réseau virtuel distant : nom du lien d’homologation | **az104-05-vnet2_to_az104-05-vnet1** |
    | Modèle de déploiement de réseau virtuel | **Gestionnaire des ressources** |
    | Je connais mon ID de ressource | non sélectionné |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Réseau virtuel | **az104-05-vnet2** |
    | Trafic vers le réseau virtuel distant | **Autoriser (par défaut)** |
    | Trafic transféré à partir du réseau virtuel distant | **Bloquer le trafic provenant de l’extérieur de ce réseau virtuel** |
    | Passerelle de réseau virtuel | **Aucun** |

    >**Remarque** : Cette étape établit deux peerings locaux : l’un d’az104-05-vnet1 à az104-05-vnet2 et l’autre d’az104-05-vnet2 à az104-05-vnet1.

    >**Remarque** : Si vous rencontrez un problème avec l’interface du portail Azure qui n’affiche pas les réseaux virtuels créés dans la tâche précédente, vous pouvez configurer le peering en exécutant les commandes PowerShell suivantes à partir de Cloud Shell :
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet2' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet1' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id
   ``` 

## Tâche 3 : Tester la connectivité intersite

Dans cette tâche, vous allez tester la connectivité entre les machines virtuelles sur les trois réseaux virtuels que vous avez connectés via le peering local et global dans la tâche précédente.

1. Dans le portail Azure, recherchez et sélectionnez **Machines virtuelles**.

1. Dans la liste des machines virtuelles, cliquez sur **az104-05-vm0**.

1. Dans le volet **az104-05-vm0**, cliquez sur **Connecter**, dans la liste déroulante, cliquez sur **RDP**, dans le volet **Connecter avec RDP**, cliquez sur **Télécharger le fichier RDP** et suivez les invites pour démarrer la session Bureau à distance.

    >**Remarque** : Cette étape fait référence à la connexion via le Bureau à distance à partir d’un ordinateur Windows. Sur un Mac, vous pouvez utiliser le client Bureau à distance disponible sur le Mac App Store. Sur un ordinateur Linux, vous pouvez utiliser un logiciel client RDP open source.

    >**Remarque** : Vous pouvez ignorer toutes les invites d’avertissement lors de la connexion aux machines virtuelles cibles.

1. Lorsque vous y êtes invité, connectez-vous avec le nom d’utilisateur **Student** et le mot de passe que vous avez configuré lors du déploiement de vos machines virtuelles via CloudShell. 

1. Dans la session Bureau à distance vers **az104-05-vm0**, cliquez avec le bouton droit de la souris sur le bouton **Démarrer** et, dans le menu contextuel, cliquez sur **Windows PowerShell (Admin)**.

1. Dans la fenêtre de console Windows PowerShell, exécutez la commande suivante pour tester la connectivité à **az104-05-vm1** (qui a l’adresse IP privée de **10.51.0.4**) sur le port TCP 3389 :

   ```powershell
   Test-NetConnection -ComputerName 10.51.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    >**Remarque** : Le test utilise TCP 3389, car il s’agit de ce port autorisé par défaut par le pare-feu du système d’exploitation.

1. Examinez la sortie de la commande et vérifiez que la connexion a réussi.

1. Dans la fenêtre de console Windows PowerShell, exécutez la commande suivante pour tester la connectivité à **az104-05-vm2** (qui a l’adresse IP privée de **10.52.0.4**) sur le port TCP 3389 :

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

1. Revenez au portail Azure sur votre ordinateur de labo et retournez dans le panneau **Machines virtuelles**.

1. Dans la liste des machines virtuelles, cliquez sur **az104-05-vm1**.

1. Dans le volet **az104-05-vm1**, cliquez sur **Connecter**, dans la liste déroulante, cliquez sur **RDP**, dans le volet **Connecter avec RDP**, cliquez sur **Télécharger le fichier RDP** et suivez les invites pour démarrer la session Bureau à distance.

    >**Remarque** : Cette étape fait référence à la connexion via le Bureau à distance à partir d’un ordinateur Windows. Sur un Mac, vous pouvez utiliser le client Bureau à distance disponible sur le Mac App Store. Sur un ordinateur Linux, vous pouvez utiliser un logiciel client RDP open source.

    >**Remarque** : Vous pouvez ignorer toutes les invites d’avertissement lors de la connexion aux machines virtuelles cibles.

1. Lorsque vous y êtes invité, connectez-vous à l’aide du nom d’utilisateur **étudiant** et du mot de passe de votre fichier de paramètres. 

1. Dans la session Bureau à distance vers **az104-05-vm1**, cliquez avec le bouton droit de la souris sur le bouton **Démarrer** et, dans le menu contextuel, cliquez sur **Windows PowerShell (Admin)**.

1. Dans la fenêtre de console Windows PowerShell, exécutez la commande suivante pour tester la connectivité à **az104-05-vm2** (qui a l’adresse IP privée **10.52.0.4**) sur le port TCP 3389 :

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    >**Remarque** : Le test utilise TCP 3389, car il s’agit de ce port autorisé par défaut par le pare-feu du système d’exploitation.

1. Examinez la sortie de la commande et vérifiez que la connexion a réussi.

## Nettoyer les ressources

>**Remarque** : N’oubliez pas de supprimer toutes les nouvelles ressources Azure que vous n’utilisez plus. La suppression des ressources inutilisées vous évitera d’encourir des frais inattendus.

>**Remarque** :  Ne vous inquiétez pas si les ressources de laboratoire ne peuvent pas être immédiatement supprimées. Parfois, les ressources ont des dépendances et leur suppression prend plus de temps. Il s’agit d’une tâche d’administrateur courante pour surveiller l’utilisation des ressources. Il vous suffit donc de consulter régulièrement vos ressources dans le portail pour voir comment se passe le nettoyage. 

1. Dans le portail Azure, ouvrez la session **PowerShell** dans le volet **Cloud Shell**.

1. Listez tous les groupes de ressources créés dans les labos de ce module en exécutant la commande suivante :

   ```powershell
   Get-AzResourceGroup -Name 'az104-05*'
   ```

1. Supprimez tous les groupes de ressources que vous avez créés dans les labos de ce module en exécutant la commande suivante :

   ```powershell
   Get-AzResourceGroup -Name 'az104-05*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Remarque** : La commande s’exécute de façon asynchrone (tel que déterminé par le paramètre -AsJob). Vous pourrez donc exécuter une autre commande PowerShell immédiatement après au cours de la même session PowerShell, mais la suppression effective du groupe de ressources peut prendre quelques minutes.

## Révision

Dans cet exercice, vous avez :

+ Approvisionné l’environnement de labo
+ Configuré le peering de réseaux virtuels locaux et globaux
+ Testé la connectivité intersite
