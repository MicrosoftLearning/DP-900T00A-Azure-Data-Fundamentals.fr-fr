---
lab:
  title: Explorer Spark Streaming dans Azure Synapse Analytics
  module: Explore fundamentals of real-time analytics
---

# Explorer Spark Streaming dans Azure Synapse Analytics

Dans cet exercice, vous utiliserez *Spark Structured Streaming* et des *tableaux delta* dans Azure Synapse Analytics pour traiter des données de diffusion en continu.

Ce labo prend environ **15** minutes.

## Avant de commencer

Vous avez besoin d’un [abonnement Azure](https://azure.microsoft.com/free) dans lequel vous avez un accès administratif.

## Approvisionner un espace de travail Synapse Analytics

Pour utiliser Synapse Analytics, vous devez approvisionner une ressource d’espace de travail Synapse Analytics dans votre abonnement Azure.

1. Ouvrez le [portail Azure](https://portal.azure.com?azure-portal=true) et connectez-vous avec les informations d’identification associées à votre abonnement Azure.

    >                 **Remarque** : Veillez à travailler dans le répertoire contenant votre abonnement, indiqué en haut à droite sous votre ID d’utilisateur. Si ce n’est pas le cas, sélectionnez l’icône de l’utilisateur et changez d’annuaire.

2. Dans le portail Azure, sur la **page d'accueil**, utilisez l’icône **&#65291; Créer une ressource** pour créer une ressource.
3. Recherchez *Azure Synapse Analytics* et créez une ressource **Azure Synapse Analytics** avec les paramètres suivants :
    - **Abonnement** : *votre abonnement Azure*
        - **Groupe de ressources** : *Créez un groupe de ressources avec un nom approprié, comme « synapse-rg »*
        - **Groupe de ressources géré** : *Entrez un nom approprié, par exemple « synapse-Managed-rg »*.
    - **Nom de l’espace de travail** : *Entrez un nom d’espace de travail unique, par exemple, « synapse-ws-<votre_nom>*.
    - **Région** : *Sélectionnez une région disponible*.
    - **Sélectionner Data Lake Storage Gen 2** : À partir de l’abonnement
        - **Nom du compte** : *Créez un nouveau compte avec un nom unique, par exemple « datalake<votre_nom> »*.
        - **Nom du système de fichiers** : *Créez un nouveau système de fichiers avec un nom unique, par exemple « fs<votre_nom> »*.

    >                 **Remarque** : Un espace de travail Synapse Analytics nécessite deux groupes de ressources dans votre abonnement Azure, à savoir un pour les ressources que vous créez explicitement et un autre pour les ressources managées utilisées par le service. Il nécessite également un compte de stockage Data Lake dans lequel stocker des données, des scripts et d’autres artefacts.

4. Une fois ces détails entrés, sélectionnez **Vérifier + créer**, puis **Créer** pour créer l’espace de travail.
5. Patientez pendant la création de l’espace de travail. Cette opération peut prendre environ cinq minutes.
6. Une fois le déploiement terminé, accédez au groupe de ressources créé et notez qu’il contient votre espace de travail Synapse Analytics et un compte de stockage Data Lake.
7. Sélectionnez votre espace de travail Synapse et, dans sa page **Vue d’ensemble**, dans la carte **Ouvrir Synapse Studio**, sélectionnez **Ouvrir** pour ouvrir Synapse Studio dans un nouvel onglet de navigateur. Synapse Studio est une interface web que vous pouvez utiliser pour travailler avec votre espace de travail Synapse Analytics.
8. Sur le côté gauche de Synapse Studio, utilisez l’icône **&rsaquo;&rsaquo;** pour étendre le menu. Cela révèle les différentes pages de Synapse Studio que vous allez utiliser pour gérer les ressources et effectuer les tâches d’analyse des données, comme illustré ici :

    ![Synapse Studio](images/synapse-studio.png)

## Créer un pool Spark

Pour utiliser Spark pour traiter les données de diffusion en continu, vous devez ajouter un pool Spark à votre espace de travail Azure Synapse.

1. Dans Synapse Studio, sélectionnez la page **Gérer**.
2. Sélectionnez l’onglet **Pools Apache Spark**, puis utilisez l’icône **&#65291; Nouveau** pour créer un pool Spark avec les paramètres suivants :
    - **Nom du pool Apache Spark** : sparkpool
    - **Famille de tailles de nœud** : À mémoire optimisée
    - **Taille de nœud** : Petite (4 vCores/32 Go)
    - **Mise à l’échelle automatique** : Activée
    - **Nombre de nœuds** 3----3
3. Examinez et créez le pool Spark, puis attendez qu’il se déploie (ce qui peut prendre quelques minutes).

## Explorer le traitement par flux

Pour explorer le traitement par flux avec Spark, vous utiliserez un notebook qui contient le code et les notes Python pour vous aider à effectuer un traitement par flux de base avec Spark Structured Streaming et des tableaux delta.

1. Téléchargez le notebook [Structured Streaming and Delta Tables.ipynb](https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/streaming/Spark%20Structured%20Streaming%20and%20Delta%20Tables.ipynb) sur votre ordinateur local (si le notebook est ouvert en tant que fichier texte dans votre navigateur, enregistrez-le dans un dossier local, veillez à l’enregistrer en tant que **Structured Streaming and Delta Tables.ipynb**, pas en tant que fichier .txt)
2. Dans Synapse Studio, sélectionnez la page **Développer**.
3. Dans le menu **&#65291;**, sélectionnez **&#8612; Importer**, et sélectionnez le fichier **Structured Streaming and Delta Tables.ipynb** sur votre ordinateur local.
4. Suivez les instructions du notebook pour l’attacher à votre pool Spark et exécuter les cellules de code qu’il contient afin d’explorer les différentes façons d’utiliser Spark pour le traitement par flux.

## Supprimer les ressources Azure

>                 **Remarque** : Si vous envisagez d’effectuer d’autres exercices qui utilisent Azure Synapse Analytics, vous pouvez ignorer cette section. Sinon, effectuez les étapes ci-dessous pour éviter des coûts Azure inutiles.

1. Fermez l’onglet de navigateur de Synapse Studio, sans enregistrer les modifications, puis revenez au Portail Azure.
1. Dans le portail Azure, dans la page **Accueil**, sélectionnez **Groupes de ressources**.
1. Sélectionnez le groupe de ressources pour votre espace de travail Synapse Analytics (et non le groupe de ressources managées) et vérifiez qu’il contient l’espace de travail Synapse, le compte de stockage et le pool Data Explorer pour votre espace de travail (si vous avez effectué l’exercice précédent, il contiendra également un pool Spark).
1. Au sommet de la page **Vue d’ensemble** de votre groupe de ressources, sélectionnez **Supprimer le groupe de ressources**.
1. Entrez le nom du groupe de ressources pour confirmer que vous souhaitez le supprimer, puis sélectionnez **Supprimer**.

    Après quelques minutes, votre espace de travail Azure Synapse et l’espace de travail managé qui lui est associé seront supprimés.
