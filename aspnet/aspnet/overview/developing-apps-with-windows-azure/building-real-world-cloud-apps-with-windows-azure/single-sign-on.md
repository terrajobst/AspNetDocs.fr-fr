---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Authentification unique (création d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 1ca93cce22487295a24aae95437b3e69dfc5b504
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617445"
---
# <a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Authentification unique (création d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour plus d’informations sur l’e-Book, consultez [le premier chapitre](introduction.md).

Il existe de nombreux problèmes de sécurité à prendre en compte lorsque vous développez une application Cloud, mais pour cette série, nous allons nous concentrer sur une seule : authentification unique. Une question que les gens posent souvent : «je crée principalement des applications pour les employés de mon entreprise. Comment héberger ces applications dans le Cloud tout en leur permettant d’utiliser le même modèle de sécurité que mes employés savent et utilisent dans l’environnement local lorsqu’ils exécutent des applications qui sont hébergées à l’intérieur du pare-feu ?» L’un des moyens d’activer ce scénario est appelé Azure Active Directory (Azure AD). Azure AD vous permet de mettre à disposition des applications métier (LOB) d’entreprise sur Internet et vous permet également de rendre ces applications disponibles pour les partenaires commerciaux.

## <a name="introduction-to-azure-ad"></a>Présentation de Azure AD

[Azure ad](https://docs.microsoft.com/azure/active-directory/) fournit [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) dans le Cloud. Les fonctionnalités clés sont les suivantes :

- Il s’intègre aux Active Directory locaux.
- Il permet l’authentification unique avec vos applications.
- Il prend en charge des normes ouvertes telles que [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation)et [OAuth 2,0](http://oauth.net/2/).
- Il prend en charge l' [API REST](https://msdn.microsoft.com/library/hh974476.aspx)d’Enterprise Graph.

Supposons que vous disposez d’un environnement Windows Server Active Directory local que vous utilisez pour permettre aux employés de se connecter aux applications intranet :

![](single-sign-on/_static/image1.png)

Ce que Azure AD vous permet de créer un répertoire dans le Cloud. Il s’agit d’une fonctionnalité gratuite et facile à configurer.

Il peut être entièrement indépendant de votre Active Directory local ; vous pouvez y placer les personnes de votre choix et les authentifier dans des applications Internet.

![Microsoft Azure Active Directory](single-sign-on/_static/image2.png)

Vous pouvez ou l’intégrer à votre AD local.

![Intégration d’AD et de WAAD](single-sign-on/_static/image3.png)

Désormais, tous les employés qui peuvent s’authentifier localement peuvent également s’authentifier sur Internet, sans avoir à ouvrir un pare-feu ou à déployer de nouveaux serveurs dans votre centre de données. Vous pouvez continuer à tirer parti de tous les environnements de Active Directory existants que vous connaissez et utilisez aujourd’hui pour offrir une fonctionnalité d’authentification unique à vos applications internes.

Une fois que vous avez établi cette connexion entre AD et Azure AD, vous pouvez également autoriser vos applications Web et vos appareils mobiles à authentifier vos employés dans le Cloud, et vous pouvez activer des applications tierces, telles qu’Office 365, SalesForce.com ou Google Apps, pour accepter votre informations d’identification des employés. Si vous utilisez Office 365, vous êtes déjà configuré avec Azure AD, car Office 365 utilise Azure AD pour l’authentification et l’autorisation.

![applications tierces](single-sign-on/_static/image4.png)

L’avantage de cette approche est que chaque fois que votre organisation ajoute ou supprime un utilisateur, ou qu’un utilisateur modifie un mot de passe, vous utilisez le même processus que celui que vous utilisez aujourd’hui dans votre environnement local. Toutes vos modifications AD locales sont propagées automatiquement vers l’environnement Cloud.

Si votre entreprise utilise ou passe à Office 365, la bonne nouvelle est que vous avez Azure AD configurer automatiquement, car Office 365 utilise Azure AD pour l’authentification. Vous pouvez facilement utiliser dans vos propres applications la même authentification que celle utilisée par Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Configurer un locataire Azure AD

un répertoire Azure AD est appelé un [locataire](https://technet.microsoft.com/library/jj573650.aspx)Azure ad et la configuration d’un locataire est relativement simple. Nous allons vous montrer comment cela se fait dans le Portail de gestion Azure afin d’illustrer les concepts, mais bien évidemment comme les autres fonctions de portail, vous pouvez également le faire à l’aide d’un script ou d’une API de gestion.

Dans le portail de gestion, cliquez sur l’onglet Active Directory.

![WAAD dans le portail](single-sign-on/_static/image5.png)

Vous disposez automatiquement d’un locataire Azure AD pour votre compte Azure, et vous pouvez cliquer sur le bouton **Ajouter** au bas de la page pour créer des répertoires supplémentaires. Vous pouvez en avoir besoin pour un environnement de test et un autre pour la production, par exemple. Réfléchissez bien à ce que vous nommez un nouveau répertoire. Si vous utilisez votre nom pour le répertoire et que vous utilisez à nouveau votre nom pour l’un des utilisateurs, cela peut prêter à confusion.

![Ajouter un répertoire](single-sign-on/_static/image6.png)

Le portail offre une prise en charge complète de la création, de la suppression et de la gestion des utilisateurs au sein de cet environnement. Par exemple, pour ajouter un utilisateur, accédez à l’onglet **utilisateurs** , puis cliquez sur le bouton **Ajouter un utilisateur** .

![Bouton Ajouter un utilisateur](single-sign-on/_static/image7.png)

![Boîte de dialogue Ajouter un utilisateur](single-sign-on/_static/image8.png)

Vous pouvez créer un utilisateur qui existe uniquement dans ce répertoire, ou vous pouvez inscrire un compte Microsoft en tant qu’utilisateur dans ce répertoire, ou l’inscrire ou un utilisateur à partir d’un autre répertoire Azure AD en tant qu’utilisateur dans cet annuaire. (Dans un répertoire réel, le domaine par défaut serait ContosoTest.onmicrosoft.com. Vous pouvez également utiliser un domaine de votre choix, tel que contoso.com.)

![Types d’utilisateurs](single-sign-on/_static/image9.png)

![Boîte de dialogue Ajouter un utilisateur](single-sign-on/_static/image10.png)

Vous pouvez affecter l’utilisateur à un rôle.

![Profil utilisateur](single-sign-on/_static/image11.png)

Et le compte est créé avec un mot de passe temporaire.

![Mot de passe temporaire](single-sign-on/_static/image12.png)

Les utilisateurs que vous créez de cette manière peuvent immédiatement se connecter à vos applications Web à l’aide de cet annuaire de Cloud.

Cependant, la solution idéale pour l’authentification unique de l’entreprise est l’onglet **intégration d’annuaire** :

![Onglet intégration de répertoire](single-sign-on/_static/image13.png)

Si vous activez l’intégration d’annuaire et [Téléchargez un outil](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), vous pouvez synchroniser ce répertoire Cloud avec votre Active Directory local existant que vous utilisez déjà dans votre organisation. Ensuite, tous les utilisateurs stockés dans votre annuaire s’afficheront dans cet annuaire de Cloud. Vos applications Cloud peuvent désormais authentifier tous vos employés à l’aide de leurs informations d’identification existantes Active Directory. Tout ceci est gratuit, à la fois l’outil de synchronisation et le Azure AD lui-même.

L’outil est un assistant facile à utiliser, comme vous pouvez le voir à partir de ces captures d’écran. Il ne s’agit pas d’instructions complètes, un simple exemple vous montre le processus de base. Pour plus d’informations sur les procédures plus détaillées, consultez les liens dans la section [ressources](#resources) à la fin du chapitre.

![Assistant Configuration de l’outil de synchronisation WAAD](single-sign-on/_static/image14.png)

Cliquez sur **suivant**, puis entrez vos informations d’identification de Azure Active Directory.

![Assistant Configuration de l’outil de synchronisation WAAD](single-sign-on/_static/image15.png)

Cliquez sur **suivant**, puis entrez vos informations d’identification Active Directory locales.

![Assistant Configuration de l’outil de synchronisation WAAD](single-sign-on/_static/image16.png)

Cliquez sur **suivant**, puis indiquez si vous souhaitez stocker un hachage de vos mots de passe ad dans le Cloud.

![Assistant Configuration de l’outil de synchronisation WAAD](single-sign-on/_static/image17.png)

Le hachage de mot de passe que vous pouvez stocker dans le Cloud est un hachage unidirectionnel. les mots de passe réels ne sont jamais stockés dans Azure AD. Si vous décidez de stocker les hachages dans le Cloud, vous devez utiliser [services ADFS](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). Il existe également [d’autres facteurs à prendre en compte lorsque vous choisissez d’utiliser ADFS ou non](https://technet.microsoft.com/library/jj573653.aspx). L’option ADFS nécessite quelques étapes de configuration supplémentaires.

Si vous choisissez de stocker les hachages dans le Cloud, vous avez terminé et l’outil démarre la synchronisation des répertoires lorsque vous cliquez sur **suivant**.

![Assistant Configuration de l’outil de synchronisation WAAD](single-sign-on/_static/image18.png)

Et en quelques minutes, vous avez terminé.

![Assistant Configuration de l’outil de synchronisation WAAD](single-sign-on/_static/image19.png)

Vous devez uniquement exécuter ce sur un contrôleur de domaine de l’organisation, sur Windows 2003 ou version ultérieure. Et n’ont pas besoin de redémarrer. Lorsque vous avez terminé, tous vos utilisateurs se trouvent dans le Cloud et vous pouvez effectuer une authentification unique à partir de n’importe quelle application Web ou mobile, à l’aide de SAML, OAuth ou WS-FED.

Parfois, nous nous sommes demandés à quel degré de sécurité est-ce que Microsoft utilise-t-il pour ses données d’entreprise sensibles ? Et la réponse est oui. Par exemple, si vous accédez au site Microsoft SharePoint interne à l’adresse [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), vous êtes invité à vous connecter.

![Connexion à Office 365](single-sign-on/_static/image20.png)

Microsoft a activé ADFS. par conséquent, lorsque vous entrez un ID Microsoft, vous êtes redirigé vers une page de connexion ADFS.

![Connexion ADFS](single-sign-on/_static/image21.png)

Et une fois que vous avez entré les informations d’identification stockées dans un compte Microsoft AD interne, vous avez accès à cette application interne.

![Site Microsoft SharePoint](single-sign-on/_static/image22.png)

Nous utilisons un serveur de connexion AD principalement parce que nous avions déjà configuré ADFS avant que Azure AD soit disponible, mais le processus de connexion passe par un répertoire Azure AD dans le Cloud. Nous avons mis en place nos documents importants, le contrôle de code source, les fichiers de gestion des performances, les rapports de ventes, et bien plus encore, dans le Cloud et utilisent exactement la même solution pour les sécuriser.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Créer une application ASP.NET qui utilise Azure AD pour l’authentification unique

Visual Studio simplifie grandement la création d’une application qui utilise Azure AD pour l’authentification unique, comme vous pouvez le constater dans quelques captures d’écran.

Lorsque vous créez une nouvelle application ASP.NET, MVC ou Web Forms, la méthode d’authentification par défaut est ASP.NET Identity. Pour modifier ce Azure AD, cliquez sur le bouton **modifier l’authentification** .

![Modifier l'authentification](single-sign-on/_static/image23.png)

Sélectionnez comptes professionnels, entrez votre nom de domaine, puis sélectionnez authentification unique.

![Boîte de dialogue Configurer l’authentification](single-sign-on/_static/image24.png)

Vous pouvez également accorder à l’application l’autorisation de lecture ou de lecture/écriture pour les données d’annuaire. Dans ce cas, il peut utiliser l' [API REST de Graph Azure](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) pour rechercher le numéro de téléphone des utilisateurs, savoir s’ils sont au bureau, à la dernière connexion, etc.

C’est tout ce que vous devez faire-Visual Studio vous demande les informations d’identification d’un administrateur pour votre Azure AD locataire, puis il configure à la fois votre projet et votre client Azure AD pour la nouvelle application.

Lorsque vous exécutez le projet, une page de connexion s’affiche et vous pouvez vous connecter avec les informations d’identification d’un utilisateur dans votre répertoire de Azure AD.

![Connexion au compte d’organisation](single-sign-on/_static/image25.png)

![Connecté](single-sign-on/_static/image26.png)

Lorsque vous déployez l’application sur Azure, il vous suffit de cocher la case **activer l’authentification** de l’organisation et, une fois encore, Visual Studio prend en charge l’ensemble de la configuration.

![Publier le site web](single-sign-on/_static/image27.png)

Ces captures d’écran proviennent d’un didacticiel pas à pas complet qui montre comment créer une application qui utilise l’authentification Azure AD : [développement d’applications ASP.net avec Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Récapitulatif

Dans ce chapitre, vous avez vu que Azure Active Directory, Visual Studio et ASP.NET, facilitent la configuration de l’authentification unique dans les applications Internet pour les utilisateurs de votre organisation. Vos utilisateurs peuvent se connecter à des applications Internet en utilisant les mêmes informations d’identification que celles qu’ils utilisent pour se connecter à l’aide d’Active Directory de votre réseau interne.

Le [chapitre suivant](data-storage-options.md) présente les options de stockage des données disponibles pour une application Cloud.

<a id="resources"></a>
## <a name="resources"></a>Ressources

Pour plus d'informations, reportez-vous aux ressources suivantes :

- [Documentation Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) Page du portail pour Azure AD de la documentation sur le site windowsazure.com. Pour obtenir des didacticiels pas à pas, consultez la section **développer** .
- [Multi-Factor Authentication Azure](https://docs.microsoft.com/azure/multi-factor-authentication/). Page du portail pour obtenir de la documentation sur l’authentification multifacteur dans Azure.
- [Options d’authentification du compte professionnel](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Explication des options d’authentification Azure AD dans la boîte de dialogue Visual Studio 2013 New-Project.
- [Modèles et pratiques Microsoft-modèle d’identité fédérée](https://msdn.microsoft.com/library/dn589790.aspx).
- [Procédure : installer l’outil de synchronisation Azure Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Carte de contenu Services ADFS 2,0](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Liens vers la documentation sur ADFS 2,0.
- [Autorisation basée sur les rôles et les listes de contrôle d’accès dans une application Windows Azure ad](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Exemple d’application.
- [Azure Active Directory blog API Graph](https://blogs.msdn.com/b/aadgraphteam/).
- [Access Control dans BYOD et l’intégration d’annuaire dans une infrastructure d’identité hybride](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Vidéo de la session Tech Ed 2014 de Gayana BAGDASARYAN.

> [!div class="step-by-step"]
> [Précédent](web-development-best-practices.md)
> [Suivant](data-storage-options.md)
