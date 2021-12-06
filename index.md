---
title: Istruzioni online
permalink: index.html
layout: home
ms.openlocfilehash: 7ad42e4108b96ec131049fa1035ab1d112d95d90
ms.sourcegitcommit: 2eb153f2856445e5afaa218a012cb92e3d48f24b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/16/2021
ms.locfileid: "132625696"
---
# <a name="content-directory"></a>Directory contenuto

I file necessari per il lab possono essere [scaricati qui](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/archive/master.zip)

In basso sono elencati i collegamenti ipertestuali a tutte le demo e a tutti gli esercizi lab.

## <a name="labs"></a>Lab

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Modulo | Lab |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
