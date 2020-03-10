---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Détection de la classe de démarrage OWIN | Microsoft Docs
author: Praburaj
description: Ce didacticiel montre comment configurer la classe de démarrage OWIN chargée. Pour plus d’informations sur OWIN, consultez vue d’ensemble du projet Katana. Ce didacticiel était...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: e1670c32049ec33dd4d1941a091a429d3929c452
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617046"
---
# <a name="owin-startup-class-detection"></a>Détection de classe de démarrage OWIN

> Ce didacticiel montre comment configurer la classe de démarrage OWIN chargée. Pour plus d’informations sur OWIN, consultez [vue d’ensemble du projet Katana](an-overview-of-project-katana.md). Ce didacticiel a été rédigé par Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan et Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).
>
> ## <a name="prerequisites"></a>Conditions préalables requises
>
> [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)

## <a name="owin-startup-class-detection"></a>Détection de classe de démarrage OWIN

 Chaque application OWIN a une classe Startup dans laquelle vous spécifiez les composants du pipeline d’application. Vous pouvez connecter votre classe Startup à l’exécution de différentes manières, selon le modèle d’hébergement que vous choisissez (OwinHost, IIS et IIS-Express). La classe Startup présentée dans ce didacticiel peut être utilisée dans toutes les applications d’hébergement. Vous connectez la classe Startup au runtime d’hébergement à l’aide de l’une des approches suivantes :

1. **Convention d’affectation de noms**: Katana recherche une classe nommée `Startup` dans l’espace de noms correspondant au nom de l’assembly ou à l’espace de noms global.
2. **Attribut OwinStartup**: il s’agit de l’approche adoptée par la plupart des développeurs pour spécifier la classe Startup. L’attribut suivant définit la classe Startup sur la classe `TestStartup` de l’espace de noms `StartupDemo`.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   L’attribut `OwinStartup` remplace la Convention d’affectation de noms. Vous pouvez également spécifier un nom convivial avec cet attribut. Toutefois, l’utilisation d’un nom convivial nécessite également l’utilisation de l’élément `appSetting` dans le fichier de configuration.
3. **Élément AppSetting dans le fichier de configuration**: l’élément `appSetting` remplace l’attribut `OwinStartup` et la Convention d’affectation des noms. Vous pouvez avoir plusieurs classes de démarrage (chacune utilisant un attribut `OwinStartup`) et configurer la classe de démarrage qui sera chargée dans un fichier de configuration à l’aide d’un balisage similaire à ce qui suit :

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   La clé suivante, qui spécifie explicitement la classe de démarrage et l’assembly peut également être utilisée :

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   Le code XML suivant dans le fichier de configuration spécifie un nom de classe de démarrage convivial de `ProductionConfiguration`.

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   Le balisage ci-dessus doit être utilisé avec l’attribut `OwinStartup` suivant qui spécifie un nom convivial et provoque l’exécution de la classe `ProductionStartup2`.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Pour désactiver la découverte du démarrage OWIN, ajoutez la `appSetting owin:AutomaticAppStartup` avec la valeur `"false"` dans le fichier Web. config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Créer une application Web ASP.NET à l’aide du démarrage OWIN

1. Créez une application Web Asp.Net vide et nommez-la **StartupDemo**. -Installer `Microsoft.Owin.Host.SystemWeb` à l’aide du gestionnaire de package NuGet. Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis **console du gestionnaire de package**. Entrez la commande suivante :

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Ajoutez une classe de démarrage OWIN. Dans Visual Studio 2017, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter une classe**.-dans la boîte de dialogue **Ajouter un nouvel élément** , entrez *OWIN* dans le champ de recherche, puis remplacez le nom par Startup.cs, puis sélectionnez **Ajouter**.

     ![](owin-startup-class-detection/_static/image1.png)

   La prochaine fois que vous voudrez ajouter une *classe de démarrage Owin*, celle-ci sera disponible dans le menu **Ajouter** .

     ![](owin-startup-class-detection/_static/image2.png)

   Vous pouvez également cliquer avec le bouton droit sur le projet, sélectionner **Ajouter**, puis sélectionner **nouvel élément**, puis sélectionner la **classe de démarrage Owin**.

     ![](owin-startup-class-detection/_static/image3.png)

- Remplacez le code généré dans le fichier *Startup.cs* par le code suivant :

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  L’expression lambda `app.Use` est utilisée pour inscrire le composant d’intergiciel (middleware) spécifié dans le pipeline OWIN. Dans ce cas, nous allons configurer la journalisation des demandes entrantes avant de répondre à la demande entrante. Le paramètre `next` est le délégué ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt;) au composant suivant dans le pipeline. L’expression lambda `app.Run` raccorde le pipeline aux demandes entrantes et fournit le mécanisme de réponse.
     > [!NOTE]
     > Dans le code ci-dessus, nous avons commenté l’attribut `OwinStartup` et nous nous appuyons sur la Convention d’exécution de la classe nommée `Startup`.-Appuyez sur ***F5*** pour exécuter l’application. L’actualisation a été atteinte plusieurs fois.

    ![](owin-startup-class-detection/_static/image4.png) Remarque : le nombre indiqué dans les images de ce didacticiel ne correspond pas au nombre que vous voyez. La chaîne de millisecondes est utilisée pour afficher une nouvelle réponse quand vous actualisez la page.
  Vous pouvez voir les informations de trace dans la fenêtre **sortie** .

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Ajouter d’autres classes de démarrage

Dans cette section, nous allons ajouter une autre classe de démarrage. Vous pouvez ajouter plusieurs classes de démarrage OWIN à votre application. Par exemple, vous souhaiterez peut-être créer des classes de démarrage pour le développement, les tests et la production.

1. Créez une nouvelle classe de démarrage OWIN et nommez-la `ProductionStartup`.
2. Remplacez le code généré par ce qui suit :

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Appuyez sur CTRL + F5 pour exécuter l’application. L’attribut `OwinStartup` spécifie la classe de démarrage de production qui est exécutée.

    ![](owin-startup-class-detection/_static/image6.png)
4. Créez une autre classe de démarrage OWIN et nommez-la `TestStartup`.
5. Remplacez le code généré par ce qui suit :

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   La surcharge d’attribut `OwinStartup` ci-dessus spécifie `TestingConfiguration` comme nom *convivial* de la classe Startup.
6. Ouvrez le fichier *Web. config* et ajoutez la clé de démarrage de l’application OWIN qui spécifie le nom convivial de la classe Startup :

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Appuyez sur CTRL + F5 pour exécuter l’application. L’élément paramètres de l’application prend l’antécédent et la configuration de test est exécutée.

    ![](owin-startup-class-detection/_static/image7.png)
8. Supprimez le nom *convivial* de l’attribut `OwinStartup` dans la classe `TestStartup`.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Remplacez la clé de démarrage de l’application OWIN dans le fichier *Web. config* par ce qui suit :

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Rétablissez l’attribut `OwinStartup` de chaque classe sur le code d’attribut par défaut généré par Visual Studio :

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Chacune des clés de démarrage d’application OWIN ci-dessous entraîne l’exécution de la classe de production.

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    La dernière clé de démarrage spécifie la méthode de configuration de démarrage. La clé de démarrage de l’application OWIN suivante vous permet de modifier le nom de la classe de configuration pour `MyConfiguration`.

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Utilisation de Owinhost. exe

1. Remplacez le fichier Web. config par le balisage suivant :

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   La dernière clé gagne, donc dans ce cas `TestStartup` est spécifié.
2. Installez Owinhost à partir de l’PMC :

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Accédez au dossier d’application (le dossier contenant le fichier *Web. config* ) et, à l’invite de commandes, tapez :

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   La fenêtre de commande affiche :

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Lancez un navigateur avec l’URL `http://localhost:5000/`.

    ![](owin-startup-class-detection/_static/image8.png)

   OwinHost a respecté les conventions de démarrage indiquées ci-dessus.
5. Dans la fenêtre commande, appuyez sur entrée pour quitter OwinHost.
6. Dans la classe `ProductionStartup`, ajoutez l’attribut OwinStartup suivant qui spécifie un nom convivial de *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. À l’invite de commandes et tapez :

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   La classe de démarrage de production est chargée.
    ![](owin-startup-class-detection/_static/image9.png)

   Notre application possède plusieurs classes de démarrage et, dans cet exemple, nous avons différé la classe de démarrage à charger jusqu’à l’exécution.
8. Testez les options de démarrage du runtime suivantes :

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
