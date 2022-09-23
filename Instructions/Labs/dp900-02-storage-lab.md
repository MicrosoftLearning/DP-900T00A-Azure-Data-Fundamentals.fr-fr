---
lab:
  title: Explorer des données non relationnelles dans Azure avec le Stockage Azure
  module: Explore Azure Storage for non-relational data
---

# <a name="explore-non-relational-data-in-azure-with-azure-storage"></a>Explorer des données non relationnelles dans Azure avec le Stockage Azure

Dans cet exercice, vous allez provisionner un compte Stockage Azure dans votre abonnement Azure et explorer les différentes façons dont vous pouvez l’utiliser pour stocker des données.

Ce labo prend environ **15** minutes.

## <a name="before-you-start"></a>Avant de commencer

Vous avez besoin d’un [abonnement Azure](https://azure.microsoft.com/free) dans lequel vous avez un accès administratif.

## <a name="provision-an-azure-storage-account"></a>Approvisionner un compte de stockage Azure

La première étape de l’utilisation du stockage Azure consiste à approvisionner un compte stockage Azure dans votre abonnement Azure.

1. Si ce n’est pas déjà fait, connectez-vous au [portail Azure](https://portal.azure.com?azure-portal=true).
1. On the Azure portal home page, select <bpt id="p1">**</bpt>&amp;#65291; Create a resource<ept id="p1">**</ept> from the upper left-hand corner and search for <bpt id="p2">*</bpt>Storage account<ept id="p2">*</ept>. Then in the resulting <bpt id="p1">**</bpt>Storage account<ept id="p1">**</ept> page, select <bpt id="p2">**</bpt>Create<ept id="p2">**</ept>.
1. Saisissez les valeurs suivantes sur la page **Créer un compte de stockage** :
    - <bpt id="p1">**</bpt>Subscription<ept id="p1">**</ept>: If you're using a sandbox, select <bpt id="p2">*</bpt>Concierge Subscription<ept id="p2">*</ept>. Otherwise, select your Azure subscription.
    - **Groupe de ressources** : Si vous utilisez un bac à sable, sélectionnez le groupe de ressources existant (qui a un nom comme *learn-xxxx...* ). Sinon, créez un groupe de ressources avec le nom de votre choix.
    - **Nom du compte de stockage** : entrez un nom unique pour votre compte de stockage en utilisant des lettres minuscules et des chiffres.
    - **Région** : sélectionnez un emplacement disponible.
    - **Niveau de performance** : *Standard*
    - **Redondance** : *stockage localement redondant (LRS)*

1. Select <bpt id="p1">**</bpt>Next: Advanced &gt;<ept id="p1">**</ept> and view the advanced configuration options. In particular, note that this is where you can enable hierarchical namespace to support Azure Data Lake Storage Gen2. Leave this option <bpt id="p1">**</bpt><bpt id="p2">&lt;u&gt;</bpt>unselected<ept id="p2">&lt;/u&gt;</ept><ept id="p1">**</ept> (you'll enable it later), and then select <bpt id="p3">**</bpt>Next: Networking &gt;<ept id="p3">**</ept> to view the networking options for your storage account.
1. Select <bpt id="p1">**</bpt>Next: Data protection &gt;<ept id="p1">**</ept> and then in the <bpt id="p2">**</bpt>Recovery<ept id="p2">**</ept> section, <bpt id="p3">&lt;u&gt;</bpt>de<ept id="p3">&lt;/u&gt;</ept>select all of the <bpt id="p4">**</bpt>Enable soft delete...<ept id="p4">**</ept> options. These options retain deleted files for subsequent recovery, but can cause issues later when you enable hierarchical namespace.
1. Passez les pages **Suivant >** restantes sans modifier les paramètres par défaut, puis dans la page **Vérifier + créer**, attendez que vos sélections soient validées et sélectionnez **Créer** pour créer votre compte Stockage Azure.
1. Wait for deployment to complete. Then go to the resource that was deployed.

## <a name="explore-blob-storage"></a>Explorer le Stockage Blob

Maintenant que vous avez un compte de stockage Azure, vous pouvez créer un conteneur pour les données blob.

1. Téléchargez le fichier JSON [product1.json](https://aka.ms/product1.json?azure-portal=true) à partir de `https://aka.ms/product1.json` et enregistrez-le sur votre ordinateur (vous pouvez l’enregistrer dans n’importe quel dossier : vous le chargerez dans le Stockage Blob ultérieurement).

    *Si le fichier JSON s’affiche dans votre navigateur, enregistrez la page en tant que **product1.json**.*

1. Dans la page Portail Azure de votre conteneur de stockage, sur le côté gauche, dans la section **stockage des données**, sélectionnez **Conteneurs**.
1. Dans la page **Conteneurs**, sélectionnez **&#65291; Conteneur** et ajoutez un nouveau conteneur nommé **données** et ayant un niveau d’accès **Privé (aucun accès anonyme)** .
1. Une fois le conteneur de **données** créé, vérifiez qu’il est répertorié dans la page **Conteneurs**.
1. In the pane on the left side, in the top section, select **Storage browser **. This page provides a browser-based interface that you can use to work with the data in your storage account.
1. Dans la page du navigateur de stockage, sélectionnez **Conteneurs de blobs** et vérifiez que votre conteneur de **données** est répertorié.
1. Sélectionnez le conteneur de **données** et notez qu’il est vide.
1. Sélectionnez **&#65291; Ajouter un répertoire** et lisez les informations concernant les dossiers avant de créer un autre répertoire nommé **produits**.
1. Dans l’Explorateur Stockage, vérifiez que la vue actuelle affiche le contenu du dossier **produits** que vous venez de créer. Notez que les barres de navigation en haut de la page indiquent le chemin **Conteneurs d’objets blob > données > produits**.
1. Dans les barres de navigation, sélectionnez les **données** à basculer vers le conteneur de **données** et notez qu’elles ne contiennent <u>pas</u> de dossier nommé **produits**.

    Folders in blob storage are virtual, and only exist as part of the path of a blob. Since the <bpt id="p1">**</bpt>products<ept id="p1">**</ept> folder contained no blobs, it isn't really there!

1. Utilisez le bouton **&#10514; Charger** pour ouvrir le panneau **Charger un blob**.
1. In the <bpt id="p1">**</bpt>Upload blob<ept id="p1">**</ept> panel, select the <bpt id="p2">**</bpt>product1.json<ept id="p2">**</ept> file you saved on your local computer previously. Then in the <bpt id="p1">**</bpt>Advanced<ept id="p1">**</ept> section, in the <bpt id="p2">**</bpt>Upload to folder<ept id="p2">**</ept> box, enter <bpt id="p3">**</bpt>product_data<ept id="p3">**</ept> and select the <bpt id="p4">**</bpt>Upload<ept id="p4">**</ept> button.
1. Fermez le panneau **Charger le blob** s’il est toujours ouvert, puis vérifiez qu’un dossier virtuel **product_data** a été créé dans le conteneur de **données**.
1. Sélectionnez le dossier **product_data** et vérifiez qu’il contient le blob **product1.json** que vous avez chargé.
1. Sur le côté gauche, dans la section **Stockage des données**, sélectionnez **Conteneurs**.
1. Ouvrez le conteneur de **données** et vérifiez que le dossier **product_data** que vous avez créé est répertorié.
1. Select the <bpt id="p1">**</bpt>&amp;#x2027;&amp;#x2027;&amp;#x2027;<ept id="p1">**</ept> icon at the right-end of the folder, and note that it doesn't display any options. Folders in a flat namespace blob container are virtual, and can't be managed.
1. Utilisez l’icône **X** en haut à droite de la page de **données** pour fermer la page et revenir à la page **Conteneurs**.

## <a name="explore-azure-data-lake-storage-gen2"></a>Explorer Azure Data Lake Storage Gen2

Azure Data Lake Store Gen2 support enables you to use hierarchical folders to organize and manage access to blobs. It also enables you to use Azure blob storage to host distributed file systems for common big data analytics platforms.

1. Téléchargez le fichier JSON [product2.json](https://aka.ms/product2.json?azure-portal=true) à partir de `https://aka.ms/product2.json` et enregistrez-le sur votre ordinateur dans le même dossier que celui dans lequel vous avez téléchargé **product1.json** précédemment. vous le téléchargerez dans le stockage Blob ultérieurement.
1. Dans la page de votre compte de stockage sur le portail Azure, sur le côté gauche, faites défiler jusqu’à la section **Paramètres**, puis sélectionnez **Mise à niveau de Data Lake Gen2**.
1. Dans la page d’accueil du portail Azure, sélectionnez **&#65291; Créer une ressource** en haut à gauche et recherchez *Compte de stockage*.
1. Une fois la mise à niveau terminée, dans le volet de gauche, dans la section supérieure, sélectionnez **Navigateur de stockage** et revenez à la racine de votre conteneur d’objets blob **data**, qui contient toujours le dossier **product_data**.
1. Sélectionnez le dossier **product_data**, puis vérifiez qu’il contient toujours le fichier **product1.json** que vous avez téléchargé précédemment.
1. Utilisez le bouton **&#10514; Charger** pour ouvrir le panneau **Charger un blob**.
1. Dans la page **Compte de stockage** qui en résulte, sélectionnez **Créer**.
1. Fermez le panneau **Charger un blob** s’il est toujours ouvert, puis vérifiez qu’un dossier **product_data** contient maintenant le fichier **product2.json**.
1. Sur le côté gauche, dans la section **Stockage des données**, sélectionnez **Conteneurs**.
1. Ouvrez le conteneur de **données** et vérifiez que le dossier **product_data** que vous avez créé est répertorié.
1. Sélectionnez l’icône **&#x2027;&#x2027;&#x2027;** tout à droite du dossier et notez qu’avec l’espace de noms hiérarchique activé, vous pouvez effectuer des tâches de configuration au niveau du dossier, y compris renommer des dossiers et définir des autorisations.
1. Utilisez l’icône **X** en haut à droite de la page de **données** pour fermer la page et revenir à la page **Conteneurs**.

## <a name="explore-azure-files"></a>Explorer Azure Files

Azure Files fournit un moyen de créer des partages de fichiers basés sur le cloud.

1. Dans la page Portail Azure de votre conteneur de stockage, sur le côté gauche, dans la section **Stockage des données**, sélectionnez **Partages de fichiers**.
1. Dans la page Partages de fichiers, sélectionnez **&#65291; Partage de fichiers** et ajoutez un nouveau partage de fichiers nommé **fichiers** et de niveau **Transaction optimisée**.
1. Dans **Partages de fichiers**, ouvrez votre nouveau partage de **fichiers**.
1. At the top of the page, select <bpt id="p1">**</bpt>Connect<ept id="p1">**</ept>. Then in the <bpt id="p1">**</bpt>Connect<ept id="p1">**</ept> pane, note that there are tabs for common operating systems (Windows, Linux, and macOS) that contain scripts you can run to connect to the shared folder from a client computer.
1. Fermez le volet **Connecter**, puis fermez la page **fichiers** pour revenir à la page **Partages de fichiers** de votre compte de stockage Azure.

## <a name="explore-azure-tables"></a>Explorer des tables Azure

Les tables Azure fournissent un magasin clé/valeur pour les applications qui doivent stocker des valeurs de données, mais n’ont pas besoin de la totalité des fonctionnalités et de la structure d’une base de données relationnelle.

1. Dans la page Portail Azure de votre conteneur de stockage, sur le côté gauche, dans la section **Stockage des données**, sélectionnez **Tables**.
1. Dans la page **Tables**, sélectionnez **&#65291; Table** et créez une table nommée **produits**.
1. Une fois la table **produits** créée, dans le volet de gauche, dans la section supérieure, sélectionnez **Navigateur de stockage**.
1. Dans l’Explorateur stockage, sélectionnez **Tables** et vérifiez que la table **produits** est répertoriée.
1. Sélectionnez la table **produits**.
1. Dans la page **produit**, sélectionnez **&#65291; Ajouter une entité**.
1. Dans le panneau **Ajouter une entité**, entrez les valeurs de clés suivantes :
    - **PartitionKey** : 1
    - **RowKey** : 1
1. Sélectionnez **Ajouter une propriété**, puis créez une nouvelle propriété avec les valeurs suivantes :

    |Nom de la propriété | Type | Valeur |
    | ------------ | ---- | ----- |
    | Nom | String | Widget |

1. Ajoutez une deuxième propriété avec les valeurs suivantes :

    |Nom de la propriété | Type | Valeur |
    | ------------ | ---- | ----- |
    | Prix | Double | 2.99 |

1. Sélectionnez **Insérer** pour insérer une ligne pour la nouvelle entité de la table.
1. Dans le navigateur de stockage, vérifiez qu’une ligne a été ajoutée à la table **produits** et qu’une colonne **Timestamp** a été créée pour indiquer à quel moment la ligne a été modifiée pour la dernière fois.
1. Ajoutez une autre entité à la table **produits** avec les propriétés suivantes :

    |Nom de la propriété | Type | Valeur |
    | ------------ | ---- | ----- |
    | PartitionKey | String | 1 |
    | RowKey | String | 2 |
    | Nom | String | Kniknak |
    | Prix | Double | 1.99 |
    | Abandonné | Boolean | true |

1. Après avoir inséré la nouvelle entité, vérifiez qu’une ligne contenant le produit abandonné est indiquée dans le tableau.

    You have manually entered data into the table using the storage browser interface. In a real scenario, application developers can use the Azure Storage Table API to build applications that read and write values to tables, making it a cost effective and scalable solution for NoSQL storage.

> **Conseil** : Si vous avez fini d’explorer le Stockage Azure, vous pouvez supprimer le groupe de ressources que vous avez créé dans cet exercice.
