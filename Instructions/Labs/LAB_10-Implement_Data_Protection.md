---
lab:
  title: "Labo\_10 : Implémenter la protection des données"
  module: Administer Data Protection
---

# Labo 10 : Sauvegarder des machines virtuelles
# Manuel de labo de l’étudiant

## Scénario du labo

Il vous a été confié la tâche d’évaluer l’utilisation d’Azure Recovery Services pour la sauvegarde et la restauration de fichiers hébergés sur des machines virtuelles Azure et sur des ordinateurs locaux. Vous devez par ailleurs identifier des moyens de protéger les données stockées dans le coffre Recovery Services contre les risques de perte de données de nature accidentelle ou malveillante.

**Remarque :** Une **[simulation de labo interactive](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2016)** est disponible et vous permet de progresser à votre propre rythme. Il peut exister de légères différences entre la simulation interactive et le labo hébergé. Toutefois, les concepts et idées de base présentés sont identiques. 

## Objectifs

Dans ce labo, vous allez :

+ Tâche 1 : Approvisionner l’environnement de laboratoire
+ Tâche 2 : Créer un coffre Recovery Services
+ Tâche 3 : Implémenter une sauvegarde au niveau d’une machine virtuelle Azure
+ Tâche 4 : Implémenter une sauvegarde de fichiers et de dossiers
+ Tâche 5 : Récupérer des fichiers à l’aide de l’agent Azure Recovery Services
+ Tâche 6 : Effectuer une récupération de fichiers à l’aide de captures instantanées de machines virtuelles Azure (facultatif)
+ Tâche 7 : Passer en revue la fonctionnalité de suppression réversible Azure Recovery Services (facultatif)

## Durée estimée : 50 minutes

## Diagramme de l'architecture

![image](../media/lab10.png)

### Instructions

## Exercice 1

## Tâche 1 : Approvisionner l’environnement de laboratoire

Dans cette tâche, vous allez déployer deux machines virtuelles qui seront utilisées pour tester différents scénarios de sauvegarde.

1. Connectez-vous au [portail Azure](https://portal.azure.com).

1. Dans le portail Azure, ouvrez **Azure Cloud Shell** en cliquant sur l’icône située en haut à droite du portail Azure.

1. Lorsque vous êtes invité à sélectionner **Bash** ou **PowerShell**, sélectionnez **PowerShell**.

    >**Remarque** : Si c’est la première fois que vous démarrez **Cloud Shell** et que vous voyez le message **Vous n’avez aucun stockage monté**, sélectionnez l’abonnement que vous utilisez dans ce labo, puis sélectionnez **Créer un stockage**.

1. Dans la barre d'outils du volet Cloud Shell, cliquez sur l'icône **Charger/Télécharger des fichiers**, dans le menu déroulant, cliquez sur **Charger** et chargez les fichiers **\\Allfiles\\Labs\\10\\az104-10-vms-edge-template.json** et **\\Allfiles\\Labs\\10\\az104-10-vms-edge-parameters.json** dans le répertoire d'origine de Cloud Shell.

1. Dans le panneau Cloud Shell, exécutez la commande suivante pour créer le groupe de ressources qui hébergera les machines virtuelles (remplacez l’espace réservé `[Azure_region]` par le nom d’une région Azure dans laquelle vous envisagez de déployer des machines virtuelles Azure). Tapez chaque ligne de commande séparément et exécutez-les une à une :

   ```powershell
   $location = '[Azure_region]'
    ```
    
   ```powershell
   $rgName = 'az104-10-rg0'
    ```
    
   ```powershell
   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. Dans le volet Cloud Shell, exécutez la commande suivante pour créer le premier réseau virtuel et déployer une machine virtuelle dans celui-ci à l’aide du modèle et des fichiers de paramètres que vous avez chargés :
    >**Remarque** : Vous serez invité à fournir un mot de passe d’administrateur.
    
   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-10-vms-edge-template.json `
      -TemplateParameterFile $HOME/az104-10-vms-edge-parameters.json `
      -AsJob
   ```

1. Réduisez le volet Cloud Shell (mais sans le fermer).

    >**Remarque** : N’attendez pas que le déploiement se termine, mais passez à la tâche suivante. Le déploiement doit prendre environ 5 minutes.

## Tâche 2 : Créer un coffre Recovery Services

Dans cette tâche, vous allez créer un coffre Recovery Services.

1. Dans le Portail Azure, recherchez et sélectionnez **Coffres Recovery Services** puis, dans le panneau **Coffres Recovery Services**, cliquez sur **+ Créer**.

1. Dans le panneau **Créer un coffre Recovery Services**, spécifiez les paramètres suivants :

    | Paramètres | Valeur |
    | --- | --- |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Resource group | nom d’un nouveau groupe de ressources **az104-10-rg1** |
    | Nom du coffre | **az104-10-rsv1** |
    | Région | nom d’une région où vous avez déployé les deux machines virtuelles dans la tâche précédente |

    >**Remarque** : Vérifiez que vous spécifiez la même région dans laquelle vous avez déployé des machines virtuelles dans la tâche précédente.

1. Cliquez sur **Vérifier + Créer**. Vérifiez que la validation a bien été effectuée et cliquez sur **Créer**.

    >**Remarque** : Attendez la fin du déploiement. Le déploiement doit prendre moins d’une minute.

1. Une fois le déploiement terminé, cliquez sur **Accéder à la ressource**.

1. Dans le panneau du coffre Recovery Services **az104-10-rsv1**, dans la section **Paramètres**, cliquez sur **Propriétés**.

1. Dans le panneau **az104-10-rsv1 - Propriétés**, cliquez sur le lien **Mettre à jour** sous l’étiquette **Configuration de la sauvegarde**.

1. Dans le panneau **Configuration de la sauvegarde**, notez que vous pouvez définir le **Type de réplication de stockage** sur **Localement redondant** ou **Géoredondant**. Conservez le paramètre **Géoredondant** par défaut et fermez le volet.

    >**Remarque** : Ce paramètre peut être configuré uniquement s’il n’existe aucun élément de sauvegarde existant.

1. Revenez dans le panneau **az104-10-rsv1 - Propriétés**, cliquez sur le lien **Mettre à jour** sous l’étiquette **Paramètres de sécurité**.

1. Dans le panneau **Paramètres de sécurité**, notez que l’option **Suppression réversible (pour les charges de travail s’exécutant dans Azure)** est **activée**.

1. Fermez le volet **Paramètres de sécurité** et, dans le panneau du coffre Recovery Services **az104-10-rsv1**, cliquez sur **Vue d’ensemble**.

## Tâche 3 : Implémenter une sauvegarde au niveau d’une machine virtuelle Azure

Dans cette tâche, vous allez implémenter une sauvegarde au niveau de la machine virtuelle Azure.

   >**Remarque** : Avant de commencer cette tâche, assurez-vous que le déploiement que vous avez lancé dans la première tâche de ce laboratoire a réussi.

1. Dans le panneau du coffre Recovery Services **az104-10-rsv1**, cliquez sur **Vue d’ensemble**, puis sur **+ Sauvegarde**.

1. Dans le panneau **Objectif de la sauvegarde**, spécifiez les paramètres suivants :

    | Paramètres | Valeur |
    | --- | --- |
    | Où s'exécute votre charge de travail ? | **Microsoft Azure** |
    | Que souhaitez-vous sauvegarder ? | **Machine virtuelle** |

1. Dans le panneau **Objectif de la sauvegarde**, cliquez sur **Sauvegarde**.

1. Dans **Stratégie de sauvegarde**, vérifiez les paramètres **DefaultPolicy** et sélectionnez **Créer une stratégie**.

1. Définissez une nouvelle stratégie de sauvegarde avec les paramètres suivants (laissez les autres avec leur valeur par défaut) :

    | Paramètre | Valeur |
    | ---- | ---- |
    | Nom de stratégie | **az104-10-backup-policy** |
    | Fréquence | **Tous les jours** |
    | Temps | **12 h 00** |
    | Fuseau horaire | nom de votre fuseau horaire local |
    | Conserver le ou les instantanés de récupération instantanée pour | **2** jour(s) |

1. Cliquez sur **OK** pour créer la stratégie, puis, dans la section **Machines virtuelles**, sélectionnez **Ajouter**.

1. Dans le panneau **Sélectionner des machines virtuelles**, sélectionnez **az-104-10-vm0**, cliquez sur **OK**, puis, dans le panneau **Sauvegarde**, cliquez sur **Activer la sauvegarde**.

    >**Remarque** : Attendez que la sauvegarde soit activée. Ce processus prend environ 2 minutes.

1. De nouveau dans le panneau du coffre Recovery Services **az104-10-rsv1**, dans la section **Éléments protégés**, cliquez sur **Éléments de sauvegarde**, puis sur l’entrée **Machine virtuelle Azure**.

1. Dans le panneau**Éléments de sauvegarde (machine virtuelle Azure)** , sélectionnez le lien **Afficher les détails** en regard de **az104-10-vm0**, puis passez en revue les valeurs des entrées **Prévérification de sauvegarde** et **État de la dernière sauvegarde**.

1. Dans le panneau Élément de sauvegarde **az104-10-vm0**, cliquez sur **Sauvegarder maintenant**, acceptez la valeur par défaut dans la liste déroulante **Conserver la sauvegarde jusqu’au**, puis cliquez sur **OK**.

    >**Remarque** : N’attendez pas que la sauvegarde se termine. Passez à la tâche suivante.

## Tâche 4 : Implémenter une sauvegarde de fichiers et de dossiers

Dans cette tâche, vous allez implémenter une sauvegarde de fichiers et de dossiers à l’aide d’Azure Recovery Services.

1. Dans le portail Azure, recherchez et sélectionnez **Machines virtuelles**, puis, dans le panneau **Machines virtuelles**, cliquez sur **az104-10-vm1**.

1. Dans le panneau **az104-10-vm1**, cliquez sur **Connecter**, dans la liste déroulante, cliquez sur **RDP**, dans le panneau **Connecter avec RDP**, cliquez sur **Télécharger le fichier RDP** et suivez les invites pour démarrer la session Bureau à distance.

    >**Remarque** : Cette étape fait référence à la connexion via le Bureau à distance à partir d’un ordinateur Windows. Sur un Mac, vous pouvez utiliser le client Bureau à distance disponible sur le Mac App Store. Sur un ordinateur Linux, vous pouvez utiliser un logiciel client RDP open source.

    >**Remarque** : Vous pouvez ignorer toutes les invites d’avertissement lors de la connexion aux machines virtuelles cibles.

1. Lorsque vous y êtes invité, connectez-vous à l’aide du nom d’utilisateur **étudiant** et du mot de passe du fichier de paramètres.

    >**Remarque :** Étant donné que le portail Azure ne prend plus en charge IE11, vous devez utiliser le navigateur Microsoft Edge pour cette tâche.

1. Dans la session Bureau à distance sur la machine virtuelle Azure **az104-10-vm1**, démarrez un navigateur web Edge, accédez au [Portail Azure](https://portal.azure.com) et connectez-vous à l’aide de vos informations d’identification.

1. Dans le portail Azure, recherchez et sélectionnez **Coffres Recovery Services**, puis, dans le panneau **Coffres Recovery Services**, cliquez sur **az104-10-rsv1**.

1. Dans le panneau du coffre Recovery Services **az104-10-rsv1**, cliquez sur **+ Sauvegarde**.

1. Dans le panneau **Objectif de la sauvegarde**, spécifiez les paramètres suivants :

    | Paramètres | Valeur |
    | --- | --- |
    | Où s'exécute votre charge de travail ? | **Local** |
    | Que souhaitez-vous sauvegarder ? | **Fichiers et dossiers** |

    >**Remarque** : Même si la machine virtuelle que vous utilisez dans cette tâche s’exécute dans Azure, vous pouvez l’utiliser pour évaluer les fonctionnalités de sauvegarde applicables à n’importe quel ordinateur local exécutant le système d’exploitation Windows Server.

1. Dans le panneau **Objectif de la sauvegarde**, cliquez sur **Préparer l’infrastructure**.

1. Dans le panneau **Préparer l’infrastructure**, cliquez sur le lien **Télécharger l’agent pour Windows Server ou pour le client Windows**.

1. Lorsque vous y êtes invité, cliquez sur **Exécuter** pour démarrer l’installation de **MARSAgentInstaller.exe** avec les paramètres par défaut.

    >**Remarque** : Sur la page **Abonnement à Microsoft Update** de l’**Assistant Installation de Microsoft Azure Recovery Services Agent**, sélectionnez l’option d’installation **Je ne souhaite pas utiliser Microsoft Update**.

1. Sur la page **Installation** de l’**Assistant Installation de Microsoft Azure Recovery Services Agent**, cliquez sur **Procéder à l’enregistrement**. L’**Assistant Inscrire un serveur** s’ouvre.

1. Basculez vers la fenêtre du navigateur web affichant le portail Azure. Dans le panneau **Préparer l’infrastructure**, cochez la case **J'ai déjà téléchargé ou j'utilise déjà la dernière version de l'agent Recovery Services**, puis cliquez sur **Télécharger**.

1. Lorsque vous y êtes invité, si vous souhaitez ouvrir ou enregistrer le fichier d’informations d’identification du coffre, cliquez sur **Enregistrer**. Le fichier d’informations d’identification du coffre est alors enregistré dans le dossier local Téléchargements.

1. Revenez à la fenêtre **Assistant Inscrire un serveur** puis, dans la page **Identification du coffre**, cliquez sur **Parcourir**.

1. Dans la boîte de dialogue **Sélectionner les informations d’identification du coffre** , accédez au dossier **Téléchargements**, cliquez sur le fichier d’informations d’identification du coffre que vous avez téléchargé, puis cliquez sur **Ouvrir**.

1. Revenez à la page **Identification du coffre**, puis cliquez sur **Suivant**.

1. Sur la page **Paramètre de chiffrement** de l’**Assistant Inscrire un serveur**, cliquez sur **Générer une phrase secrète**.

1. Sur la page **Paramètre de chiffrement** de l’**Assistant Inscrire un serveur**, cliquez sur le bouton **Parcourir** en regard de l’option **Définir un emplacement pour enregistrer la phrase secrète**.

1. Dans la boîte de dialogue **Rechercher un dossier**, sélectionnez le dossier **Documents** et cliquez sur **OK**.

1. Cliquez sur **Terminer**, passez en revue l’avertissement **Sauvegarde Microsoft Azure**, puis cliquez sur **Oui** et attendez que l’inscription soit terminée.

    >**Remarque** : Dans un environnement de production, vous devez stocker le fichier de phrase secrète dans un emplacement sécurisé autre que le serveur sauvegardé.

1. Sur la page **Inscription du serveur** de l’**Assistant Inscrire un serveur**, passez en revue l’avertissement concernant l’emplacement du fichier de phrase secrète, vérifiez que la case **Démarrer Microsoft Azure Recovery Services Agent** est activée et cliquez sur **Fermer**. La console **Sauvegarde Microsoft Azure** s’ouvre alors automatiquement.

1. Dans la console **Sauvegarde Microsoft Azure**, dans le panneau **Actions**, cliquez sur **Planifier une sauvegarde**.

1. Sur la page **Mise en route** de l’**Assistant Planifier la sauvegarde**, cliquez sur **Suivant**.

1. Sur la page **Sélectionner les éléments à sauvegarder**, cliquez sur **Ajouter des éléments**.

1. Dans la boîte de dialogue **Sélectionner des éléments**, développez **C:\\Windows\\System32\\drivers\\etc\\** , sélectionnez **hosts**, puis cliquez sur **OK** :

1. Sur la page **Sélectionner les éléments à sauvegarder**, cliquez sur **Suivant**.

1. Sur la page **Spécifier la planification de sauvegarde**, assurez-vous que l’option **Jour** est sélectionnée. Dans la première zone de liste déroulante sous la zone **Aux heures suivantes (le maximum autorisé est trois fois par jour)** , sélectionnez **4 h 30**, puis cliquez sur **Suivant**.

1. Sur la page **Sélectionner une stratégie de rétention**, acceptez les paramètres par défaut, puis cliquez sur **Suivant**.

1. Sur la page **Choisir un type de sauvegarde initiale**, acceptez les valeurs par défaut, puis cliquez sur **Suivant**.

1. Sur la page **Confirmation**, cliquez sur **Terminer**. Une fois la planification de sauvegarde créée, cliquez sur **Fermer**.

1. Dans le panneau Actions de la console **Sauvegarde Microsoft Azure**, cliquez sur **Sauvegarder maintenant**.

    >**Remarque** : L’option permettant d’exécuter la sauvegarde à la demande devient disponible une fois que vous créez une sauvegarde planifiée.

1. Dans l’Assistant Sauvegarder maintenant, sur la page **Sélectionner un élément de sauvegarde**, vérifiez que l’option **Fichiers et Dossiers** est sélectionnée et cliquez sur **Suivant**.

1. Sur la page **Conserver la sauvegarde jusqu’au**, acceptez le paramètre par défaut, puis cliquez sur **Suivant**.

1. Sur la page **Confirmation**, cliquez sur **Sauvegarder**.

1. Une fois la sauvegarde terminée, cliquez sur **Fermer**, puis fermez Sauvegarde Microsoft Azure.

1. Basculez vers la fenêtre du navigateur web affichant le portail Azure, revenez au volet**Coffre Recovery Services**, puis, dans la section **Éléments protégés**, cliquez sur **Éléments de sauvegarde**.

1. Dans le panneau **az104-10-rsv1 - Éléments de sauvegarde**, cliquez sur **Agent de sauvegarde Azure**.

1. Dans le panneau **Éléments de sauvegarde (agent de sauvegarde Azure)** , vérifiez qu’il existe une entrée faisant référence au lecteur **C:\\** de **az104-10-vm1**.

## Tâche 5 : Récupérer des fichiers à l’aide de l’agent Azure Recovery Services (facultatif)

Dans cette tâche, vous allez restaurer des fichiers à l’aide de l’agent Azure Recovery Services.

1. Dans la session Bureau à distance vers **az104-10-vm1**, ouvrez l’Explorateur de fichiers, accédez au dossier **C:\\Windows\\ System32\\,\\etc\\** et supprimez le fichier **hosts**.

1. Ouvrez Sauvegarde Microsoft Azure, puis cliquez sur **Récupérer des données** dans le panneau **Actions**. L’**Assistant Récupérer des données** s’ouvre.

1. Sur la page **Prise en main** de **l’Assistant Récupérer des données**, vérifiez que l’option **Ce serveur (az104-10-vm1.)** est sélectionnée et cliquez sur **Suivant**.

1. Sur la page **Sélectionner le mode de récupération**, vérifiez que l’option **Fichiers et dossiers individuels** est sélectionnée, puis cliquez sur **Suivant**.

1. Sur la page **Sélectionner le volume et la date**, dans la liste déroulante **Sélectionner le volume**, sélectionnez **C:\\** , acceptez la sélection par défaut de la sauvegarde disponible, puis cliquez sur **Monter**.

    >**Remarque** : Attendez que l’opération de montage se termine. Ceci peut prendre environ 2 minutes.

1. Sur la page **Parcourir et récupérer des fichiers**, notez la lettre de lecteur du volume de récupération et lisez le conseil concernant l’utilisation de robocopy.

1. Cliquez sur **Démarrer**, développez le dossier **Système Windows**, puis cliquez sur **Invite de commandes**.

1. À partir de l’invite de commandes, exécutez la commande suivante pour copier le fichier **hosts** à l’emplacement d’origine (remplacez `[recovery_volume]` par la lettre de lecteur du volume de récupération que vous avez identifié précédemment) :

   ```sh
   robocopy [recovery_volume]:\Windows\System32\drivers\etc C:\Windows\system32\drivers\etc hosts /r:1 /w:1
   ```

1. Revenez à l’**Assistant Récupérer des données** et, dans **Parcourir et récupérer des fichiers**, cliquez sur **Démonter**. À l’invite de confirmation, cliquez sur **Oui**.

1. Terminez la session Bureau à distance.

## Tâche 6 : Effectuer une récupération de fichiers à l’aide de captures instantanées de machines virtuelles Azure (facultatif)

Dans cette tâche, vous allez restaurer un fichier à partir de la sauvegarde basée sur des captures instantanées au niveau de la machine virtuelle Azure.

1. Basculez vers la fenêtre du navigateur exécutée sur votre ordinateur de labo et affichez le portail Azure.

1. Dans le portail Azure, recherchez et sélectionnez **Machines virtuelles**, puis, dans le panneau **Machines virtuelles**, cliquez sur **az104-10-vm0**.

1. Dans le panneau**az104-10-vm0**, cliquez sur **Connecter**, dans la liste déroulante, cliquez sur **RDP**, dans le panneau **Connecter avec RDP**, cliquez sur **Télécharger le fichier RDP** et suivez les invites pour démarrer la session Bureau à distance.

    >**Remarque** : Cette étape fait référence à la connexion via le Bureau à distance à partir d’un ordinateur Windows. Sur un Mac, vous pouvez utiliser le client Bureau à distance disponible sur le Mac App Store. Sur un ordinateur Linux, vous pouvez utiliser un logiciel client RDP open source.

    >**Remarque** : Vous pouvez ignorer toutes les invites d’avertissement lors de la connexion aux machines virtuelles cibles.

1. Lorsque vous y êtes invité, connectez-vous à l’aide du nom d’utilisateur **étudiant** et du mot de passe du fichier de paramètres.

   >**Remarque :** Étant donné que le portail Azure ne prend plus en charge IE11, vous devez utiliser le navigateur Microsoft Edge pour cette tâche.

1. Dans la session Bureau à distance vers **az104-10-vm0**, cliquez sur **Démarrer**, développez le dossier **Système Windows**, puis cliquez sur **Invite de commandes**.

1. À partir de l’invite de commandes, exécutez la commande suivante pour supprimer le fichier **hosts** :

   ```sh
   del C:\Windows\system32\drivers\etc\hosts
   ```

   >**Remarque** : La restauration de ce fichier à partir de la sauvegarde basée sur des captures instantanées au niveau de la machine virtuelle Azure se fera plus tard dans cette tâche.

1. Dans la session Bureau à distance sur la machine virtuelle Azure **az104-10-vm0**, démarrez un navigateur web Edge, accédez au [Portail Azure](https://portal.azure.com) et connectez-vous à l’aide de vos informations d’identification.

1. Dans le portail Azure, recherchez et sélectionnez **Coffres Recovery Services**, puis, dans le panneau **Coffres Recovery Services**, cliquez sur **az104-10-rsv1**.

1. Dans le panneau du coffre Recovery Services **az104-10-rsv1**, dans la section **Éléments protégés**, cliquez sur **Éléments de sauvegarde**.

1. Dans le panneau **az104-10-rsv1 - Éléments de sauvegarde**, cliquez sur **Machine virtuelle Azure**.

1. Dans le panneau **Éléments de sauvegarde (Machine virtuelle Azure),** sélectionnez **Afficher les détails** en regard de **az104-10-vm0**.

1. Dans le panneau Élément de sauvegarde **az104-10-vm0**, cliquez sur **Récupération de fichier**.

    >**Remarque** : Vous avez la possibilité d’exécuter la récupération peu après le démarrage de la sauvegarde en fonction de l’instantané cohérent de l’application.

1. Dans le panneau **Récupération de fichiers**, acceptez le point de récupération par défaut et cliquez sur **Télécharger l’exécutable**.

    >**Remarque** : Le script monte les disques à partir du point de récupération sélectionné en tant que lecteurs locaux dans le système d’exploitation à partir duquel le script est exécuté.

1. Cliquez sur **Télécharger** puis, lorsque vous êtes invité à exécuter ou à enregistrer **IaaSVMILRExeForWindows.exe**, cliquez sur **Enregistrer**.

1. Dans la fenêtre Explorateur de fichiers, double-cliquez sur le fichier que vous venez de télécharger.

1. Lorsque vous êtes invité à indiquer le mot de passe à partir du portail, copiez le mot de passe de la zone de texte **Mot de passe pour exécuter le script** du volet **Récupération de fichiers**, collez-le dans l’invite de commandes, puis appuyez sur **Entrée**.

    >**Remarque** : Cette opération ouvre une fenêtre Windows PowerShell affichant la progression du montage.

    >**Remarque** : Si vous recevez un message d’erreur à ce stade, actualisez la fenêtre du navigateur web et répétez les trois dernières étapes.

1. Attendez que le processus de montage se termine, passez en revue les messages d’information dans la fenêtre Windows PowerShell, notez la lettre de lecteur affectée au volume hébergeant **Windows** et démarrez l’Explorateur de fichiers.

1. Dans l’Explorateur de fichiers, accédez à la lettre de lecteur hébergeant la capture instantanée du volume hébergeant le système d’exploitation que vous avez identifié à l’étape précédente et passez en revue son contenu.

1. Basculez dans la fenêtre **Invite de commandes**.

1. À partir de l’invite de commandes, exécutez la commande suivante pour copier le fichier **hosts** à l’emplacement d’origine (remplacez `[os_volume]` par la lettre de lecteur du volume hébergeant le système d’exploitation que vous avez identifié précédemment) :

   ```sh
   robocopy [os_volume]:\Windows\System32\drivers\etc C:\Windows\system32\drivers\etc hosts /r:1 /w:1
   ```

1. Revenez au volet **Récupération de fichiers** dans le portail Azure, puis cliquez sur **Démonter les disques**.

1. Terminez la session Bureau à distance.

## Tâche 7 : Passer en revue la fonctionnalité de suppression réversible Azure Recovery Services

1. Sur l’ordinateur de labo, dans le portail Azure, recherchez et sélectionnez **Coffres Recovery Services**, puis, dans le panneau **Coffres Recovery Services**, cliquez sur **az104-10-rsv1**.

1. Dans le panneau du coffre Recovery Services **az104-10-rsv1**, dans la section **Éléments protégés**, cliquez sur **Éléments de sauvegarde**.

1. Dans le panneau **az104-10-rsv1 - Éléments de sauvegarde**, cliquez sur **Agent de sauvegarde Azure**.

1. Dans le panneau **Éléments de sauvegarde (Sauvegarde Azure Agent)** , cliquez sur l’entrée représentant la sauvegarde de **az104-10-vm1**.

1. Dans le panneau **C:\\ on az104-10-vm1.** , sélectionnez **Afficher les détails** en regard de **az104-10-vm1.** .

1. Dans le panneau Détail, cliquez sur **az104-10-vm1**.

1. Dans le panneau **az104-10-vm1.** Serveurs protégés, cliquez sur **Supprimer**.

1. Dans le panneau **Supprimer**, spécifiez les paramètres suivants.

    | Paramètres | Valeur |
    | --- | --- |
    | TAPEZ LE NOM DU SERVEUR | **az104-10-vm1.** |
    | Motif | **Recyclage du serveur Dev/Test** |
    | Commentaires | **az104 10 lab** |

    >**Remarque** : Veillez à inclure la période de fin lors de la saisie du nom du serveur.

1. Cochez la case en regard de l’étiquette **Des données de sauvegarde existent pour 1 élément de sauvegarde associé à ce serveur. En cliquant sur « Confirmer », je comprends que toutes les données de sauvegarde cloud seront supprimées définitivement. Il est impossible d’annuler cette opération. Une alerte peut être envoyée aux administrateurs de cet abonnement pour les informer de cette suppression**. Cliquez ensuite sur **Supprimer**.

    >**Remarque** : L’opération échoue, car la fonctionnalité **Suppression réversible** doit être désactivée.

1. Revenez au panneau **az104-10-rsv1 - Éléments de sauvegarde**, puis cliquez sur **Machines virtuelles Azure**.

1. Dans le panneau **az104-10-rsv1 - Éléments de sauvegarde**, cliquez sur **Machine virtuelle Azure**.

1. Dans le panneau **Éléments de sauvegarde (Machine virtuelle Azure),** sélectionnez **Afficher les détails** en regard de **az104-10-vm0**.

1. Dans le panneau **az104-10-vm0** Élément de sauvegarde, cliquez sur **Arrêter la sauvegarde**.

1. Dans le panneau **Arrêter la sauvegarde**, sélectionnez **Supprimer les données de sauvegarde**, spécifiez les paramètres suivants, puis cliquez sur **Arrêter la sauvegarde** :

    | Paramètres | Valeur |
    | --- | --- |
    | Tapez le nom de l'élément de sauvegarde | **az104-10-vm0** |
    | Motif | **Autres** |
    | Commentaires | **az104 10 lab** |

1. Revenez au volet **az104-10-rsv1 - Éléments de sauvegarde**, puis cliquez sur **Actualiser**.

    >**Remarque** : L’entrée **Machine virtuelle Azure** répertorie toujours **1** élément de sauvegarde.

1. Cliquez sur l’entrée **Machine virtuelle Azure** et, dans le panneau **Éléments de sauvegarde (machine virtuelle Azure)** , cliquez sur l’entrée **az104-10-vm0**.

1. Dans le panneau Élément de sauvegarde de **az104-10-vm0**, notez que vous avez la possibilité **d’annuler la suppression** de la sauvegarde.

    >**Remarque** : Cette fonctionnalité est fournie par la fonctionnalité de suppression réversible, qui est activée par défaut pour les sauvegardes de machines virtuelles Azure.

1. Revenez dans le panneau du coffre Recovery Services **az104-10-rsv1**, puis, dans la section **Paramètres**, cliquez sur **Propriétés**.

1. Dans le panneau **az104-10-rsv1 - Propriétés**, cliquez sur le lien **Mettre à jour** sous l’étiquette **Paramètres de sécurité**.

1. Dans le panneau **Paramètres de sécurité**, désactivez **Suppression réversible (pour les charges de travail s’exécutant dans Azure)** et **Fonctionnalités de sécurité (pour les charges de travail s’exécutant localement)** , puis cliquez sur **Enregistrer**.

    >**Remarque** : Cela n’affecte pas les éléments déjà à l’état de suppression réversible.

1. Fermez le volet **Paramètres de sécurité** et, dans le panneau du coffre Recovery Services **az104-10-rsv1**, cliquez sur **Vue d’ensemble**.

1. Revenez au volet Élément de sauvegarde de **az104-10-vm0** et cliquez sur **Annuler la suppression**.

1. Dans le panneau **Annuler la suppression de az104-10-vm0**, cliquez sur **Annuler la suppression**.

1. Attendez que l’opération de suppression soit terminée, actualisez la page du navigateur web, si nécessaire, revenez au volet Élément de sauvegarde de **az104-10-vm0**, puis cliquez sur **Supprimer les données de sauvegarde**.

1. Dans le panneau **Supprimer les données de sauvegarde**, spécifiez les paramètres suivants et cliquez sur **Supprimer** :

    | Paramètres | Valeur |
    | --- | --- |
    | Tapez le nom de l'élément de sauvegarde | **az104-10-vm0** |
    | Motif | **Autres** |
    | Commentaires | **az104 10 lab** |

1. Répétez les étapes au début de cette tâche pour supprimer les éléments de sauvegarde pour **az104-10-vm1**.

## Nettoyer les ressources

>**Remarque** : N’oubliez pas de supprimer toutes les nouvelles ressources Azure que vous n’utilisez plus. La suppression des ressources inutilisées vous évitera d’encourir des frais inattendus.

>**Remarque** :  Ne vous inquiétez pas si les ressources de laboratoire ne peuvent pas être immédiatement supprimées. Parfois, les ressources ont des dépendances et leur suppression prend plus de temps. Il s’agit d’une tâche d’administrateur courante pour surveiller l’utilisation des ressources. Il vous suffit donc de consulter régulièrement vos ressources dans le portail pour voir comment se passe le nettoyage. 

1. Dans le portail Azure, ouvrez la session **PowerShell** dans le volet **Cloud Shell**.

1. Listez tous les groupes de ressources créés dans les labos de ce module en exécutant la commande suivante :

   ```powershell
   Get-AzResourceGroup -Name 'az104-10*'
   ```

1. Supprimez tous les groupes de ressources que vous avez créés dans les labos de ce module en exécutant la commande suivante :

   ```powershell
   Get-AzResourceGroup -Name 'az104-10*' | Remove-AzResourceGroup -Force -AsJob
   ```

   >**Remarque** : Si vous le souhaitez, vous pouvez envisager de supprimer le groupe de ressources généré automatiquement avec le préfixe **AzureBackupRG_** (aucuns frais supplémentaires ne sont associés à son existence).

    >**Remarque** : La commande s’exécute de façon asynchrone (tel que déterminé par le paramètre -AsJob). Vous pourrez donc exécuter une autre commande PowerShell immédiatement après au cours de la même session PowerShell, mais la suppression effective du groupe de ressources peut prendre quelques minutes.

## Révision

Dans cet exercice, vous avez :

+ Approvisionné l’environnement de labo
+ Créé un coffre Recovery Services
+ Implémenté une sauvegarde au niveau d’une machine virtuelle Azure
+ Implémenté une sauvegarde de fichiers et de dossiers
+ Récupéré des fichiers à l’aide de l’agent Azure Recovery Services
+ Effectué une récupération de fichiers à l’aide de captures instantanées de machines virtuelles Azure
+ Passé en revue la fonctionnalité de suppression réversible Azure Recovery Services
