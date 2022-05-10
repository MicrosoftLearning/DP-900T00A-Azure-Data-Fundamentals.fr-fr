---
title: Instructions hébergées en ligne
permalink: index.html
layout: home
ms.openlocfilehash: 928f59a9cdc6a3f70d5ad651fb1b5a45b405cee8
ms.sourcegitcommit: 1117342052bce0bbd5a703bd1f763862b9129807
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/16/2022
ms.locfileid: "140682424"
---
# <a name="azure-data-fundamentals-exercises"></a>Principes de base sur les données Azure

Utilisez les liens ci-dessous pour faire les exercices pratiques de labo du cours Microsoft [DP-900 *Principes de base sur les données Microsoft Azure*](https://docs.microsoft.com/learn/certifications/courses/dp-900t00).

Pour effectuer ces exercices, vous devez disposer d’un abonnement Microsoft Azure. Si votre instructeur ne vous en a pas fourni un, vous pouvez vous inscrire pour un essai gratuit sur [https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains ’/Instructions/Labs’" %}
| Module | Laboratoire |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
