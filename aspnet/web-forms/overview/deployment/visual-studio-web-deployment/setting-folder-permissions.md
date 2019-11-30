---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : définition des autorisations des dossiers | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, par utilisez...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 410525bb2e3f6e5a0be6d7d6b33fb3a40509041a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614940"
---
# <a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Déploiement Web ASP.NET à l’aide de Visual Studio : définition des autorisations des dossiers

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou de Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).

## <a name="overview"></a>Vue d'ensemble de

Dans ce didacticiel, vous allez définir les autorisations de dossier pour le dossier *ELMAH* dans le site Web déployé afin que l’application puisse créer des fichiers journaux dans ce dossier.

Quand vous testez une application Web dans Visual Studio à l’aide du Serveur Visual Studio Development (Cassini) ou IIS Express, l’application s’exécute sous votre identité. Vous êtes probablement administrateur sur votre ordinateur de développement et vous avez l’autorisation totale d’effectuer n’importe quel fichier dans n’importe quel dossier. Toutefois, quand une application s’exécute sous IIS, elle s’exécute sous l’identité définie pour le pool d’applications auquel le site est affecté. Il s’agit généralement d’un compte défini par le système qui a des autorisations limitées. Par défaut, elle dispose des autorisations de lecture et d’exécution sur les fichiers et dossiers de votre application Web, mais elle ne dispose pas d’un accès en écriture.

Cela devient un problème si votre application crée ou met à jour des fichiers, ce qui est un besoin courant dans les applications Web. Dans l’application Contoso University, ELMAH crée des fichiers XML dans le dossier *ELMAH* afin d’enregistrer des détails sur les erreurs. Même si vous n’utilisez pas ELMAH, votre site peut permettre aux utilisateurs de charger des fichiers ou d’effectuer d’autres tâches qui écrivent des données dans un dossier de votre site.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Journalisation des erreurs de test et création de rapports

Pour voir comment l’application ne fonctionne pas correctement dans IIS (même si elle a été testée dans Visual Studio), vous pouvez provoquer une erreur normalement enregistrée par ELMAH, puis ouvrir le journal des erreurs ELMAH pour afficher les détails. Si ELMAH n’a pas pu créer un fichier XML et stocke les détails de l’erreur, un rapport d’erreurs vide s’affiche.

Ouvrez un navigateur et accédez à `http://localhost/ContosoUniversity`, puis demandez une URL non valide telle que *Studentsxxx. aspx*. Vous voyez une page d’erreur générée par le système au lieu de la page *GenericErrorPage. aspx* , car le paramètre `customErrors` dans le fichier Web. config est « RemoteOnly » et que vous exécutez IIS localement :

![Page d’erreurs HTTP 404](setting-folder-permissions/_static/image1.png)

Exécutez maintenant *ELMAH. axd* pour afficher le rapport d’erreurs. Une fois que vous êtes connecté avec les informations d’identification du compte d’administrateur (&quot;admin&quot; et &quot;devpwd&quot;), vous voyez une page de journal des erreurs vide, car ELMAH n’a pas pu créer un fichier XML dans le dossier *ELMAH* :

![Journal des erreurs vide](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Définir l’autorisation d’écriture sur le dossier ELMAH

Vous pouvez définir les autorisations de dossier manuellement ou en faire une partie automatique du processus de déploiement. Le fait de le rendre automatique requiert du code MSBuild complexe et, puisque vous n’avez à le faire que la première fois que vous déployez, les étapes suivantes expliquent comment le faire manuellement. (Pour plus d’informations sur la façon d’effectuer cette partie du processus de déploiement, consultez [définition des autorisations de dossier sur Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) sur le blog de Sayed Hashimi.)

1. Dans l' **Explorateur de fichiers**, accédez à *C:\inetpub\wwwroot\ContosoUniversity*. Cliquez avec le bouton droit sur le dossier *ELMAH* , sélectionnez **Propriétés**, puis sélectionnez l’onglet **sécurité** .
2. Cliquez sur **Modifier**.
3. Dans la boîte de dialogue **autorisations pour ELMAH** , sélectionnez **DefaultAppPool**, puis activez la case à cocher **écrire** dans la colonne **autoriser** .

    ![Autorisations pour le dossier ELMAH](setting-folder-permissions/_static/image3.png)

    (Si vous ne voyez pas **DefaultAppPool** dans la liste **noms d’utilisateurs ou de groupes** , vous avez probablement utilisé une autre méthode que celle spécifiée dans ce didacticiel pour configurer IIS et ASP.net 4 sur votre ordinateur. Dans ce cas, identifiez l’identité utilisée par le pool d’applications affecté à l’application Contoso University et accordez une autorisation d’accès en écriture à cette identité. Consultez les liens sur les identités du pool d’applications à la fin de ce didacticiel.) Cliquez sur **OK** dans les deux boîtes de dialogue.

## <a name="retest-error-logging-and-reporting"></a>Retester la journalisation des erreurs et les rapports

Testez en provoquant à nouveau une erreur de la même façon (demandez une URL incorrecte) et exécutez la page du **Journal des erreurs** . Cette fois-ci, l’erreur s’affiche sur la page.

![Page du journal des erreurs ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Récapitulatif

Vous avez maintenant terminé toutes les tâches nécessaires pour que Contoso University fonctionne correctement dans IIS sur votre ordinateur local. Dans le didacticiel suivant, vous allez rendre le site accessible au public en le déployant sur Azure.

## <a name="more-information"></a>Plus d'informations

Dans cet exemple, la raison pour laquelle ELMAH n’a pas pu enregistrer les fichiers journaux était relativement évidente. Vous pouvez utiliser le suivi IIS dans les cas où la cause du problème n’est pas si évidente ; consultez [Troubleshooting failed requests using Tracing in IIS 7 (](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) en anglais) sur le site IIS.net.

Pour plus d’informations sur la façon d’accorder des autorisations aux identités du pool d’applications, consultez [identités du pool d’applications](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) et [contenu sécurisé dans IIS via les listes](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) de contrôle d’accès du système de fichiers sur le site IIS.net.

> [!div class="step-by-step"]
> [Précédent](deploying-to-iis.md)
> [Suivant](deploying-to-production.md)
