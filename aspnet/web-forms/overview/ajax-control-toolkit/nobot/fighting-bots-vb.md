---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Lutte contre les robots (VB) | Microsoft Docs
author: wenz
description: Les robots automatisés mettent en plâtre des journaux et d’autres sites Web avec spam, en envoyant des formulaires de commentaires sans aucune intervention de l’utilisateur. Le contrôle NoBot dans le ASP.NET AJAX con...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: a8ca71b96cb84c97b1a60ae6a3d1a129cd1b0b10
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606393"
---
# <a name="fighting-bots-vb"></a>Lutte contre les robots (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)

> Les robots automatisés mettent en plâtre des journaux et d’autres sites Web avec spam, en envoyant des formulaires de commentaires sans aucune intervention de l’utilisateur. Le contrôle NoBot dans ASP.NET AJAX Control Toolkit peut aider à combattre ces robots.

## <a name="overview"></a>Vue d'ensemble de

Les robots automatisés mettent en plâtre des journaux et d’autres sites Web avec spam, en envoyant des formulaires de commentaires sans aucune intervention de l’utilisateur. Le contrôle NoBot dans ASP.NET AJAX Control Toolkit peut aider à combattre ces robots.

## <a name="steps"></a>Étapes

Une approche courante pour empêcher les robots est d’utiliser des tests Turing publics entièrement automatisés pour signaler les ordinateurs et les êtres humains. Un test Turing était à l’origine un test dans lequel quelqu’un devait décider si un partenaire de communication est un homme ou un ordinateur. Sur le Web, un CAPTCHA se compose généralement d’une image avec des lettres distordues. L’idée est que seul un humain peut lire les lettres sur l’image, alors que les algorithmes OCR échouent.

Cette approche présente plusieurs avantages et inconvénients, mais une discussion n’entre pas dans le cadre de ce didacticiel. Il existe toutefois un contrôle dans la boîte à outils de contrôle ASP.NET AJAX qui offre une approche similaire : `NoBot`. Il est plus facile à surmonter qu’un CAPTCHA, mais il est très facile à utiliser et les tarifs sont extrêmement bien sur les sites Web tels que les blogs où il est considéré comme un succès si la plupart des tentatives de courrier indésirable échouent, ce que le contrôle `NoBot` peut faire.

`NoBot` intercepte la publication du formulaire Web ASP.NET en cours si au moins l’une de ces conditions est remplie :

- Le navigateur ne parvient pas à résoudre un puzzle JavaScript (par exemple, lorsque JavaScript est désactivé)
- L’utilisateur a soumis le formulaire à rapide
- L’adresse IP du client a envoyé le formulaire trop souvent dans un laps de temps donné.

Pour vérifier ces conditions, le contrôle `NoBot` requiert ces attributs (tous facultatifs) :

- `ResponseMinimumDelaySeconds` le nombre minimal de secondes entre les publications
- `CutoffWindowSeconds` durée de l’intervalle de temps pendant laquelle les publications d’une adresse IP sont des mesures
- `CutoffMaximumInstances` nombre maximal de secondes par intervalle de temps

La balise suivante exige qu’au moins deux secondes s’écoulent entre les publications et qu’il n’y ait que cinq publications (postbacks) ou moins dans un intervalle de 30 secondes :

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

Puis, comme d’habitude, veillez à inclure la `ScriptManager` dans la page afin que la bibliothèque AJAX ASP.NET soit chargée et que la boîte à outils de contrôle puisse être utilisée :

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

Dans la mesure où la plupart des vérifications `NoBot` se produisent côté serveur, vous devez vérifier le résultat de ces validations. Pour ce faire, vous pouvez appeler la méthode `IsValid()` de `NoBot`. Elle possède un argument (en tant que paramètre `out``ByRef` paramètre) qui est de type `NoBotState`. Sa représentation sous forme de chaîne contient la raison de l’échec de la vérification et `Valid` dans le cas contraire. Le code suivant génère un message en fonction du résultat de `NoBot`:

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

Enfin, vous avez besoin d’un formulaire à envoyer et d’un élément label pour générer le message, et vous avez terminé !

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

Quand vous exécutez ce script et que vous désactivez JavaScript ou que vous envoyez le formulaire au cours des deux premières secondes ou que vous envoyez le formulaire sept fois dans les trente secondes, vous recevez un message d’erreur. Toutefois, utilisez ce contrôle avec prudence, car seulement environ 90-95% des utilisateurs ont activé JavaScript, par conséquent 5-10% des utilisateurs échoueront `NoBot`test.

[![ce message d’erreur a pu être provoqué par un bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)

Ce message d’erreur peut avoir été provoqué par un bot ([cliquez pour afficher l’image en taille réelle](fighting-bots-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](fighting-bots-cs.md)
