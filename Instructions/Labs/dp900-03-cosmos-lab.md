---
lab:
  title: Explorer Azure Cosmos DB
  module: Explore fundamentals of Azure Cosmos DB
---
# Explorer Azure Cosmos DB

Dans cet exercice, vous allez provisionner un compte Azure Cosmos DB dans votre abonnement Azure et explorer les différentes façons dont vous pouvez l’utiliser pour stocker des données non relationnelles.

Ce labo prend environ **15** minutes.

## Avant de commencer

Vous avez besoin d’un [abonnement Azure](https://azure.microsoft.com/free) dans lequel vous avez un accès administratif.

## Créer un compte Cosmos DB

Pour utiliser Cosmos DB, vous devez provisionner un compte Cosmos DB dans votre abonnement Azure. Dans cet exercice, vous allez provisionner un compte Cosmos DB qui utilise Azure Cosmos DB for NoSQL.

1. Dans le portail Azure, sélectionnez **+ Créer une ressource** en haut à gauche, puis recherchez *Azure Cosmos DB*.  Dans les résultats, sélectionnez **Azure Cosmos DB**, puis sélectionnez **Créer**.
1. Dans la vignette **Azure Cosmos DB for NoSQL**, sélectionnez **Créer**.
1. Entrez les détails suivants, puis sélectionnez **Vérifier + créer** :
    - **Abonnement** : Si vous utilisez un bac à sable, sélectionnez *Abonnement concierge*. Sinon, sélectionnez votre abonnement Azure.
    - **Groupe de ressources** : Si vous utilisez un bac à sable, sélectionnez le groupe de ressources existant (qui a un nom comme *learn-xxxx...* ). Sinon, créez un groupe de ressources avec le nom de votre choix.
    - **Nom du compte** : Entrer un nom unique
    - **Emplacement** : choisissez un emplacement recommandé.
    - **Mode de capacité** : Débit provisionné
    - **Appliquer la remise de niveau gratuit** : Sélectionner Appliquer si disponible
    - **Limiter le débit total du compte** : Désélectionné
1. Une fois que la configuration a été validée, sélectionnez **Créer**.
1. Attendez la fin du déploiement. Accédez ensuite à la ressource déployée.

## Créer un exemple de base de données

*Tout au long de cette procédure, fermez les conseils affichés dans le portail*.

1. Dans la page de votre nouveau compte Cosmos DB, dans le volet de gauche, sélectionnez **Explorateur de données**.
1. Dans la page **Data Explorer**, sélectionnez **Lancer le démarrage rapide**.
1. Dans l’onglet **Nouveau conteneur**, passez en revue les paramètres prédéfini de l’exemple de base de données, puis sélectionnez **OK**.
1. Observez l’état dans le volet en bas de l’écran jusqu’à ce que la base de données **SampleDB** et le conteneur **SampleContainer** aient été créés (ce qui peut prendre une minute environ).

## Afficher et créer des éléments

1. Dans la page Explorateur de données, développez la base de données **SampleDB** et le conteneur **SampleContainer**, puis sélectionnez **Éléments** pour voir une liste d’éléments dans le conteneur. Les éléments représentent des données de produits, chacune avec un ID unique et d’autres propriétés.
1. Sélectionnez l’un des éléments de la liste pour voir une représentation JSON des données de l’élément.
1. En haut de la page, sélectionnez **Nouvel élément** pour créer un nouvel élément vide.
1. Modifiez le fichier JSON du nouvel élément comme suit, puis sélectionnez **Enregistrer**.

    ```json
    {
        "name": "Road Helmet,45",
        "id": "123456789",
        "categoryID": "123456789",
        "SKU": "AB-1234-56",
        "description": "The product called \"Road Helmet,45\" ",
        "price": 48.74
    }
    ```

1. Après avoir enregistré le nouvel élément, notez que des propriétés de métadonnées supplémentaires sont ajoutées automatiquement.

## Interroger la base de données

1. Dans la page **Explorateur de données**, sélectionnez l’icône **Nouvelle requête SQL**.
1. Dans l’éditeur de requête SQL, examinez la requête par défaut (`SELECT * FROM c`) et utilisez le bouton `SELECT * FROM c` pour l’exécuter.
1. Consultez les résultats, qui incluent la représentation JSON complète de tous les éléments.
1. Modifiez la requête de la manière suivante :

    ```sql
    SELECT *
    FROM c
    WHERE CONTAINS(c.name,"Helmet")
    ```

1. Utilisez le bouton **Exécuter la requête** pour exécuter la requête révisée et passer en revue les résultats, notamment des entités JSON pour tous les éléments avec un champ **name** contenant le texte « Helmet ».
1. Fermez l’éditeur de requête SQL, en ignorant vos modifications.

    Vous avez vu comment créer et interroger des entités JSON dans une base de données Cosmos DB en utilisant l’interface de l’Explorateur de données dans le portail Azure. Dans un scénario réel, un développeur d’applications utilise l’un des nombreux kits SDK spécifiques au langage de programmation pour appeler l’API NoSQL et utiliser les données de la base de données.

> **Conseil** : Si vous avez fini d’explorer Azure Cosmos DB, vous pouvez supprimer le groupe de ressources que vous avez créé dans cet exercice.
