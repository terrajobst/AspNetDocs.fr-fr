---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Ajouter un nouvel élément à la base de données | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 692269a2c11e529af78f24feca74bba704b5b54b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557287"
---
# <a name="add-a-new-item-to-the-database"></a>Ajouter un nouvel élément à la base de données

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Dans cette section, vous allez ajouter la possibilité pour les utilisateurs de créer un nouveau livre. Dans App. js, ajoutez le code suivant au modèle de vue :

[!code-javascript[Main](part-9/samples/sample1.js)]

Dans index. cshtml, remplacez le balisage suivant :

[!code-html[Main](part-9/samples/sample2.html)]

Par :

[!code-html[Main](part-9/samples/sample3.html)]

Ce balisage crée un formulaire pour l’envoi d’un nouvel auteur. Les valeurs de la liste déroulante auteur sont liées aux données du `authors` observable dans le modèle de vue. Pour les autres entrées de formulaire, les valeurs sont liées aux données de la propriété `newBook` du modèle de vue.

Le gestionnaire d’envoi du formulaire est lié à la fonction `addBook` :

[!code-html[Main](part-9/samples/sample4.html)]

La fonction `addBook` lit les valeurs actuelles des entrées de formulaire liées aux données pour créer un objet JSON. Ensuite, il publie l’objet JSON dans `/api/books`.

> [!div class="step-by-step"]
> [Précédent](part-8.md)
> [Suivant](part-10.md)
