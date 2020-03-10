---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Envoi de données de formulaire HTML dans API Web ASP.NET : chargement de fichier et multipart MIME-ASP.NET 4. x'
author: MikeWasson
description: Ce didacticiel montre comment charger des fichiers sur une API Web. Elle explique également comment traiter les données MIME à parties multiples.
ms.author: riande
ms.date: 06/21/2012
ms.custom: seoapril2019
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: f5aaebb96f631dfb6b0da1fbca96cd93a6a7fe2d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557567"
---
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Envoi de données de formulaire HTML dans API Web ASP.NET : Téléchargement de fichiers et MIME à parties multiples

par [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Partie 2 : Téléchargement de fichiers et MIME à parties multiples

Ce didacticiel montre comment charger des fichiers sur une API Web. Elle explique également comment traiter les données MIME à parties multiples.

> [!NOTE]
> [Téléchargez le projet terminé](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).

Voici un exemple de formulaire HTML pour le chargement d’un fichier :

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Ce formulaire contient un contrôle d’entrée de texte et un contrôle d’entrée de fichier. Lorsqu’un formulaire contient un contrôle d’entrée de fichier, l’attribut **enctype** doit toujours être &quot;&quot;multipart/form-data, qui spécifie que le formulaire sera envoyé en tant que message MIME à parties multiples.

Le format d’un message MIME à parties multiples est plus facile à comprendre en examinant un exemple de demande :

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Ce message est divisé en deux *parties*, une pour chaque contrôle de formulaire. Les limites de la pièce sont indiquées par les lignes qui commencent par des tirets.

> [!NOTE]
> La limite de la partie comprend un composant aléatoire (&quot;41184676334&quot;) pour garantir que la chaîne limite n’apparaît pas accidentellement dans une partie de message.

Chaque partie de message contient un ou plusieurs en-têtes, suivis du contenu de la partie.

- L’en-tête Content-disposition comprend le nom du contrôle. Pour les fichiers, il contient également le nom de fichier.
- L’en-tête Content-type décrit les données de la partie. Si cet en-tête est omis, la valeur par défaut est text/plain.

Dans l’exemple précédent, l’utilisateur a chargé un fichier nommé GrandCanyon. jpg, avec le type de contenu image/JPEG ; et la valeur de l’entrée de texte était &quot;vacances d’été&quot;.

## <a name="file-upload"></a>Chargement de fichiers

Examinons à présent un contrôleur d’API Web qui lit les fichiers à partir d’un message MIME à parties multiples. Le contrôleur lira les fichiers de façon asynchrone. L’API Web prend en charge les actions asynchrones à l’aide du [modèle de programmation basé sur les tâches](https://msdn.microsoft.com/library/dd460693.aspx). Tout d’abord, voici le code si vous ciblez .NET Framework 4,5, qui prend en charge les mots clés **Async** et **await** .

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Notez que l’action du contrôleur n’utilise aucun paramètre. Cela est dû au fait que nous traiterons le corps de la requête à l’intérieur de l’action, sans appeler un formateur de type de média.

La méthode **IsMultipartContent** vérifie si la requête contient un message MIME à parties multiples. Si ce n’est pas le cas, le contrôleur retourne le code d’état HTTP 415 (type de média non pris en charge).

La classe **MultipartFormDataStreamProvider** est un objet d’assistance qui alloue des flux de fichiers pour les fichiers téléchargés. Pour lire le message MIME à parties multiples, appelez la méthode **ReadAsMultipartAsync** . Cette méthode extrait toutes les parties du message et les écrit dans les flux fournis par **MultipartFormDataStreamProvider**.

Une fois la méthode terminée, vous pouvez obtenir des informations sur les fichiers à partir de la propriété **FileData** , qui est une collection d’objets **MultipartFileData** .

- **MultipartFileData. FileName** est le nom de fichier local sur le serveur où le fichier a été enregistré.
- **MultipartFileData. Headers** contient l’en-tête du composant (*pas* l’en-tête de la demande). Vous pouvez l’utiliser pour accéder aux en-têtes Content\_disposition et Content-type.

Comme son nom l’indique, **ReadAsMultipartAsync** est une méthode asynchrone. Pour effectuer le travail une fois la méthode terminée, utilisez une [tâche de continuation](https://msdn.microsoft.com/library/ee372288.aspx) (.net 4,0) ou le mot clé **await** (.net 4,5).

Voici la version .NET Framework 4,0 du code précédent :

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Lecture des données de contrôle de formulaire

Le formulaire HTML que j’ai montré précédemment avait un contrôle Text Input.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Vous pouvez récupérer la valeur du contrôle à partir de la propriété **FormData** de **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** est une **NameValueCollection** qui contient des paires nom/valeur pour les contrôles de formulaire. La collection peut contenir des clés dupliquées. Prenons le formulaire suivant :

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Le corps de la demande peut se présenter comme suit :

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

Dans ce cas, la collection **FormData** contient les paires clé/valeur suivantes :

- voyage : aller-retour
- Options : ne pas arrêter
- Options : dates
- siège : fenêtre
