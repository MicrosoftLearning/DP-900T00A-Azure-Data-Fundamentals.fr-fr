---
lab:
  title: Explorer Azure Synapse Data Explorer
  module: Explore fundamentals of real-time analytics
---

# <a name="explore-azure-synapse-data-explorer"></a>Explorer Azure Synapse Data Explorer

Dans cet exercice, vous allez utiliser Azure Synapse Data Explorer pour analyser les données de séries chronologiques.

Ce labo prend environ **25** minutes.

## <a name="before-you-start"></a>Avant de commencer

Vous avez besoin d’un [abonnement Azure](https://azure.microsoft.com/free) dans lequel vous avez un accès administratif.

## <a name="provision-a-synapse-analytics-workspace"></a>Approvisionner un espace de travail Synapse Analytics

> **Conseil** : Si vous avez déjà un espace de travail Azure Synapse de l’exercice précédent, ignorez cette section et passez directement à la section **[Créer un pool Data Explorer](#create-a-data-explorer-pool)** .

1. Ouvrez le portail Azure à l’adresse [https://portal.azure/com](https://portal.azure.com?azure-portal=true) et connectez-vous avec les informations d'identification associées à votre abonnement Azure.

    > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Ensure you are working in the directory containing your subscription - indicated at the top right under your user ID. If not, select the user icon and switch directory.

1. Dans le portail Azure, sur la **page d'accueil**, utilisez l’icône **&#65291; Créer une ressource** pour créer une ressource.
1. Recherchez *Azure Synapse Analytics* et créez une ressource **Azure Synapse Analytics** avec les paramètres suivants :
    - **Abonnement** : *votre abonnement Azure*
        - **Groupe de ressources** : *Créez un groupe de ressources avec un nom approprié, comme « synapse-rg »*
        - **Groupe de ressources géré** : *Entrez un nom approprié, par exemple « synapse-Managed-rg »*.
    - **Nom de l’espace de travail** : *Entrez un nom d’espace de travail unique, par exemple, « synapse-ws-<votre_nom>*.
    - **Région** : *Sélectionnez une région disponible*.
    - **Sélectionner Data Lake Storage Gen 2** : À partir de l’abonnement
        - **Nom du compte** : *Créez un nouveau compte avec un nom unique, par exemple « datalake<votre_nom> »*.
        - **Nom du système de fichiers** : *Créez un nouveau système de fichiers avec un nom unique, par exemple « fs<votre_nom> »*.

    > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: A Synapse Analytics workspace requires two resource groups in your Azure subscription; one for resources you explicitly create, and another for managed resources used by the service. It also requires a Data Lake storage account in which to store data, scripts, and other artifacts.

1. Une fois ces détails entrés, sélectionnez **Vérifier + créer**, puis **Créer** pour créer l’espace de travail.
1. Patientez pendant la création de l’espace de travail. Cette opération peut prendre environ cinq minutes.
1. Une fois le déploiement terminé, accédez au groupe de ressources créé et notez qu’il contient votre espace de travail Synapse Analytics et un compte de stockage Data Lake.
1. Sélectionnez votre espace de travail Synapse et, dans sa page **Vue d’ensemble**, dans la carte **Ouvrir Synapse Studio**, sélectionnez **Ouvrir** pour ouvrir Synapse Studio dans un nouvel onglet de navigateur. Synapse Studio est une interface web que vous pouvez utiliser pour travailler avec votre espace de travail Synapse Analytics.
1. Sur le côté gauche de Synapse Studio, utilisez l’icône **&rsaquo;&rsaquo;** pour développer le menu. Cela permet d’afficher les différentes pages de Synapse Studio qui vous permettront de gérer les ressources et d’effectuer des tâches d’analytique de données

## <a name="create-a-data-explorer-pool"></a>Créer un pool Data Explorer

1. Dans Synapse Studio, sélectionnez la page **Gérer**.
1. Sélectionnez l’onglet **Pools Data Explorer**, puis utilisez l’icône **&#65291; Nouveau** pour créer un nouveau pool avec les paramètres suivants :
    - **Nom du pool Data Explorer** : dxpool
    - **Charge de travail** : optimisé pour le calcul
    - **Taille** : Très petite (2 cœurs)
1. Sélectionnez **Suivant : Paramètres supplémentaires >** et activez le paramètre **Ingestion de streaming**. Cela permet à Data Explorer d’ingérer de nouvelles données à partir d’une source de streaming comme Azure Event Hubs.
1. Sélectionnez **Examiner et créer** pour créer le pool Data Explorer, puis attendez qu’il soit déployé (ce qui peut prendre 15 minutes ou plus. L’état passera de *En cours de création* à *En ligne*).

## <a name="create-a-database-and-ingest-data"></a>Créer une base de données et ingérer des données

1. Dans Synapse Studio, sélectionnez la page **Données**.
1. Assurez-vous que l’onglet **Espace de travail** est sélectionné et, si nécessaire, sélectionnez l’icône **&#8635;** en haut à gauche de la page pour actualiser la vue et lister les **bases de données Data Explorer**.
1. Développez les **bases de données Data Explorer** et vérifiez que **dxpool** est répertorié.
1. Dans le volet **Données**, utilisez l’icône **&#65291;** pour créer une nouvelle **base de données Data Explorer** dans le pool **dxpool** nommé **iot-data**.
1. En attendant que la base de données soit créée, téléchargez le fichier **devices.csv** à partir de [https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/streaming/data/devices.csv](https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/streaming/data/devices.csv?azure-portal=true), et enregistrez-le dans le dossier de votre choix sur votre ordinateur local.
1. Dans Synapse Studio, attendez la création de la base de données si nécessaire, puis dans le menu **...** de la nouvelle base de données **iot-data**, sélectionnez **Ouvrir dans Azure Data Explorer**.
1. Dans le nouvel onglet de navigateur contenant Azure Data Explorer sous l’onglet **Données**, sélectionnez **Ingérer les nouvelles données**.
1. Dans la page **Destination**, sélectionnez les paramètres suivants :
    - **Cluster** : *le pool Data Explorer **dxpool** dans votre espace de travail Azure Synapse*
    - **Base de données** : iot-data
    - **Table** : créer une nouvelle table nommée **appareils**
1. Sélectionnez **Suivant : source** et dans la page **Source** , sélectionnez les options suivantes :
    - **Type de source** : fichier
    - **Fichiers** : charger le fichier **devices.csv** à partir de votre ordinateur local.
1. Sélectionnez **Suivant : schéma**, puis dans la page **Schéma**, vérifiez que les paramètres suivants sont corrects :
    - **Type de compression** : non compressé
    - **Format de données** : CSV
    - **Ignorer le premier enregistrement** : sélectionné
    - **Mappage** : devices_mapping
1. Ensure the column data types have been correctly identified as <bpt id="p1">*</bpt>Time (datetime)<ept id="p1">*</ept>, <bpt id="p2">*</bpt>Device (string)<ept id="p2">*</ept>, and <bpt id="p3">*</bpt>Value (long)<ept id="p3">*</ept>). Then select <bpt id="p1">**</bpt>Next: Start Ingestion<ept id="p1">**</ept>.
1. Une fois l’ingestion terminée, sélectionnez **Fermer**.
1. Dans Azure Data Explorer, sous l’onglet **Requête**, assurez-vous que la base de données **iot-data** est sélectionnée, puis, dans le volet requête, entrez la requête suivante.

    ```kusto
    devices
    ```

1. Dans la barre d’outils, sélectionnez **&#9655; Exécuter** pour exécuter la requête, et examinez les résultats, qui doivent ressembler à ceci :

    | Temps | Appareil | Valeur |
    | --- | --- | --- |
    | 2022-01-01T00:00:00Z | Dev1 | 7 |
    | 2022-01-01T00:00:01Z | Dev2 | 4 |
    | ... | ... | ... |

    Si vos résultats correspondent, vous avez correctement créé la table **appareils** à partir des données du fichier.

    > <bpt id="p1">**</bpt>Tip<ept id="p1">**</ept>: In this example, you imported a very small amount of batch data from a file, which is fine for the purposes of this exercise. In reality, you can use Data Explorer to analyze much larger volumes of data; and since you enabled stream ingestion, you could also have configured Data Explorer to ingest data into the table from a streaming source such as Azure Event Hubs.

## <a name="use-kusto-query-language-to-query-the-table-in-synapse-studio"></a>Utiliser le langage de requête Kusto pour interroger la table dans Synapse Studio

1. Fermez l’onglet du navigateur Azure Data Explorer et revenez à l’onglet contenant Synapse Studio.
1. On the <bpt id="p1">**</bpt>Data<ept id="p1">**</ept> page, expand the <bpt id="p2">**</bpt>iot-data<ept id="p2">**</ept> database and its <bpt id="p3">**</bpt>Tables<ept id="p3">**</ept> folder. Then in the <bpt id="p1">**</bpt>...<ept id="p1">**</ept> menu for the <bpt id="p2">**</bpt>devices<ept id="p2">**</ept> table, select <bpt id="p3">**</bpt>New KQL Script<ept id="p3">**</ept><ph id="ph1"> &gt; </ph><bpt id="p4">**</bpt>Take 1000 rows<ept id="p4">**</ept>.
1. Review the generated query and its results. The query should contain the following code:

    ```kusto
    devices
    | take 1000
    ```

    Les résultats de la requête contiennent les 1 000 premières lignes de données.

1. Modifiez la requête de la manière suivante :

    ```kusto
    devices
    | where Device == 'Dev1'
    ```

1. Select <bpt id="p1">**</bpt>&amp;#9655; Run<ept id="p1">**</ept> to run the query. Then review the results, which should contain only the rows for the <bpt id="p1">*</bpt>Dev1<ept id="p1">*</ept> device.

1. Modifiez la requête de la manière suivante :

    ```kusto
    devices
    | where Device == 'Dev1'
    | where Time > datetime(2022-01-07)
    ```

1. Exécutez la requête et consultez les résultats, qui doivent contenir uniquement les lignes de l’appareil *Dev1* ultérieures au 7 janvier 2022.

1. Modifiez la requête de la manière suivante :

    ```kusto
    devices
    | where Time between (datetime(2022-01-01 00:00:00) .. datetime(2022-07-01 23:59:59))
    | summarize AvgVal = avg(Value) by Device
    | sort by Device asc
    ```

1. Exécutez la requête et consultez les résultats, qui doivent contenir la valeur moyenne de l’appareil enregistrée entre le 1er janvier et le 7 janvier 2022 dans l’ordre croissant du nom des appareils.

1. Fermez l’onglet requête KQL, en ignorant vos modifications.

## <a name="delete-azure-resources"></a>Supprimer les ressources Azure

Maintenant que vous avez terminé l’exploration d’Azure Synapse Analytics, vous devez supprimer les ressources que vous avez créées afin d’éviter des coûts Azure inutiles.

1. Fermez l’onglet de navigateur de Synapse Studio, sans enregistrer les modifications, puis revenez au Portail Azure.
1. Dans le portail Azure, dans la page **Accueil**, sélectionnez **Groupes de ressources**.
1. Sélectionnez le groupe de ressources pour votre espace de travail Synapse Analytics (et non le groupe de ressources managées) et vérifiez qu’il contient l’espace de travail Synapse, le compte de stockage et le pool Data Explorer pour votre espace de travail (si vous avez effectué l’exercice précédent, il contiendra également un pool Spark).
1. Au sommet de la page **Vue d’ensemble** de votre groupe de ressources, sélectionnez **Supprimer le groupe de ressources**.
1. Entrez le nom du groupe de ressources pour confirmer que vous souhaitez le supprimer, puis sélectionnez **Supprimer**.

    Après quelques minutes, votre espace de travail Azure Synapse et l’espace de travail managé qui lui est associé seront supprimés.
