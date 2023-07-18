---
demo:
  title: "Démonstration 03\_: Administrer les ressources Azure"
  module: Administer Administer Azure Resources
---
# 03 - Administrer les ressources Azure

## Démonstration -- Portail Azure

Dans cette démonstration, nous allons explorer le portail Azure.

**Informations de référence** : [Gérer les paramètres et les préférences du portail Azure](https://docs.microsoft.com/azure/azure-portal/set-preferences)

**Informations de référence** : [Créer un tableau de bord dans le portail Azure](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards)

**Informations de référence** : [Comment créer une demande de support Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)

1. Accédez au portail Azure.

1. Sélectionnez l’icône **Support et Résolution des problèmes** dans la bannière du haut. Passez en revue les liens **Ressources de support**. 

1. Sélectionnez l’icône **Paramètres** dans la bannière supérieure.  Passez en revue les paramètres **Apparence + Vues de démarrage**. 

1. Utilisez le menu de gauche et sélectionnez **Tableau de bord**. **Modifiez** le tableau de bord en utilisant la **Galerie de vignettes**. Expliquez les options de personnalisation. 

1. Demandez aux étudiants s’ils ont des questions sur l’utilisation du portail Azure. 

## Démonstration -- Cloud Shell

Dans cette démonstration, nous allons expérimenter avec Cloud Shell.

**Informations de référence** : [Démarrage rapide pour Azure Cloud Shell](https://learn.microsoft.com/en-us/azure/cloud-shell/quickstart?tabs=azurecli)

**Configurer Cloud Shell**

1.  Accédez au **portail Azure**.

1.  Cliquez sur l’icône **Cloud Shell** sur la bannière du haut.

1.  Dans la page Bienvenue dans l’interpréteur de commandes, notez vos sélections pour Bash ou PowerShell.  Sélectionnez **PowerShell**.

1.  Expliquez pourquoi Azure Cloud Shell nécessite un partage de fichiers Azure pour conserver les fichiers. Si nécessaire, configurez le partage du stockage. 

**Expérimenter Azure PowerShell et Bash**

1. Vérifiez que le shell **PowerShell** est sélectionné et essayez quelques commandes. Par exemple, **Get-AzSubscription** et **Get-AzResourceGroup**.

1. Montrez le fonctionnement de l’auto-complétion. Montrez comment effacer l’écran avec **cls**. 

1. Vérifiez que le shell **Bash** est sélectionné et essayez quelques commandes. Par exemple, **az account list** et **az resource list**.

1. Demandez aux étudiants s’ils ont des questions sur l’utilisation des commandes PowerShell ou Bash. 

**Expérimenter l’éditeur Cloud Shell (facultatif)**

1. Pour utiliser l’éditeur cloud, sélectionnez l’icône avec des **accolades**.

1. Sélectionnez un fichier dans le volet de navigation gauche.  Par exemple, **.profile**.

1. Notez sur la bannière supérieure de l’éditeur les sélections pour Paramètres (taille de texte et police) et Charger/Télécharger les fichiers.

1. Notez les points de suspension ( **\...** ) à l’extrême droite pour **Enregistrer**, **Fermer l’éditeur** et **Ouvrir le fichier**.

1. Expérimentez en fonction du temps disponible, puis **fermez** l’éditeur cloud.

1. Fermez Cloud Shell.

## Démonstration - Modèles de démarrage rapide

Dans cette démonstration, nous allons explorer les modèles de démarrage rapide.

**Informations de référence** : [Tutoriel - Créer et déployer un modèle - Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell)

1. Commencez par accéder à la  [galerie Modèles de démarrage rapide Azure](https://learn.microsoft.com/en-us/samples/browse/?expanded=azure&products=azure-resource-manager). Notez qu’il existe des exemples JSON et Bicep. 

1. Demandez aux étudiants si des modèles spécifiques les intéressent. Si ce n’est pas le cas, sélectionnez un modèle. Par exemple, le modèle [Déployer une machine virtuelle Windows simple avec des étiquettes](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/vm-tags/).

1. Expliquez comment le bouton **Déployer dans Azure** vous permet de déployer le modèle directement via le portail Azure.

1. **Déployez** le modèle JSON, et expliquez comment vous pouvez modifier le modèle et le fichier de paramètres. Passez en revue l’objectif des fichiers. Quand vous en avez le temps, passez en revue la syntaxe. 

1. Revenez à la galerie d’exemples de code et recherchez un modèle Bicep. Par exemple, [Créer un compte de stockage standard](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/storage-account-create/). 

1. **Déployez** le modèle Bicep, et expliquez comment vous pouvez modifier le modèle et le fichier de paramètres. Quand vous en avez le temps, passez en revue la syntaxe. 
