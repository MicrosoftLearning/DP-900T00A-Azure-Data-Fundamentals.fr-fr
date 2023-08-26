---
lab:
  title: Explorer l’analytique de données dans Microsoft Fabric
  module: Explore fundamentals of large-scale data analytics
---

# Explorer l’analytique de données dans Microsoft Fabric

Dans cet exercice, vous allez explorer l’ingestion et l’analytique de données dans un Lakehouse Microsoft Fabric.

Ce labo prend environ **25** minutes.

> **Remarque** : Vous aurez besoin d’une licence Microsoft Fabric pour effectuer cet exercice. Consultez [Bien démarrer avec Fabric](https://learn.microsoft.com/fabric/get-started/fabric-trial) pour plus d’informations sur l’activation d’une licence d’essai Fabric gratuite. Vous aurez besoin pour cela d’un compte *scolaire* ou *professionnel* Microsoft. Si vous n’en avez pas, vous pouvez vous [inscrire à un essai de Microsoft Office 365 E3 ou version ultérieure](https://www.microsoft.com/microsoft-365/business/compare-more-office-365-for-business-plans).

## Créer un espace de travail

Avant d’utiliser des données dans Fabric, créez un espace de travail avec l’essai gratuit de Fabric activé.

1. Connectez-vous à [Microsoft Fabric](https://app.fabric.microsoft.com) sur `https://app.fabric.microsoft.com`.
2. Dans la barre de menus à gauche, sélectionnez **Espaces de travail** (l’icône ressemble à &#128455;).
3. Créez un nouvel espace de travail avec le nom de votre choix et sélectionnez un mode de licence dans la section **Avancé** qui comprend la capacité Fabric (*Essai*, *Premium* ou *Fabric*).
4. Lorsque votre nouvel espace de travail s’ouvre, il doit être vide.

    ![Capture d’écran d’un espace de travail vide dans Power BI.](./images/new-workspace.png)

## Créer un lakehouse.

Maintenant que vous disposez d’un espace de travail, il est temps de passer à l’expérience *Engineering données* dans le portail et de créer un data lakehouse pour vos fichiers de données.

1. En bas à gauche du portail, passez à l’expérience **Engineering données**.

    ![Capture d’écran du menu du sélecteur d’expérience.](./images/fabric-switcher.png)

    La page d’accueil de l’engineering données comprend des vignettes permettant de créer des ressources d’ingénierie données couramment utilisées.

2. Dans la page d’accueil de l’**Engineering données**, créez un **Lakehouse** avec le nom de votre choix.

    Au bout d’une minute environ, un nouveau lakehouse est créé :

    ![Capture d’écran d’un nouveau lakehouse.](./images/new-lakehouse.png)

3. Affichez le nouveau lakehouse et notez que le volet **Explorateur de lakehouse** à gauche vous permet de parcourir les tables et les fichiers présents dans le lakehouse :
    - Le dossier **Tables** contient des tables que vous pouvez interroger à l’aide de SQL. Les tables d’un lakehouse Microsoft Fabric sont basées sur le format de fichier *Delta Lake* open source, qui est couramment utilisé dans Apache Spark.
    - Le dossier **Fichiers** contient des fichiers de données du stockage OneLake pour le lakehouse qui ne sont pas associés à des tables delta managées. Vous pouvez également créer des *raccourcis* dans ce dossier pour référencer des données qui sont stockées en externe.

    Actuellement, il n’y a pas de tables ou de fichiers dans le lakehouse.

## Réception de données

Un moyen simple d’ingérer des données consiste à utiliser une activité **Copier des données** dans un pipeline afin d’extraire les données d’une source et de les copier dans un fichier dans le lakehouse.

1. Dans la page **Accueil** de votre lakehouse, dans le menu **Obtenir des données**, sélectionnez **Nouveau pipeline de données**, puis créez un pipeline de données nommé **Ingérer des données de ventes**.
1. Dans l’Assistant **Copie de données**, dans la page **Choisir une source de données**, sélectionnez l’exemple de jeu de données **Retail Data Model from Wide World Importers**.

    ![Capture d’écran de la page Choisir une source de données](./images/choose-data-source.png)

1. Sélectionnez **Suivant** et affichez les tables de la source de données dans la page **Se connecter à la source de données**.
1. Sélectionnez la table **dimension_stock_item**, qui contient les données des produits. Sélectionnez ensuite **Suivant** pour accéder à la page **Choisir la destination des données**.
1. Dans la page **Choisir la destination des données**, sélectionnez votre lakehouse existant. Sélectionnez ensuite **Suivant**.
1. Définissez les options de destination des données suivantes, puis sélectionnez **Suivant** :
    - **Dossier racine** : Tables
    - **Paramètres de chargement** : Charger dans la nouvelle table
    - **Nom de la table de destination** : dimension_stock_item
    - **Mappages de colonnes** : *Laisser les mappages par défaut tels qu’ils sont*
    - **Activer la partition** : *Non sélectionné*
1. Dans la page **Vérifier + enregistrer**, vérifiez que l’option **Démarrer immédiatement le transfert de données** est sélectionnée, puis sélectionnez **Enregistrer + exécuter**.

    Un nouveau pipeline contenant une activité **Copier des données** est créé, comme illustré ici :

    ![Capture d’écran d’un pipeline avec une activité Copier des données.](./images/copy-data-pipeline.png)

    Lorsque le pipeline commence à s’exécuter, vous pouvez superviser son statut dans le volet **Sortie** sous le concepteur de pipeline. Utilisez l’icône **&#8635;** (*Actualiser*) pour actualiser le statut et attendez qu’il indique une réussite.

1. Dans la barre de menus du hub à gauche, sélectionnez votre lakehouse.
1. Dans la page **Accueil**, dans le volet **Lakehouse explorer**, développez **Tables** et vérifiez que la table **dimension_stock_item** a été créée.

    > **Remarque** : Si la nouvelle table est répertoriée comme *non identifiée*, utilisez le bouton **Actualiser** dans la barre d’outils du lakehouse pour actualiser la vue.

1. Sélectionnez la table **dimension_stock_item** pour afficher son contenu.

    ![Capture d’écran de la table dimension_stock_item.](./images/dimProduct.png)

## Interroger des données dans un lakehouse

Maintenant que vous avez ingéré des données dans une table dans le lakehouse, vous pouvez utiliser SQL pour les interroger.

1. En haut à droite de la page Lakehouse, basculez sur **Point de terminaison SQL** pour votre lakehouse.

    ![Capture d’écran du menu Point de terminaison SQL.](./images/endpoint-switcher.png)

1. Dans la barre d’outils, sélectionnez **Nouvelle requête SQL**. Entrez ensuite le code SQL suivant dans l’éditeur de requête :

    ```sql
    SELECT Brand, COUNT(StockItemKey) AS Products
    FROM dimension_stock_item
    GROUP BY Brand
    ```

1. Sélectionnez le bouton **&#9655; Exécuter** pour exécuter la requête et passer en revue les résultats, ce qui devrait révéler qu’il existe deux valeurs de marque (*N/A* et *Northwind*) et afficher le nombre de produits de chacune d’elles.

    ![Capture d’écran d’une requête SQL.](./images/sql-query.png)

## Visualiser les données dans un lakehouse

Les lakehouses Microsoft Fabric organisent toutes les tables dans un modèle de données, que vous pouvez utiliser pour créer des visualisations et des rapports.

1. En bas à gauche de la page, sous le volet **Explorateur**, sélectionnez l’onglet **Modèle** pour voir le modèle de données des tables du lakehouse (dans le cas présent, il n’y a qu’une seule table).

    ![Capture d’écran de la page du modèle dans un lakehouse Fabric.](./images/fabric-model.png)

1. Dans la barre d’outils, sélectionnez **Nouveau rapport** pour ouvrir un nouvel onglet de navigateur contenant le concepteur de rapports Power BI.
1. Dans le concepteur de rapports :
    1. Dans le volet **Données**, développez la table **dimension_stock_item** et sélectionnez les champs **Brand** et **StockItemKey**.
    1. Dans le volet **Visualisations**, sélectionnez la visualisation **Graphique à barres empilées** (il s’agit de la première liste). Vérifiez ensuite que l’**Axe Y** contient le champ **Brand** et remplacez l’agrégation dans l’**Axe X** par **Count** afin d’avoir le champ **Count of StockItemKey**. Enfin, redimensionnez la visualisation dans le canevas de rapport pour remplir l’espace disponible.

        ![Capture d’écran d’un rapport Power BI.](./images/fabric-report.png)

    > **Conseil** : Vous pouvez utiliser les icônes **>>** pour masquer les volets du concepteur de rapports afin de voir le rapport plus clairement.

1. Dans le menu **Fichier**, sélectionnez **Enregistrer** pour enregistrer le rapport en tant que **Rapport de quantité de marques** dans votre espace de travail Fabric.

    Maintenant, vous pouvez fermer l’onglet du navigateur contenant le rapport pour revenir à votre lakehouse. Vous trouverez le rapport dans la page de votre espace de travail dans le portail Microsoft Fabric.

## Nettoyer les ressources

Si vous avez terminé d’explorer Microsoft Fabric, vous pouvez supprimer l’espace de travail que vous avez créé pour cet exercice.

1. Dans la barre de gauche, sélectionnez l’icône de votre espace de travail pour afficher tous les éléments qu’il contient.
2. Dans le menu **...** de la barre d’outils, sélectionnez **Paramètres de l’espace de travail**.
3. Dans la section **Autre**, sélectionnez **Supprimer cet espace de travail**.
