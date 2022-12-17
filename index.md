---
title: Instructions hébergées en ligne
permalink: index.html
layout: home
---

# <a name="azure-data-fundamentals-exercises"></a>Principes de base sur les données Azure

Ces exercices pratiques sont conçus pour accompagner le contenu des formations sur [Microsoft Learn](https://docs.microsoft.com/training/).

Pour effectuer ces exercices, vous avez besoin d’un abonnement Microsoft Azure dans lequel vous disposez d’autorisations d’administration. Vous pouvez vous inscrire à un essai gratuit sur [https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Exercice |
| --- |
{% for activity in labs  %}| [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
