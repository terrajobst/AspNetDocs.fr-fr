---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: Dans cette vidéo, Chris pixels montre comment rendre persistant l’état d’un ou plusieurs objets dans un contrôle utilisateur. Tout d’abord, vous créez un contrôle utilisateur qui représente le abilit...
ms.author: riande
ms.date: 04/02/2009
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: c87bd6c5c993a1bde8f8a84f6d53b431e54541d9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78635820"
---
# <a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[Comment faire] : rendre persistant l’état d’un contrôle utilisateur pendant une publication (postback)

par [Chris pixels](https://twitter.com/chrispels)

Dans cette vidéo, Chris pixels montre comment rendre persistant l’état d’un ou plusieurs objets dans un contrôle utilisateur. Tout d’abord, un contrôle utilisateur est créé, qui représente la possibilité pour un utilisateur de spécifier des critères de filtre pour une recherche. En outre, une classe de filtre auxiliaire est créée pour stocker les informations de filtre. Plusieurs éléments d’interface utilisateur sont ajoutés au contrôle de filtre, ainsi que certaines méthodes et propriétés pour stocker les informations de filtre actuelles dans l’instance de classe de filtre. Ensuite, la persistance des contrôles utilisateur est implémentée à l’aide de la méthode RegisterRequiresControlState et des méthodes Save/Restore associées. Ces méthodes stockent l’instance de la classe de filtre et ses données lors des publications (postbacks) de page. Enfin, il existe une discussion sur le stockage de plusieurs objets dans l’implémentation de l’état du contrôle.

[&#9654;Regarder la vidéo (23 minutes)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
