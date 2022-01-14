---
lab:
    title: 'Labo 03 : Provisionner des services de données Azure non relationnelles'
    module: 'Module 03 : Explorer les données non relationnelles dans Azure'
---

## Instructions
Dans l’exemple de scénario, vous avez décidé de créer les magasins de données suivants :

* Cosmos DB pour contenir des informations sur le volume d’articles en stock. Vous devez stocker les informations actuelles et historiques sur les niveaux de volume, afin de pouvoir suivre la variation des niveaux dans le temps. Les données sont enregistrées quotidiennement.
* Un magasin Data Lake pour la conservation des données de production et de qualité.
* Un conteneur d’objets blob pour y conserver les images des produits fabriqués par l’entreprise.
* Stockage de fichiers pour le partage de rapports.

Dans ce labo, vous allez provisionner et configurer le compte Cosmos DB, puis le tester en créant une base de données, un conteneur et un exemple de document. Vous allez aussi provisionner un compte Stockage Azure qui peut fournir un stockage d’objets blob, de fichiers et Data Lake.

1.	Accédez au module Learn de Microsoft à l’adresse https://aka.ms/dp900lab03-fra et terminez les unités suivantes dans le navigateur : 
