---
lab:
  title: Explorer l’analytique en temps réel dans Microsoft Fabric
  module: Explore real-time analytics in Microsoft Fabric
---

# Explorer l’analytique en temps réel dans Microsoft Fabric

Microsoft Fabric fournit Real-Time Intelligence, qui vous permet créer des solutions analytiques pour les flux de données en temps réel. Dans cet exercice, vous allez utiliser les fonctionnalités de Real-Time Intelligence dans Microsoft Fabric pour ingérer, analyser et visualiser un flux en temps réel de données d’une société de taxis.

Ce labo prend environ **30** minutes.

> **Remarque** : pour effectuer cet exercice, vous avez besoin d’un [locataire Microsoft Fabric](https://learn.microsoft.com/fabric/get-started/fabric-trial).

## Créer un espace de travail

Avant d’utiliser des données dans Fabric, vous devez créer un espace de travail dans un locataire avec la fonctionnalité Fabric activée.

1. Accédez à la [page d’accueil de Microsoft Fabric](https://app.fabric.microsoft.com/home?experience=fabric) sur `https://app.fabric.microsoft.com/home?experience=fabric` dans un navigateur et connectez-vous avec vos informations d’identification Fabric.
1. Dans la barre de menus à gauche, sélectionnez **Espaces de travail** (l’icône ressemble à &#128455;).
1. Créez un espace de travail avec le nom de votre choix et sélectionnez un mode de licence qui inclut la capacité Fabric (*Essai*, *Premium* ou *Fabric*).
1. Lorsque votre nouvel espace de travail s’ouvre, il doit être vide.

    ![Capture d’écran d’un espace de travail vide dans Fabric.](./images/new-workspace.png)

## Créer un flux d’événements

Vous êtes maintenant prêt à rechercher et à ingérer des données en temps réel à partir d’une source de diffusion en continu. Pour ce faire, vous allez commencer dans le hub en temps réel Fabric.

> **Conseil** : la première fois que vous utilisez le hub en temps réel, certains conseils de *prise en main* peuvent s’afficher. Vous pouvez les fermer.

1. Dans la barre de menus de gauche, sélectionnez le **hub en temps réel**.

    Le hub en temps réel offre un moyen simple de rechercher et de gérer des sources de données de streaming.

    ![Capture d’écran du hub en temps réel dans Fabric.](./images/real-time-hub.png)

1. Dans le hub en temps réel, dans la section **Se connecter à**, sélectionnez **Sources de données**.
1. Recherchez l’exemple de source de données **Yellow taxi** et sélectionnez **Se connecter**. Ensuite, dans l’assistant de **connexion**, nommez la source `taxi` et modifiez le nom d’eventstream par défaut pour le remplacer par `taxi-data`. Le flux par défaut associé à ces données sera automatiquement nommé *taxi-data-stream* :

    ![Capture d’écran d’un nouvel eventstream.](./images/name-eventstream.png)

1. Sélectionnez **Suivant** et attendez que la source et l’eventstream soient créés, puis sélectionnez **Ouvrir l’eventstream**. L’eventstream affiche la source **taxi** et **taxi-data-stream** sur le canevas de conception :

   ![Capture d’écran du canevas d’eventstream.](./images/new-taxi-stream.png)

## Créer un eventhouse

L’eventstream ingère les données boursières en temps réel, mais n’en fait rien actuellement. Nous allons créer un eventhouse dans lequel nous pouvons stocker les données capturées dans une table.

1. Sélectionnez **Créer** dans la barre de menus de gauche. Dans la page *Nouveau*, sous la section *Real-Time Intelligence*, sélectionnez **Eventhouse**. Donnez-lui un nom unique de votre choix.

    >**Note** : si l’option **Créer** n’est pas épinglée à la barre latérale, vous devez d’abord sélectionner l’option avec des points de suspension (**...**).

    Fermez toutes les invites ou conseils affichés jusqu’à ce que le nouvel eventhouse vide soit visible :

    ![Capture d’écran d’un nouvel eventhouse](./images/create-eventhouse.png)

1. Dans le volet de gauche, notez que votre eventhouse contient une base de données KQL portant le même nom que l’eventhouse. Vous pouvez créer des tables pour vos données en temps réel dans cette base de données ou créer des bases de données supplémentaires si nécessaire.
1. Sélectionnez la base de données et notez qu’il existe un *ensemble de requêtes* associé. Ce fichier contient des exemples de requêtes KQL que vous pouvez utiliser pour commencer à interroger les tables de votre base de données.

    Toutefois, il n’existe actuellement aucune table à interroger. Nous allons résoudre ce problème en obtenant des données de l’eventstream dans une nouvelle table.

1. Dans la page principale de votre base de données KQL, sélectionnez **Obtenir des données**.
1. Pour la source de données, sélectionnez **Eventstream** > **Evenstream existant**.
1. Dans le volet **Sélectionner ou créer une table de destination**, créez une table nommée `taxi`. Ensuite, dans le volet **Configurer la source de données**, sélectionnez votre espace de travail et l’eventstream **taxi-data**, puis nommez la connexion `taxi-table`.

   ![Capture d’écran de la configuration du chargement d’une table à partir d’un eventstream.](./images/configure-destination.png)

1. Utilisez le bouton **Suivant** pour effectuer les étapes d’inspection des données, puis terminez la configuration. Fermez ensuite la fenêtre de configuration pour afficher votre eventhouse avec la table stock.

   ![Capture d’écran et de l’eventhouse avec une table.](./images/eventhouse-with-table.png)

    La connexion entre le flux et la table a été créée. Vérifions cela dans l’eventstream.

1. Dans la barre de menus de gauche, sélectionnez le hub **en temps réel**, puis affichez la page **Mes flux de données**. Dans le menu **...** du flux **taxi-data-stream**, sélectionnez **Ouvrir l’eventstream**.

    L’eventstream affiche désormais une destination pour le flux :

   ![Capture d’écran d’un eventstream avec une destination.](./images/eventstream-destination.png)

    > **Conseil** : sélectionnez la destination sur le canevas de conception et, si aucun aperçu des données n’est affiché sous celui-ci, sélectionnez **Actualiser**.

    Dans cet exercice, vous avez créé un eventstream très simple qui capture des données en temps réel et les charge dans une table. Dans une solution réelle, vous ajouteriez généralement des transformations pour agréger les données sur des fenêtres temporelles (par exemple, pour capturer le prix moyen de chaque action sur des périodes de cinq minutes).

    Examinons maintenant comment interroger et analyser les données capturées.

## Interroger les données capturées

L’eventstream capture les données des courses des taxis en temps réel et les charge dans une table de votre base de données KQL. Vous pouvez interroger cette table pour afficher les données capturées.

1. Dans la barre de menus de gauche, sélectionnez la base de données de votre eventhouse.
1. Sélectionnez l’*ensemble de requêtes* de votre base de données.
1. Dans le volet de requête, modifiez le premier exemple de requête, comme illustré ici :

    ```kql
    taxi
    | take 100
    ```

1. Sélectionnez le code de requête et exécutez-le pour afficher 100 lignes de données depuis la table.

    ![Capture d’écran d’une requête KQL.](./images/kql-stock-query.png)

1. Passez en revue les résultats, puis modifiez la requête pour afficher le nombre de courses de taxi pour chaque heure.

    ```kql
    taxi
    | summarize PickupCount = count() by bin(todatetime(tpep_pickup_datetime), 1h)
    ```

1. Sélectionnez la requête modifiée et exécutez-la pour afficher les résultats.
1. Attendez quelques secondes et réexécutez-la, en notant que le nombre de courses changent à mesure que de nouvelles données sont ajoutées à la table à partir du flux en temps réel.

## Nettoyer les ressources

Dans cet exercice, vous avez créé un eventhouse, ingéré des données en temps réel à l’aide d’un eventstream, interrogé les données ingérées dans une table de base de données KQL, créé un tableau de bord en temps réel pour visualiser les données en temps réel et configuré une alerte à l’aide de l’activateur.

Si vous avez fini d’explorer l’intelligence en temps réel dans Fabric, vous pouvez supprimer l’espace de travail que vous avez créé pour cet exercice.

1. Dans la barre de gauche, sélectionnez l’icône de votre espace de travail.
2. Dans la barre d’outils, sélectionnez **Paramètres de l’espace de travail**.
3. Dans la section **Général**, sélectionnez **Supprimer cet espace de travail**.
