---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Tests unitaires des applications Signalr | Microsoft Docs
author: bradygaster
description: Cet article explique comment utiliser les fonctionnalités de test unitaire de Signalr 2,0.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: 2cf2e88f141d89971439dc1fc4979849f8dded47
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578672"
---
# <a name="unit-testing-signalr-applications"></a>Tests unitaires des applications SignalR

de [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cet article décrit l’utilisation des fonctionnalités de test unitaire de Signalr 2.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versions logicielles utilisées dans cette rubrique
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr version 2
>
>
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).

<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Tests unitaires des applications Signalr

Vous pouvez utiliser les fonctionnalités de test unitaire dans Signalr 2 pour créer des tests unitaires pour votre application Signalr. Signalr 2 comprend l’interface [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) , qui peut être utilisée pour créer un objet fictif afin de simuler vos méthodes de concentrateur pour le test.

Dans cette section, vous allez ajouter des tests unitaires pour l’application créée dans le [didacticiel prise en main](../getting-started/tutorial-getting-started-with-signalr.md) à l’aide de [xUnit.net](https://github.com/xunit/xunit) et [MOQ](https://github.com/Moq/moq4).

XUnit.net sera utilisé pour contrôler le test. MOQ sera utilisé pour créer un objet [fictif](http://en.wikipedia.org/wiki/Mock_object) à des fins de test. D’autres infrastructures factices peuvent être utilisées si vous le souhaitez. [NSubstitute](http://nsubstitute.github.io/) est également un bon choix. Ce didacticiel montre comment configurer l’objet factice de deux manières : tout d’abord, à l’aide d’un objet `dynamic` (introduit dans .NET Framework 4) et du second, à l’aide d’une interface.

### <a name="contents"></a>Contenu

Ce didacticiel contient les sections suivantes.

- [Tests unitaires avec Dynamic](#dynamic)
- [Tests unitaires par type](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Tests unitaires avec Dynamic

Dans cette section, vous allez ajouter un test unitaire pour l’application créée dans le [didacticiel prise en main](../getting-started/tutorial-getting-started-with-signalr.md) à l’aide d’un objet dynamique.

1. Installez l' [extension du testeur xUnit](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) pour Visual Studio 2013.
2. Terminez le [didacticiel prise en main](../getting-started/tutorial-getting-started-with-signalr.md)ou téléchargez l’application terminée à partir de [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Si vous utilisez la version de téléchargement de l’application Prise en main, ouvrez la **console du gestionnaire de package** , puis cliquez sur **restaurer** pour ajouter le package signalr au projet.

    ![Restaurer les packages](unit-testing-signalr-applications/_static/image1.png)
4. Ajoutez un projet à la solution pour le test unitaire. Dans **Explorateur de solutions** , cliquez avec le bouton droit sur votre solution, puis sélectionnez **Ajouter**, **nouveau projet...** . Sous le **C#** nœud, sélectionnez le nœud **Windows** . Sélectionnez **bibliothèque de classes**. Nommez le nouveau projet **TestLibrary** , puis cliquez sur **OK**.

    ![Créer une bibliothèque de tests](unit-testing-signalr-applications/_static/image2.png)
5. Ajoutez une référence dans le projet de bibliothèque de tests au projet SignalRChat. Cliquez avec le bouton droit sur le projet **TestLibrary** , puis sélectionnez **Ajouter**, **référence..** .. Sélectionnez le nœud **projets** sous le nœud de la **solution** , puis cochez **SignalRChat**. Cliquez sur **OK**.

    ![Ajouter une référence de projet](unit-testing-signalr-applications/_static/image3.png)
6. Ajoutez les packages Signalr, MOQ et XUnit au projet **TestLibrary** . Dans la **console du gestionnaire de package**, définissez la liste déroulante **projet par défaut** sur **TestLibrary**. Exécutez les commandes suivantes dans la fenêtre de console :

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Installer des packages](unit-testing-signalr-applications/_static/image4.png)
7. Créez le fichier de test. Cliquez avec le bouton droit sur le projet **TestLibrary** , puis cliquez sur **Ajouter...** , **classe**. Nommez la nouvelle classe **tests.cs**.
8. Remplacez le contenu de Tests.cs par le code suivant.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    Dans le code ci-dessus, un client de test est créé à l’aide de l’objet `Mock` de la bibliothèque [MOQ](https://github.com/Moq/moq4) , de type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (dans signalr 2,1, assignez `dynamic` pour le paramètre de type.) L’interface `IHubCallerConnectionContext` est l’objet proxy avec lequel vous appelez des méthodes sur le client. La fonction `broadcastMessage` est ensuite définie pour le client fictif afin qu’elle puisse être appelée par la classe `ChatHub`. Le moteur de test appelle ensuite la méthode `Send` de la classe `ChatHub`, qui à son tour appelle la fonction fictive `broadcastMessage`.
9. Générez la solution en appuyant sur **F6**.
10. Exécutez le test unitaire. Dans Visual Studio, sélectionnez **test**, **Windows**, **Explorateur de tests**. Dans la fenêtre Explorateur de tests, cliquez avec le bouton droit sur **HubsAreMockableViaDynamic** et sélectionnez **exécuter les tests sélectionnés**.

    ![Explorateur de tests](unit-testing-signalr-applications/_static/image5.png)
11. Vérifiez que le test a réussi en vérifiant le volet inférieur dans la fenêtre Explorateur de tests. La fenêtre indique que le test a réussi.

    ![Test réussi](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Tests unitaires par type

Dans cette section, vous allez ajouter un test pour l’application créée dans le [didacticiel prise en main](../getting-started/tutorial-getting-started-with-signalr.md) à l’aide d’une interface qui contient la méthode à tester.

1. Effectuez les étapes 1-7 dans le didacticiel [sur les tests unitaires avec dynamique](#dynamic) ci-dessus.
2. Remplacez le contenu de Tests.cs par le code suivant.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    Dans le code ci-dessus, une interface est créée et définit la signature de la méthode `broadcastMessage` pour laquelle le moteur de test créera un client fictif. Un client fictif est ensuite créé à l’aide de l’objet `Mock`, de type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (dans signalr 2,1, assignez `dynamic` pour le paramètre de type.) L’interface `IHubCallerConnectionContext` est l’objet proxy avec lequel vous appelez des méthodes sur le client.

    Le test crée ensuite une instance de `ChatHub`, puis crée une version fictive de la méthode `broadcastMessage`, qui est appelée à son tour en appelant la méthode `Send` sur le concentrateur.
3. Générez la solution en appuyant sur **F6**.
4. Exécutez le test unitaire. Dans Visual Studio, sélectionnez **test**, **Windows**, **Explorateur de tests**. Dans la fenêtre Explorateur de tests, cliquez avec le bouton droit sur **HubsAreMockableViaDynamic** et sélectionnez **exécuter les tests sélectionnés**.

    ![Explorateur de tests](unit-testing-signalr-applications/_static/image7.png)
5. Vérifiez que le test a réussi en vérifiant le volet inférieur dans la fenêtre Explorateur de tests. La fenêtre indique que le test a réussi.

    ![Test réussi](unit-testing-signalr-applications/_static/image8.png)
