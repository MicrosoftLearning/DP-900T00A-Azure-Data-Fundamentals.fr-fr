---
lab:
  title: Explorer Azure Cosmos DB
  module: Explore fundamentals of Azure Cosmos DB
---
# <a name="explore-azure-cosmos-db"></a>Explorer Azure Cosmos DB

Dans cet exercice, vous allez provisionner un compte Azure Cosmos DB dans votre abonnement Azure et explorer les différentes façons dont vous pouvez l’utiliser pour stocker des données non relationnelles.

Ce labo prend environ **15** minutes.

## <a name="before-you-start"></a>Avant de commencer

Vous avez besoin d’un [abonnement Azure](https://azure.microsoft.com/free) dans lequel vous avez un accès administratif.

## <a name="create-a-cosmos-db-account"></a>Créer un compte Cosmos DB

To use Cosmos DB, you must provision a Cosmos DB account in your Azure subscription. In this exercise, you'll provision a Cosmos DB account that uses the core (SQL) API.

1. In the Azure portal, select <bpt id="p1">**</bpt>+ Create a resource<ept id="p1">**</ept> at the top left, and search for <bpt id="p2">*</bpt>Azure Cosmos DB<ept id="p2">*</ept>.  In the results, select <bpt id="p1">**</bpt>Azure Cosmos DB<ept id="p1">**</ept> and select  <bpt id="p2">**</bpt>Create<ept id="p2">**</ept>.
1. Dans la vignette **Core (SQL) - Recommandé**, sélectionnez **Créer**.
1. Entrez les détails suivants, puis sélectionnez **Vérifier + créer** :
    - <bpt id="p1">**</bpt>Subscription<ept id="p1">**</ept>: If you're using a sandbox, select <bpt id="p2">*</bpt>Concierge Subscription<ept id="p2">*</ept>. Otherwise, select your Azure subscription.
    - **Groupe de ressources** : Si vous utilisez un bac à sable, sélectionnez le groupe de ressources existant (qui a un nom comme *learn-xxxx...* ). Sinon, créez un groupe de ressources avec le nom de votre choix.
    - **Nom du compte** : Entrer un nom unique
    - **Emplacement** : choisissez un emplacement recommandé.
    - **Mode de capacité** : Débit provisionné
    - **Appliquer la remise de niveau gratuit** : Sélectionner Appliquer si disponible
    - **Limiter le débit total du compte** : Désélectionné
1. Une fois que la configuration a été validée, sélectionnez **Créer**.
1. Wait for deployment to complete. Then go to the deployed resource.

## <a name="create-a-sample-database"></a>Créer un exemple de base de données

*Tout au long de cette procédure, fermez les conseils affichés dans le portail*.

1. Dans la page de votre nouveau compte Cosmos DB, dans le volet de gauche, sélectionnez **Explorateur de données**.
1. Dans la page **Data Explorer**, sélectionnez **Lancer le démarrage rapide**.
1. Dans l’onglet **Nouveau conteneur**, passez en revue les paramètres prédéfini de l’exemple de base de données, puis sélectionnez **OK**.
1. Observez l’état dans le volet en bas de l’écran jusqu’à ce que la base de données **SampleDB** et le conteneur **SampleContainer** aient été créés (ce qui peut prendre une minute environ).

## <a name="view-and-create-items"></a>Afficher et créer des éléments

1. In the Data Explorer page, expand the <bpt id="p1">**</bpt>SampleDB<ept id="p1">**</ept> database and the <bpt id="p2">**</bpt>SampleContainer<ept id="p2">**</ept> container, and select <bpt id="p3">**</bpt>Items<ept id="p3">**</ept> to see a list of items in the container. The items represent addresses, each with a unique id and other properties.
1. Sélectionnez l’un des éléments de la liste pour voir une représentation JSON des données de l’élément.
1. En haut de la page, sélectionnez **Nouvel élément** pour créer un nouvel élément vide.
1. Modifiez le fichier JSON du nouvel élément comme suit, puis sélectionnez **Enregistrer**.

    ```json
    {
        "address": "1 Any St.",
        "id": "123456789"
    }
    ```

1. Après avoir enregistré le nouvel élément, notez que des propriétés de métadonnées supplémentaires sont ajoutées automatiquement.

## <a name="query-the-database"></a>Interroger la base de données

1. Dans la page **Explorateur de données**, sélectionnez l’icône **Nouvelle requête SQL**.
1. Dans l’éditeur de requête SQL, examinez la requête par défaut (`SELECT * FROM c`) et utilisez le bouton `SELECT * FROM c` pour l’exécuter.
1. Consultez les résultats, qui incluent la représentation JSON complète de tous les éléments.
1. Modifiez la requête de la manière suivante :

    ```sql
    SELECT c.id, c.address
    FROM c
    WHERE CONTAINS(c.address, "Any St.")
    ```

1. Utilisez le bouton **Exécuter la requête** pour exécuter la requête révisée et passer en revue les résultats, ce qui inclut des entités JSON pour tous les éléments avec un champ **address** contenant le texte « Any St ».
1. Fermez l’éditeur de requête SQL, en ignorant vos modifications.

    You've seen how to create and query JSON entities in a Cosmos DB database by using the data explorer interface in the Azure portal. In a real scenario, an application developer would use one of the many programming language specific software development kits (SDKs) to call the core (SQL) API and work with data in the database.

> **Conseil** : Si vous avez fini d’explorer Azure Cosmos DB, vous pouvez supprimer le groupe de ressources que vous avez créé dans cet exercice.
