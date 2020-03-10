---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : définition des autorisations de dossier-6 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 85a77a196cf3458bbb2e6308838a846936cd070b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630507"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : définition des autorisations de dossier-6 sur 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour obtenir une présentation de la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui présente les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions SQL Server autres que SQL Server Compact et montre comment déployer vers Azure App Service Web Apps, consultez [déploiement Web ASP.net à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Présentation

Dans ce didacticiel, vous allez définir les autorisations de dossier pour le dossier *ELMAH* dans le site Web déployé afin que l’application puisse créer des fichiers journaux dans ce dossier.

Quand vous testez une application Web dans Visual Studio à l’aide de l’Serveur Visual Studio Development (Cassini), l’application s’exécute sous votre identité. Vous êtes probablement administrateur sur votre ordinateur de développement et vous avez l’autorisation totale d’effectuer n’importe quel fichier dans n’importe quel dossier. Toutefois, quand une application s’exécute sous IIS, elle s’exécute sous l’identité définie pour le pool d’applications auquel le site est affecté. Il s’agit généralement d’un compte défini par le système qui a des autorisations limitées. Par défaut, elle dispose des autorisations de lecture et d’exécution sur les fichiers et dossiers de votre application Web, mais elle ne dispose pas d’un accès en écriture.

Cela devient un problème si votre application crée ou met à jour des fichiers, ce qui est un besoin courant dans les applications Web. Dans l’application Contoso University, ELMAH crée des fichiers XML dans le dossier *ELMAH* afin d’enregistrer des détails sur les erreurs. Même si vous n’utilisez pas ELMAH, votre site peut permettre aux utilisateurs de charger des fichiers ou d’effectuer d’autres tâches qui écrivent des données dans un dossier de votre site.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Test de la journalisation des erreurs et des rapports

Pour voir comment l’application ne fonctionne pas correctement dans IIS (même si elle a été testée dans Visual Studio), vous pouvez provoquer une erreur normalement enregistrée par ELMAH, puis ouvrir le journal des erreurs ELMAH pour afficher les détails. Si ELMAH n’a pas pu créer un fichier XML et stocke les détails de l’erreur, un rapport d’erreurs vide s’affiche.

Ouvrez un navigateur et accédez à `http://localhost/ContosoUniversity`, puis demandez une URL non valide telle que *Studentsxxx. aspx*. Vous voyez une page d’erreur générée par le système au lieu de la page *GenericErrorPage. aspx* , car le paramètre `customErrors` dans le fichier Web. config est « RemoteOnly » et que vous exécutez IIS localement :

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Exécutez maintenant *ELMAH. axd* pour afficher le rapport d’erreurs. Une page de journal des erreurs vide s’affiche, car ELMAH n’a pas pu créer un fichier XML dans le dossier *ELMAH* :

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Définition de l’autorisation d’écriture sur le dossier ELMAH

Vous pouvez définir les autorisations de dossier manuellement ou en faire une partie automatique du processus de déploiement. Le fait de le rendre automatique requiert du code MSBuild complexe et, puisque vous n’avez à le faire que la première fois que vous déployez, ce didacticiel vous indique uniquement comment le faire manuellement. (Pour plus d’informations sur la façon d’effectuer cette partie du processus de déploiement, consultez [définition des autorisations de dossier sur Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) sur le blog de Sayed Hashimi.)

Dans l' **Explorateur Windows**, accédez à *C:\inetpub\wwwroot\ContosoUniversity*. Cliquez avec le bouton droit sur le dossier *ELMAH* , sélectionnez **Propriétés**, puis sélectionnez l’onglet **sécurité** .

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Si vous ne voyez pas **DefaultAppPool** dans la liste **noms d’utilisateurs ou de groupes** , vous avez probablement utilisé une autre méthode que celle spécifiée dans ce didacticiel pour configurer IIS et ASP.net 4 sur votre ordinateur. Dans ce cas, identifiez l’identité utilisée par le pool d’applications affecté à l’application Contoso University et accordez une autorisation d’accès en écriture à cette identité. Consultez les liens sur les identités du pool d’applications à la fin de ce didacticiel.)

Cliquez sur **Modifier**. Dans la boîte de dialogue **autorisations pour ELMAH** , sélectionnez **DefaultAppPool**, puis activez la case à cocher **écrire** dans la colonne **autoriser** .

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Cliquez sur **OK** dans les deux boîtes de dialogue.

## <a name="retesting-error-logging-and-reporting"></a>Test de la journalisation des erreurs et des rapports

Testez en provoquant à nouveau une erreur de la même façon (demandez une URL incorrecte) et exécutez la page du **Journal des erreurs** . Cette fois-ci, l’erreur s’affiche sur la page.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Vous devez également disposer de l’autorisation en écriture sur le dossier de données de l' *application\_* , car vous avez SQL Server Compact fichiers de base de données dans ce dossier et vous souhaitez pouvoir mettre à jour les données de ces bases de données. Dans ce cas, toutefois, vous n’avez rien à faire, car le processus de déploiement définit automatiquement l’autorisation d’écriture sur le dossier de données de l' *application\_* .

Vous avez maintenant terminé toutes les tâches nécessaires pour que Contoso University fonctionne correctement dans IIS sur votre ordinateur local. Dans le didacticiel suivant, vous allez rendre le site accessible au public en le déployant sur un fournisseur d’hébergement.

## <a name="more-information"></a>Plus d'informations

Dans cet exemple, la raison pour laquelle ELMAH n’a pas pu enregistrer les fichiers journaux était relativement évidente. Vous pouvez utiliser le suivi IIS dans les cas où la cause du problème n’est pas si évidente ; consultez [Troubleshooting failed requests using Tracing in IIS 7 (](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) en anglais) sur le site IIS.net.

Pour plus d’informations sur la façon d’accorder des autorisations aux identités du pool d’applications, consultez [identités du pool d’applications](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) et [contenu sécurisé dans IIS via les listes](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) de contrôle d’accès du système de fichiers sur le site IIS.net.

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
