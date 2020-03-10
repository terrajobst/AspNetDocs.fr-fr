---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Appel de l’API Web à partir d’uneC#application Windows Phone 8 ()-ASP.net 4. x
author: rmcmurray
description: 'Didacticiel avec code : créez une application API Web ASP.NET dans ASP.NET 4. x qui fournit un catalogue de livres à une application Windows Phone 8.'
ms.author: riande
ms.date: 10/09/2013
ms.custom: seoapril2019
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: c5da14a6856f551343b6fb14f0aedc659e792f6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614736"
---
# <a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Appel de l’API web à partir d’une application Windows Phone 8 (C#)

par [Robert McMurray](https://github.com/rmcmurray)

Dans ce didacticiel, vous allez apprendre à créer un scénario complet de bout en bout constitué d’une application API Web ASP.NET qui fournit un catalogue de livres à une application Windows Phone 8.

### <a name="overview"></a>Présentation

Les services RESTful comme API Web ASP.NET simplifier la création d’applications basées sur HTTP pour les développeurs en extrayant l’architecture pour les applications côté serveur et côté client. Au lieu de créer un protocole propriétaire pour la communication, les développeurs d’API Web doivent simplement publier les méthodes HTTP nécessaires pour leur application (par exemple : obtenir, publier, PUT, supprimer) et les développeurs d’application cliente doivent uniquement utiliser méthodes HTTP nécessaires à leur application.

Dans ce didacticiel de bout en bout, vous allez apprendre à utiliser l’API Web pour créer les projets suivants :

- Dans la [première partie de ce didacticiel](#STEP1), vous allez créer une application API Web ASP.net qui prend en charge toutes les opérations de création, lecture, mise à jour et suppression (CRUD) pour gérer un catalogue de livres. Cette application utilisera l' [exemple de fichier XML (Books. Xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) à partir de MSDN.
- Dans la [deuxième partie de ce didacticiel](#STEP2), vous allez créer une application Windows Phone 8 interactive qui récupère les données à partir de votre application API Web.

#### <a name="prerequisites"></a>Conditions préalables requises

- Visual Studio 2013 avec le kit de développement logiciel (SDK) Windows Phone 8 installé
- Windows 8 ou version ultérieure sur un système 64 bits avec Hyper-V installé
- Pour obtenir la liste des conditions requises supplémentaires, consultez la section *configuration* requise de la page de téléchargement de [Windows Phone SDK 8,0](https://www.microsoft.com/download/details.aspx?id=35471) .

> [!NOTE]
> Si vous envisagez de tester la connectivité entre l’API Web et Windows Phone 8 projets sur votre système local, vous devez suivre les instructions de l’article *[connexion de l’émulateur Windows Phone 8 aux applications d’API Web sur un ordinateur local](https://go.microsoft.com/fwlink/?LinkId=324014)* pour configurer votre environnement de test.

<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Étape 1 : création du projet de librairie d’API Web

La première étape de ce didacticiel de bout en bout consiste à créer un projet d’API Web qui prend en charge toutes les opérations CRUD. Notez que vous allez ajouter le Windows Phone projet d’application à cette solution à l' [étape 2](#STEP2) de ce didacticiel.

1. Ouvrez **Visual Studio 2013**.
2. Cliquez sur **fichier**, sur **nouveau**, puis sur **projet**.
3. Quand la **boîte de dialogue Nouveau projet** s’affiche, développez **installé**, puis **modèles**, puis **visuel C#** , puis **Web**.

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Cliquez sur l’image à développer                                                                |

4. Mettez en surbrillance **application Web ASP.net**, entrez **Bookstore** comme nom de projet, puis cliquez sur **OK**.
5. Quand la boîte **de dialogue Nouveau projet ASP.net** s’affiche, sélectionnez le modèle **API Web** , puis cliquez sur **OK**.

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Cliquez sur l’image à développer                                                                |

6. Lorsque le projet d’API Web s’ouvre, supprimez l’exemple de contrôleur du projet :

    1. Développez le dossier **contrôleurs** dans l’Explorateur de solutions.
    2. Cliquez avec le bouton droit sur le fichier **ValuesController.cs** , puis cliquez sur **supprimer**.
    3. Cliquez sur **OK** lorsque vous êtes invité à confirmer la suppression.
7. Ajoutez un fichier de données XML au projet d’API Web. ce fichier contient le contenu du catalogue de la librairie :

   1. Cliquez avec le bouton droit sur le dossier de données de l' **application\_** dans l’Explorateur de solutions, puis cliquez sur **Ajouter**et sur **nouvel élément**.
   2. Quand la boîte de dialogue **Ajouter un nouvel élément** s’affiche, mettez en surbrillance le modèle de **fichier XML** .
   3. Nommez le fichier **books. xml**, puis cliquez sur **Ajouter**.
   4. Une fois le fichier **books. xml** ouvert, remplacez le code dans le fichier par le code XML du fichier Sample **books. xml** sur MSDN : 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Enregistrez et fermez le fichier XML.

8. Ajoutez le modèle Bookstore au projet d’API Web. ce modèle contient la logique de création, lecture, mise à jour et suppression (CRUD) pour l’application Bookstore :

   1. Cliquez avec le bouton droit sur le dossier **modèles** dans l’Explorateur de solutions, cliquez sur **Ajouter**, puis sur **classe**.
   2. Quand la boîte de dialogue **Ajouter un nouvel élément** s’affiche, nommez le fichier de classe **BookDetails.cs**, puis cliquez sur **Ajouter**.
   3. Une fois le fichier **BookDetails.cs** ouvert, remplacez le code dans le fichier par ce qui suit : 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Enregistrez et fermez le fichier **BookDetails.cs** .

9. Ajoutez le contrôleur Bookstore au projet d’API Web :

   1. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier **Controllers** , puis cliquez sur **Ajouter**et sur **contrôleur**.
   2. Quand la boîte de dialogue **Ajouter une structure** s’affiche, sélectionnez **contrôleur d’API Web 2-vide**, puis cliquez sur **Ajouter**.
   3. Quand la boîte de dialogue **Ajouter un contrôleur** s’affiche, nommez le contrôleur **BooksController**, puis cliquez sur **Ajouter**.
   4. Une fois le fichier **BooksController.cs** ouvert, remplacez le code dans le fichier par ce qui suit : 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Enregistrez et fermez le fichier **BooksController.cs** .

10. Générez l’application API Web pour rechercher les erreurs.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Étape 2 : ajout du projet de catalogue Windows Phone 8 Bookstore

L’étape suivante de ce scénario de bout en bout consiste à créer l’application de catalogue pour Windows Phone 8. Cette application utilise le modèle d’application *Windows Phone lié* pour l’interface utilisateur par défaut, et elle utilise l’application API Web que vous avez créée à l' [étape 1](#STEP1) de ce didacticiel comme source de données.

1. Cliquez avec le bouton droit sur la solution **Bookstore** dans l’Explorateur de solutions, puis cliquez sur **Ajouter**, puis sur **nouveau projet**.
2. Quand la boîte **de dialogue Nouveau projet** s’affiche, développez **installé**, puis **visuel C#** , puis **Windows Phone**.
3. Mettez en surbrillance **Windows Phone application liée aux DataBound**, entrez **BookCatalog** pour le nom, puis cliquez sur **OK**.
4. Ajoutez le package NuGet Json.NET au projet **BookCatalog** :

    1. Cliquez avec le bouton droit sur **références** pour le projet **BookCatalog** dans l’Explorateur de solutions, puis cliquez sur **gérer les packages NuGet**.
    2. Quand la boîte de dialogue **gérer les packages NuGet** s’affiche, développez la section **en ligne** et mettez en surbrillance **NuGet.org**.
    3. Entrez **JSON.net** dans le champ de recherche, puis cliquez sur l’icône de recherche.
    4. Mettez en surbrillance **JSON.net** dans les résultats de la recherche, puis cliquez sur **installer**.
    5. Une fois l’installation terminée, cliquez sur **Fermer**.
5. Ajoutez le modèle **BookDetails** au projet **BookCatalog** ; contient un modèle générique de la classe Bookstore :

   1. Cliquez avec le bouton droit sur le projet **BookCatalog** dans l’Explorateur de solutions, cliquez sur **Ajouter**, puis sur **nouveau dossier**.
   2. Nommez le nouveau dossier **modèles**.
   3. Cliquez avec le bouton droit sur le dossier **modèles** dans l’Explorateur de solutions, cliquez sur **Ajouter**, puis sur **classe**.
   4. Quand la boîte de dialogue **Ajouter un nouvel élément** s’affiche, nommez le fichier de classe **BookDetails.cs**, puis cliquez sur **Ajouter**.
   5. Une fois le fichier **BookDetails.cs** ouvert, remplacez le code dans le fichier par ce qui suit : 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Enregistrez et fermez le fichier **BookDetails.cs** .

6. Mettez à jour la classe **MainViewModel.cs** pour inclure la fonctionnalité de communication avec l’application de l’API Web Bookstore :

   1. Développez le dossier **ViewModels** dans l’Explorateur de solutions, puis double-cliquez sur le fichier **MainViewModel.cs** .
   2. Une fois le fichier **MainViewModel.cs** ouvert, remplacez le code dans le fichier par le code suivant : Notez que vous devrez mettre à jour la valeur de la constante `apiUrl` avec l’URL réelle de votre API Web : 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Enregistrez et fermez le fichier **MainViewModel.cs** .

7. Mettez à jour le fichier **MainPage. Xaml** pour personnaliser le nom de l’application :

   1. Double-cliquez sur le fichier **MainPage. Xaml** dans l’Explorateur de solutions.
   2. Lorsque le fichier **MainPage. Xaml** est ouvert, recherchez les lignes de code suivantes : 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Remplacez ces lignes par ce qui suit : 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Enregistrez et fermez le fichier **MainPage. Xaml** .

8. Mettez à jour le fichier **DetailsPage. Xaml** pour personnaliser les éléments affichés :

   1. Double-cliquez sur le fichier **DetailsPage. Xaml** dans l’Explorateur de solutions.
   2. Lorsque le fichier **DetailsPage. Xaml** est ouvert, recherchez les lignes de code suivantes : 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Remplacez ces lignes par ce qui suit : 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Enregistrez et fermez le fichier **DetailsPage. Xaml** .

9. Générez l’application Windows Phone pour vérifier les erreurs.

### <a name="step-3-testing-the-end-to-end-solution"></a>Étape 3 : test de la solution de bout en bout

Comme mentionné dans la section *conditions préalables* de ce didacticiel, lorsque vous testez la connectivité entre l’API web et Windows Phone 8 projets sur votre système local, vous devez suivre les instructions de l’article *[connexion de l’émulateur Windows Phone 8 aux applications API Web sur un ordinateur local](https://go.microsoft.com/fwlink/?LinkId=324014)* pour configurer votre environnement de test.

Une fois que l’environnement de test est configuré, vous devez définir l’application Windows Phone comme projet de démarrage. Pour ce faire, mettez en surbrillance l’application **BookCatalog** dans l’Explorateur de solutions, puis cliquez sur **définir comme projet de démarrage**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Cliquez sur l’image à développer |

Quand vous appuyez sur F5, Visual Studio démarre à la fois l’émulateur Windows Phone, qui affiche une &quot;Veuillez patienter&quot; message pendant que les données d’application sont récupérées à partir de votre API Web :

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Cliquez sur l’image à développer |

Si tout est correct, le catalogue doit s’afficher :

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Cliquez sur l’image à développer |

Si vous appuyez sur n’importe quel titre de livre, l’application affiche la description du livre :

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Cliquez sur l’image à développer |

Si l’application ne peut pas communiquer avec votre API Web, un message d’erreur s’affiche :

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Cliquez sur l’image à développer |

Si vous appuyez sur le message d’erreur, tous les détails supplémentaires sur l’erreur s’affichent :

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Cliquez sur l’image à développer                                                                 |
