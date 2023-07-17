---
demo:
  title: "Démonstration\_10\_: Administrer la protection des données"
  module: Administer Data Protection
---

# 10 - Administrer la protection des données

## Sauvegarder des partages de fichiers Azure

Dans cette démonstration, nous allons explorer la sauvegarde d’un partage de fichiers dans le portail Azure.

> **Remarque :**  Cette démonstration nécessite un compte de stockage avec un partage de fichiers. 

**Informations de référence** : [Sauvegarder des partages de fichiers Azure dans le portail Azure](https://docs.microsoft.com/azure/backup/backup-afs)

**Créer un coffre Recovery Services**

1. Utilisez le portail Azure.

1. Recherchez et sélectionnez **Coffres Recovery Services**.

1. Créez un **coffre Recovery Services**. Expliquez pourquoi le coffre doit se trouver dans la même région que le partage de fichiers. 

1. Attendez que le coffre soit créé. 

**Configurer la sauvegarde des fichiers Azure**

1. Accédez au **Centre de sauvegarde** et créez une instance **Sauvegarde**.

1. Passez en revue et expliquez les choix de la liste déroulante **Type de source de données**. Sélectionnez **Fichiers Azure (Stockage Azure)** . 

1. Sélectionnez votre **coffre**.

1. **Continuez** la configuration de la sauvegarde. Sélectionnez le compte de stockage et le partage de fichiers spécifiques que vous voulez sauvegarder.  

1. Dans les **Détails de la stratégie**, cliquez sur **Modifier cette stratégie**. Expliquez l’objectif des stratégies de sauvegarde. Passez en revue la **planification des sauvegardes** et **la plage de conservation**.  

1. **Activez la sauvegarde** pour enregistrer vos modifications. 

1. Quand vous en avez le temps, expliquez comment **restaurer** une **instance Sauvegarde**. Expliquez aussi comment superviser vos **travaux de sauvegarde**. 

## Sauvegarder des machines virtuelles Azure

Dans cette démonstration, nous allons planifier une sauvegarde quotidienne d’une machine virtuelle dans un coffre Recovery Services.

> **Remarque :**  Cette démonstration nécessite une machine virtuelle et un coffre Recovery Services.

**Informations de référence** : [Tutoriel - Sauvegarder plusieurs machines virtuelles Azure](https://docs.microsoft.com/azure/backup/tutorial-backup-vm-at-scale)

1. Utilisez le portail Azure.

1. Accédez au **Centre de sauvegarde** et créez une instance **Sauvegarde**.

1. Sélectionnez **Machines virtuelles Azure** comme **Type de source de données**, puis sélectionnez le coffre.

1. Passez en revue la **Stratégie par défaut**. La stratégie par défaut sauvegarde la machine virtuelle une fois par jour. Les sauvegardes quotidiennes sont conservées pendant 30 jours. Les instantanés de récupération instantanée sont conservés pendant deux jours.

1. Utilisez **Activer la sauvegarde** pour enregistrer votre configuration.

1. Quand vous en avez le temps, expliquez comment **Sauvegarder maintenant**. Expliquez aussi comment passer en revue vos **travaux de sauvegarde**.  

