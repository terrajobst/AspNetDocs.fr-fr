---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement vers IIS en tant qu’environnement de test-5 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5d85232ff2cb229d771d517db7173721c9e277bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78635127"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement vers IIS en tant qu’environnement de test-5 sur 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour obtenir une présentation de la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui présente les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions SQL Server autres que SQL Server Compact et montre comment déployer vers Azure App Service Web Apps, consultez [déploiement Web ASP.net à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Présentation

Ce didacticiel montre comment déployer une application Web ASP.NET sur l’ordinateur local.

Lorsque vous développez une application, vous testez généralement en l’exécutant dans Visual Studio. Par défaut, cela signifie que vous utilisez le Serveur Visual Studio Development (également appelé Cassini). Le Serveur Visual Studio Development facilite le test pendant le développement dans Visual Studio, mais il ne fonctionne pas exactement comme IIS. Par conséquent, il est possible qu’une application s’exécute correctement lorsque vous la Testez dans Visual Studio, mais échoue quand elle est déployée sur IIS dans un environnement d’hébergement.

Vous pouvez tester votre application de manière plus fiable en procédant comme suit :

1. Utilisez IIS Express ou IIS complet au lieu du Serveur Visual Studio Development quand vous testez dans Visual Studio pendant le développement. Cette méthode émule généralement plus précisément la manière dont votre site s’exécutera sous IIS. Toutefois, cette méthode ne teste pas votre processus de déploiement et ne vérifie pas que le résultat du processus de déploiement s’exécutera correctement.
2. Déployez l’application sur votre ordinateur de développement à l’aide du même processus que celui que vous utiliserez ultérieurement pour le déployer dans votre environnement de production. Cette méthode valide votre processus de déploiement en plus de la validation que votre application s’exécutera correctement sous IIS.
3. Déployez l’application dans un environnement de test aussi proche que possible de votre environnement de production. Étant donné que l’environnement de production de ces didacticiels est un fournisseur d’hébergement tiers, l’environnement de test idéal serait un deuxième compte avec le fournisseur d’hébergement. Vous ne devez utiliser ce second compte que pour le test, mais il serait configuré de la même façon que le compte de production.

Ce didacticiel présente les étapes de l’option 2. Les instructions pour l’option 3 sont fournies à la fin du didacticiel [sur le déploiement dans l’environnement de production](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . à la fin de ce didacticiel, des liens vers des ressources sont disponibles pour l’option 1.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Configuration de l’application pour qu’elle s’exécute dans un niveau de confiance moyen

Avant d’installer IIS et de le déployer, vous devez modifier un paramètre de fichier Web. config afin que le site s’exécute plus comme dans un environnement d’hébergement partagé standard.

En général, les fournisseurs d’hébergement exécutent votre site Web dans un niveau de *confiance moyen*, ce qui signifie qu’il n’est pas autorisé de faire certaines choses. Par exemple, le code d’application ne peut pas accéder au registre Windows et ne peut pas lire ou écrire des fichiers qui se trouvent en dehors de la hiérarchie des dossiers de votre application. Par défaut, votre application s’exécute avec un *niveau de confiance élevé* sur votre ordinateur local, ce qui signifie que l’application peut être en mesure d’effectuer des opérations qui échouent lorsque vous la déployez en production. Par conséquent, pour que l’environnement de test reflète plus précisément l’environnement de production, vous configurez l’application pour qu’elle s’exécute dans un niveau de confiance moyen.

Dans le fichier Web. config de l’application, ajoutez un élément **Trust** dans l’élément **System. Web** , comme illustré dans cet exemple.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

L’application s’exécute maintenant dans un niveau de confiance moyen dans IIS, même sur votre ordinateur local. Ce paramètre vous permet d’intercepter le plus tôt possible toutes les tentatives de code d’application qui pourraient échouer en production.

> [!NOTE]
> Si vous utilisez Migrations Entity Framework Code First, assurez-vous que la version 5,0 ou une version ultérieure est installée. Dans Entity Framework version 4,3, les migrations requièrent une confiance totale pour mettre à jour le schéma de la base de données.

## <a name="installing-iis-and-web-deploy"></a>Installation d’IIS et de Web Deploy

Pour effectuer un déploiement sur IIS sur votre ordinateur de développement, IIS et Web Deploy doivent être installés. Celles-ci ne sont pas incluses dans la configuration par défaut de Windows 7. Si vous avez déjà installé IIS et Web Deploy, passez à la section suivante.

L’utilisation de l' [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) est la méthode recommandée pour installer iis et Web Deploy, car le Web Platform Installer installe une configuration recommandée pour IIS et installe automatiquement les composants requis pour iis et Web Deploy si nécessaire.

Pour exécuter Web Platform Installer pour installer IIS et Web Deploy, utilisez le lien suivant. Si vous avez déjà installé IIS, Web Deploy ou l’un de ses composants requis, le Web Platform Installer installe uniquement ce qui est manquant.

- [Installer IIS et Web Deploy à l’aide de WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Définition du pool d’applications par défaut sur .NET 4

Après l’installation d’IIS, exécutez le **Gestionnaire des services Internet** pour vous assurer que le .NET Framework version 4 est affecté au pool d’applications par défaut.

Dans le menu **Démarrer** de Windows, sélectionnez **exécuter**, entrez « inetmgr », puis cliquez sur **OK**. (Si la commande **exécuter** ne se trouve pas dans votre menu **Démarrer** , vous pouvez appuyer sur la touche Windows et R pour l’ouvrir. Ou cliquez avec le bouton droit sur la barre des tâches, cliquez sur **Propriétés**, sélectionnez l’onglet **menu Démarrer** , cliquez sur **personnaliser**, puis sélectionnez **exécuter la commande**.)

Dans le volet **connexions** , développez le nœud du serveur, puis sélectionnez **pools d’applications**. Dans le volet **pools d’applications** , si **DefaultAppPool** est affecté à la version 4 du .NET Framework, comme dans l’illustration suivante, passez à la section suivante.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Si vous ne voyez que deux pools d’applications et que les deux sont définis sur la .NET Framework 2,0, vous devez installer ASP.NET 4 dans IIS :

- Ouvrez une fenêtre d’invite de commandes en cliquant avec le bouton droit sur **invite de commandes** dans le menu **Démarrer** de Windows, puis en sélectionnant **exécuter en tant qu’administrateur**. Exécutez ensuite [aspnet\_regiis. exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) pour installer ASP.net 4 dans IIS, à l’aide des commandes suivantes. (Dans les systèmes 64 bits, remplacez « Framework » par « Framework64 ».)

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP. NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Cette commande crée de nouveaux pools d’applications pour le .NET Framework 4, mais le pool d’applications par défaut est toujours défini sur 2,0. Vous allez déployer une application qui cible .NET 4 vers ce pool d’applications. vous devez donc modifier le pool d’applications en .NET 4.

Si vous avez fermé le **Gestionnaire des services Internet**, réexécutez-le, développez le nœud du serveur, puis cliquez sur **pools d’applications** pour afficher à nouveau le volet **pools d’applications** .

Dans le volet **pools d’applications** , cliquez sur **DefaultAppPool**, puis dans le volet **actions** , cliquez sur **paramètres de base**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

Dans la boîte de dialogue **modifier le pool d’applications** , remplacez **.NET Framework version** par **.NET Framework v 4.0.30319** , puis cliquez sur **OK**.

[![Selecting_. NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Vous êtes maintenant prêt à publier sur IIS.

## <a name="publishing-to-iis"></a>Publication sur IIS

Vous pouvez déployer de plusieurs façons à l’aide de Visual Studio 2010 et Web Deploy :

- Utilisez la publication en un clic de Visual Studio.
- Créez un *package de déploiement* et installez-le à l’aide de l’interface utilisateur du gestionnaire des services Internet. Le package de déploiement se compose d’un fichier *. zip* qui contient tous les fichiers et les métadonnées nécessaires à l’installation d’un site dans IIS.
- Créez un package de déploiement et installez-le à l’aide de la ligne de commande.

Le processus que vous avez parcouru dans les didacticiels précédents pour configurer Visual Studio afin d’automatiser les tâches de déploiement s’applique à toutes ces trois méthodes. Dans ces didacticiels, vous allez utiliser la première de ces méthodes. Pour plus d’informations sur l’utilisation des packages de déploiement, consultez [mappage de contenu de déploiement ASP.net](https://msdn.microsoft.com/library/bb386521.aspx).

Avant la publication, assurez-vous que vous exécutez Visual Studio en mode administrateur. (Dans le menu **Démarrer** de Windows 7, cliquez avec le bouton droit sur l’icône de la version de Visual Studio que vous utilisez et sélectionnez **exécuter en tant qu’administrateur**.) Le mode administrateur est requis pour la publication uniquement lorsque vous publiez sur l’ordinateur local.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet ContosoUniversity (pas sur le projet CONTOSOUNIVERSITY. dal), puis sélectionnez **publier**.

L'Assistant **Publier le site Web** s'ouvre.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Dans la liste déroulante, sélectionnez **&lt;nouveau...&gt;** .

Dans la boîte de dialogue **nouveau profil** , entrez « test », puis cliquez sur **OK**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Ce nom est le même que le nœud central du fichier de transformation Web. test. config que vous avez créé précédemment. Cette correspondance est ce qui entraîne l’application des transformations Web. test. config lorsque vous publiez à l’aide de ce profil.

L’Assistant passe automatiquement à l’onglet **connexion** .

Dans la zone **URL du service** , entrez *localhost*.

Dans la zone **site/application** , entrez *Default Web site/ContosoUniversity*.

Dans la zone **URL de destination** , entrez `http://localhost/ContosoUniversity`.

Le paramètre de l' **URL de destination** n’est pas obligatoire. Une fois le déploiement de l’application terminé, Visual Studio ouvre automatiquement votre navigateur par défaut à cette URL. Si vous ne souhaitez pas que le navigateur s’ouvre automatiquement après le déploiement, laissez cette zone vide.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Cliquez sur **valider la connexion** pour vérifier que les paramètres sont corrects et que vous pouvez vous connecter à IIS sur l’ordinateur local.

Une coche verte vérifie que la connexion a réussi.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Cliquez sur **suivant** pour passer à l’onglet **paramètres** .

La zone de liste déroulante **configuration** spécifie la configuration de build à déployer. La valeur par défaut est Release, ce que vous voulez.

Laissez la case à cocher **Supprimer les fichiers supplémentaires à la destination** désactivée. Étant donné qu’il s’agit de votre premier déploiement, il n’y aura pas encore de fichiers dans le dossier de destination.

Dans la section **bases de données** , entrez la valeur suivante dans la zone chaîne de connexion pour **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Le processus de déploiement placera cette chaîne de connexion dans le fichier Web. config déployé, car **utiliser cette chaîne de connexion au moment** de l’exécution est sélectionné.

Également sous **SchoolContext**, sélectionnez **appliquer migrations code First**. Avec cette option, le processus de déploiement configure le fichier Web. config déployé pour spécifier l’initialiseur de `MigrateDatabaseToLatestVersion`. Cet initialiseur met automatiquement à jour la base de données avec la version la plus récente lorsque l’application accède pour la première fois à la base de données après le déploiement.

Dans la zone chaîne de connexion pour **DefaultConnection**, entrez la valeur suivante :

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Laissez **mettre à jour la base de données** vide. La base de données d’appartenance sera déployée en copiant le fichier. sdf dans l’application\_données, et vous ne souhaitez pas que le processus de déploiement fasse quoi que ce soit d’autre avec cette base de données.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Cliquez sur **suivant** pour passer à l’onglet **Aperçu** .

Dans l’onglet **Aperçu** , cliquez sur **Démarrer l’aperçu** pour afficher la liste des fichiers qui seront copiés.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Cliquez sur **Publier**.

Si Visual Studio n’est pas en mode administrateur, vous pouvez obtenir un message d’erreur qui indique une erreur d’autorisation. Dans ce cas, fermez Visual Studio, ouvrez-le en mode administrateur, puis réessayez de publier.

Si Visual Studio est en mode administrateur, la fenêtre **sortie** signale la réussite de la génération et de la publication.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Le navigateur s’ouvre automatiquement sur l’ordinateur local à partir de la page d’hébergement de Contoso University qui s’exécute dans IIS.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Test dans l’environnement de test

Notez que l’indicateur d’environnement affiche « (test) » au lieu de « (dev) », qui indique que la transformation *Web. config* de l’indicateur d’environnement a réussi.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Exécutez la page **students** pour vérifier que la base de données déployée ne dispose d’aucun étudiant. Lorsque vous sélectionnez cette page, le chargement peut prendre quelques minutes, car Code First crée la base de données, puis exécute la méthode `Seed`. (Ce n’est pas le cas lorsque vous étiez sur la page d’hébergement parce que l’application n’a pas encore essayé d’accéder à la base de données.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Exécutez la page des **enseignants** pour vérifier que code First amorcé la base de données avec les données de l’instructeur :

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Sélectionnez **Ajouter des élèves** dans le menu **élèves** , ajoutez un étudiant, puis affichez le nouvel étudiant dans la page des **étudiants** pour vérifier que vous pouvez écrire correctement dans la base de données :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Dans le menu **cours** , sélectionnez **mettre à jour les crédits**. La page **mettre à jour les crédits** requiert des autorisations d’administrateur, donc la page **de connexion** s’affiche. Entrez les informations d’identification du compte d’administrateur que vous avez créées précédemment (« admin » et « pas $ w0rd »). La page **mettre à jour les crédits** s’affiche, qui vérifie que le compte d’administrateur que vous avez créé dans le didacticiel précédent a été correctement déployé dans l’environnement de test.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Vérifiez qu’un dossier *ELMAH* existe uniquement avec le fichier d’espace réservé.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Examen des modifications automatiques de Web. config pour Migrations Code First

Ouvrez le fichier *Web. config* dans l’application déployée sur *C:\inetpub\wwwroot\ContosoUniversity* . vous pouvez voir où le processus de déploiement a configuré migrations code First pour mettre à jour automatiquement la base de données vers la dernière version.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Le processus de déploiement a également créé une nouvelle chaîne de connexion pour Migrations Code First à utiliser exclusivement pour la mise à jour du schéma de base de données :

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Cette chaîne de connexion supplémentaire vous permet de spécifier un compte d’utilisateur pour les mises à jour de schéma de base de données et un autre compte d’utilisateur pour l’accès aux données d’application. Par exemple, vous pouvez attribuer le rôle de propriétaire de la base de\_dB à Migrations Code First et DB\_DataReader et DB\_des rôles DataWriter à l’application. Il s’agit d’un modèle de défense en profondeur courant qui empêche le code potentiellement malveillant dans l’application de modifier le schéma de base de données. (Par exemple, cela peut se produire dans le cas d’une attaque par injection SQL réussie.) Ce modèle n’est pas utilisé par ces didacticiels. Il ne s’applique pas à SQL Server Compact, et il ne s’applique pas lorsque vous migrez vers SQL Server dans un didacticiel ultérieur de cette série. Le site Cytanium offre un seul compte d’utilisateur pour accéder à la base de données SQL Server que vous créez sur Cytanium. Si vous êtes en mesure d’implémenter ce modèle dans votre scénario, vous pouvez le faire en procédant comme suit :

1. Dans l’onglet **paramètres** de l’Assistant **publier le site Web** , entrez la chaîne de connexion qui spécifie un utilisateur disposant d’autorisations de mise à jour complète du schéma de base de données et désactivez la case à cocher **utiliser cette chaîne de connexion au moment** de l’exécution. Dans le fichier Web. config déployé, il s’agit de la chaîne de connexion `DatabasePublish`.
2. Créez une transformation de fichier Web. config pour la chaîne de connexion que vous souhaitez que l’application utilise au moment de l’exécution.

Vous avez maintenant déployé votre application sur IIS sur votre ordinateur de développement et vous l’avez testée ici. Cela permet de vérifier que le processus de déploiement a copié le contenu de l’application à l’emplacement approprié (à l’exclusion des fichiers que vous ne souhaitez pas déployer) et que Web Deploy configuré IIS correctement au cours du déploiement. Dans le didacticiel suivant, vous allez exécuter un test supplémentaire qui recherche une tâche de déploiement qui n’a pas encore été effectuée : définition des autorisations de dossier sur le dossier *ELMAH* .

## <a name="more-information"></a>Plus d'informations

Pour plus d’informations sur l’exécution d’IIS ou de IIS Express dans Visual Studio, consultez les ressources suivantes :

- [IIS Express vue d’ensemble](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) du site IIS.net.
- [Présentation de IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) sur le blog de Scott Guthrie.
- [Comment : spécifier le serveur Web pour les projets Web dans Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Principales différences entre IIS et le serveur de développement ASP.net](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) sur le site ASP.net.
- [Testez votre Application ASP.NET MVC ou Web Forms sur IIS 7 en 30 secondes](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) sur le blog de Rick Anderson. Cette entrée fournit des exemples de la façon dont les tests avec le Serveur Visual Studio Development (Cassini) ne sont pas aussi fiables que les tests dans IIS Express et pourquoi le test dans IIS Express n’est pas aussi fiable que le test dans IIS.

Pour plus d’informations sur les problèmes susceptibles de survenir lorsque votre application s’exécute dans un niveau de confiance moyen, consultez [hébergement d’Applications ASP.net en confiance moyenne](http://www.4guysfromrolla.com/articles/100307-1.aspx) sur les 4 Guys à partir du site Rolla.

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
