---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement sur l’environnement de production-7 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: db838633accdedd7c0693b126a007e254ca681e4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74627191"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement sur l’environnement de production-7 sur 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour obtenir une présentation de la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui présente les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions SQL Server autres que SQL Server Compact et montre comment déployer vers Azure App Service Web Apps, consultez [déploiement Web ASP.net à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Vue d'ensemble de

Dans ce didacticiel, vous configurez un compte avec un fournisseur d’hébergement et déployez votre application Web ASP.NET dans l’environnement de production à l’aide de la fonctionnalité de publication en un clic de Visual Studio.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Sélection d’un fournisseur d’hébergement

Pour l’application Contoso University et cette série de didacticiels, vous avez besoin d’un fournisseur qui prend en charge ASP.NET 4 et Web Deploy. Une société d’hébergement spécifique a été choisie afin que les didacticiels illustrent l’expérience complète du déploiement sur un site Web en ligne. Chaque société d’hébergement fournit des fonctionnalités différentes, et l’expérience de déploiement sur leurs serveurs varie quelque peu. Toutefois, le processus décrit dans ce didacticiel est courant pour le processus global. Le fournisseur d’hébergement utilisé pour ce didacticiel, Cytanium.com, est l’un des nombreux disponibles, et son utilisation dans ce didacticiel ne constitue pas une approbation ou une recommandation.

Lorsque vous êtes prêt à sélectionner votre propre fournisseur d’hébergement, vous pouvez comparer les fonctionnalités et les prix dans la [Galerie de fournisseurs](https://www.microsoft.com/web/hosting) sur le site Microsoft.com/Web.

## <a name="creating-an-account"></a>Création d’un compte

Créez un compte sur le fournisseur sélectionné. Si la prise en charge d’une base de données SQL Server complète est un ajout supplémentaire, vous n’avez pas besoin de la sélectionner pour ce didacticiel, mais vous en aurez besoin pour le didacticiel [sur la migration vers SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) plus loin dans cette série.

Pour ces didacticiels, vous n’êtes pas obligé d’inscrire un nouveau nom de domaine. Vous pouvez effectuer un test pour vérifier que le déploiement a réussi à l’aide de l’URL temporaire attribuée au site par le fournisseur.

Une fois le compte créé, vous recevez généralement un e-mail de bienvenue contenant toutes les informations dont vous avez besoin pour déployer et gérer votre site. Les informations que votre fournisseur d’hébergement envoie vous seront similaires à ce qui est présenté ici. Le courrier électronique de bienvenue Cytanium envoyé aux nouveaux propriétaires de compte comprend les informations suivantes :

- URL vers le site du panneau de configuration du fournisseur, dans lequel vous pouvez gérer les paramètres de votre site. L’ID et le mot de passe que vous avez spécifiés sont inclus dans cette partie de l’e-mail de bienvenue afin d’en faciliter la référence. (Les deux ont été remplacées par une valeur de démonstration pour cette illustration.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- La version .NET Framework par défaut et les informations sur la façon de le modifier. La plupart des sites d’hébergement ont par défaut la valeur 2,0, qui fonctionne avec les applications ASP.NET qui ciblent le .NET Framework 2,0, 3,0 ou 3,5. Toutefois, Contoso University étant une application .NET Framework 4, vous devez modifier ce paramètre. (Pour une application ASP.NET 4,5, vous utiliseriez le paramètre .NET 4,0.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- URL temporaire que vous pouvez utiliser pour accéder à votre site Web. Lorsque ce compte a été créé, « contosouniversity.com » a été entré en tant que nom de domaine existant. Par conséquent, l’URL temporaire est `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informations sur la configuration des bases de données et les chaînes de connexion dont vous avez besoin pour y accéder :

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informations sur les outils et les paramètres pour le déploiement de votre site. (L’e-mail de Cytanium mentionne également WebMatrix, qui est omis ici.)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Définition de la version de .NET Framework

L’e-mail de bienvenue Cytanium contient un lien vers des instructions sur la façon de modifier la version du .NET Framework. Ces instructions expliquent que cela peut être effectué par le biais du panneau de configuration Cytanium. Les autres fournisseurs disposent de sites du panneau de configuration qui s’affichent différemment, ou ils peuvent vous indiquer de le faire d’une autre façon.

Accédez à l’URL du panneau de configuration. Une fois connecté avec votre nom d’utilisateur et votre mot de passe, le panneau de configuration s’affiche.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

Dans la zone **espaces d’hébergement** , maintenez le pointeur sur l’icône Web et sélectionnez **sites Web** dans le menu.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

Dans la zone **sites Web** , cliquez sur **contosouniversity.com** (le nom du site que vous avez utilisé lors de la création du compte).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

Dans la zone **Propriétés du site Web** , sélectionnez l’onglet **Extensions** .

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Remplacez ASP.NET du **pipeline intégré 2,0** par **4,0 (pipeline intégré)** , puis cliquez sur **mettre à jour**.

## <a name="publishing-to-the-hosting-provider"></a>Publication sur le fournisseur d’hébergement

L’e-mail de bienvenue du fournisseur d’hébergement comprend tous les paramètres dont vous avez besoin pour publier le projet, et vous pouvez entrer ces informations manuellement dans un profil de publication. Mais vous utiliserez une méthode plus simple et moins sujette aux erreurs pour configurer le déploiement sur le fournisseur : vous allez télécharger un fichier *. publishsettings* et l’importer dans un profil de publication.

Dans votre navigateur, accédez au panneau de configuration Cytanium, sélectionnez **Web** , puis sélectionnez **sites Web.**

![Panneau de configuration sélection de sites Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Sélectionnez le site Web **contosouniversity.com** .

![Panneau de configuration, sélection de contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Sélectionnez l’onglet **publication Web** .

![Onglet publication Web du panneau de configuration](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Créez les informations d’identification à utiliser pour la publication Web en entrant un nom d’utilisateur et un mot de passe. Vous pouvez entrer les mêmes informations d’identification que celles que vous utilisez pour ouvrir une session sur le panneau de configuration. Cliquez ensuite sur **activer**.

![Panneau de configuration-créer des informations d’identification de publication](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Cliquez sur **Télécharger le profil de publication pour ce site Web**.

![Panneau de configuration-Télécharger le profil de publication](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Lorsque vous êtes invité à ouvrir ou enregistrer le fichier, enregistrez-le.

![Enregistrer le fichier de profil de publication](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

Dans **Explorateur de solutions** dans Visual Studio, cliquez avec le bouton droit sur le projet ContosoUniversity et sélectionnez **publier**. La boîte de dialogue **publier le site Web** s’ouvre sous l’onglet **Aperçu** avec le profil de **test** sélectionné, car il s’agit du dernier profil que vous avez utilisé.

Sélectionnez l’onglet **Profil** , puis cliquez sur **Importer**.

![Bouton d’importation de l’Assistant publier le site Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

Dans la boîte de dialogue **Importer les paramètres de publication** , sélectionnez le fichier *. publishsettings* que vous avez téléchargé, puis cliquez sur **ouvrir**. L’Assistant passe à l’onglet connexion avec tous les champs renseignés.

![Onglet de connexion de l’Assistant publier le site Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Le fichier. publishsettings place l’URL permanente planifiée pour le site dans la zone URL de destination, mais si vous n’avez pas encore acheté ce domaine, remplacez la valeur par l’URL temporaire. Pour cet exemple, l’URL est  *[http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com).* Le seul objectif de cette zone est de spécifier l’URL à laquelle le navigateur s’ouvrira automatiquement après le déploiement. Si vous laissez le champ vide, la seule conséquence est que le navigateur ne démarre pas automatiquement après le déploiement.

Cliquez sur **valider la connexion** pour vérifier que les paramètres sont corrects et que vous pouvez vous connecter au serveur. Comme vous l’avez vu précédemment, une coche verte vérifie que la connexion a réussi.

Lorsque vous cliquez sur valider la connexion, une boîte de dialogue **erreur de certificat** peut s’afficher. Si vous le faites, vérifiez que le nom du serveur correspond à ce que vous attendez. Si c’est le cas, sélectionnez **enregistrer ce certificat pour les futures sessions de Visual Studio** , puis cliquez sur **accepter**. (Cette erreur signifie que le fournisseur d’hébergement a choisi d’éviter les dépenses liées à l’achat d’un certificat SSL pour l’URL vers laquelle vous effectuez le déploiement. Si vous préférez établir une connexion sécurisée à l’aide d’un certificat valide, contactez votre fournisseur d’hébergement.)

![Erreur de certificat](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Cliquez sur **Next**.

Dans la section **bases de données** de l’onglet **paramètres** , entrez les mêmes valeurs que celles que vous avez entrées pour le profil de publication de test. Vous trouverez les chaînes de connexion dont vous avez besoin dans les listes déroulantes.

- Dans la zone chaîne de connexion pour **SchoolContext,** sélectionnez `Data Source=|DataDirectory|School-Prod.sdf`
- Sous **SchoolContext**, sélectionnez **appliquer migrations code First**.
- Dans la zone chaîne de connexion pour **DefaultConnection**, sélectionnez `Data Source=|DataDirectory|aspnet-Prod.sdf`
- Sous **DefaultConnection**, laissez **mettre à jour la base de données** désactivée.

![Onglet Paramètres de l’Assistant publier le site Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Cliquez sur **Next**.

Dans l’onglet **Aperçu** , cliquez sur **Démarrer l’aperçu** pour afficher la liste des fichiers qui seront copiés. Vous voyez la même liste que celle que vous avez vue précédemment lorsque vous avez effectué le déploiement sur IIS sur l’ordinateur local.

Avant de publier, modifiez le nom du profil afin que votre fichier de transformation Web. production. config s’applique. Sélectionnez l’onglet **Profil** , puis cliquez sur **gérer les profils**.

![Assistant publier le site Web-gérer les profils](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

Dans la boîte de dialogue **modifier les profils de publication Web** , sélectionnez le profil de production, cliquez sur **Renommer**, puis remplacez le nom du profil par production. Cliquez ensuite sur **Fermer**.

![Boîte de dialogue Modifier les profils de publication Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Cliquez sur **Publier**.

L’application est publiée sur le fournisseur d’hébergement. Le résultat s’affiche dans la fenêtre **sortie** .

![Fenêtre sortie après le déploiement](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Le navigateur s’ouvre automatiquement à l’URL que vous avez entrée dans la zone **URL de destination** sous l’onglet **connexion** de l’Assistant **publier le site Web** . Vous voyez la même page d’hébergement que lorsque vous exécutez le site dans Visual Studio, à l’exception du fait qu’il n’y a pas d’indicateur d’environnement « (test) » ou « (dev) » dans la barre de titre. Cela indique que la transformation *Web. config* de l’indicateur d’environnement fonctionnait correctement.

> [!NOTE]
> Si vous voyez toujours « (test) » dans le titre, supprimez le dossier *obj* du projet ContosoUniversity et redéployez. Dans les versions préliminaires du logiciel, le fichier de transformation appliqué précédemment (Web. test. config) peut être appliqué à nouveau bien que vous utilisiez le profil de production.

[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Avant d’exécuter une page qui provoque l’accès à la base de données, assurez-vous que ELMAH peut consigner les erreurs qui se produisent.

## <a name="setting-folder-permissions-for-elmah"></a>Définition des autorisations de dossier pour ELMAH

Comme vous le savez dans le didacticiel précédent de cette série, vous devez vous assurer que l’application dispose des autorisations en écriture sur le dossier de votre application où ELMAH stocke les fichiers journaux des erreurs. Lorsque vous déployez sur IIS localement sur votre ordinateur, vous définissez ces autorisations manuellement. Dans cette section, vous allez apprendre à définir des autorisations sur Cytanium. (Certains fournisseurs d’hébergement peuvent ne pas vous permettre de le faire ; ils peuvent proposer un ou plusieurs dossiers prédéfinis avec des autorisations d’écriture. Dans ce cas, vous devez modifier votre application pour utiliser les dossiers spécifiés.)

Vous pouvez définir des autorisations sur les dossiers dans le panneau de configuration Cytanium. Accédez à l’URL du panneau de configuration et sélectionnez **Gestionnaire de fichiers**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

Dans la zone **Gestionnaire de fichiers** , sélectionnez **contosouniversity.com** , puis **wwwroot** pour afficher le dossier racine de l’application. Cliquez sur l’icône de cadenas en regard de **ELMAH**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

Dans la fenêtre autorisations de **fichier**/**dossier** , activez les cases à cocher **lecture** et **écriture** pour **contosouniversity.com** , puis cliquez sur **définir les autorisations**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Assurez-vous que ELMAH a accès en écriture au dossier *ELMAH* en générant une erreur, puis en affichant le rapport d’erreurs ELMAH. Demandez une URL non valide telle que *Studentsxxx. aspx*. Comme précédemment, vous voyez la page *GenericErrorPage. aspx* . Cliquez sur le lien **déconnexion** , puis exécutez *ELMAH. axd*. Vous recevez d’abord la page **de connexion** , ce qui confirme que la transformation *Web. config* a ajouté l’autorisation ELMAH. Une fois connecté, vous voyez le rapport qui affiche l’erreur que vous venez de vous produire.

[![ELMAH. axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Test dans l’environnement de production

Exécutez la page **students** . L’application tente d’accéder à la base de données School pour la première fois, ce qui déclenche Migrations Code First pour créer la base de données. Lorsque la page s’affiche après un certain délai, elle indique qu’il n’y a pas d’élèves.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Exécutez la page des **instructeurs** pour vérifier que les données de départ ont correctement inséré les données de l’instructeur dans la base de données.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Comme vous l’avez fait dans l’environnement de test, vous souhaitez vérifier que les mises à jour de base de données fonctionnent dans l’environnement de production, mais que vous ne souhaitez généralement pas entrer de données de test dans votre base de données de production. Pour ce didacticiel, vous allez utiliser la même méthode que dans le test. Toutefois, dans une application réelle, vous souhaiterez peut-être trouver une méthode qui valide la réussite des mises à jour de la base de données sans introduire de données de test dans la base de données de production. Dans certaines applications, il peut être pratique d’ajouter quelque sorte, puis de le supprimer.

Ajoutez un étudiant, puis affichez les données que vous avez entrées dans la page des **étudiants** pour vérifier que vous pouvez mettre à jour les données dans la base de données.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Vérifiez que les règles d’autorisation fonctionnent correctement en sélectionnant **mettre à jour les crédits** dans le menu **cours** . La page **de connexion** s’affiche. Entrez les informations d’identification de votre compte d’administrateur, cliquez sur **se connecter pour**afficher la page **mettre à jour les crédits** .

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Si la connexion est établie, la page **mettre à jour les crédits** s’affiche. Cela indique que la base de données d’appartenance ASP.NET (avec le compte d’administrateur unique) a été déployée avec succès.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Vous avez maintenant déployé et testé votre site avec succès et il est disponible publiquement sur Internet.

## <a name="creating-a-more-reliable-test-environment"></a>Création d’un environnement de test plus fiable

Comme expliqué dans le didacticiel [sur le déploiement dans l’environnement de test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) , l’environnement de test le plus fiable serait un deuxième compte au niveau du fournisseur d’hébergement, comme le compte de production. Cela serait plus onéreux que d’utiliser IIS local comme environnement de test, car vous devrez vous inscrire pour un deuxième compte d’hébergement. Mais s’il empêche les erreurs ou pannes de site de production, vous pouvez décider qu’il est intéressant de le coûter.

La majeure partie du processus de création et de déploiement sur un compte de test est similaire à ce que vous avez déjà fait pour effectuer un déploiement en production :

- Créez un fichier de transformation *Web. config* .
- Créez un compte au niveau du fournisseur d’hébergement.
- Créez un profil de publication et déployez-le sur le compte test.

### <a name="preventing-public-access-to-the-test-site"></a>Empêcher l’accès public au site de test

Un point important à prendre en compte pour le compte de test est qu’il est en ligne sur Internet, mais que vous ne voulez pas que le public l’utilise. Pour conserver le site privé, vous pouvez utiliser une ou plusieurs des méthodes suivantes :

- Contactez le fournisseur d’hébergement pour définir des règles de pare-feu qui autorisent l’accès au site de test uniquement à partir des adresses IP que vous utilisez pour le test.
- Déguisez l’URL de façon à ce qu’elle ne soit pas similaire à l’URL du site public.
- Utilisez un fichier *robots. txt* pour vous assurer que les moteurs de recherche n’analyseront pas le site de test et les liens de rapport vers ce dernier dans les résultats de la recherche.

La première de ces méthodes est évidemment la plus sécurisée, mais la procédure pour qui est spécifique à chaque fournisseur d’hébergement et n’est pas traitée dans ce didacticiel. Si vous utilisez votre fournisseur d’hébergement pour n’autoriser que votre adresse IP à accéder à l’URL du compte de test, vous n’avez en théorie pas à vous soucier des moteurs de recherche qui l’analysent. Mais même dans ce cas, le déploiement d’un fichier *robots. txt* est une bonne idée comme une sauvegarde au cas où la règle de pare-feu serait jamais accidentellement désactivée.

Le fichier *robots. txt* est placé dans votre dossier de projet et doit contenir le texte suivant :

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

La ligne `User-agent` indique aux moteurs de recherche que les règles du fichier s’appliquent à tous les robots d’indexation (robots) du moteur de recherche et que la ligne `Disallow` spécifie qu’aucune page du site ne doit être analysée.

Vous souhaitez probablement que les moteurs de recherche cataloguent votre site de production. vous devez donc exclure ce fichier du déploiement de production. Pour ce faire, consultez Comment **puis-je exclure des fichiers ou des dossiers spécifiques du déploiement ?** dans le [Forum aux questions sur le déploiement de projets d’application Web ASP.net](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Veillez à spécifier l’exclusion uniquement pour le profil de publication de production.

La création d’un deuxième compte d’hébergement est une approche de l’utilisation d’un environnement de test qui n’est pas obligatoire, mais peut être utile pour les dépenses ajoutées. Dans les didacticiels suivants, vous allez continuer à utiliser IIS comme environnement de test.

Dans le didacticiel suivant, vous allez mettre à jour le code de l’application et déployer vos modifications dans les environnements de test et de production.

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
