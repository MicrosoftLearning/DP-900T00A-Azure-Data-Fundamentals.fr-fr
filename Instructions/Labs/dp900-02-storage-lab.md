---
lab:
  title: Explorer le Stockage Azure
  module: Explore Azure Storage for non-relational data
---

# Explorer le Stockage Azure

Dans cet exercice, vous allez provisionner un compte Stockage Azure dans votre abonnement Azure et explorer les différentes façons dont vous pouvez l’utiliser pour stocker des données.

Ce labo prend environ **15** minutes.

## Avant de commencer

Vous avez besoin d’un [abonnement Azure](https://azure.microsoft.com/free) dans lequel vous avez un accès administratif.

## Approvisionner un compte de stockage Azure

La première étape de l’utilisation du stockage Azure consiste à approvisionner un compte stockage Azure dans votre abonnement Azure.

1. Si ce n’est pas déjà fait, connectez-vous au [portail Azure](https://portal.azure.com?azure-portal=true).
1. Dans la page d’accueil du portail Azure, sélectionnez **&#65291; Créer une ressource** en haut à gauche et recherchez *Compte de stockage*. Dans la page **Compte de stockage** qui en résulte, sélectionnez **Créer**.
1. Saisissez les valeurs suivantes sur la page **Créer un compte de stockage** :
    - **Abonnement**: Sélectionnez votre abonnement Azure.
    - **Groupe de ressources** : créez un nouveau groupe de ressources portant le nom de votre choix.
    - **Nom du compte de stockage** : entrez un nom unique pour votre compte de stockage en utilisant des lettres minuscules et des chiffres.
    - **Région** : sélectionnez un emplacement disponible.
    - **Niveau de performance** : *Standard*
    - **Redondance** : *stockage localement redondant (LRS)*

1. Sélectionnez **Suivant : avancé >** et examinez les options de configuration avancées. En particulier, notez que vous pouvez ici activer l’espace de noms hiérarchique pour prendre en charge Azure Data Lake Storage Gen2. Laissez cette option **<u>désactivée</u>** (vous l’activerez plus tard), puis sélectionnez **Suivant : Réseau >** pour voir les options réseau de votre compte de stockage.
1. Sélectionnez **Suivant : Protection des données >** , puis dans la section **Récupération**, <u>dé</u>sélectionnez toutes les options **Activer la suppression réversible...** . Ces options conservent les fichiers supprimés pour la récupération suivante, mais peuvent provoquer des problèmes plus tard lorsque vous activez l’espace de noms hiérarchique.
1. Passez les pages **Suivant >** restantes sans modifier les paramètres par défaut, puis dans la page **Vérifier**, attendez que vos sélections soient validées et sélectionnez **Créer** pour créer votre compte Stockage Azure.
1. Attendez la fin du déploiement. Accédez ensuite à la ressource qui a été déployée.

## Explorer le Stockage Blob

Maintenant que vous avez un compte de stockage Azure, vous pouvez créer un conteneur pour les données blob.

1. Téléchargez le fichier JSON [product1.json](https://aka.ms/product1.json?azure-portal=true) à partir de `https://aka.ms/product1.json` et enregistrez-le sur votre ordinateur (vous pouvez l’enregistrer dans n’importe quel dossier : vous le chargerez dans le Stockage Blob ultérieurement).

    *Si le fichier JSON s’affiche dans votre navigateur, enregistrez la page en tant que **product1.json**.*

1. Dans la page Portail Azure de votre conteneur de stockage, sur le côté gauche, dans la section **stockage des données**, sélectionnez **Conteneurs**.
1. Dans la page **Conteneurs**, sélectionnez **&#65291; Conteneur** et ajoutez un nouveau conteneur nommé **données** et ayant un niveau d’accès **Privé (aucun accès anonyme)** .
1. Une fois le conteneur de **données** créé, vérifiez qu’il est répertorié dans la page **Conteneurs**.
1. Dans le volet de gauche, dans la section supérieure, sélectionnez **Navigateur de stockage**. Cette page fournit une interface basée sur un navigateur que vous pouvez utiliser pour travailler avec les données de votre compte de stockage.
1. Dans la page du navigateur de stockage, sélectionnez **Conteneurs de blobs** et vérifiez que votre conteneur de **données** est répertorié.
1. Sélectionnez le conteneur de **données** et notez qu’il est vide.
1. Sélectionnez **&#65291; Ajouter un répertoire** et lisez les informations concernant les dossiers avant de créer un autre répertoire nommé **produits**.
1. Dans le navigateur de stockage, vérifiez que la vue actuelle affiche le contenu du dossier **produits** que vous venez de créer. Notez que les barres de navigation en haut de la page indiquent le chemin **Conteneurs d’objets blob > données > produits**.
1. Dans les barres de navigation, sélectionnez les **données** à basculer vers le conteneur de **données** et notez qu’elles ne contiennent <u>pas</u> de dossier nommé **produits**.

    Les dossiers dans le Stockage Blob sont virtuels et existent uniquement dans le cadre du chemin d’accès d’un Blob. Étant donné que le dossier **produits** ne contenait pas de blob, ce n’est pas vraiment le cas !

1. Utilisez le bouton **&#10514; Charger** pour ouvrir le panneau **Charger un blob**.
1. Dans le panneau **Charger un blob**, sélectionnez précédemment le fichier **product1.json** que vous avez enregistré sur votre ordinateur local. Ensuite, dans la section **Avancé**, dans la zone **Charger dans un dossier**, entrez **product_data** et sélectionnez le bouton **Charger**.
1. Fermez le panneau **Charger le blob** s’il est toujours ouvert, puis vérifiez qu’un dossier virtuel **product_data** a été créé dans le conteneur de **données**.
1. Sélectionnez le dossier **product_data** et vérifiez qu’il contient le blob **product1.json** que vous avez chargé.
1. Sur le côté gauche, dans la section **Stockage des données**, sélectionnez **Conteneurs**.
1. Ouvrez le conteneur de **données** et vérifiez que le dossier **product_data** que vous avez créé est répertorié.
1. Sélectionnez l’icône **&#x2027;&#x2027;&#x2027;** tout à droite du dossier. Vous voyez qu’elle n’affiche aucune option. Les dossiers dans un conteneur d’objets blob d’un espace de noms plat sont virtuels et ne peuvent pas être gérés.
1. Utilisez l’icône **X** en haut à droite de la page de **données** pour fermer la page et revenir à la page **Conteneurs**.

## Explorer Azure Data Lake Storage Gen2

Le support Azure Data Lake Store Gen2 vous permet d’utiliser des dossiers hiérarchiques pour organiser et gérer l’accès aux blobs. Il vous permet également d’utiliser le Stockage Blob Azure pour héberger des systèmes de fichiers distribués pour les plateformes d’analytique Big Data courantes.

1. Téléchargez le fichier JSON [product2.json](https://aka.ms/product2.json?azure-portal=true) à partir de `https://aka.ms/product2.json` et enregistrez-le sur votre ordinateur dans le même dossier que celui dans lequel vous avez téléchargé **product1.json** précédemment. vous le téléchargerez dans le stockage Blob ultérieurement.
1. Dans la page de votre compte de stockage sur le portail Azure, sur le côté gauche, faites défiler jusqu’à la section **Paramètres**, puis sélectionnez **Mise à niveau de Data Lake Gen2**.
1. Dans la page **Mise à niveau de Data Lake Gen2**, développez et effectuez chaque étape pour mettre à niveau votre compte de stockage afin d’activer l’espace de noms hiérarchique et la prise en charge d’Azure Data Lake Storage Gen 2. Cette opération peut prendre un certain temps.
1. Une fois la mise à niveau terminée, dans le volet de gauche, dans la section supérieure, sélectionnez **Navigateur de stockage** et revenez à la racine de votre conteneur d’objets blob **data**, qui contient toujours le dossier **product_data**.
1. Sélectionnez le dossier **product_data**, puis vérifiez qu’il contient toujours le fichier **product1.json** que vous avez téléchargé précédemment.
1. Utilisez le bouton **&#10514; Charger** pour ouvrir le panneau **Charger un blob**.
1. Dans le panneau **Charger un blob**, sélectionnez le fichier **product2.json** que vous avez enregistré sur votre ordinateur local. Ensuite, sélectionnez le bouton **Charger**.
1. Fermez le panneau **Charger un blob** s’il est toujours ouvert, puis vérifiez qu’un dossier **product_data** contient maintenant le fichier **product2.json**.
1. Sur le côté gauche, dans la section **Stockage des données**, sélectionnez **Conteneurs**.
1. Ouvrez le conteneur de **données** et vérifiez que le dossier **product_data** que vous avez créé est répertorié.
1. Sélectionnez l’icône **&#x2027;&#x2027;&#x2027;** tout à droite du dossier et notez qu’avec l’espace de noms hiérarchique activé, vous pouvez effectuer des tâches de configuration au niveau du dossier, y compris renommer des dossiers et définir des autorisations.
1. Utilisez l’icône **X** en haut à droite de la page de **données** pour fermer la page et revenir à la page **Conteneurs**.

## Explorer Azure Files

Azure Files fournit un moyen de créer des partages de fichiers basés sur le cloud.

1. Dans la page Portail Azure de votre conteneur de stockage, sur le côté gauche, dans la section **Stockage des données**, sélectionnez **Partages de fichiers**.
1. Dans la page Partages de fichiers, sélectionnez **&#65291; Partage de fichiers** et ajoutez un nouveau partage de fichiers nommé **fichiers** et de niveau **Transaction optimisée**.
1. Dans **Partages de fichiers**, ouvrez votre nouveau partage de **fichiers**.
1. En haut de la page, sélectionnez **Connecter**. Ensuite, dans le volet **Connecter**, notez qu’il existe des onglets pour les systèmes d’exploitation communs (Windows, Linux et macOS) qui contiennent des scripts que vous pouvez exécuter pour vous connecter au dossier partagé à partir d’un ordinateur client.
1. Fermez le volet **Connecter**, puis fermez la page **fichiers** pour revenir à la page **Partages de fichiers** de votre compte de stockage Azure.

## Explorer des tables Azure

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

    Vous avez entré manuellement les données dans la table à l’aide de l’interface de navigateur de stockage. Dans un scénario réel, les développeurs d’applications peuvent utiliser le stockage Azure API Table pour créer des applications qui lisent et écrivent des valeurs dans des tables, ce qui en fait une solution rentable et évolutive pour le stockage NoSQL.

> **Conseil** : Si vous avez fini d’explorer le Stockage Azure, vous pouvez supprimer le groupe de ressources que vous avez créé dans cet exercice.
