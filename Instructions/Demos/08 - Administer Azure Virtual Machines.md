---
demo:
  title: "Démonstration 08\_: Administrer des machines virtuelles Azure"
  module: Administer Azure Virtual Machines
---


# 08 - Administrer des machines virtuelles Azure

## Démonstration - Créer des machines virtuelles dans le portail

Dans cette démonstration, nous allons créer une machine virtuelle Windows et y accéder dans le portail.

**Informations de référence**

[Démarrage rapide : Créer une machine virtuelle Windows dans le portail Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

[Démarrage rapide : Créer une machine virtuelle Linux dans le portail Azure](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal)

[Se connecter à une machine virtuelle avec Bastion](https://learn.microsoft.com/azure/bastion/tutorial-create-host-portal#connect)

**Créer la machine virtuelle**

**Remarque :**  Ces étapes couvrent seulement quelques paramètres de machine virtuelle. N’hésitez pas à explorer et à couvrir d’autres domaines. Vous pouvez créer une machine virtuelle Windows ou Linux, en fonction de votre public.

1. Utilisez le portail Azure.

1. Recherchez **Machines virtuelles**. 

1. Créez une machine virtuelle de base. Passez en revue les options de disponibilité, les images et les règles de trafic entrant.

1. Décrivez de l’importance de la création d’un compte d’administrateur sécurisé.

1. Créez la machine virtuelle et attendez que la ressource soit déployée.  

**Connectez-vous à la machine virtuelle.**

1. Il existe plusieurs façons de **se connecter** à la machine virtuelle. 

1. Pour un serveur Windows, vous pouvez utiliser **RDP**, comme montré dans le guide de démarrage rapide. 

1. Pour un serveur Linux, vous pouvez utiliser **SSH**, comme montré dans le guide de démarrage rapide. 

1. Pour l’un ou l’autre des serveurs, vous pouvez vous connecter avec le service **Bastion** (guide de démarrage rapide). Expliquez pourquoi Bastion est préféré à RDP ou à SSH. 

## Configurer la disponibilité des machines virtuelles

Dans cette démonstration, nous allons explorer les options de mise à l’échelle des machines virtuelles.

**Informations de référence**

[Créer des machines virtuelles dans un groupe identique à l’aide du portail Azure](https://learn.microsoft.com/azure/virtual-machine-scale-sets/flexible-virtual-machine-scale-sets-portal)

1. Utilisez le portail Azure.

1. Recherchez et sélectionnez **Groupes de machines virtuelles identiques**. 

1. Créez un **groupe de machines virtuelles identiques**. Passez en revue l’objectif des groupes de machines virtuelles identiques. Passez en revue la différence entre les modes d’orchestration **Uniforme** et **Flexible**. Expliquez que votre sélection peut affecter vos options de mise à l’échelle. 

1. Accédez à l’onglet **Mise à l’échelle**. 

1. Expliquez comment  **Mise à l’échelle manuelle** et **Stratégie de mise à l’échelle** sont utilisés. 

1. Passez à une stratégie de mise à l’échelle **Personnalisée**. 

1. Expliquez comment **Seuil du processeur (%)** est utilisé pour effectuer un scale-out et un scale-in dans les instances de machine virtuelle. 

