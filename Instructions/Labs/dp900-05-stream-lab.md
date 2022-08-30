---
lab:
  title: Découvrir Azure Stream Analytics
  module: Explore data analytics in Azure
---

## <a name="explore-azure-stream-analytics"></a>Découvrir Azure Stream Analytics

Dans cet exercice, vous allez provisionner un espace de travail Azure Stream Analytics dans votre abonnement Azure et l’utiliser pour traiter un flux de données en temps réel.

> <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: The exercise is part of a module on Microsoft Learn, and includes an option to use a <bpt id="p2">*</bpt>sandbox<ept id="p2">*</ept> Azure subscription. However, if you are completing this exercise as part of an instructor-led class, you should use the Azure subscription provided as part of the class instead of the sandbox.

Avant de commencer l’exercice sur Microsoft Learn, vous devez préparer un environnement Cloud Shell pour votre abonnement Azure.

1. Connectez-vous à votre abonnement Azure dans le [portail Azure](https://portal.azure.com) à l’adresse `https://portal.azure.com`, en utilisant vos informations d’identification d’abonnement Azure.
2. Use the <bpt id="p1">**</bpt>[<ph id="ph1">\&gt;</ph>_]<ept id="p1">**</ept> button to the right of the search bar at the top of the page to create a new Cloud Shell in the Azure portal, selecting a <bpt id="p2">***</bpt>Bash<ept id="p2">***</ept> environment and creating storage if prompted. The cloud shell provides a command line interface in a pane at the bottom of the Azure portal, as shown here:

    ![Portail Azure avec un volet Cloud Shell](./images/cloud-shell.png)

3. Note that you can resize the cloud shell by dragging the separator bar at the top of the pane, or by using the <bpt id="p1">**</bpt>&amp;#8212;<ept id="p1">**</ept>, <bpt id="p2">**</bpt>&amp;#9723;<ept id="p2">**</ept>, and <bpt id="p3">**</bpt>X<ept id="p3">**</ept> icons at the top right of the pane to minimize, maximize, and close the pane. For more information about using the Azure Cloud Shell, see the <bpt id="p1">[</bpt>Azure Cloud Shell documentation<ept id="p1">](https://docs.microsoft.com/azure/cloud-shell/overview)</ept>.

4. Vous êtes maintenant prêt à effectuer l’exercice sur Microsoft Learn. Veillez simplement à utiliser l’environnement Cloud Shell dans votre portail Azure au lieu de l’environnement vide dans le module Learn (qui est destiné aux utilisateurs d’un abonnement bac à sable dans le cadre d’un apprentissage auto-rythmé).

    Utilisez le lien ci-dessous pour ouvrir l’exercice sur Microsoft Learn.

    **[Accéder à Microsoft Learn](https://docs.microsoft.com/learn/modules/explore-fundamentals-stream-processing/5-exercise-stream-analytics#create-azure-resources)**

> **Formations pour aller plus loin** : Quand vous aurez le temps, vous pourrez revenir à ce module Microsoft Learn pour essayer les autres exercices qu’il contient, notamment ceux qui vous feront découvrir Spark Streaming et Azure Synapse Data Explorer.
