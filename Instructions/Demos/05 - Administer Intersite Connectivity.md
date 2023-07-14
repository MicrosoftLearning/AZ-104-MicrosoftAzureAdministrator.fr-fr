---
demo:
  title: "Démonstration 05\_: Administrer la connectivité intersite"
  module: Administer Intersite Connectivity
---

# 05 - Administrer la connectivité intersite

## Configurer VNET Peering

**Remarque :**  Pour cette démonstration, vous aurez besoin de deux réseaux virtuels.

**Informations de référence** : [Connecter des réseaux virtuels avec le peering VNet - Tutoriel](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**Configurer le peering de réseaux virtuels sur le premier réseau virtuel**

1. Dans le **portail Azure**, sélectionnez le premier réseau virtuel. Examinez la valeur du peering. 

1. Sous **Paramètres**, sélectionnez **Peerings** et **+ Ajoutez** un nouveau peering.

1. Configurez le peering du deuxième réseau virtuel. Utilisez les icônes d’informations pour passer en revue les différents paramètres. 

1. Une fois le peering terminé, passez en revue **l’état du peering**. 

**Confirmer le peering de réseaux virtuels sur le deuxième réseau virtuel**

1. Dans le **portail Azure**, sélectionnez le second réseau virtuel.

1. Sous **Paramètres**, sélectionnez **Peerings**.

1. Notez qu’un appairage a été créé automatiquement.  Notez que **l’état du peering** est **Connecté**.

1. Dans le labo, les étudiants vont créer un peering et tester la connexion entre les machines virtuelles. 

## Configurer le routage et les points de terminaison réseau

Dans cette démonstration, nous allons découvrir comment créer une table de routage, définir une route personnalisée et associer la route à un sous-réseau.

**Remarque :**  Cette démonstration nécessite un réseau virtuel avec au moins un sous-réseau.

**Informations de référence** : [Router le trafic réseau - Tutoriel - Portail Azure](https://learn.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal#create-a-route-table)

**Créer une table de routage**

1. Quand vous en avez le temps, passez en revue le diagramme du tutoriel. Expliquez pourquoi vous devez créer une route définie par l’utilisateur. 

1. Accédez au portail Azure.

1. Recherchez et sélectionnez **Tables de routage**. Expliquez quand la **propagation des routes de passerelle** doit être utilisée. 

1. Créez une table de routage et expliquez les paramètres inhabituels. 

1. Attendez que la nouvelle table de routage soit déployée.

**Ajouter un itinéraire**

1.  Sélectionnez votre nouvelle table de routage, puis  **Routes**.

1.  Créez une nouvelle **route**. Décrivez les différents **types de tronçons** disponibles. 

1.  Créez la nouvelle route, puis attendez que la ressource soit déployée.
 
**Associer une table de routage à un sous-réseau**

1.  Accédez au sous-réseau que vous souhaitez associer à la table de routage.

1.  Sélectionnez **Table de routage**, puis choisissez votre nouvelle table de routage. 

1.  **Enregistrez** vos modifications.

