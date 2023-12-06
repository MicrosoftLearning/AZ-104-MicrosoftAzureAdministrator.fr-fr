---
demo:
  title: "Démonstration\_02\_: Administrer la gouvernance et la conformité"
  module: Administer Governance and Compliance
---

# 02 - Administrer la gouvernance et la conformité

## Configurer des abonnements

Cette zone n’a pas de démonstration formelle. 

**Informations de référence** : [Créer un abonnement Azure supplémentaire](https://docs.microsoft.com/azure/cost-management-billing/manage/create-subscription)

## Configurer Azure Policy

Dans cette démonstration, nous allons travailler avec des stratégies Azure.

**Informations de référence** : [Tutoriel : Créer des stratégies pour appliquer la conformité - Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Attribution d’une stratégie**

1.  Accédez au portail Azure.

2.  Recherchez et sélectionnez **Stratégie**.

3.  Sélectionnez **Affectations**, puis **Affecter une stratégie**.

5.  Expliquez  **l’étendue**, qui détermine les ressources ou le regroupement de ressources auxquelles l’affectation de stratégie est appliquée.

6.  Sélectionnez les points de suspension de **Définition de stratégie** pour ouvrir la liste des définitions disponibles. Prenez un certain temps pour passer en revue les définitions de stratégie intégrées.

7.  Recherchez et sélectionnez la stratégie **Emplacements autorisés**. Cette stratégie vous permet de restreindre les emplacements que votre organisation peut spécifier lors du déploiement de ressources.

8.  Accédez à l’onglet **Paramètres** et, dans la liste déroulante, sélectionnez un ou plusieurs emplacements autorisés.

9.  Cliquez sur **Vérifier + créer**, puis sur **créer** pour créer la stratégie.

**Créer et attribuer une définition d’initiative**

1.  Revenez à la page Azure Policy et sélectionnez **Définitions** sous Création.

2.  Sélectionnez **Définition d’initiative** en haut de la page.

3.  Fournissez un **Nom** et une **description**.

4.  **Créez une nouvelle**  Catégorie.

5.  Dans le volet droit,  **ajoutez** la stratégie **Emplacements autorisés**.

6.  Ajoutez une stratégie supplémentaire de votre choix.

7.  Sélectionnez **Enregistrer** pour enregistrer vos modifications, puis **Affecter** pour affecter votre définition d’initiative à votre abonnement.

**Vérifier la conformité**

1.  Revenez à la page de service Azure Policy.

2.  Sélectionnez **Conformité**.

3.  Passez en revue l’état de votre stratégie et de votre définition.

**Vérifier les tâches de correction**

1.  Revenez à la page de service Azure Policy.

2.  Sélectionnez **Correction**.

3.  Passez en revue les tâches de correction listées.

4. Quand vous en avez le temps, supprimez la stratégie et l’initiative. 

## Configurer le contrôle d’accès en fonction du rôle (RBAC)

Dans cette démonstration, nous allons découvrir les attributions de rôles.

**Informations de référence** : [Tutoriel : Accorder un accès utilisateur aux ressources Azure en utilisant le portail Azure - RBAC Azure](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)

**Informations de référence** : [Démarrage rapide - Vérifier l’accès d’un utilisateur aux ressources Azure - RBAC Azure](https://docs.microsoft.com/azure/role-based-access-control/check-access)

**Localiser le volet Contrôle d’accès**

1.  Accédez au portail Azure et sélectionnez un groupe de ressources. Notez le groupe de ressources que vous utilisez.

2.  Sélectionnez le volet **Contrôle d’accès (IAM)** .

3.  Ce volet sera disponible pour de nombreuses ressources différentes pour vous permettre de contrôler les autorisations.

**Examiner les autorisations de rôle**

1.  Sélectionnez l’onglet **Rôles** (en haut).

1.  Passez en revue le grand nombre de rôles intégrés qui sont disponibles.

1.  Double-cliquez sur un rôle, puis sélectionnez **Autorisations** (en haut).

1.  Poursuivez l’exploration du rôle jusqu’à voir les actions  **Lecture, Écriture et Suppression** pour ce rôle.

1.  Revenez au volet **Contrôle d’accès (IAM)** .

**Ajouter une attribution de rôle**

1.  Créez un utilisateur ou sélectionnez un utilisateur existant.

1.  Sélectionnez **Ajouter une attribution de rôle**, puis sélectionnez un rôle. Par exemple, *propriétaire*.

1.  Sélectionnez **Vérifier l’accès**.

1.  Passez en revue les autorisations de l’utilisateur.

1.  Notez que vous pouvez **Refuser les affectations**.
