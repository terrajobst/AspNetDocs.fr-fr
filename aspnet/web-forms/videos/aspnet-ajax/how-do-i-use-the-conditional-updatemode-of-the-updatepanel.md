---
uid: web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
title: '[Comment faire :] Utiliser la UpdateMode conditionnelle de l’UpdatePanel ? | Microsoft Docs'
author: JoeStagner
description: ASP.NET AJAX UpdatePanel comprend une propriété UpdateMode qui peut avoir la valeur’Always’ou’Conditional'. La valeur par défaut est toujours, auquel cas le UpdatePan...
ms.author: riande
ms.date: 08/01/2007
ms.assetid: 10b5bad3-4c18-464f-9454-0b3e60b7b8be
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
msc.type: video
ms.openlocfilehash: c05d4f262d56dfba858443b830d72ff0520b65d7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628645"
---
# <a name="how-do-i-use-the-conditional-updatemode-of-the-updatepanel"></a><span data-ttu-id="9438b-105">[Comment faire :] Utiliser la UpdateMode conditionnelle de l’UpdatePanel ?</span><span class="sxs-lookup"><span data-stu-id="9438b-105">[How Do I:] Use the Conditional UpdateMode of the UpdatePanel?</span></span>

<span data-ttu-id="9438b-106">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="9438b-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="9438b-107">ASP.NET AJAX UpdatePanel comprend une propriété UpdateMode qui peut avoir la valeur’Always’ou’Conditional'.</span><span class="sxs-lookup"><span data-stu-id="9438b-107">The ASP.NET AJAX UpdatePanel includes an UpdateMode property that may be set to 'Always' or 'Conditional'.</span></span> <span data-ttu-id="9438b-108">La valeur par défaut est toujours, auquel cas l’UpdatePanel met toujours à jour son contenu pendant une publication (postback) asynchrone.</span><span class="sxs-lookup"><span data-stu-id="9438b-108">The default is Always, in which case the UpdatePanel will always update its content during an asynchronous postback.</span></span> <span data-ttu-id="9438b-109">Dans cette vidéo, nous allons apprendre comment définir UpdateMode sur Conditional. dans ce cas, UpdatePanel met à jour son contenu uniquement lorsque notre code côté serveur appelle sa méthode Update.</span><span class="sxs-lookup"><span data-stu-id="9438b-109">In this video we learn how we can set the UpdateMode to Conditional, in which case the UpdatePanel will only update its content when our server-side code calls its Update method.</span></span> <span data-ttu-id="9438b-110">Cela vous permet d’utiliser une logique conditionnelle dans C# votre code ou Visual Basic pour déterminer si UpdatePanel met à jour son contenu pendant la publication (postback) asynchrone actuelle.</span><span class="sxs-lookup"><span data-stu-id="9438b-110">This allows you to use conditional logic in your C# or Visual Basic code to determine whether the UpdatePanel will update its content during the current asynchronous postback.</span></span>

[<span data-ttu-id="9438b-111">&#9654;Regarder la vidéo (13 minutes)</span><span class="sxs-lookup"><span data-stu-id="9438b-111">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-conditional-updatemode-of-the-updatepanel)

> [!div class="step-by-step"]
> <span data-ttu-id="9438b-112">[Précédent](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
> [Suivant](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span><span class="sxs-lookup"><span data-stu-id="9438b-112">[Previous](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
[Next](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span></span>
