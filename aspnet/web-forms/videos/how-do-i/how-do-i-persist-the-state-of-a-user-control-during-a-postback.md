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
# <a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a><span data-ttu-id="0aab4-103">[Comment faire] : rendre persistant l’état d’un contrôle utilisateur pendant une publication (postback)</span><span class="sxs-lookup"><span data-stu-id="0aab4-103">[How Do I]: Persist the State of a User Control During a Postback</span></span>

<span data-ttu-id="0aab4-104">par [Chris pixels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="0aab4-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="0aab4-105">Dans cette vidéo, Chris pixels montre comment rendre persistant l’état d’un ou plusieurs objets dans un contrôle utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0aab4-105">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="0aab4-106">Tout d’abord, un contrôle utilisateur est créé, qui représente la possibilité pour un utilisateur de spécifier des critères de filtre pour une recherche.</span><span class="sxs-lookup"><span data-stu-id="0aab4-106">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="0aab4-107">En outre, une classe de filtre auxiliaire est créée pour stocker les informations de filtre.</span><span class="sxs-lookup"><span data-stu-id="0aab4-107">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="0aab4-108">Plusieurs éléments d’interface utilisateur sont ajoutés au contrôle de filtre, ainsi que certaines méthodes et propriétés pour stocker les informations de filtre actuelles dans l’instance de classe de filtre.</span><span class="sxs-lookup"><span data-stu-id="0aab4-108">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="0aab4-109">Ensuite, la persistance des contrôles utilisateur est implémentée à l’aide de la méthode RegisterRequiresControlState et des méthodes Save/Restore associées.</span><span class="sxs-lookup"><span data-stu-id="0aab4-109">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="0aab4-110">Ces méthodes stockent l’instance de la classe de filtre et ses données lors des publications (postbacks) de page.</span><span class="sxs-lookup"><span data-stu-id="0aab4-110">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="0aab4-111">Enfin, il existe une discussion sur le stockage de plusieurs objets dans l’implémentation de l’état du contrôle.</span><span class="sxs-lookup"><span data-stu-id="0aab4-111">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="0aab4-112">&#9654;Regarder la vidéo (23 minutes)</span><span class="sxs-lookup"><span data-stu-id="0aab4-112">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
