---
demo:
  title: "Démonstration 06\_: Administrer la gestion du trafic réseau"
  module: Administer Network Traffic Management
---


# 06 - Administrer la gestion du trafic réseau

## Configurer Azure Load Balancer

Dans cette démonstration, nous allons apprendre à créer un équilibreur de charge public. 

**Remarque :**  Cette démonstration nécessite un réseau virtuel avec au moins un sous-réseau. 

**Référence** : [Démarrage rapide : Créer un équilibreur de charge public pour équilibrer la charge des machines virtuelles en utilisant le portail Azure](https://learn.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal)

**Afficher la fonctionnalité Aidez-moi à choisir du portail**

1. Accédez au portail Azure.

1. Recherchez et sélectionnez **Équilibrage de charge - Aidez-moi à choisir**.

1. Utilisez l’Assistant pour parcourir les différents scénarios.
   
**Créer un équilibreur de charge**

1. Continuez dans le portail Azure.

1. Recherchez et sélectionnez **Équilibreur de charge**. **Créez** un équilibreur de charge. 

1. Sous l’onglet **Informations de base**, décidez de la **Référence SKU**, du **Type** et du **Niveau**.

1. Sous l’onglet **Configuration d’adresse IP frontale**, décidez d’utiliser une adresse IP publique ou non.

1. Sous l’onglet **Pools de back-ends**, sélectionnez le réseau virtuel avec la plage d’adresses IP.

1. Sous l’onglet **Règles de trafic entrant**, créez une règle d’équilibrage de charge. Décidez des paramètres tels que **Protocole**, **Ports**, **Sondes d’intégrité** et **Persistance de session**. 


## Configurer Azure Application Gateway

Dans cette démonstration, nous allons apprendre à créer une instance Azure Application Gateway. 

**Remarque** : Pour simplifier les choses, créez des réseaux virtuels et des sous-réseaux lorsque vous procédez à la configuration. 

**Référence** : [Démarrage rapide : Diriger le trafic web avec Azure Application Gateway - Portail Azure](https://learn.microsoft.com/azure/application-gateway/quick-create-portal)

**Créer l’instance Azure Application Gateway**

1. Accédez au portail Azure.

1. Recherchez et sélectionnez **Azure Application Gateway**.

1. **Créez** une nouvelle passerelle.

1. Sous l’onglet **Informations de base**, décidez du **Niveau**, de la **Mise à l’échelle automatique** et du **Nombre d’instances**.

1. Sous l’onglet **Front-ends**, décidez des types d’adresses IP.

1. Sous l’onglet **Back-ends**, décidez quand utiliser un pool de back-ends vide.

1. Sous l’onglet **Configuration**, décidez des règles de routage. Comparez avec les règles de l’équilibreur de charge.

1. Expliquez qu’une fois la passerelle créée, vous ajouterez des cibles de back-end et vous les testerez. 
