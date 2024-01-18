---
title: Istruzioni ospitate online
permalink: index.html
layout: home
---

# Directory contenuto

I file necessari per il lab possono essere [scaricati qui](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/archive/master.zip)

In basso sono elencati i collegamenti ipertestuali a tutti gli esercizi lab.

## Esercitazioni

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Modulo | Lab |
| --- | --- | 
{% for activity in labs %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
