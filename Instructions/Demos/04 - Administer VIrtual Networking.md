---
demo:
  title: "Démonstration 04\_: Administrer les réseaux virtuels"
  module: Administer Virtual Networking
---

# 04 - Administrer les réseaux virtuels

## Configurer des réseaux virtuels

Dans cette démonstration, vous allez créer des réseaux virtuels.

**Informations de référence** : [Démarrage rapide : Créer un réseau virtuel - Portail Azure](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## Créer un réseau virtuel dans le portail

1.  Connectez-vous au portail Azure et recherchez  **Réseaux virtuels**.

1.  Créez un réseau virtuel, en expliquant au fur et à mesure les paramètres de base. Veillez à créer au moins un sous-réseau. 

1.  Vérifiez que votre réseau virtuel a été créé.

## Configurer des groupes de sécurité réseau

Dans cette démonstration, vous allez explorer les groupes de sécurité réseau et les points de terminaison de service.

**Informations de référence** : [Restreindre l’accès aux ressources PaaS - Tutoriel - Portail Azure](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)

**Créer un groupe de sécurité réseau**

1. Accédez au portail Azure.

1. Recherchez et sélectionnez  **Groupes de sécurité réseau**.

1. Créez un groupe de sécurité réseau en expliquant les paramètres au fur et à mesure. 
 
1. Attendez que le nouveau NSG soit déployé.

**Explorer les règles de trafic entrant et sortant**

1. Sélectionnez votre nouveau NSG.

1. Expliquez comment le groupe de sécurité réseau peut être associé à des sous-réseaux ou à des interfaces réseau.

1. Expliquez l’objectif des règles de trafic entrant et sortant.  

1. Expliquez les règles de trafic entrant et sortant par défaut. 

1. Créez une nouvelle règle, en expliquant les paramètres au fur et à mesure. Expliquez plus particulièrement la sélection du service (comme HTTPS) et les paramètres de priorité. 

## Configurer Azure DNS

Dans cette démonstration, vous allez explorer Azure DNS.

**Informations de référence** : [Tutoriel : Héberger votre domaine et votre sous-domaine - Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)


**Création d’une zone DNS**

1. Accédez au portail Azure.

1. Recherchez le service **Zones DNS**.

1. Créez une **zone DNS** et expliquez l’objectif de la zone. Pour le nom, vous pouvez utiliser contoso.internal.com.

1.  Attendez que la zone DNS soit créée. Il peut être nécessaire  **d’actualiser** la page.

**Ajouter un jeu d’enregistrements DNS**

**Informations de référence** : [Tutoriel : Créer un enregistrement d’alias pour référencer un enregistrement de ressource de zone](https://learn.microsoft.com/azure/dns/tutorial-alias-rr)

1. Une fois votre zone créée, sélectionnez **+Jeu d’enregistrements**.

1. Utilisez la liste déroulante **Type** pour voir les différents types d’enregistrements. Expliquez comment les différents types d’enregistrements sont utilisés. Notez que les informations de l’enregistrement changent à mesure que vous sélectionnez différents types d’enregistrements.

1. Créez un enregistrement **A** à titre d’exemple. 

