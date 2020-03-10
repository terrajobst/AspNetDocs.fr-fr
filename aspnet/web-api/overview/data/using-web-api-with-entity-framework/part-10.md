---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publier l’application sur Azure Azure App Service | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: a9a7b74a07c71b47253c0af304c7a26ffa4eaae5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622387"
---
# <a name="publish-the-app-to-azure-azure-app-service"></a>Publier l’application sur Azure Azure App Service

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

La dernière étape consiste à publier l’application sur Azure. Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **publier**.

![](part-10/_static/image1.png)

Cliquez sur **publier** pour appeler la boîte de dialogue **publier le site Web** . Si vous avez activé l' **hôte dans le Cloud lors de** la création initiale du projet, la connexion et les paramètres sont déjà configurés. Dans ce cas, cliquez simplement sur l’onglet **paramètres** et cochez &quot;exécuter migrations code First&quot;. (Si vous n’avez pas vérifié l' **hôte dans le Cloud** au début, suivez les étapes de la [section suivante](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Pour déployer l’application, cliquez sur **publier**. Vous pouvez afficher la progression de la publication dans la fenêtre **activité de publication Web** . (Dans le menu **affichage** , sélectionnez **autres fenêtres**, puis sélectionnez **activité de publication Web**.)

![](part-10/_static/image4.png)

Une fois que Visual Studio a terminé le déploiement de l’application, le navigateur par défaut s’ouvre automatiquement à l’URL du site Web déployé, et l’application que vous avez créée s’exécute maintenant dans le Cloud. L’URL dans la barre d’adresse du navigateur indique que le site est chargé à partir d’Internet.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Déploiement sur un nouveau site Web

Si vous n’avez pas activé l' **ordinateur hôte dans le Cloud lors de** la création initiale du projet, vous pouvez configurer une nouvelle application Web maintenant. Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **publier**. Sélectionnez l’onglet **Profil** , puis cliquez sur **Microsoft Azure sites Web**. Si vous n’êtes pas actuellement connecté à Azure, vous êtes invité à vous connecter.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

Dans la boîte de dialogue **sites Web existants** , cliquez sur **nouveau**.

![](part-10/_static/image9.png)

Entrez un nom de site. Sélectionnez votre abonnement Azure et la région. Sous **serveur de base de données**, sélectionnez **créer un nouveau serveur**ou sélectionnez un serveur existant. Cliquez sur **Créer**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Cliquez sur l’onglet **paramètres** et cochez &quot;exécuter migrations code First&quot;. Ils cliquent ensuite sur **Publier**.

> [!div class="step-by-step"]
> [Précédent](part-9.md)
