---
lab:
  title: Explorer l’analytique de données dans Azure avec Azure Synapse Analytics
  module: Explore fundamentals of large-scale data warehousing
---

# <a name="explore-data-analytics-in-azure-with-azure-synapse-analytics"></a>Explorer l’analytique de données dans Azure avec Azure Synapse Analytics

Dans cet exercice, vous allez provisionner un espace de travail Azure Synapse Analytics dans votre abonnement Azure et l’utiliser pour ingérer et interroger des données.

Ce labo prend environ **30** minutes.

## <a name="before-you-start"></a>Avant de commencer

Vous avez besoin d’un [abonnement Azure](https://azure.microsoft.com/free) dans lequel vous avez un accès administratif.

## <a name="provision-an-azure-synapse-analytics-workspace"></a>Provisionner un espace de travail Azure Synapse Analytics

Pour utiliser Azure Synapse Analytics, vous devez provisionner une ressource d’espace de travail Azure Synapse Analytics dans votre abonnement Azure.

1. Ouvrez le portail Azure à l’adresse [https://portal.azure.com](https://portal.azure.com?azure-portal=true) et connectez-vous avec les informations d'identification associées à votre abonnement Azure.

    > <bpt id="p1">**</bpt>Tip<ept id="p1">**</ept>:  Ensure you are working in the directory containing your subscription - indicated at the top right under your user ID. If not, select the user icon and switch directory.

2. Dans le portail Azure, sur la **page d'accueil**, utilisez l’icône **&#65291; Créer une ressource** pour créer une ressource.
3. Recherchez *Azure Synapse Analytics* et créez une ressource **Azure Synapse Analytics** avec les paramètres suivants :
    - **Abonnement** : *votre abonnement Azure*
        - **Groupe de ressources** : *Créez un groupe de ressources avec un nom approprié, comme « synapse-rg »*
        - **Groupe de ressources géré** : *Entrez un nom approprié, par exemple « synapse-Managed-rg »*.
    - **Nom de l’espace de travail** : *entrez un nom d’espace de travail unique, par exemple « synapse-ws-<votre_nom> »* .
    - **Région** : *Sélectionnez l’une des régions suivantes* :
        - Australie Est
        - USA Centre
        - USA Est 2
        - Europe Nord
        - États-Unis - partie centrale méridionale
        - Asie Sud-Est
        - Sud du Royaume-Uni
        - Europe Ouest
        - USA Ouest
        - USA Ouest 2
    - **Sélectionner Data Lake Storage Gen 2** : À partir de l’abonnement
        - **Nom du compte** : *Créez un nouveau compte avec un nom unique, par exemple « datalake<votre_nom> »*.
        - **Nom du système de fichiers** : *Créez un nouveau système de fichiers avec un nom unique, par exemple « fs<votre_nom> »*.

    > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: A Synapse Analytics workspace requires two resource groups in your Azure subscription; one for resources you explicitly create, and another for managed resources used by the service. It also requires a Data Lake storage account in which to store data, scripts, and other artifacts.

4. Une fois ces détails entrés, sélectionnez **Vérifier + créer**, puis **Créer** pour créer l’espace de travail.
5. Patientez pendant la création de l’espace de travail. Cette opération peut prendre environ cinq minutes.
6. Une fois le déploiement terminé, accédez au groupe de ressources créé et notez qu’il contient votre espace de travail Synapse Analytics et un compte de stockage Data Lake.
7. Sélectionnez votre espace de travail Synapse et, dans sa page **Vue d’ensemble**, dans la carte **Ouvrir Synapse Studio**, sélectionnez **Ouvrir** pour ouvrir Synapse Studio dans un nouvel onglet de navigateur. Synapse Studio est une interface web que vous pouvez utiliser pour travailler avec votre espace de travail Synapse Analytics.
8. Sur le côté gauche de Synapse Studio, utilisez l'icône **&rsaquo;&rsaquo;** pour développer le menu. Cela révèle les différentes pages de Synapse Studio que vous allez utiliser pour gérer les ressources et effectuer les tâches d’analyse des données, comme illustré ici :

    ![Image représentant le menu de Synapse Studio développé pour gérer les ressources et effectuer des tâches d’analyse des données](images/synapse-studio.png)

## <a name="ingest-data"></a>Réception de données

L’une des tâches clés que vous pouvez effectuer avec Azure Synapse Analytics consiste à définir des *pipelines* qui transfèrent (et, le cas échéant, transforment) des données à partir d’un large éventail de sources vers votre espace de travail à des fins d’analyse.

1. Dans Synapse Studio, dans la page **Accueil**, sélectionnez **Ingérer** et choisissez **Tâche de copie intégrée** pour ouvrir l’outil **Copier des données**.
2. Dans l’outil Copier des données, à l’étape **Propriétés**, assurez-vous que les options **Tâche de copie intégrée** et **Exécuter une fois maintenant** sont sélectionnées, puis cliquez sur **Suivant >**.
3. À l’étape **Source**, dans la sous-étape **Jeu de données**, sélectionnez les paramètres suivants :
    - **Type de source** : Tous
    - **Connexion** : *créez une connexion et, dans le volet **Service lié** qui s’affiche, sous l’onglet **Fichier**, sélectionnez **HTTP**. Ensuite, poursuivez et créez une connexion à un fichier de données à l’aide des paramètres suivants :*
        - **Nom** : Produits d’AdventureWorks
        - **Description** : Liste de produits via HTTP
        - **Se connecter via un runtime d'intégration** : AutoResolveIntegrationRuntime
        - **URL de base** : `https://raw.githubusercontent.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/master/Azure-Synapse/products.csv`
        - **Validation du certificat de serveur** : Activer
        - **Type d’authentification** : Anonyme
4. Une fois la connexion créée, dans la sous-étape **Source/Jeu de données**, vérifiez que les paramètres suivants sont activés, puis sélectionnez **Suivant >**  :
    - **URL relative** : *Laissez vide*
    - **Méthode de demande** : GET
    - **En-têtes supplémentaires** : *Laissez vide*
    - **Copie binaire** : <u>Non</u> sélectionné
    - **Expiration du délai de la demande** : *Laissez vide*
    - **Nombre maximal de connexions simultanées** : *Laisser vide*
5. À l’étape **Source**, dans la sous-étape **Configuration**, sélectionnez **Aperçu des données** pour afficher un aperçu des données du produit que votre pipeline va recevoir, puis fermez l’aperçu.
6. Après avoir affiché un aperçu des données, à l’étape **Source/Configuration**, assurez-vous que les paramètres suivants sont activés, puis sélectionnez **Suivant >** :
    - **Format de fichier** : DelimitedText
    - **Séparateur de colonne** : virgule (,)
    - **Délimiteur de lignes** : Saut de ligne (\n)
    - **Première ligne comme en-tête** : Sélectionné
    - **Type de compression** : Aucune
7. À l’étape **Cible**, dans la sous-étape **Jeu de données**, sélectionnez les paramètres suivants :
    - **Type de cible** : Azure Data Lake Storage Gen 2
    - **Connexion** : *sélectionnez la connexion existante à votre magasin de lac de données (cela a été créé pour vous lorsque vous avez créé l’espace de travail).*
8. Une fois la connexion sélectionnée, dans l’étape **Cible/Jeu de données**, vérifiez que les paramètres suivants sont sélectionnés, puis sélectionnez **Suivant >**  :
    - **Chemin d’accès du dossier** : *Accédez au dossier dans votre système de fichiers*
    - **Nom du fichier** : products.csv
    - **Comportement de copie** : Aucun
    - **Nombre maximal de connexions simultanées** : *Laisser vide*
    - **Taille de bloc (Mo)** : *Laissez vide*
9. On the <bpt id="p1">**</bpt>Target<ept id="p1">**</ept> step, in the <bpt id="p2">**</bpt>Configuration<ept id="p2">**</ept> substep, ensure that the following properties are selected. Then select <bpt id="p1">**</bpt>Next &gt;<ept id="p1">**</ept>:
    - **Format de fichier** : DelimitedText
    - **Séparateur de colonne** : virgule (,)
    - **Délimiteur de lignes** : Saut de ligne (\n)
    - **Ajouter un en-tête au fichier** : Sélectionné
    - **Type de compression** : Aucune
    - **Nombre maximal de lignes par fichier** : *Laissez vide*
    - **Préfixe du nom de fichier** : *Laissez vide*
10. À l’étape **Paramètres**, entrez les paramètres suivants, puis cliquez sur **Suivant >** :
    - **Nom de la tâche** : Copier les produits
    - **Description de la tâche** : Copie des données des produits
    - **Tolérance de panne** : *Laissez vide*
    - **Activer la journalisation** : <u>Non</u> sélectionné
    - **Activer le mode de préproduction** : <u>Non</u> sélectionné
11. À l’étape **Vérifier et terminer**, à la sous-étape **Vérifier**, lisez le résumé, puis cliquez sur **Suivant >**.
12. À l’étape **Déploiement**, attendez que le pipeline soit déployé, puis cliquez sur **Terminer**.
13. Dans Synapse Studio, sélectionnez la page **Superviser**, puis sous l’onglet **Exécutions du pipeline**, attendez que le pipeline **Copier les produits** se termine avec l’état **Réussi** (vous pouvez utiliser le bouton **&#8635; Actualiser** dans la page Exécutions du pipeline pour actualiser l’état).
14. On the <bpt id="p1">**</bpt>Data<ept id="p1">**</ept> page, select the <bpt id="p2">**</bpt>Linked<ept id="p2">**</ept> tab and expand the <bpt id="p3">**</bpt>Azure Data Lake Storage Gen 2<ept id="p3">**</ept> hierarchy until you see the file storage for your Synapse workspace. Then select the file storage to verify that a file named <bpt id="p1">**</bpt>products.csv<ept id="p1">**</ept> has been copied to this location, as shown here:

    ![Image montrant la hiérarchie Azure Data Lake Storage Gen 2 développée dans Synapse Studio, avec le stockage de fichiers pour votre espace de travail Synapse](images/synapse-storage.png)

## <a name="use-a-sql-pool-to-analyze-data"></a>Utiliser un pool SQL pour analyser des données

Now that you've ingested some data into your workspace, you can use Synapse Analytics to query and analyze it. One of the most common ways to query data is to use SQL, and in Synapse Analytics you can use a <bpt id="p1">*</bpt>SQL pool<ept id="p1">*</ept> to run SQL code.

1. Dans Synapse Studio, cliquez avec le bouton droit sur le fichier **products.csv** dans le stockage de fichiers de votre espace de travail Synapse, pointez sur **Nouveau script SQL**, puis choisissez **Sélectionner les 100 premières lignes**.
2. Dans le volet **Script SQL 1** qui s’ouvre, passez en revue le code SQL qui a été généré, et qui doit ressembler à ceci :

    ```SQL
    -- This is auto-generated code
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://datalakexx.dfs.core.windows.net/fsxx/products.csv',
            FORMAT = 'CSV',
            PARSER_VERSION='2.0'
        ) AS [result]
    ```

    Ce code ouvre un ensemble de lignes à partir du fichier texte que vous avez importé et récupère les 100 premières lignes de données.

3. Dans la liste **Se connecter à**, vérifiez que l’option **Intégré** est cochée : cela représente le pool SQL intégré qui a été créé avec votre espace de travail.
4. Dans la barre d’outils, utilisez le bouton **&#9655; Exécuter** pour exécuter le code SQL, puis passez en revue les résultats, qui doivent ressembler à ceci :

    | C1 | c2 | c3 | c4 |
    | -- | -- | -- | -- |
    | ProductID | ProductName | Catégorie | ListPrice |
    | 771 | Mountain-100 Silver, 38 | VTT | 3399.9900 |
    | 772 | Mountain-100 Silver, 42 | VTT | 3399.9900 |
    | ... | ... | ... | ... |

5. Note the results consist of four columns named C1, C2, C3, and C4; and that the first row in the results contains the names of the data fields. To fix this problem, add a HEADER_ROW = TRUE parameters to the OPENROWSET function as shown here (replacing <bpt id="p1">*</bpt>datalakexx<ept id="p1">*</ept> and <bpt id="p2">*</bpt>fsxx<ept id="p2">*</ept> with the names of your data lake storage account and file system), and then rerun the query:

    ```SQL
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://datalakexx.dfs.core.windows.net/fsxx/products.csv',
            FORMAT = 'CSV',
            PARSER_VERSION='2.0',
            HEADER_ROW = TRUE
        ) AS [result]
    ```

    À présent, les résultats ressemblent à ceci :

    | ProductID | ProductName | Catégorie | ListPrice |
    | -- | -- | -- | -- |
    | 771 | Mountain-100 Silver, 38 | VTT | 3399.9900 |
    | 772 | Mountain-100 Silver, 42 | VTT | 3399.9900 |
    | ... | ... | ... | ... |

6. Modifiez la requête comme suit (en remplaçant *datalakexx* et *fsxx* par les noms de votre compte de stockage Data Lake et de votre système de fichiers) :

    ```SQL
    SELECT
        Category, COUNT(*) AS ProductCount
    FROM
        OPENROWSET(
            BULK 'https://datalakexx.dfs.core.windows.net/fsxx/products.csv',
            FORMAT = 'CSV',
            PARSER_VERSION='2.0',
            HEADER_ROW = TRUE
        ) AS [result]
    GROUP BY Category;
    ```

7. Exécutez la requête modifiée, qui doit retourner un jeu de résultats contenant le nombre de produits dans chaque catégorie, comme ceci :

    | Catégorie | ProductCount |
    | -- | -- |
    | Bib Shorts | 3 |
    | Porte-vélos | 1 |
    | ... | ... |

8. In the <bpt id="p1">**</bpt>Properties<ept id="p1">**</ept> pane for <bpt id="p2">**</bpt>SQL Script 1<ept id="p2">**</ept>, change the <bpt id="p3">**</bpt>Name<ept id="p3">**</ept> to <bpt id="p4">**</bpt>Count Products by Category<ept id="p4">**</ept>. Then in the toolbar, select <bpt id="p1">**</bpt>Publish<ept id="p1">**</ept> to save the script.

9. Fermez le volet du script **Nombre de produits par catégorie**.

10. Dans Synapse Studio, sélectionnez la page **Développer** et notez que votre script SQL **Nombre de produits par catégorie** y a été enregistré.

11. Select the <bpt id="p1">**</bpt>Count Products by Category<ept id="p1">**</ept> SQL script to reopen it. Then ensure that the script is connected to the <bpt id="p1">**</bpt>Built-in<ept id="p1">**</ept> SQL pool and run it to retrieve the product counts.

12. Dans le volet **Résultats**, sélectionnez la vue **Graphique**, puis sélectionnez les paramètres suivants pour le graphique :
    - **Type de graphique** : Colonne
    - **Colonne de catégorie** : Catégorie
    - **Colonnes de légende (série)** : ProductCount
    - **Position de la légende** : bas - centre
    - **Étiquette de légende (série)** : *Laissez vide*
    - **Valeur minimale de la légende (série)** : *Laissez vide*
    - **Valeur maximale de la légende (série)** : *Laissez vide*
    - **Étiquette de catégorie** : *Laissez vide*

    Le graphique obtenu doit ressembler à ceci :

    ![Image montrant la vue de graphique du nombre de produits](images/column-chart.png)

## <a name="use-a-spark-pool-to-analyze-data"></a>Utiliser un pool Spark pour analyser des données

While SQL is a common language for querying structured datasets, many data analysts find languages like Python useful to explore and prepare data for analysis. In Azure Synapse Analytics, you can run Python (and other) code in a <bpt id="p1">*</bpt>Spark pool<ept id="p1">*</ept>; which uses a distributed data processing engine based on Apache Spark.

1. Dans Synapse Studio, sélectionnez la page **Gérer**.
2. Sélectionnez l’onglet **Pools Apache Spark**, puis utilisez l’icône **&#65291; Nouveau** pour créer un pool Spark avec les paramètres suivants :
    - **Nom du pool Apache Spark** : spark
    - **Famille de tailles de nœud** : À mémoire optimisée
    - **Taille de nœud** : Petite (4 vCores/32 Go)
    - **Mise à l’échelle automatique** : Activée
    - **Nombre de nœuds** 3----3
3. Examinez et créez le pool Spark, puis attendez qu’il se déploie (ce qui peut prendre quelques minutes).
4. When the Spark pool has been deployed, in Synapse Studio, on the <bpt id="p1">**</bpt>Data<ept id="p1">**</ept> page, browse to the file system for your Synapse workspace. Then right-click <bpt id="p1">**</bpt>products.csv<ept id="p1">**</ept>, point to <bpt id="p2">**</bpt>New notebook<ept id="p2">**</ept>, and select <bpt id="p3">**</bpt>Load to DataFrame<ept id="p3">**</ept>.
5. Dans le volet **Notebook 1** qui s’ouvre, dans la liste **Attacher à**, sélectionnez le pool Spark **spark** créé précédemment et assurez-vous que le **Langage** est défini sur **PySpark (Python)**.
6. Examinez le code dans la première (et unique) cellule du notebook, qui doit se présenter comme suit :

    ```Python
    %%pyspark
    df = spark.read.load('abfss://fsxx@datalakexx.dfs.core.windows.net/products.csv', format='csv'
    ## If header exists uncomment line below
    ##, header=True
    )
    display(df.limit(10))
    ```

7.                  **Conseil** : Veillez à travailler dans le répertoire contenant votre abonnement, indiqué en haut à droite sous votre ID d’utilisateur.

    > **Remarque** : Si une erreur se produit parce que le noyau Python n’est pas encore disponible, réexécutez la cellule.

8. Au final, les résultats devraient apparaître sous la cellule et ressembler à ceci :

    | _c0_ | _c1_ | _c2_ | _c3_ |
    | -- | -- | -- | -- |
    | ProductID | ProductName | Catégorie | ListPrice |
    | 771 | Mountain-100 Silver, 38 | VTT | 3399.9900 |
    | 772 | Mountain-100 Silver, 42 | VTT | 3399.9900 |
    | ... | ... | ... | ... |

9. Décommentez la ligne *,header=True* (car le fichier products.csv contient les en-têtes de colonnes dans la première ligne), afin que votre code ressemble à ceci :

    ```Python
    %%pyspark
    df = spark.read.load('abfss://fsxx@datalakexx.dfs.core.windows.net/products.csv', format='csv'
    ## If header exists uncomment line below
    , header=True
    )
    display(df.limit(10))
    ```

10. Réexécutez la cellule et vérifiez que les résultats ressemblent à ceci :

    | ProductID | ProductName | Catégorie | ListPrice |
    | -- | -- | -- | -- |
    | 771 | Mountain-100 Silver, 38 | VTT | 3399.9900 |
    | 772 | Mountain-100 Silver, 42 | VTT | 3399.9900 |
    | ... | ... | ... | ... |

    Notez que le fait d’exécuter à nouveau la cellule prend moins de temps, car le pool Spark est déjà démarré.

11. Dans les résultats, utilisez l’icône **&#65291; Code** pour ajouter une nouvelle cellule de code au notebook.
12. Dans la nouvelle cellule de code vide, ajoutez le code suivant :

    ```Python
    df_counts = df.groupBy(df.Category).count()
    display(df_counts)
    ```

13. Sélectionnez **&#9655; Exécuter** à gauche pour exécuter la nouvelle cellule de code, et examinez les résultats, qui doivent ressembler à ceci :

    | Catégorie | count |
    | -- | -- |
    | Oreillettes | 3 |
    | Roues | 14 |
    | ... | ... |

14. Si ce n’est pas le cas, sélectionnez l’icône de l’utilisateur et changez d’annuaire.

    ![Image montrant la vue de graphique du nombre de catégories](images/bar-chart.png)

15. Fermez le volet **Notebook 1** et abandonnez vos modifications.

## <a name="delete-azure-resources"></a>Supprimer les ressources Azure

Si vous avez fini d’explorer Azure Synapse Analytics, vous devriez supprimer les ressources que vous avez créées afin d’éviter des coûts Azure inutiles.

1. Fermez l’onglet du navigateur Synapse Studio et revenez dans le portail Azure.
2. Dans le portail Azure, dans la page **Accueil**, sélectionnez **Groupes de ressources**.
3. Sélectionnez le groupe de ressources pour votre espace de travail Synapse Analytics (et non le groupe de ressources managé) et vérifiez qu’il contient l’espace de travail Synapse, le compte de stockage et le pool Spark pour votre espace de travail.
4. Au sommet de la page **Vue d’ensemble** de votre groupe de ressources, sélectionnez **Supprimer le groupe de ressources**.
5. Entrez le nom du groupe de ressources pour confirmer que vous souhaitez le supprimer, puis sélectionnez **Supprimer**.

    Après quelques minutes, votre espace de travail Azure Synapse et l’espace de travail managé qui lui est associé seront supprimés.
