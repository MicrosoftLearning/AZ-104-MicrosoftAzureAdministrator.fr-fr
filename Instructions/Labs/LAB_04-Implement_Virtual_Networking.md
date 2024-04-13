---
lab:
  title: "Labo\_04\_: Implémenter des réseaux virtuels"
  module: Implement Virtual Networking
---

# Labo 04 : Implémenter des réseaux virtuels

## Présentation du labo

Ce labo est le premier de trois labos qui sont axés sur la mise en réseau virtuelle. Dans ce labo, vous découvrez les principes de base de la mise en réseau virtuelle et de la mise en sous-réseau. Vous découvrez comment protéger votre réseau avec des groupes de sécurité réseau et des groupes de sécurité d’application. Vous découvrez aussi les zones et les enregistrements DNS. 

Ce labo nécessite un abonnement Azure. Le type de votre abonnement peut affecter la disponibilité des fonctionnalités dans ce labo. Vous pouvez changer la région, mais les étapes sont écrites de façon à utiliser **USA Est**.

## Durée estimée : 50 minutes

## Scénario du labo 

Votre organisation mondiale prévoit d’implémenter des réseaux virtuels. L’objectif immédiat est de prendre en charge toutes les ressources existantes. Cependant, l’organisation est en phase de croissance et veut s’assurer qu’elle dispose de capacités supplémentaires pour faire face à cette croissance.

Le réseau virtuel **CoreServicesVnet** compte le plus grand nombre de ressources. Une croissance importante est prévue, il faut donc un espace d’adressage important pour ce réseau virtuel.

Le réseau virtuel **ManufacturingVnet** contient des systèmes pour les opérations du site de fabrication. L’organisation prévoit un grand nombre d’appareils connectés internes à partir desquels ses systèmes pourront récupérer des données. 

## Simulations de labo interactives

Il existe plusieurs simulations de laboratoire interactives qui peuvent vous être utiles pour ce sujet. La simulation vous permet de parcourir un scénario similaire, à votre propre rythme. Il existe des différences entre la simulation interactive et ce labo, mais bon nombre des principaux concepts sont les mêmes. Un abonnement Azure n’est pas nécessaire. 

+ [Sécuriser le trafic réseau](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2013). Créer une machine virtuelle, un réseau virtuel et un groupe de sécurité réseau. Ajouter des règles de groupe de sécurité réseau pour autoriser et interdire le trafic.
  
+ [Créer un réseau virtuel simple](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%204). Créer un réseau virtuel avec deux machines virtuelles. Démontrer que les machines virtuelles peuvent communiquer. 

+ [Concevoir et implémenter un réseau virtuel dans Azure](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Design%20and%20implement%20a%20virtual%20network%20in%20Azure). Créer un groupe de ressources et créer des réseaux virtuels avec des sous-réseaux.  

+ [Implémenter la mise en réseau virtuelle](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208). Créer et configurer un réseau virtuel, déployer des machines virtuelles, configurer des groupes de sécurité réseau et configurer Azure DNS.

## Diagramme de l'architecture

![Configuration réseau](../media/az104-lab04-architecture.png)

Ces réseaux virtuels et ces sous-réseaux sont structurés de manière à prendre en charge les ressources existantes, tout en permettant la croissance prévue. Nous allons créer ces réseaux virtuels et ces sous-réseaux pour poser les bases de notre infrastructure réseau.

>**Le saviez-vous ?** C’est une bonne pratique que d’éviter le chevauchement des plages d’adresses IP afin de réduire les problèmes et de simplifier leur résolution. Le chevauchement est une préoccupation sur l’ensemble du réseau, que ce soit dans le cloud ou en local. De nombreuses organisations conçoivent un schéma d’adressage IP à l’échelle de l’entreprise pour éviter le chevauchement et prévoir la croissance future.

## Compétences de tâche

+ Tâche 1 : Créer un réseau virtuel avec des sous-réseaux en utilisant le portail.
+ Tâche 2 : Créer un réseau virtuel et des sous-réseaux en utilisant un modèle.
+ Tâche 3 : Créer et configurer la communication entre un groupe de sécurité d’application et un groupe de sécurité réseau.
+ Tâche 4 : Configurer des zones Azure DNS publiques et privées.
  
## Tâche 1 : Créer un réseau virtuel avec des sous-réseaux en utilisant le portail

L’organisation prévoit une forte croissance pour les services de base. Dans cette tâche, vous créez le réseau virtuel et les sous-réseaux associés pour prendre en charge les ressources existantes et la croissance prévue. Dans cette tâche, vous allez utiliser le portail Azure. 

1. Connectez-vous au **portail Azure** - `https://portal.azure.com`.
   
1. Recherchez et sélectionnez `Virtual Networks`.

1. Sélectionnez **Créer** dans la page Réseaux virtuels.

1. Renseignez l’onglet **Informations de base** pour CoreServicesVnet.  

    |  **Option**         | **Valeur**            |
    | ------------------ | -------------------- |
    | Groupe de ressources     | `az104-rg4` (si nécessaire, créez-en un) |
    | Nom               | `CoreServicesVnet`     |
    | Région             | (US) **USA Est**         |

1. Passez à l’onglet **Adresses IP**.

    |  **Option**         | **Valeur**            |
    | ------------------ | -------------------- |

    | Espace d’adressage IPv4 | `10.20.0.0/16` (séparez les entrées)    |

1. Sélectionnez **+ Ajouter un sous-réseau**. Renseignez les informations de nom et d’adresse pour chaque sous-réseau. Veillez à sélectionner **Ajouter** pour chaque nouveau sous-réseau. 

    | **Sous-réseau**             | **Option**           | **Valeur**              |
    | ---------------------- | -------------------- | ---------------------- |
    | SharedServicesSubnet   | Nom du sous-réseau          | `SharedServicesSubnet`   |
    |                        | Adresse de début     | `10.20.10.0`          |
    |                        | Taille                 | `/24` |
    | DatabaseSubnet         | Nom du sous-réseau          | `DatabaseSubnet`         |
    |                        | Adresse de début     | `10.20.20.0`        |
    |                        | Taille                 | `/24` |

    >**Remarque :** Chaque réseau virtuel doit avoir au moins un sous-réseau. Rappelez-vous que cinq adresses IP seront toujours réservées : prenez donc cela en compte dans votre planification. 

1. Pour terminer la création de CoreServicesVnet et des sous-réseaux associés, sélectionnez **Vérifier + créer**.

1. Vérifiez que la validation de votre configuration a réussi, puis sélectionnez **Créer**.

1. Attendez que le réseau virtuel soit déployé, puis sélectionnez **Accéder à la ressource**.

1. Prenez un moment pour vérifier l’**Espace d’adressage** et les **Sous-réseaux**. Notez vos autres choix dans le panneau **Paramètres**. 

1. Dans la section **Automation**, sélectionnez **Exporter le modèle**, puis attendez que le modèle soit généré.

1. **Téléchargez** le modèle.

1. Sur la machine locale, accédez au dossier **Téléchargements**, puis **Extrayez tous** les fichiers dans le fichier zip téléchargé. 

1. Avant de continuer, vérifiez que vous disposez du fichier **template.json**. Vous allez utiliser ce modèle pour créer le réseau manufacturingVnet dans la tâche suivante. 
 
## Tâche 2 : Créer un réseau virtuel et des sous-réseaux en utilisant un modèle

Dans cette tâche, vous créez le réseau virtuel ManufacturingVnet et les sous-réseaux associés. L’organisation prévoit la croissance des bureaux associés à la fabrication : les sous-réseaux sont donc dimensionnés pour la croissance attendue. Pour cette tâche, vous utilisez un modèle pour créer les ressources. 

1. Recherchez le fichier **template.json** exporté dans la tâche précédente. Il doit normalement se trouver dans votre dossier **Téléchargements**.

1. Modifiez le fichier en utilisant l’éditeur de votre choix. De nombreux éditeurs ont une fonctionnalité permettant de *modifier toutes les occurrences*. Si vous utilisez Visual Studio Code, veillez à travailler dans une **fenêtre approuvée** et non pas dans le **mode restreint**. Consultez le diagramme de l’architecture pour vérifier les détails. 

### Apporter des modifications au réseau virtuel ManufacturingVnet

1. Remplacez toutes les occurrences de **CoreServicesVnet** par `ManufacturingVnet`. 

1. Remplacez toutes les occurrences de **10.20.0.0** par `10.30.0.0`. 

### Apporter des modifications aux sous-réseaux ManufacturingVnet

1. Changez toutes les occurrences de **SharedServicesSubnet** en `SensorSubnet1`.

1. Changez toutes les occurrences de **10.20.10.0/24** en `10.30.20.0/24`.

1. Changez toutes les occurrences de **DatabaseSubnet** en `SensorSubnet2`.

1. Changez toutes les occurrences de **10.20.20.0/24** en `10.30.21.0/24`.

1. Passez en revue le fichier et vérifiez que tout est correct.

1. Veillez à **Enregistrer** vos modifications.

>**Remarque :** Un fichier de modèle terminé se trouve dans le répertoire des fichiers du labo. 

### Apporter des modifications au fichier de paramètres

1. Recherchez le fichier **parameters.json** exporté dans la tâche précédente. Il doit normalement se trouver dans votre dossier **Téléchargements**.

1. Modifiez le fichier en utilisant l’éditeur de votre choix.

1. Remplacez l’occurrence de **CoreServicesVnet** par `ManufacturingVnet`.

1. **Enregistrez** les changements apportés.
   
### Déployer le modèle personnalisé

1. Dans le portail, recherchez et sélectionnez **Déployer un modèle personnalisé**.

1. Sélectionnez **Créer votre propre modèle dans l’éditeur**, puis **Charger le fichier**.

1. Sélectionnez le fichier **templates.json** avec vos modifications liées à la fabrication, puis sélectionnez **Enregistrer**.

1. Sélectionnez **Vérifier + créer**, puis **Créer**.

1. Attendez que le modèle soit déployé, puis vérifiez (dans le portail) que le réseau virtuel et les sous-réseaux liés à la fabrication ont bien été créés.

>**Remarque :** Si vous devez effectuer plusieurs déploiements, il se peut que certaines ressources aient été créées correctement et que le déploiement échoue. Vous pouvez supprimer manuellement ces ressources et réessayer. 
   
## Tâche 3 : Créer et configurer la communication entre un groupe de sécurité d’application et un groupe de sécurité réseau

Dans cette tâche, nous créons un groupe de sécurité d’application et un groupe de sécurité réseau. Le groupe de sécurité réseau aura une règle de sécurité de trafic entrant qui autorise le trafic provenant du groupe de sécurité d’application. Le groupe de sécurité réseau aura également une règle de trafic sortant qui refuse l’accès à Internet. 

### Créer un groupe de sécurité d’application

1. Dans le portail Azure, recherchez et sélectionnez `Application security groups`.

1. Cliquez sur **Créer** et fournissez les informations de base.

    | Paramètre | Valeur |
    | -- | -- |
    | Abonnement | *votre abonnement* |
    | Resource group | **az104-rg4** |
    | Nom | `asg-web` |
    | Région | **USA Est**  |

1. Cliquez sur **Vérifier + créer** puis, après la validation, cliquez sur **Créer**.

### Créer le groupe de sécurité réseau et l’associer au sous-réseau du groupe de sécurité d’application

1. Dans le portail Azure, recherchez et sélectionnez `Network security groups`.

1. Sélectionnez **+ Créer**, puis fournissez les informations sous l’onglet **Informations de base**. 

    | Paramètre | Valeur |
    | -- | -- |
    | Abonnement | *votre abonnement* |
    | Resource group | **az104-rg4** |
    | Nom | `myNSGSecure` |
    | Région | **USA Est**  |

1. Cliquez sur **Vérifier + créer** puis, après la validation, cliquez sur **Créer**.

1. Une fois le groupe de sécurité réseau déployé, cliquez sur **Accéder à la ressource**.

1. Sous **Paramètres**, cliquez sur **Sous-réseaux**, puis sur **Associer**.

    | Paramètre | Valeur |
    | -- | -- |
    | Réseau virtuel | **CoreServicesVnet (az104-rg4)** |
    | Sous-réseau | **SharedServicesSubnet** |

1. Cliquez sur **OK** pour enregistrer l’association.

### Configurer une règle de sécurité entrante pour autoriser le trafic du groupe de sécurité d’application

1. Continuez à travailler avec votre groupe de sécurité réseau. Dans la zone **Paramètres**, sélectionnez **Règles de sécurité de trafic entrant**.

1. Passez en revue les règles de trafic entrant par défaut. Notez que seuls les autres réseaux virtuels et les équilibreurs de charge ont une autorisation d’accès.

1. Sélectionnez **Ajouter**.

1. Dans le panneau **Ajouter une règle de sécurité entrante**, utilisez les informations suivantes pour ajouter une règle de port de trafic entrant. Cette règle autorise le trafic du groupe de sécurité d’application. Quand vous avez terminé, sélectionnez **Ajouter**.

    | Paramètre | Valeur |
    | -- | -- |
    | Source | **Groupe de sécurité d’application** |
    | Groupes de sécurité d’application sources | **asg-web** |
    | Source port ranges |  * |
    | Destination | **Any** |
    | Service | **Personnalisé** (notez vos autres choix) |
    | Plages de ports de destination | **80,443** |
    | Protocol | **TCP** |
    | Action | **Autoriser** |
    | Priorité | **100** |
    | Nom | `AllowASG` |

### Configurer une règle de groupe de sécurité réseau de trafic sortant qui refuse l’accès à Internet

1. Après avoir créé votre règle de groupe de sécurité réseau de trafic entrant, sélectionnez **Règles de sécurité de trafic sortant**es. 

1. Notez la règle **AllowInternetOutboundRule**. Notez également que la règle ne peut pas être supprimée et que la priorité est 65001.

1. Sélectionnez **+ Ajouter**, puis configurez une règle de trafic sortant qui refuse l’accès à Internet. Quand vous avez terminé, sélectionnez **Ajouter**.

    | Paramètre | Valeur |
    | -- | -- |
    | Source | **Any** |
    | Source port ranges |  * |
    | Destination | **Balise du service** |
    | Identification de destination | **Internet** |
    | Service | **Personnalisée** |
    | Plages de ports de destination | **8080** |
    | Protocol | **Any** |
    | Action | **Deny** |
    | Priority | **4096** |
    | Nom | **DenyAnyCustom8080Outbound** |


## Tâche 4 : Configurer des zones Azure DNS publiques et privées

Dans cette tâche, vous allez créer et configurer des zones DNS publiques et privées. 

### Configurer une zone DNS publique

Vous pouvez configurer Azure DNS pour résoudre les noms d’hôtes dans votre domaine public. Par exemple, si vous avez acheté le nom de domaine contoso.xyz auprès d’un bureau d’enregistrement de noms de domaine, vous pouvez configurer Azure DNS pour héberger le domaine `contoso.com` et résoudre www.contoso.xyz en l’adresse IP de votre serveur web ou de votre application web.

1. Dans le portail, recherchez et sélectionnez `DNS zones`.

1. Sélectionnez **+ Créer**.

1. Configurez l’onglet **Informations de base**.

    | Propriété | Valeur    |
    |:---------|:---------|
    | Abonnement | **Sélectionnez votre abonnement** |
    | Resource group | **az04-rg4** |
    | Nom | `contoso.com` (s’il est réservé, adaptez le nom) |
    | Région |**USA Est** (passez en revue l’icône d’informations) |

1. Sélectionnez **Vérifier + créer**, puis **Créer**.
   
1. Attendez que la zone DNS soit déployée, puis sélectionnez **Accéder à la ressource**.

1. Dans le panneau **Vue d’ensemble**, notez les noms des quatre serveurs de noms Azure DNS affectés à la zone. **Copiez** une des adresses de serveur de noms. Vous en aurez besoin à une étape ultérieure. 
  
1. Sélectionnez **+ Jeu d’enregistrements**. Vous ajoutez un enregistrement de liaison de réseau virtuel pour chaque réseau virtuel qui nécessite la prise en charge de la résolution de noms privés.

    | Propriété | Valeur    |
    |:---------|:---------|
    | Nom | **www** |
    | Type | **A** |
    | TTL | **1** |
    | Adresse IP | **10.1.1.4** |

>**Remarque :**  Dans un scénario réel, vous allez entrer l’adresse IP publique de votre serveur web.

1. Sélectionnez **OK**, puis vérifiez que **contoso.com** a un jeu d’enregistrements A nommé **www**.

1. Ouvrez une invite de commandes et exécutez la commande suivante :

   ```sh
   nslookup www.contoso.com <name server name>
   ```
1. Vérifiez que le nom d’hôte www.contoso.com est résolu en l’adresse IP que vous avez fournie. Cela confirme que la résolution de noms fonctionne correctement.

### Configurer une zone DNS privée

Une zone DNS privée fournit des services de résolution de noms au sein de réseaux virtuels. Une zone DNS privée est accessible seulement depuis les réseaux virtuels auxquels elle est liée et n’est pas accessible depuis Internet. 

1. Dans le portail, recherchez et sélectionnez `Private dns zones`.

1. Sélectionnez **+ Créer**.

1. Dans l’onglet **De base** de Créer une zone DNS privée, entrez les informations indiquées dans le tableau ci-dessous :

    | Propriété | Valeur    |
    |:---------|:---------|
    | Abonnement | **Sélectionnez votre abonnement** |
    | Resource group | **az04-rg4** |
    | Nom | `private.contoso.com` (adaptez si vous avez dû renommer) |
    | Région |**USA Est** |

1. Sélectionnez **Vérifier + créer**, puis **Créer**.
   
1. Attendez que la zone DNS soit déployée, puis sélectionnez **Accéder à la ressource**.

1. Notez que dans le panneau **Vue d’ensemble**, aucun enregistrement de serveur de noms n’est présent. 

1. Sélectionnez **+ Liaisons de réseau virtuel**, puis **+ Ajouter**. 

    | Propriété | Valeur    |
    |:---------|:---------|
    | Nom de la liaison | `manufacturing-link` |
    | Réseau virtuel | `ManufacturingVnet` |

1. Sélectionnez **OK** et attendez que la liaison soit créée. 

1. Dans le panneau **Vue d’ensemble**, sélectionnez **+ Jeu d’enregistrements**. Vous allez maintenant ajouter un enregistrement pour chaque machine virtuelle qui a besoin de la prise en charge de la résolution de noms privés.

    | Propriété | Valeur    |
    |:---------|:---------|
    | Nom | **sensorvm** |
    | Type | **A** |
    | TTL | **1** |
    | Adresse IP | **10.1.1.4** |

 >**Remarque :**  Dans un scénario réel, vous allez entrer l’adresse IP d’une machine virtuelle spécifique liée à la fabrication.

## Nettoyage de vos ressources

Si vous travaillez avec **votre propre abonnement**, prenez un moment pour supprimer les ressources du labo. Ceci garantit que les ressources sont libérées et que les coûts sont réduits. Le moyen le plus simple de supprimer les ressources du labo est de supprimer le groupe de ressources du labo. 

+ Dans le Portail Azure, sélectionnez le groupe de ressources, **Supprimer le groupe de ressources**, **Entrer le nom du groupe de ressources**, puis cliquez sur **Supprimer**.
+ `Remove-AzResourceGroup -Name resourceGroupName` en utilisant Azure PowerShell.
+ `az group delete --name resourceGroupName` en utilisant l’interface CLI.
 
## Points clés

Félicitations, vous avez terminé le labo. Voici les principaux points à retenir pour ce labo. 

+ Un réseau virtuel est une représentation de votre propre réseau dans le cloud. 
+ Lors de la conception de réseaux virtuels, c’est une bonne pratique que d’éviter le chevauchement des plages d’adresses IP. Ceci va réduire les problèmes et simplifie leur résolution.
+ Un sous-réseau est une plage d’adresses IP dans votre réseau virtuel. Vous pouvez diviser un réseau virtuel en plusieurs sous-réseaux pour plus de sécurité et une meilleure organisation.
+ Un groupe de sécurité réseau contient des règles de sécurité qui autorisent ou interdisent le trafic réseau. Il existe des règles entrantes et sortantes par défaut, que vous pouvez personnaliser en fonction de vos besoins.
+ Les groupes de sécurité d’application sont utilisés pour protéger des groupes de serveurs ayant une fonction commune, comme des serveurs web ou des serveurs de base de données.
+ Azure DNS est un service d’hébergement pour les domaines DNS, qui fournit la résolution des noms. Vous pouvez configurer Azure DNS pour résoudre les noms d’hôtes dans votre domaine public.  Vous pouvez aussi utiliser des zones DNS privées pour affecter des noms DNS à des machines virtuelles dans vos réseaux virtuels Azure.

## En savoir plus grâce à l’apprentissage auto-rythmé

+ [Présentation des réseaux virtuels Azure](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/). Concevoir et implémenter une infrastructure Azure Networking de base, comme des réseaux virtuels, des adresses IP publiques et privées, DNS, l’appairage de réseaux virtuels, le routage et la traduction NAT de réseau virtuel Azure.
+ [Concevoir un schéma d’adressage IP](https://learn.microsoft.com/training/modules/design-ip-addressing-for-azure/). Identifier les fonctionnalités d’adressage IP privé et public des réseaux virtuels Azure et locaux.
+ [Sécuriser et isoler l’accès aux ressources Azure en utilisant des groupes de sécurité réseau et des points de terminaison de service](https://learn.microsoft.com/training/modules/secure-and-isolate-with-nsg-and-service-endpoints/). Les points de terminaison de service et les groupes de sécurité réseau vous permettent de protéger vos machines virtuelles et services Azure contre tout accès réseau non autorisé.
+ [Héberger votre domaine sur Azure DNS](https://learn.microsoft.com/training/modules/host-domain-azure-dns/). Créez une zone DNS pour votre nom de domaine. Créez des enregistrements DNS pour mapper le domaine à une adresse IP. Testez si la résolution du nom de domaine aboutit à votre serveur web.
  
