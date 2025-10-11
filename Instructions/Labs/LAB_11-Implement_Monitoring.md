---
lab:
  title: 'Labo 11 : Implémenter le monitoring'
  module: Administer Monitoring
---

# Labo 11 : Implémenter la supervision

## Présentation du labo

Dans ce labo, vous allez découvrir Azure Monitor. Vous apprenez à créer une alerte et à l’envoyer dans un groupe d’actions. Vous déclenchez et testez l’alerte et vérifier le journal d’activité.  

Ce labo nécessite un abonnement Azure. Le type de votre abonnement peut affecter la disponibilité des fonctionnalités dans ce labo. Vous pouvez changer la région, mais les étapes sont écrites de façon à utiliser **USA Est**.

## Durée estimée : 40 minutes

## Scénario du labo

Votre organisation a migré son infrastructure dans Azure. Il est important que les Administrateurs soient avertis de toute modification importante de l’infrastructure. Vous envisagez d’examiner les fonctionnalités d’Azure Monitor, y compris Log Analytics.

## Diagramme de l'architecture

![Diagramme des tâches d’architecture](../media/az104-lab11-architecture.png)

## Compétences de tâche

+ Tâche 1 : Utilisez un modèle pour approvisionner une infrastructure.
+ Tâche 2 : Créez une alerte.
+ Tâche 3 : Configurez des notifications de groupe d’actions.
+ Tâche 4 : Déclenchez une alerte et confirmez son fonctionnement.
+ Tâche 5 : Configurez une règle de traitement des alertes.
+ Tâche 6 : Utilisez des requêtes de journal Azure Monitor.

## Tâche 1 : Utilisez un modèle pour approvisionner une infrastructure.

Dans cette tâche, vous allez déployer une machine virtuelle qui sera utilisée pour tester les scénarios de supervision.

1. Téléchargez les fichiers du labo **\\Allfiles\\Lab11\\az104-11-vm-template.json** sur votre ordinateur.

1. Connectez-vous au **portail Azure** - `https://portal.azure.com`.

1. Dans le Portail Azure, recherchez et sélectionnez `Deploy a custom template`.

1. Dans la page du déploiement personnalisé, sélectionnez **Créer votre propre modèle dans l’éditeur**.

1. Sur la page de modification du modèle, sélectionnez **Charger le fichier**.

1. Recherchez et sélectionnez le fichier **\\Allfiles\\Lab11\\az104-11-vm-template.json**, puis sélectionnez **Ouvrir**.

1. Sélectionnez **Enregistrer**.

1. Utilisez les informations suivantes pour remplir les champs du déploiement personnalisé, en laissant tous les autres champs avec leur valeur par défaut :

    | Paramètre       | Valeur         | 
    | ---           | ---           |
    | Abonnement  | Votre abonnement Azure |
    | Resource group| `az104-rg11` (Si nécessaire, sélectionnez **Créer**)
    | Région        | **USA Est**   |
    | Nom d’utilisateur      | `localadmin`   |
    | Mot de passe      | Fournir un mot de passe complexe |
    
1. Sélectionnez **Vérifier + créer**, puis **Créer**.

1. Attendez la fin du déploiement, puis cliquez sur **Accéder au groupe de ressources**.

1. Passez en revue les ressources déployées. Il doit y avoir un réseau virtuel avec une seule machine virtuelle.

**Configurer Azure Monitor pour les machines virtuelles (cette opération sera utilisée dans la dernière tâche)**

1. Dans le portail, recherchez et sélectionnez **Surveiller**.

1. Prenez une minute pour passer en revue tous les outils pour les insights, la détection, le triage et le diagnostic disponibles.

1. Sélectionnez **Affichage** dans la zone **Insights de machine virtuelle**, puis sélectionnez **Configurer les insights**.

1. Sélectionnez **Activer** en regard de votre machine virtuelle, puis **Activer dans le panneau Intégration d’Azure Monitor - Insights**.

1. Prenez les valeurs par défaut pour les règles de collecte de données et l’abonnement, puis sélectionnez **Configurer**. 

1. L’installation et la configuration de l’agent de machine virtuelle prennent quelques minutes. Passez à l’étape suivante. 
   
## Tâche 2 : Créer une alerte

Dans cette tâche, vous créez une alerte relative à la suppression d’une machine virtuelle. 

1. Continuez sur la page **Surveiller**, sélectionnez **Alertes**. 

1. Sélectionnez **+ Créer**, puis **Règle d’alerte**. 

1. Sélectionnez la zone de l’abonnement, puis **Appliquer**. Cette alerte s’applique à toutes les machines virtuelles de l’abonnement. Vous pouvez également spécifier une machine en particulier. 

1. Sélectionnez l’onglet **Condition**, puis le lien **Afficher tous les signaux**.

1. Recherchez et sélectionnez **Supprimer la machine virtuelle (Machines Virtuelles)**. Notez les autres signaux intégrés. Sélectionnez **Appliquer**

1. Dans la zone **Alerte logique ** (faites défiler vers le bas de l’écran), passez en revue les sélections **Niveau de l’événement**. Conservez la valeur **Toute la sélection** par défaut.

1. Passez en revue les sélections **État**. Conservez la valeur **Toute la sélection** par défaut.

1. Laissez le volet **Créer une règle d’alerte** ouvert pour la tâche suivante.

## Tâche 3 : Configurez des notifications de groupe d’actions.

Dans cette tâche, si l’alerte est déclenchée, envoyez une notification par e-mail à l’équipe des opérations. 

1. Continuez à travailler sur votre alerte. Sélectionnez **Utiliser des groupes d’actions**, puis sélectionnez **Créer un groupe d’actions** dans le panneau **Sélectionner un groupe d’actions**.

    >**Le saviez-vous ?** Vous pouvez ajouter jusqu’à cinq groupes d’actions à une règle d’alerte. Les groupes d’actions sont exécutés simultanément, sans ordre spécifique. Plusieurs règles d’alerte peuvent utiliser le même groupe d’actions. 

1. Sous l’onglet **Informations de base**, entrez les valeurs suivantes pour chaque paramètre.

    | Paramètre | Valeur |
    |---------|---------|
    | **Détails du projet** |
    | Abonnement | votre abonnement |
    | Resource group | **az104-rg11** |
    | Région | **Global** (par défaut) |
    | **Détails de l’instance** |
    | Nom du groupe d’actions | `Alert the operations team` (doit être unique dans le groupe de ressources) |
    | Nom d’affichage | `AlertOpsTeam` |

1. Sélectionnez **Suivant : Notifications**, puis entrez les valeurs suivantes pour chaque paramètre.

    | Paramètre | Valeur |
    |---------|---------|
    | Type de notification | Sélectionnez **E-mail/SMS/Push/Voix**. |
    | Nom | `VM was deleted` |

1. Sélectionnez **E-mail**, puis dans la zone **E-mail**, entrez votre adresse de messagerie. Cliquez ensuite sur **OK**. 

    >**Remarque :** Vous devriez recevoir une notification par e-mail indiquant que vous avez été ajouté à un groupe d’actions. Il peut y avoir quelques minutes de retard, mais c’est le signe que la règle a été déployée.

1. Sélectionnez **Vérifier + créer**, puis **Créer**.
   
1. Une fois le groupe d’actions créé, passez à l’onglet **Suivant : Détails**, puis entrez les valeurs suivantes pour chaque paramètre.

    | Paramètre | Valeur |
    |---------|---------|
    | Nom de la règle d’alerte | `VM was deleted` |
    | Description de la règle d'alerte | `A VM in your resource group was deleted` |

1. Sélectionnez **Vérifier + créer** pour valider votre entrée, puis sélectionnez **Créer**.

## Tâche 4 : Déclenchez une alerte et confirmez son fonctionnement.

Dans cette tâche, vous déclenchez l’alerte et confirmez l’envoi d’une notification. 

>**Remarque :** Si vous supprimez la machine virtuelle avant le déploiement de la règle d’alerte, celle-ci risque de ne pas se déclencher. 

1. Dans le portail, recherchez et sélectionnez **Machines virtuelles**.

1. Cochez la case correspondant à la machine virtuelle **az104-vm0**.

1. Sélectionnez **Supprimer** dans la barre de menus.

1. Cochez la case pour **Appliquer la suppression forcée**. Cochez la case en bas confirmant que vous souhaitez supprimer les ressources, puis sélectionnez **Supprimer**. 

1. Dans la barre de titre, sélectionnez l’icône **Notifications** et attendez jusqu’à la suppression correcte de **vm0**.

1. Vous devriez recevoir une notification par e-mail qui indique, **Remarque importante : Azure Monitor alerte que la machine virtuelle a été supprimée a été activée...** Dans le cas contraire, ouvrez votre programme de messagerie et recherchez un e-mail provenant de azure-noreply@microsoft.com.

    ![Capture d’écran de l’e-mail d’alerte.](../media/az104-lab11-alert-email.png)
   
1. Dans le menu de ressources du portail Azure, sélectionnez **Surveiller**, puis **Alertes** dans le menu de gauche.

1. Vous devriez avoir trois alertes détaillées qui ont été générées en supprimant **vm0**.

   >**Remarque :** L’envoi de l’e-mail d’alerte et la mise à jour des alertes dans le portail peut prendre quelques minutes. Si vous ne souhaitez pas attendre, passez à la tâche suivante, puis revenez. 

1. Sélectionnez le nom de l’une des alertes (par exemple, **la machine virtuelle a été supprimée**). Un volet **Détails sur l’alerte** s’affiche pour présenter plus de détails sur l’événement.

## Tâche 5 : Configurez une règle de traitement des alertes.

Dans cette tâche, vous créez une règle d’alerte pour supprimer les notifications pendant une période de maintenance. 

1. Continuez dans le panneau **Alertes**, sélectionnez **Règles de traitement des alertes**, puis **+ Créer**. 
   
1. Sélectionnez votre **abonnement**, puis **Appliquer**.
   
1. Sélectionnez **Suivant : Paramètres de règle**, puis **Supprimer les notifications**.
   
1. Sélectionnez **Suivant : Planification**.
   
1. Par défaut, la règle fonctionne tout le temps, sauf si vous la désactivez ou configurez une planification. Vous allez définir une règle pour supprimer des notifications pendant la maintenance de nuit.
Entrez ces paramètres pour la planification de la règle de traitement des alertes :

    | Paramètre | Valeur |
    |---------|---------|
    | Appliquer la règle | À une heure spécifique |
    | Démarrer | Entrez la date d’aujourd’hui à 22h00. |
    | End | Entrez la date de demain à 7h00. |
    | Fuseau horaire | Sélectionnez le fuseau horaire local. |

    ![Capture d’écran de la section de planification d’une règle de traitement des alertes](../media/az104-lab11-alert-processing-rule-schedule.png)

1. Sélectionnez **Suivant : Détails**, puis entrez ces paramètres :

    | Paramètre | Valeur |
    |---------|---------|
    | Groupe de ressources | **az104-rg11** |
    | Nom de la règle | `Planned Maintenance` |
    | Description | `Suppress notifications during planned maintenance.` |

1. Sélectionnez **Vérifier + créer** pour valider votre entrée, puis sélectionnez **Créer**.

## Tâche 6 : Utilisez des requêtes de journal Azure Monitor.

Dans cette tâche, vous allez utiliser Azure Monitor pour interroger les données capturées à partir de la machine virtuelle.

1. Dans le Portail Azure, recherchez et sélectionnez `Monitor`, puis cliquez sur **Journaux**.

1. Si nécessaire, fermez l’écran de démarrage. 

1. Si nécessaire, sélectionnez une étendue, votre **Abonnement**. Sélectionnez **Appliquer**. 

1. Sous l’onglet **Requêtes**, sélectionnez **Machine virtuelle** (volet de gauche). Vous devrez peut-être rouvrir le panneau.

    ![Capture d’écran de l’onglet Requêtes.](../media/az104-lab11-queries.png)

1. Passez en revue les requêtes disponibles. **Exécutez** (pointez sur la requête) la requête **Compter les pulsations**.

1. Vous devriez recevoir un nombre de pulsations pour le moment où la machine virtuelle était en cours d’exécution.

1. Sur le côté droit de l’écran, sélectionnez la liste déroulante en regard du **mode Simple**. Choisissez le **mode KQL**. Passez en revue la requête. Cette requête utilise la table de *pulsations*.

1. Remplacez la requête par celle-ci, puis cliquez sur **Exécuter**. Passez en revue le graphique obtenu. 

   ```
    InsightsMetrics
    | where TimeGenerated > ago(1h)
    | where Name == "UtilizationPercentage"
    | summarize avg(Val) by bin(TimeGenerated, 5m), Computer //split up by computer
    | render timechart
   ```

    >**Note :** si la requête ne se colle pas correctement, essayez de la coller dans le Bloc-notes, puis copiez-la et collez-la à nouveau dans le champ de requête.

1. Comme vous avez du temps, passez en revue et exécutez d’autres requêtes. 

    >**Le saviez-vous ?** : Si vous souhaitez vous exercer avec d’autres requêtes, il existe un [Environnement de démonstration Log Analytics](https://learn.microsoft.com/azure/azure-monitor/logs/log-analytics-tutorial#open-log-analytics).
    
    >**Le saviez-vous ?** : Une fois que vous trouvez une requête qui vous convient, vous pouvez créer une requête à partir de celle-ci. 

## Nettoyage de vos ressources

Si vous travaillez avec **votre propre abonnement**, prenez un moment pour supprimer les ressources du labo. Ceci garantit que les ressources sont libérées et que les coûts sont réduits. Le moyen le plus simple de supprimer les ressources du labo est de supprimer le groupe de ressources du labo. 

+ Dans le Portail Azure, sélectionnez le groupe de ressources, **Supprimer le groupe de ressources**, **Entrer le nom du groupe de ressources**, puis cliquez sur **Supprimer**.
+ `Remove-AzResourceGroup -Name resourceGroupName` en utilisant Azure PowerShell.
+ `az group delete --name resourceGroupName` en utilisant l’interface CLI.

## Développer votre apprentissage avec Copilot
Copilot peut vous aider à apprendre à utiliser les outils de script Azure. Copilot peut également aider dans des domaines non couverts dans le labo ou quand vous avez besoin de plus d’informations. Ouvrez un navigateur Edge et choisissez Copilot (en haut à droite), ou accédez à *copilot.microsoft.com*. Prenez quelques minutes pour essayer ces invites.

+ Pour quelles étapes de configuration de base faut-il recevoir une alerte dans Azure lorsqu’une machine virtuelle est en panne ?
+ Comment puis-je être averti lorsqu’une alerte Azure est déclenchée ?
+ Créez une requête Azure Monitor pour fournir des informations sur les performances du processeur de machine virtuelle.

## En savoir plus grâce à l’apprentissage auto-rythmé

+ [Améliorez la réponse aux incidents avec des alertes dans Azure](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/). Répondez aux incidents et aux activités de votre infrastructure via les fonctionnalités d’alerte d’Azure Monitor.
+ [Surveillez vos machines virtuelles Azure avec Azure Monitor](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/). Surveillez vos machines virtuelles Azure en utilisant Azure Monitor pour collecter et analyser des journaux et métriques de client et d’hôte de la machine virtuelle.

## Points clés

Félicitations, vous avez terminé le labo. Voici les principaux points à retenir de ce labo. 

+ Les alertes vous permettent de détecter et de résoudre des problèmes avant que les utilisateurs ne les remarquent dans votre infrastructure ou votre application.
+ Vous pouvez définir une alerte sur n’importe quelle source de données de métrique ou de journal dans la plateforme de données Azure Monitor.
+ Une règle d’alerte supervise vos données et capture un signal indiquant un évènement sur la ressource spécifiée.
+ Une alerte est déclenchée si les conditions de la règle d’alerte sont remplies. Plusieurs actions (e-mail, SMS, envoi (push), voix) peuvent être déclenchées.
+ Les groupes d’actions incluent des personnes qui doivent être averties d’une alerte.
