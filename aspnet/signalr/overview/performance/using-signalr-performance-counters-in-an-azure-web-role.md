---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Utilisation des compteurs de performance Signalr dans un rôle Web Azure | Microsoft Docs
author: guardrex
description: Comment installer et utiliser les compteurs de performance Signalr dans un rôle Web Azure.
keywords: ASP. NET, signalr, compteur de performances, rôle Web Azure
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 969a2ce43a7cb8d649555daf282f900401c0c914
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578980"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Utilisation des compteurs de performance Signalr dans un rôle Web Azure

Par [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Les compteurs de performance signalr permettent de surveiller les performances de votre application dans un rôle Web Azure. Les compteurs sont capturés par diagnostics Microsoft Azure. Vous installez les compteurs de performance Signalr sur Azure avec *signalr. exe*, le même outil que celui utilisé pour les applications autonomes ou locales. Étant donné que les rôles Azure sont temporaires, vous configurez une application pour installer et enregistrer les compteurs de performance Signalr lors du démarrage.

## <a name="prerequisites"></a>Conditions préalables requises

* Visual Studio 2015 ou [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
* [Microsoft Azure SDK pour Visual Studio](https://azure.microsoft.com/downloads/) **Remarque : redémarrez votre ordinateur après avoir installé le kit de développement logiciel (SDK).**
* Abonnement Microsoft Azure : pour vous inscrire pour obtenir un compte d’évaluation Azure gratuit, consultez la page [version d’évaluation gratuite d’Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Création d’une application de rôle Web Azure qui expose les compteurs de performance Signalr

1. Ouvrez Visual Studio.

2. Dans Visual Studio, sélectionnez **Fichier** > **Nouveau** > **Projet**.

3. Dans la boîte de dialogue **nouveau projet** , sélectionnez la catégorie **visuel C#**  > **Cloud** sur la gauche, puis sélectionnez le modèle de **service Cloud Azure** . Nommez l’application **SignalRPerfCounters** et sélectionnez **OK**.

   ![Nouvelle application Cloud](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > Si vous ne voyez pas la catégorie de modèle **Cloud** ou le modèle de **service Cloud Azure** , vous devez installer la charge de travail de **développement Azure** pour Visual Studio 2017. Cliquez sur le lien **ouvrir le Visual Studio installer** en bas à gauche de la boîte de dialogue **nouveau projet** pour ouvrir Visual Studio installer. Sélectionnez la charge de travail **développement Azure** , puis choisissez **modifier** pour commencer l’installation de la charge de travail.
   >
   > ![Charge de travail de développement Azure dans Visual Studio Installer](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. Dans la boîte de dialogue **nouveau service Cloud Microsoft Azure** , sélectionnez **rôle Web ASP.net** et sélectionnez le bouton > pour ajouter le rôle au projet. Sélectionnez **OK**.

   ![Ajouter un rôle Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. Dans la boîte de dialogue **nouvelle application Web ASP.net-WebRole1** , sélectionnez le modèle **MVC** , puis sélectionnez **OK**.

   ![Ajouter MVC et l’API Web](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. Dans **Explorateur de solutions**, ouvrez le fichier *Diagnostics. wadcfgx* sous **WebRole1**.

   ![Diagnostics de Explorateur de solutions. wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. Remplacez le contenu du fichier par la configuration suivante et enregistrez le fichier :

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. Ouvrez la **console du gestionnaire de package** dans **Outils** > **Gestionnaire de package NuGet**. Entrez les commandes suivantes pour installer la dernière version de Signalr et du package d’utilitaires Signalr :

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. Configurez l’application pour installer les compteurs de performance Signalr dans l’instance de rôle lors du démarrage ou du recyclage. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **WebRole1** et sélectionnez **Ajouter** > **nouveau dossier**. Nommez le nouveau dossier *Startup*.

   ![Ajouter un dossier de démarrage](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. Copiez le fichier *signalr. exe* (ajouté avec le package **Microsoft. Aspnet. signalr. utils** ) à partir du dossier du projet \<>/SignalRPerfCounters/packages/Microsoft.Aspnet.SignalR.utils.\<version >/Tools dans le dossier de *démarrage* que vous avez créé à l’étape précédente.

11. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Startup* , puis sélectionnez **Ajouter** > **élément existant**. Dans la boîte de dialogue qui s’affiche, sélectionnez *signalr. exe* , puis sélectionnez **Ajouter**.

    ![Ajouter signalr. exe au projet](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. Cliquez avec le bouton droit sur le dossier de *démarrage* que vous avez créé. Sélectionnez **Ajouter** > **Nouvel élément**. Sélectionnez le nœud **général** , sélectionnez **fichier texte**, puis nommez le nouvel élément *SignalRPerfCounterInstall. cmd*. Ce fichier de commandes installe les compteurs de performance Signalr dans le rôle Web.

    ![Créer un fichier de commandes d’installation du compteur de performance Signalr](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. Lorsque Visual Studio crée le fichier *SignalRPerfCounterInstall. cmd* , il s’ouvre automatiquement dans la fenêtre principale. Remplacez le contenu du fichier par le script suivant, puis enregistrez et fermez le fichier. Ce script exécute *signalr. exe*, qui ajoute les compteurs de performance signalr à l’instance de rôle.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. Sélectionnez le fichier *signalr. exe* dans **Explorateur de solutions**. Dans les **Propriétés**du fichier, affectez à **copier dans le répertoire de sortie** la valeur **toujours copier**.

    ![Définir la copie dans le répertoire de sortie sur toujours copier](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. Répétez l’étape précédente pour le fichier *SignalRPerfCounterInstall. cmd* .

16. Cliquez avec le bouton droit sur le fichier *SignalRPerfCounterInstall. cmd* , puis sélectionnez **Ouvrir avec**. Dans la boîte de dialogue qui s’affiche, sélectionnez **Éditeur binaire** et sélectionnez **OK**.

    ![Ouvrir avec l’éditeur binaire](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. Dans l’éditeur binaire, sélectionnez des octets de début dans le fichier et supprimez-les. Enregistrez et fermez le fichier.

    ![Supprimer les octets de début](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. Ouvrez *fichier ServiceDefinition. csdef* et ajoutez une tâche de démarrage qui exécute le fichier *SignalrPerfCounterInstall. cmd* au démarrage du service :

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. Ouvrez `Views/Shared/_Layout.cshtml` et supprimez le script d’offre groupée jQuery à la fin du fichier.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. Ajoutez un client JavaScript qui appelle en continu la méthode `increment` sur le serveur. Ouvrez `Views/Home/Index.cshtml` et remplacez le contenu par le code suivant :

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. Créez un nouveau dossier dans le projet **WebRole1** nommé *hubs*. Cliquez avec le bouton droit sur le dossier *hubs* dans **Explorateur de solutions** , puis sélectionnez **Ajouter** > **nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez la catégorie **Web** > **signalr** , puis sélectionnez le modèle d’élément **classe de concentrateur signalr (v2)** . Nommez le nouveau hub *MyHub.cs* et sélectionnez **Ajouter**.

    ![Ajout de la classe de concentrateur Signalr au dossier hubs dans la boîte de dialogue Ajouter un nouvel élément](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* s’ouvre automatiquement dans la fenêtre principale. Remplacez le contenu par le code suivant, puis enregistrez et fermez le fichier :

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. *[Manivelle. exe](signalr-connection-density-testing-with-crank.md)* est un outil de test de densité de connexion fourni avec le code base de signalr. Étant donné que manivelle requiert une connexion permanente, vous en ajoutez une à votre site pour l’utiliser lors du test. Ajoutez un nouveau dossier au projet **WebRole1** appelé *PersistentConnections*. Cliquez avec le bouton droit sur ce dossier, puis sélectionnez **ajouter** > **classe**. Nommez le nouveau fichier de classe *MyPersistentConnections.cs* et sélectionnez **Ajouter**.

24. Visual Studio ouvre le fichier *MyPersistentConnections.cs* dans la fenêtre principale. Remplacez le contenu par le code suivant, puis enregistrez et fermez le fichier :

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. À l’aide de la classe `Startup`, les objets Signalr démarrent au démarrage de OWIN. Ouvrez ou créez *Startup.cs* et remplacez le contenu par le code suivant :

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    Dans le code ci-dessus, l’attribut `OwinStartup` marque cette classe pour démarrer OWIN. La méthode `Configuration` démarre Signalr.

26. Testez votre application dans le Microsoft Azure Emulator en appuyant sur **F5**.

    > [!NOTE]
    > Si vous rencontrez un **FileLoadException** sur **MapSignalR**, modifiez les redirections de liaison dans *Web. config* comme suit :

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. Attendez environ une minute. Ouvrez la fenêtre outil Cloud Explorer dans Visual Studio (**affichez** > **Cloud Explorer**) et développez le chemin `(Local)/Storage Accounts/(Development)/Tables`. Double-cliquez sur **WADPerformanceCountersTable**. Vous devez voir les compteurs Signalr dans les données de la table. Si vous ne voyez pas la table, vous devrez peut-être entrer à nouveau vos informations d’identification Azure Storage. Vous devrez peut-être sélectionner le bouton **Actualiser** pour afficher la table dans **Cloud Explorer** ou sélectionner le bouton **Actualiser** dans la fenêtre ouvrir une table pour afficher les données dans la table.

    ![Sélection de la table des compteurs de performance WAD dans Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Indication des compteurs collectés dans le tableau des compteurs de performance WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. Pour tester votre application dans le Cloud, mettez à jour le fichier **ServiceConfiguration. Cloud. cscfg** et définissez la `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` sur une chaîne de connexion de compte de stockage Azure valide.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Déployez l’application dans votre abonnement Azure. Pour plus d’informations sur le déploiement d’une application sur Azure, consultez [comment créer et déployer un service Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Patientez quelques minutes. Dans **Cloud Explorer**, recherchez le compte de stockage que vous avez configuré ci-dessus et recherchez la table `WADPerformanceCountersTable`. Vous devez voir les compteurs Signalr dans les données de la table. Si vous ne voyez pas la table, vous devrez peut-être entrer à nouveau vos informations d’identification Azure Storage. Vous devrez peut-être sélectionner le bouton **Actualiser** pour afficher la table dans **Cloud Explorer** ou sélectionner le bouton **Actualiser** dans la fenêtre ouvrir une table pour afficher les données dans la table.

Merci à [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) pour le contenu d’origine utilisé dans ce didacticiel.
