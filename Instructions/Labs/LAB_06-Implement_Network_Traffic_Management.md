---
lab:
  title: '06 : Implémenter la gestion du trafic'
  module: Module 06 - Network Traffic Management
ms.openlocfilehash: 81fd0fefc28cbf9eb59935e93bb548c69d677cf5
ms.sourcegitcommit: 6df80c7697689bcee3616cdd665da0a38cdce6cb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2022
ms.locfileid: "146587455"
---
# <a name="lab-06---implement-traffic-management"></a>Labo 06 : Implémenter la gestion du trafic
# <a name="student-lab-manual"></a>Manuel de labo pour étudiant

## <a name="lab-scenario"></a>Scénario du labo

Vous avez été chargé de tester la gestion du trafic réseau ciblant les machines virtuelles Azure dans la topologie de réseau hub and spoke, que Contoso compte implémenter dans son environnement Azure (au lieu de créer la topologie de maillage, que vous avez testée dans le labo précédent). Ce test doit inclure l’implémentation de la connectivité entre les spokes en s’appuyant sur des itinéraires définis par l’utilisateur qui forcent le trafic à circuler via le hub, ainsi que la distribution du trafic entre les machines virtuelles à l’aide des équilibreurs de charge de couche 4 et de couche 7. À cet effet, vous avez l’intention d’utiliser Azure Load Balancer (couche 4) et Azure Application Gateway (couche 7).

>**Remarque** : Ce labo, par défaut, nécessite un total de 8 processeurs virtuels disponibles dans la série Standard_Dsv3 dans la région que vous choisissez pour le déploiement, car il implique le déploiement de quatre machines virtuelles Azure de Standard_D2s_v3 référence SKU. Si vos étudiants utilisent des comptes d’évaluation, avec la limite de 4 processeurs virtuels, vous pouvez utiliser une taille de machine virtuelle qui ne nécessite qu’un seul processeur virtuel (par exemple, Standard_B1s).

## <a name="objectives"></a>Objectifs

Dans ce labo, vous allez :

+ Tâche 1 : Approvisionner l’environnement de laboratoire
+ Tâche 2 : Configurer une topologie de réseau hub-and-spoke
+ Tâche 3 : Tester la transitivité de l’appairage de réseaux virtuels
+ Tâche 4 : Configurer le routage dans la topologie hub and spoke
+ Tâche 5 : Mettre en œuvre l'équilibreur de charges Azure
+ Tâche 6 : Implémenter une passerelle Azure Application Gateway

## <a name="estimated-timing-60-minutes"></a>Durée estimée : 60 minutes

## <a name="architecture-diagram"></a>Diagramme de l'architecture

![image](../media/lab06.png)


## <a name="instructions"></a>Instructions

### <a name="exercise-1"></a>Exercice 1

#### <a name="task-1-provision-the-lab-environment"></a>Tâche 1 : Approvisionner l’environnement de laboratoire

Dans cette tâche, vous allez déployer quatre machines virtuelles dans la même région Azure. Les deux premiers résideront dans un réseau virtuel hub, tandis que chacun des deux restants résidera dans un réseau virtuel spoke distinct.

1. Connectez-vous au [portail Azure](https://portal.azure.com).

1. Dans le portail Azure, ouvrez **Azure Cloud Shell** en cliquant sur l’icône située en haut à droite du portail Azure.

1. Lorsque vous êtes invité à sélectionner **Bash** ou **PowerShell**, sélectionnez **PowerShell**.

    >**Remarque** : Si c’est la première fois que vous démarrez **Cloud Shell** et que vous voyez le message **Vous n’avez aucun stockage monté**, sélectionnez l’abonnement que vous utilisez dans ce labo, puis sélectionnez **Créer un stockage**.

1. Dans la barre d'outils du volet Cloud Shell, cliquez sur l'icône **Télécharger des fichiers**, dans le menu déroulant, cliquez sur **Charger** et téléchargez les fichiers **\\Allfiles\\Labs\\06\\az104-06-vms-loop-template.json** and **\\Allfiles\\Labs\\06\\az104-06-vms-loop-parameters.json** dans le répertoire d'origine de Cloud Shell.

1. Modifiez le fichier **Paramètres** que vous venez de charger et modifiez le mot de passe. Si vous avez besoin d’aide pour modifier le fichier dans Shell, demandez à votre instructeur de l’aide. Comme meilleure pratique, les secrets, comme les mots de passe, doivent être stockés de manière plus sécurisée dans le Key Vault. 

1. Dans le volet Cloud Shell, exécutez ce qui suit pour créer le premier groupe de ressources qui hébergera l’environnement du labo (remplacez l’espace réservé « [Azure_region] » par le nom d’une région Azure où vous envisagez de déployer des machines virtuelles Azure)(vous pouvez utiliser la cmdlet « (Get-AzLocation).Location » pour obtenir la liste des régions) :

    ```powershell 
    $location = '[Azure_region]'
    ```
    
    Maintenant le nom du groupe de ressources :
    ```powershell
    $rgName = 'az104-06-rg1'
    ```
    
    Enfin, créez le groupe de ressources dans votre emplacement souhaité :
    ```powershell
    New-AzResourceGroup -Name $rgName -Location $location
    ```


1. Dans le volet Cloud Shell, exécutez la commande suivante pour créer les trois réseaux virtuels et quatre machines virtuelles Azure dans celles-ci à l’aide du modèle et des fichiers de paramètres que vous avez chargés :

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-06-vms-loop-template.json `
      -TemplateParameterFile $HOME/az104-06-vms-loop-parameters.json
   ```

    >**Remarque** : Attendez que le déploiement se termine avant de passer à l’étape suivante. Ce processus prend environ 5 minutes.

    >**Remarque** : Si vous avez reçu une erreur indiquant que la taille de machine virtuelle n’est pas disponible, demandez à votre instructeur de l’aide et essayez ces étapes.
    > 1. Cliquez sur le bouton `{}` dans votre CloudShell, sélectionnez le fichier **az104-06-vms-loop-parameters.json** dans la barre latérale gauche et prenez note de la valeur de paramètre `vmSize`.
    > 1. Vérifiez l’emplacement dans lequel le groupe de ressources « az104-06-rg1 » est déployé. Vous pouvez exécuter `az group show -n az104-06-rg1 --query location` dans votre CloudShell pour l’obtenir.
    > 1. Exécutez `az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name"` dans votre CloudShell.
    > 1. Remplacez la valeur du paramètre `vmSize` par l’une des valeurs retournées par la commande que vous venez d’exécuter. S’il n’y a pas de valeurs retournées, vous devrez peut-être choisir une autre région à déployer. Vous pouvez également choisir un autre nom de famille, comme « Standard_B1s ».
    > 1. Redéployez maintenant vos modèles en exécutant à nouveau la commande `New-AzResourceGroupDeployment`. Vous pouvez appuyer sur le bouton en haut quelques fois, ce qui amènerait la dernière commande exécutée.

1. Dans le volet Cloud Shell, exécutez ce qui suit pour installer l’extension Network Watcher sur les machines virtuelles Azure déployées à l’étape précédente :

   ```powershell
   $rgName = 'az104-06-rg1'
   $location = (Get-AzResourceGroup -ResourceGroupName $rgName).location
   $vmNames = (Get-AzVM -ResourceGroupName $rgName).Name

   foreach ($vmName in $vmNames) {
     Set-AzVMExtension `
     -ResourceGroupName $rgName `
     -Location $location `
     -VMName $vmName `
     -Name 'networkWatcherAgent' `
     -Publisher 'Microsoft.Azure.NetworkWatcher' `
     -Type 'NetworkWatcherAgentWindows' `
     -TypeHandlerVersion '1.4'
   }
   ```

    >**Remarque** : Attendez que le déploiement se termine avant de passer à l’étape suivante. Ce processus prend environ 5 minutes.



1. Fermez le volet Cloud Shell.

#### <a name="task-2-configure-the-hub-and-spoke-network-topology"></a>Tâche 2 : Configurer une topologie de réseau hub-and-spoke

Dans cette tâche, vous allez configurer le peering local entre les réseaux virtuels que vous avez déployés dans les tâches précédentes afin de créer une topologie de réseau hub and spoke.

1. Dans le portail Azure, recherchez et sélectionnez **Réseaux virtuels**.

1. Passez en revue les réseaux virtuels que vous avez créés dans la tâche précédente.

    >**Remarque** : Le modèle que vous avez utilisé pour le déploiement des trois réseaux virtuels garantit que les plages d’adresses IP des trois réseaux virtuels ne se chevauchent pas.

1. Dans la liste des réseaux virtuels, sélectionnez **az104-06-vnet2**.

1. Dans le panneau **az104-06-vnet2**, sélectionnez **Propriétés**. 

1. Dans le panneau Propriétés **az104-06-vnet2\|** , enregistrez la valeur de la propriété **ID de ressource**.

1. Revenez à la liste des réseaux virtuels et sélectionnez **az104-06-vnet3**.

1. Dans le panneau **az104-06-vnet3**, sélectionnez **Propriétés**. 

1. Dans le panneau Propriétés **az104-06-vnet3\|** , enregistrez la valeur de la propriété **ID de ressource**.

    >**Remarque** : Vous aurez besoin des valeurs de la propriété ResourceID pour les deux réseaux virtuels plus loin dans cette tâche.

    >**Remarque** : Il s'agit d'une solution de contournement qui résout le problème du portail Azure qui, de temps en temps, n'affiche pas le réseau virtuel nouvellement approvisionné lors de la création de réseaux virtuels homologues.

1. Dans la liste des réseaux virtuels, cliquez sur **az104-06-vnet01**.

1. Dans le panneau de réseau virtuel **az104-06-vnet01**, dans la section **Paramètres**, cliquez sur **Peerings**, puis sur **+ Ajouter**.

1. Créez un peering avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) et cliquez sur **Ajouter** :

    | Paramètre | Valeur |
    | --- | --- |
    | Ce réseau virtuel : nom du lien d’homologation | **az104-06-vnet01_to_az104-06-vnet2** |
    | Trafic vers le réseau virtuel distant | **Autoriser (par défaut)** |
    | Trafic transféré à partir du réseau virtuel distant | **Bloquer le trafic provenant de l’extérieur de ce réseau virtuel** |
    | Passerelle de réseau virtuel | **Aucun (par défaut)** |
    | Réseau virtuel distant : nom du lien d’homologation | **az104-06-vnet2_to_az104-06-vnet01** |
    | Modèle de déploiement de réseau virtuel | **Gestionnaire des ressources** |
    | Je connais mon ID de ressource | enabled |
    | ID de ressource | valeur du paramètre resourceID **az104-06-vnet2** que vous avez enregistrée précédemment dans cette tâche |
    | Trafic vers le réseau virtuel distant | **Autoriser (par défaut)** |
    | Trafic transféré à partir du réseau virtuel distant | **Autoriser (par défaut)** |
    | Passerelle de réseau virtuel | **Aucun (par défaut)** |

    >**Remarque** : Attendez que l’opération se termine.

    >**Remarque** : Cette étape établit deux peerings locaux : l’un d’az104-06-vnet01 d’az104-06-vnet2 et l’autre d’az104-06-vnet2 d’az104-06-vnet01.

    >**Remarque** : **Autoriser le trafic transféré** doit être activé afin de faciliter le routage entre les réseaux virtuels spoke, que vous allez implémenter plus loin dans ce labo.

1. Dans le panneau de réseau virtuel **az104-06-vnet01**, dans la section **Paramètres**, cliquez sur **Peerings**, puis sur **+ Ajouter**.

1. Créez un peering avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) et cliquez sur **Ajouter** :

    | Paramètre | Valeur |
    | --- | --- |
    | Ce réseau virtuel : nom du lien d’homologation | **az104-06-vnet01_to_az104-06-vnet3** |
    | Trafic vers le réseau virtuel distant | **Autoriser (par défaut)** |
    | Trafic transféré à partir du réseau virtuel distant | **Bloquer le trafic provenant de l’extérieur de ce réseau virtuel** |
    | Passerelle de réseau virtuel | **Aucun (par défaut)** |
    | Réseau virtuel distant : nom du lien d’homologation | **az104-06-vnet3_to_az104-06-vnet01** |
    | Modèle de déploiement de réseau virtuel | **Gestionnaire des ressources** |
    | Je connais mon ID de ressource | enabled |
    | ID de ressource | valeur du paramètre resourceID **az104-06-vnet3** que vous avez enregistrée précédemment dans cette tâche |
    | Trafic vers le réseau virtuel distant | **Autoriser (par défaut)** |
    | Trafic transféré à partir du réseau virtuel distant | **Autoriser (par défaut)** |
    | Passerelle de réseau virtuel | **Aucun (par défaut)** |

    >**Remarque** : Cette étape établit deux peerings locaux : l’un d’az104-06-vnet01 d’az104-06-vnet3 et l’autre d’az104-06-vnet3 d’az104-06-vnet01. Cette opération termine la configuration de la topologie hub-and-spoke (avec deux réseaux virtuels spoke).

    >**Remarque** : **Autoriser le trafic transféré** doit être activé afin de faciliter le routage entre les réseaux virtuels spoke, que vous allez implémenter plus loin dans ce labo.

#### <a name="task-3-test-transitivity-of-virtual-network-peering"></a>Tâche 3 : Tester la transitivité de l’appairage de réseaux virtuels

Dans cette tâche, vous allez tester la transitivité d’appairage de réseaux virtuels à l’aide de Network Watcher.

1. Dans le Portail Azure, recherchez et sélectionnez **Network Watcher**.

1. Dans le panneau **Network Watcher**, développez la liste des régions Azure et vérifiez que le service est activé dans la région que vous utilisez. 

1. Dans le panneau **Network Watcher**, accédez à la **résolution des problèmes de connexion**.

1. Dans le panneau **Network Watcher - Résolution des problèmes de connexion**, lancez une vérification avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Groupe de ressources | **az104-06-rg1** |
    | Type de source | **Machine virtuelle** |
    | Machine virtuelle | **az104-06-vm0** |
    | Destination | **Spécifier manuellement** |
    | URI, FQDN ou IPv4 | **10.62.0.4** |
    | Protocol | **TCP** |
    | Port de destination | **3389** |

    > **Remarque** : **10.62.0.4** représente l’adresse IP privée **d’az104-06-vm2**

1. Cliquez sur **Vérifier** et attendez que les résultats de la vérification de connectivité soient retournés. Vérifiez que l’état est **Accessible**. Examinez le chemin du réseau et notez que la connexion est directe, sans saut intermédiaire entre les machines virtuelles.

    > **Remarque** : Cela est attendu, étant donné que le réseau virtuel hub est appairé directement avec le premier réseau virtuel spoke.

1. Dans le panneau **Network Watcher - Résolution des problèmes de connexion**, lancez une vérification avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Groupe de ressources | **az104-06-rg1** |
    | Type de source | **Machine virtuelle** |
    | Machine virtuelle | **az104-06-vm0** |
    | Destination | **Spécifier manuellement** |
    | URI, FQDN ou IPv4 | **10.63.0.4** |
    | Protocol | **TCP** |
    | Port de destination | **3389** |

    > **Remarque** : **10.63.0.4** représente l’adresse IP privée **d’az104-06-vm3**

1. Cliquez sur **Vérifier** et attendez que les résultats de la vérification de connectivité soient retournés. Vérifiez que l’état est **Accessible**. Examinez le chemin du réseau et notez que la connexion est directe, sans saut intermédiaire entre les machines virtuelles.

    > **Remarque** : Cela est attendu, étant donné que le réseau virtuel hub est appairé directement avec le deuxième réseau virtuel spoke.

1. Dans le panneau **Network Watcher - Résolution des problèmes de connexion**, lancez une vérification avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Groupe de ressources | **az104-06-rg1** |
    | Type de source | **Machine virtuelle** |
    | Machine virtuelle | **az104-06-vm2** |
    | Destination | **Spécifier manuellement** |
    | URI, FQDN ou IPv4 | **10.63.0.4** |
    | Protocol | **TCP** |
    | Port de destination | **3389** |

1. Cliquez sur **Vérifier** et attendez que les résultats de la vérification de connectivité soient retournés. Notez que le statut doit être **Accessible**.

    > **Remarque** : Cela est attendu, étant donné que les deux réseaux virtuels spoke ne sont pas appairés les uns avec les autres (l’apparaige de réseaux virtuels n’est pas transitif).

#### <a name="task-4-configure-routing-in-the-hub-and-spoke-topology"></a>Tâche 4 : Configurer le routage dans la topologie hub and spoke

Dans cette tâche, vous allez configurer et tester le routage entre les deux réseaux virtuels spoke en activant le transfert IP sur l’interface réseau de la machine virtuelle **az104-06-vm0**, en activant le routage au sein de son système d’exploitation et en configurant des itinéraires définis par l’utilisateur sur le réseau virtuel spoke.

1. Sur le portail Azure, recherchez et sélectionnez **Machines virtuelles**.

1. Dans le panneau **Machines virtuelles**, dans la liste des machines virtuelles, cliquez sur **az104-06-vm0**.

1. Dans le panneau de la machine virtuelle **az104-06-vm0**, dans la section **Paramètres**, cliquez sur **Mise en réseau**.

1. Cliquez sur le lien **az104-06-nic0** en regard de l’étiquette de **l’interface réseau**, puis, dans le panneau d’interface réseau **az104-06-nic0**, dans la section **Paramètres**, cliquez sur **Configurations IP**.

1. Définissez le **transfert IP** sur **Activé** et enregistrez la modification.

   > **Remarque** : Ce paramètre est requis pour qu’**az104-06-vm0** fonctionne en tant que routeur, ce qui acheminera le trafic entre deux réseaux virtuels spoke.

   > **Remarque** : Vous devez maintenant configurer le système d’exploitation de la machine virtuelle **az104-06-vm0** pour prendre en charge le routage.

1. Dans le Portail Azure, revenez au panneau de machine virtuelle Azure **az104-06-vm0**, puis cliquez sur **Vue d’ensemble**.

1. Dans le panneau **az104-06-vm0**, dans la section **Opérations**, cliquez sur **Exécuter la commande** et, dans la liste des commandes, cliquez sur **RunPowerShellScript**.

1. Dans le panneau **Exécuter le script de commande**, tapez ce qui suit et cliquez sur **Exécuter** pour installer le rôle d’accès à distance Windows serveur.

   ```powershell
   Install-WindowsFeature RemoteAccess -IncludeManagementTools
   ```

   > **Remarque** : Attendez la confirmation que la commande s’est terminée correctement.

1. Dans le panneau **Exécuter le script de commande** , tapez ce qui suit et cliquez sur **Exécuter** pour installer le service de rôle de routage.

   ```powershell
   Install-WindowsFeature -Name Routing -IncludeManagementTools -IncludeAllSubFeature

   Install-WindowsFeature -Name "RSAT-RemoteAccess-Powershell"

   Install-RemoteAccess -VpnType RoutingOnly

   Get-NetAdapter | Set-NetIPInterface -Forwarding Enabled
   ```

   > **Remarque** : Attendez la confirmation que la commande s’est terminée correctement.

   > **Remarque** : Vous devez maintenant créer et configurer des itinéraires définis par l’utilisateur sur les réseaux virtuels spoke.

1. Dans le Portail Azure, recherchez et sélectionnez **Tables de routage**, puis, dans le panneau **Tables de routage**, cliquez sur **+ Créer**.

1. Créez une table de routage avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Groupe de ressources | **az104-06-rg1** |
    | Emplacement | nom de la région Azure dans laquelle vous avez créé les réseaux virtuels |
    | Nom | **az104-06-rt23** |
    | Propager des itinéraires de passerelle | **Non** |

1. Cliquez sur **Examiner et créer**. Laissez la validation se produire, puis cliquez sur **Créer** à nouveau pour envoyer votre déploiement.

   > **Remarque** : Attendez que la table de routage soit créée. Ce processus prend environ 3 minutes.

1. Cliquez sur **Accéder à la ressource**.

1. Dans le panneau de la table de routage **az104-06-rt23**, dans la section **Paramètres**, cliquez sur **Itinéraires**, puis sur **+ Ajouter**.

1. Ajoutez un nouvel itinéraire avec les paramètres suivants :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom de l’itinéraire | **az104-06-route-vnet2-to-vnet3** |
    | Destination du préfixe d’adresse | **Adresses IP** |
    | Plages d’adresses IP/CIDR de destination | **10.63.0.0/20** |
    | Type de tronçon suivant | **Appliance virtuelle** |
    | adresse de tronçon suivant | **10.60.0.4** |

1. Cliquez sur **OK**

1. Revenez dans le panneau de la table de routage **az104-06-rt23**, dans la section **Paramètres**, cliquez sur **Sous-réseaux**, puis sur **+ Associer**.

1. Associez la table de routage **az104-06-rt23** au sous-réseau suivant :

    | Paramètre | Valeur |
    | --- | --- |
    | Réseau virtuel | **az104-06-vnet2** |
    | Subnet | **subnet0** |

1. Cliquez sur **OK**

1. Revenez au panneau **Tables de routage**, puis cliquez sur **+ Créer**.

1. Créez une table de routage avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Groupe de ressources | **az104-06-rg1** |
    | Région | nom de la région Azure dans laquelle vous avez créé les réseaux virtuels |
    | Nom | **az104-06-rt32** |
    | Propager des itinéraires de passerelle | **Non** |

1. Cliquez sur Examiner et créer. Laissez la validation se produire, puis appuyez sur Créer à nouveau pour envoyer votre déploiement.

   > **Remarque** : Attendez que la table de routage soit créée. Ce processus prend environ 3 minutes.

1. Cliquez sur **Accéder à la ressource**.

1. Dans le panneau de la table de routage **az104-06-rt32**, dans la section **Paramètres**, cliquez sur **Itinéraires**, puis sur **+ Ajouter**.

1. Ajoutez un nouvel itinéraire avec les paramètres suivants :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom de l’itinéraire | **az104-06-route-vnet3-to-vnet2** |
    | Destination du préfixe d’adresse | **Adresses IP** |
    | Plages d’adresses IP/CIDR de destination | **10.62.0.0/20** |
    | Type de tronçon suivant | **Appliance virtuelle** |
    | adresse de tronçon suivant | **10.60.0.4** |

1. Cliquez sur **OK**

1. Revenez dans le panneau de la table de routage **az104-06-rt32**, dans la section **Paramètres**, cliquez sur **Sous-réseaux**, puis sur **+ Associer**.

1. Associez la table de routage **az104-06-rt32** au sous-réseau suivant :

    | Paramètre | Valeur |
    | --- | --- |
    | Réseau virtuel | **az104-06-vnet3** |
    | Subnet | **subnet0** |

1. Cliquez sur **OK**

1. Dans le Portail Azure, revenez au panneau **Network Watcher - Résolution des problèmes de connexion**.

1. Dans le panneau **Network Watcher - Résolution des problèmes de connexion**, lancez une vérification avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Groupe de ressources | **az104-06-rg1** |
    | Type de source | **Machine virtuelle** |
    | Machine virtuelle | **az104-06-vm2** |
    | Destination | **Spécifier manuellement** |
    | URI, FQDN ou IPv4 | **10.63.0.4** |
    | Protocol | **TCP** |
    | Port de destination | **3389** |

1. Cliquez sur **Vérifier** et attendez que les résultats de la vérification de connectivité soient retournés. Vérifiez que l’état est **Accessible**. Passez en revue le chemin d’accès réseau et notez que le trafic a été routé via la carte réseau **10.60.0.4**, affectée à la carte réseau **az104-06-nic0**. Si l’état est **inaccessible**, vous devez arrêter, puis démarrer az104-06-vm0.

    > **Remarque** : Cela est attendu, car le trafic entre les réseaux virtuels spoke est désormais routé via la machine virtuelle située dans le réseau virtuel hub, qui fonctionne comme un routeur.

    > **Remarque** : Vous pouvez utiliser **Network Watcher** pour afficher la topologie du réseau.

#### <a name="task-5-implement-azure-load-balancer"></a>Tâche 5 : Mettre en œuvre l'équilibreur de charges Azure

Dans cette tâche, vous allez implémenter un Azure Load Balancer devant les deux machines virtuelles Azure dans le réseau virtuel hub

1. Dans le Portail Azure, recherchez et sélectionnez **Équilibreurs de charge** et, dans le panneau **Équilibreurs de charge**, cliquez sur **+ Créer**.

1. Créez un équilibreur de charge avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) puis cliquez sur **Suivant : Configuration IP front-end** :

    | Paramètre | Valeur |
    | --- | --- |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Groupe de ressources | **az104-06-rg1** |
    | Nom | **az104-06-lb4** |
    | Région| nom de la région Azure dans laquelle vous avez déployé toutes les autres ressources dans ce laboratoire |
    | Référence SKU | **Standard** |
    | Type | **Public** |
    
1. Sous l’onglet **Configuration IP frontale**, cliquez sur **Ajouter une configuration IP frontale** et utilisez le paramètre suivant avant de cliquer sur **Ajouter**.   
     
    | Paramètre | Valeur |
    | --- | --- |
    | Nom | tout nom unique |
    | Adresse IP publique | **Création** |
    | Nom de l’adresse IP publique | **az104-06-pip4** |

1. Cliquez sur **Examiner et créer**. Laissez la validation se produire, puis appuyez sur **Créer** à nouveau pour envoyer votre déploiement.

    > **Remarque** : Attendez que l’équilibreur de charge Azure soit provisionné. Ce processus prend environ 2 minutes.

1. Dans le volet de déploiement, cliquez sur **Accéder à la ressource**.

1. Dans le panneau de l’équilibreur de charge **az104-06-lb4**, dans la section **Paramètres**, cliquez sur **Pools principaux**, puis cliquez sur **+ Ajouter**.

1. Créez un pool back-end avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom | **az104-06-lb4-be1** |
    | Réseau virtuel | **az104-06-vnet01** |
    | Version de l’adresse IP | **IPv4** |
    | Machine virtuelle | **az104-06-vm0** |
    | Adresse IP des machines virtuelles | **ipconfig1 (10.60.0.4)** |
    | Machine virtuelle | **az104-06-vm1** |
    | Adresse IP des machines virtuelles | **ipconfig1 (10.60.1.4)** |

1. Cliquez sur **Ajouter**

1. Attendez que le pool principal soit créé, dans la section **Paramètres**, cliquez sur **Sondes d’intégrité**, puis cliquez sur **+ Ajouter**.

1. Ajoutez une sonde d’intégrité avec les paramètres suivants :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom | **az104-06-lb4-hp1** |
    | Protocol | **TCP** |
    | Port | **80** |
    | Intervalle | **5** |
    | Seuil de défaillance sur le plan de l’intégrité | **2** |

1. Cliquez sur **Ajouter**

1. Attendez que la sonde d’intégrité soit créée, dans la section **Paramètres**, cliquez sur **Règles d’équilibrage de charge**, puis cliquez sur **+ Ajouter**.

1. Ajoutez un équilibrage de charge entrante avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom | **az104-06-lb4-lbrule1** |
    | Version de l’adresse IP | **IPv4** |
    | Adresse IP du front-end | **sélectionnez LoadBalancerFrontEnd dans la liste déroulante**
    | Protocol | **TCP** |
    | Port | **80** |
    | Port principal | **80** |
    | Pool principal | **az104-06-lb4-be1** |
    | Sonde d’intégrité | **az104-06-lb4-hp1** |
    | Persistance de session | **Aucun** |
    | Délai d’inactivité (minutes) | **4** |
    | Réinitialisation du protocole TCP | **Désactivé** |
    | Adresse IP flottante (retour direct du serveur) | **Désactivé** |

1. Cliquez sur **Ajouter**

1. Attendez que la règle d’équilibrage de charge soit créée, dans la section **Paramètres**, cliquez sur **Configuration IP frontale** et notez la valeur de l’adresse **IP publique**.

1. Démarrez une autre fenêtre de navigateur et naviguez vers l'adresse IP que vous avez identifiée à l'étape précédente.

1. Vérifiez que la fenêtre du navigateur affiche le message **Hello World à partir d’az104-06-vm0** ou **Hello World à partir d’az104-06-vm1**.

1. Ouvrez une autre fenêtre de navigateur, mais cette fois en utilisant le mode InPrivate et vérifiez si la machine virtuelle cible change (comme indiqué par le message).

    > **Remarque** : Vous devrez peut-être actualiser la fenêtre du navigateur ou l’ouvrir à nouveau à l’aide du mode InPrivate.

#### <a name="task-6-implement-azure-application-gateway"></a>Tâche 6 : Implémenter une passerelle Azure Application Gateway

Dans cette tâche, vous allez implémenter Azure Application Gateway devant les deux machines virtuelles Azure dans les réseaux virtuels spoke.

1. Dans le Portail Azure, recherchez et sélectionnez **Réseaux virtuels**.

1. Dans le panneau **Réseaux virtuels** et, dans la liste des réseaux virtuels, cliquez sur **az104-06-vnet01**.

1. Dans le panneau de réseau virtuel **az104-06-vnet01**, dans la section **Paramètres**, cliquez sur **Sous-réseaux**, puis sur **+ Sous-réseau**.

1. Ajoutez un sous-réseau avec les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom | **subnet-appgw** |
    | Plage d’adresses de sous-réseau | **10.60.3.224/27** |

1. Cliquez sur **Enregistrer**.

    > **Remarque** : Ce sous-réseau sera utilisé par les instances Azure Application Gateway, que vous allez déployer ultérieurement dans cette tâche. Application Gateway nécessite un sous-réseau dédié de /27 ou de taille supérieure.

1. Dans le Portail Azure, recherchez et sélectionnez **Application Gateways**, puis, dans le panneau **Application Gateways**, cliquez sur **+ Créer**.

1. Sous l’onglet **Informations de base** du volet **Créer une passerelle applicative**, spécifiez les paramètres suivants (conservez les valeurs par défaut pour les autres) :

    | Paramètre | Valeur |
    | --- | --- |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Groupe de ressources | **az104-06-rg1** |
    | Nom de passerelle applicative | **az104-06-appgw5** |
    | Région | nom de la région Azure dans laquelle vous avez déployé toutes les autres ressources dans ce laboratoire |
    | Niveau | **Standard V2** |
    | Activer la mise à l’échelle automatique | **Non** |
    | HTTP2 | **Désactivé** |
    | Réseau virtuel | **az104-06-vnet01** |
    | Subnet | **subnet-appgw** |

1. Cliquez sur **Suivant : Frontends >**  et, dans l'onglet **Frontends** du panneau **Créer une passerelle applicative**, cliquez sur **Ajouter un nouveau**, et spécifiez les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Type d'adresse IP de front-end | **Public** |
    | Adresse IP publique| nom d’une nouvelle adresse IP publique **az104-06-pip5** |

1. Cliquez sur **Suivant : Back-ends >** , sous l’onglet **Back-ends** du panneau **Créer une passerelle d’application**, cliquez sur **Ajouter un pool de back-ends** et, dans le panneau **Ajouter un pool de back-ends**, spécifiez les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom | **az104-06-appgw5-be1** |
    | Ajouter un pool back-end sans cible | **Non** |
    | Type cible | **Adresse IP ou nom de domaine complet** |
    | Cible | **10.62.0.4** |
    | Type cible | **Adresse IP ou nom de domaine complet** |
    | Cible | **10.63.0.4** |

    > **Remarque** : Les cibles représentent les adresses IP privées des machines virtuelles dans les réseaux virtuels spoke **az104-06-vm2** et **az104-06-vm3**.

1. Cliquez sur **Ajouter**, cliquez sur **Suivant : Configuration >** et, dans l'onglet **Configuration** du panneau **Créer une passerelle applicative**, cliquez sur **+ Ajouter une règle de routage**.

1. Dans le panneau **Ajouter une règle de routage**, sous l’onglet **Écouteur**, spécifiez les paramètres suivants :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom de la règle | **az104-06-appgw5-rl1** |
    | Priorité | **10** |
    | Nom de l’écouteur | **az104-06-appgw5-rl1l1** |
    | Adresse IP du front-end | **Public** |
    | Protocol | **HTTP** |
    | Port | **80** |
    | Type d’écouteur | **De base** |
    | URL de page d’erreur | **Non** |

1. Basculez vers l’onglet **Cibles principales** du panneau **Ajouter une règle de routage** et spécifiez les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Type cible | **Pool back-end** |
    | Cible de back-end | **az104-06-appgw5-be1** |

1. Cliquez sur **Ajouter de nouveau** sous la zone de texte **Paramètres principaux**, puis, dans le panneau **Ajouter un paramètre principal**, spécifiez les paramètres suivants (laissez les autres avec leurs valeurs par défaut) :

    | Paramètre | Valeur |
    | --- | --- |
    | Nom des paramètres HTTP | **az104-06-appgw5-http1** |
    | Protocole back-end | **HTTP** |
    | Port principal | **80** |
    | Affinité basée sur les cookies | **Désactiver** |
    | Vidage des connexions | **Désactiver** |
    | Délai d'expiration de la demande (secondes) | **20** |

1. Cliquez sur **Ajouter** dans le panneau **Ajouter un paramètre HTTP**, puis de nouveau dans le panneau **Ajouter une règle de routage**, cliquez sur **Ajouter**.

1. Cliquez sur **Suivant : Balises >** , suivi de **Suivant : Vérifier + créer**, puis sur **Créer**.

    > **Remarque** : Attendez la création de l’instance Application Gateway. Ceci peut prendre environ 8 minutes.

1. Dans le Portail Azure, recherchez et sélectionnez **Application Gateways**, puis, dans le panneau **Application Gateways**, cliquez sur **az104-06-appgw5**.

1. Dans le panneau Application Gateway **az104-06-appgw5**, notez la valeur de **l’adresse IP publique frontend**.

1. Démarrez une autre fenêtre de navigateur et naviguez vers l'adresse IP que vous avez identifiée à l'étape précédente.

1. Vérifiez que la fenêtre du navigateur affiche le message **Hello World à partir d’az104-06-vm2** ou **Hello World à partir d’az104-06-vm3**.

1. Ouvrez une autre fenêtre de navigateur, mais cette fois en utilisant le mode InPrivate et vérifiez si la machine virtuelle cible change (comme indiqué par le message affiché sur la page web).

    > **Remarque** : Vous devrez peut-être actualiser la fenêtre du navigateur ou l’ouvrir à nouveau à l’aide du mode InPrivate.

    > **Remarque** : Le ciblage des machines virtuelles sur plusieurs réseaux virtuels n'est pas une configuration courante, mais elle vise à illustrer le fait qu’Application Gateway est capable de cibler des machines virtuelles sur plusieurs réseaux virtuels (ainsi que des points d'extrémité dans d'autres régions Azure ou même en dehors d'Azure), contrairement à Azure Load Balancer, qui équilibre la charge entre les machines virtuelles du même réseau virtuel.

#### <a name="clean-up-resources"></a>Nettoyer les ressources

>**Remarque** : N’oubliez pas de supprimer toutes les nouvelles ressources Azure que vous n’utilisez plus. La suppression des ressources inutilisées vous évitera d’encourir des frais inattendus.

>**Remarque** :  Ne vous inquiétez pas si les ressources lab ne peuvent pas être immédiatement supprimées. Parfois, les ressources ont des dépendances et prennent plus de temps à supprimer. Il s’agit d’une tâche d’administrateur courante pour surveiller l’utilisation des ressources. Il vous suffit donc de consulter régulièrement vos ressources dans le portail pour voir comment se passe le nettoyage. 

1. Dans le portail Azure, ouvrez la session **PowerShell** dans le volet **Cloud Shell**.

1. Listez tous les groupes de ressources créés dans les labos de ce module en exécutant la commande suivante :

   ```powershell
   Get-AzResourceGroup -Name 'az104-06*'
   ```

1. Supprimez tous les groupes de ressources que vous avez créés dans les labos de ce module en exécutant la commande suivante :

   ```powershell
   Get-AzResourceGroup -Name 'az104-06*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Remarque** : La commande s’exécute de façon asynchrone (tel que déterminé par le paramètre -AsJob). Vous pourrez donc exécuter une autre commande PowerShell immédiatement après au cours de la même session PowerShell, mais la suppression effective du groupe de ressources peut prendre quelques minutes.

#### <a name="review"></a>Révision

Dans cet exercice, vous avez :

+ Approvisionné l’environnement lab
+ Topologie de réseau hub-and-spoke configurée
+ Transitivité de l’appairage de réseaux virtuels testée
+ Tâche 4 : Configurer le routage dans la topologie hub and spoke
+ Tâche 5 : Mettre en œuvre l'équilibreur de charges Azure
+ Tâche 6 : Implémenter une passerelle Azure Application Gateway
