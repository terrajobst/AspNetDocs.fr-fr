---
uid: whitepapers/overview
title: Livres blancs | Microsoft Docs
author: rick-anderson
description: Sur cette page, vous trouverez des livres blancs qui vous aideront à installer et à configurer ASP.NET, et à vous aider à écrire des applications ASP.NET sécurisées, rapides et flexibles.
ms.author: riande
ms.date: 11/15/2011
ms.assetid: d5e79470-01f2-4d65-8077-11c3e10a6784
msc.legacyurl: /whitepapers
msc.type: content
ms.openlocfilehash: 1a3a9fe5d685d4b38efe666fc88ff57016482ada
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640853"
---
# <a name="whitepapers"></a>Livres blancs

> Sur cette page, vous trouverez des livres blancs qui vous aideront à installer et à configurer ASP.NET, et à vous aider à écrire des applications ASP.NET sécurisées, rapides et flexibles.
> 
> - [ASP.NET 4](#aspnet4)
> - [Livres blancs sur la sécurité ASP.NET](#security)
> - [Livres blancs d’installation et de configuration](#setup)
> - [Livres blancs SQL Server](#sql)
> - [Livres blancs généraux](#general)

<a id="aspnet4"></a>
## <a name="aspnet-4"></a>ASP.NET 4

Informations relatives à ASP.NET 4 et Visual Studio 2010.

[Notes de publication de ASP.NET MVC 4](mvc4-beta-release-notes.md "mvc4-notes de publication")

Ce document décrit les nouvelles fonctionnalités et améliorations apportées à ASP.NET MVC 4 developer preview pour Visual Studio 2010, ainsi que les notes d’installation et les problèmes connus.

[Notes de publication de ASP.NET MVC 3](mvc3-release-notes.md "MvC3-notes de publication")

Ce document décrit les nouvelles fonctionnalités et améliorations introduites dans ASP.NET MVC 3, ainsi que les notes d’installation et les problèmes connus.

[Vue d’ensemble du développement web ASP.NET 4 et Visual Studio 2010](aspnet4/index.md "aspnet4")

De nombreux changements passionnants pour ASP.NET sont à venir dans le .NET Framework version 4. Ce document donne une vue d’ensemble des nouvelles fonctionnalités incluses dans la version à venir.

[Modifications importantes de la version bêta 2 de ASP.NET 4](aspnet4/breaking-changes.md "modifications avec rupture")

Ce document décrit les modifications apportées à la version bêta 2 de .NET Framework version 4 (c’est-à-dire la version bêta 2 de ASP.NET 4) pouvant affecter potentiellement les applications créées à l’aide de versions antérieures, y compris la version ASP.NET 4 bêta 1.

[Nouveautés d’ASP.NET MVC 2](what-is-new-in-aspnet-mvc.md "Nouveautés de ASPNET MVC")

Ce document décrit les nouvelles fonctionnalités et améliorations introduites dans ASP.NET MVC 2.

[Mise à niveau d’une application ASP.NET MVC 1.0 vers ASP.NET MVC 2](aspnet-mvc2-upgrade-notes.md "ASPNET-MVC2-Upgrade-notes")

ASP.NET MVC 2 peut être installé côte à côte avec ASP.NET MVC 1,0 sur le même serveur. Cela permet aux développeurs d’applications de choisir quand mettre à niveau une application ASP.NET MVC 1,0 vers ASP.NET MVC 2. Ce document décrit à la fois la mise à niveau manuelle et l’utilisation d’un assistant dans Visual...

<a id="security"></a>
## <a name="aspnet-security-whitepapers"></a>Livres blancs sur la sécurité ASP.NET

La sécurité est un aspect important des applications Internet, et ces livres blancs expliquent comment concevoir et implémenter des applications ASP.NET sécurisées.

[Instrumenter ASP.NET 2,0 applications pour la sécurité](https://msdn.microsoft.com/library/ms998325.aspx)

Cet article explique comment utiliser des événements de contrôle d’État personnalisés pour instrumenter votre application ASP.NET afin d’effectuer le suivi des opérations et des événements liés à la sécurité. ASP.NET version 2,0 fournit une surveillance de l’intégrité qui inclut l’instrumentation pour de nombreuses normes...

[Effectuer une révision du déploiement de la sécurité pour ASP.NET 2,0](https://msdn.microsoft.com/library/ms998367.aspx)

Cet article explique comment effectuer une révision du déploiement de sécurité pour une application ASP.NET 2,0 afin d’identifier les vulnérabilités de sécurité potentielles introduites par des paramètres de configuration inappropriés. La majeure partie du processus de révision implique la réalisation de...

[Utiliser ADAM pour les rôles dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998331.aspx)

Cet article explique comment développer un site Web ASP.NET qui utilise Active Directory mode application (ADAM) pour stocker des rôles ASP.NET. Il vous montre comment configurer ADAM et le magasin de stratégies du gestionnaire d’autorisations (AzMan), comment créer des rôles et...

[Utiliser le gestionnaire d’autorisations (AzMan) avec ASP.NET 2,0](https://msdn.microsoft.com/library/ms998336.aspx)

Cet article explique comment utiliser le gestionnaire d’autorisations (AzMan) conjointement avec l’API du gestionnaire de rôles ASP.NET pour gérer les rôles, vérifier l’appartenance au rôle d’utilisateur et autoriser les rôles à effectuer des opérations spécifiques sur un magasin de stratégies AzMan. La procédure...

[Utiliser l’appartenance dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998347.aspx)

Cet article explique comment utiliser la fonctionnalité d’appartenance dans les applications ASP.NET version 2,0. Il vous montre comment utiliser deux fournisseurs d’appartenances différents : ActiveDirectoryMembershipProvider et SqlMembershipProvider. La fonctionnalité d’appartenance...

[Utiliser le gestionnaire de rôles dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998314.aspx)

Cet article explique comment utiliser le gestionnaire de rôles ASP.NET 2,0. Le gestionnaire de rôles facilite la tâche de gestion des rôles et d’exécution de l’autorisation basée sur les rôles dans votre application. Il montre comment configurer les différents fournisseurs de rôles à utiliser avec votre...

[Utiliser l’authentification Windows dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998358.aspx)

Cet article explique comment configurer et utiliser l’authentification Windows dans une application Web ASP.NET. L’authentification Windows est l’approche privilégiée chaque fois que les utilisateurs font partie de votre domaine Windows. Cette approche vous permet d’utiliser un magasin d’identités existant...

[Effectuer une révision du code de sécurité pour le code managé (activité de référence)](https://msdn.microsoft.com/library/ms998364.aspx)

Cet article explique comment effectuer des révisions du code de sécurité. Ce module présente les étapes impliquées dans l’activité et les techniques d’analyse de vos résultats. Utilisez cette procédure avec « liste de questions de sécurité : code managé (.NET Framework 2,0) »...

[Effectuer une révision du déploiement de la sécurité pour ASP.NET 2,0](https://msdn.microsoft.com/library/ms998367.aspx)

Cet article explique comment effectuer une révision du déploiement de sécurité pour une application ASP.NET 2,0 afin d’identifier les vulnérabilités de sécurité potentielles introduites par des paramètres de configuration inappropriés. La majeure partie du processus de révision implique la réalisation de...

[Implémenter la délégation Kerberos pour Windows 2000](https://msdn.microsoft.com/library/aa302400.aspx)

La délégation Kerberos vous permet de transmettre une identité authentifiée sur plusieurs niveaux physiques d’une application pour prendre en charge l’authentification et l’autorisation en aval. Cette procédure décrit les étapes de configuration requises pour effectuer ce travail.

[Utiliser l’emprunt d’identité et la délégation dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998351.aspx)

Cet article explique comment et quand vous devez utiliser l’emprunt d’identité dans les applications ASP.NET 2,0. Par défaut, l’emprunt d’identité est désactivé et vous pouvez accéder aux ressources à l’aide de l’identité de processus de l’application Web ASP.NET. Toutefois, vous pouvez utiliser...

[Créer un modèle de menace pour une application Web au moment de la conception](https://msdn.microsoft.com/library/ms978527.aspx)

Ce guide décrit une approche pour créer un modèle de menace pour une application Web. L’activité de modélisation des menaces vous aide à modéliser votre conception de sécurité afin que vous puissiez exposer les failles de conception de sécurité potentielles et les vulnérabilités avant de vous investir...

### <a name="forms-authentication"></a>Authentification par formulaire

[Protéger l’authentification par formulaire dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998310.aspx)

Cet article explique comment configurer et utiliser en toute sécurité l’authentification par formulaire avec les applications ASP.NET 2,0. Les facteurs clés à prendre en compte incluent la sécurisation correcte du ticket d’authentification et la sécurisation du stockage des identités des utilisateurs et l’accès à ce magasin. ...

[Utiliser l’authentification par formulaire avec Active Directory dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998360.aspx)

Cet article explique comment utiliser l’authentification par formulaire avec Microsoft® Active Directory Service d’annuaire® à l’aide de ActiveDirectoryMembershipProvider. La procédure vous montre comment configurer le fournisseur et créer et authentifier des utilisateurs...

[Utiliser l’authentification par formulaire avec Active Directory dans plusieurs domaines dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998345.aspx)

Cet article explique comment utiliser l’authentification par formulaire avec Microsoft® Active Directory Service d’annuaire® à l’aide de ActiveDirectoryMembershipProvider. La procédure vous montre comment configurer le fournisseur et créer et authentifier des utilisateurs...

[Utiliser l’authentification par formulaire avec SQL Server dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998317.aspx)

Cet article explique comment vous pouvez utiliser l’authentification par formulaire avec le fournisseur d’appartenances SQL Server. L’authentification par formulaire avec SQL Server est plus applicable dans les situations où les utilisateurs de votre application ne font pas partie de votre domaine Windows, et par conséquent,...

[Créer des objets GenericPrincipal avec l’authentification par formulaire dans ASP.NET 1,1](https://msdn.microsoft.com/library/aa302399.aspx)

Cet article explique comment créer et gérer des objets GenericPrincipal et FormsIdentity lors de l’utilisation de l’authentification par formulaire.

[Utiliser l’authentification par formulaire avec Active Directory dans ASP.NET 1,1](https://msdn.microsoft.com/library/aa302397.aspx)

Cet article explique comment implémenter l’authentification par formulaire sur une banque d’informations d’identification Active Directory.

[Utiliser l’authentification par formulaire avec SQL Server dans ASP.NET 1,1](https://msdn.microsoft.com/library/aa302398.aspx)

Cet article explique comment implémenter l’authentification par formulaire sur un SQL Server magasin d’informations d’identification. Elle vous montre également comment stocker des résumés de mot de passe dans la base de données.

### <a name="user-input-data-validation"></a>Validation des données d’entrée utilisateur

[Validation des demandes - Prévention des attaques par script](request-validation.md "demande-validation")

Ce document décrit la fonctionnalité de validation de la demande de ASP.NET où, par défaut, l’application n’est pas en mesure de traiter le contenu HTML non encodé envoyé au serveur. Cette fonctionnalité de validation de demande peut être désactivée lorsque l’application a été...

[Empêcher les scripts entre sites dans ASP.NET](https://msdn.microsoft.com/library/ms998274.aspx)

Cet article explique comment vous aider à protéger vos applications ASP.NET contre les attaques de script entre sites en utilisant des techniques de validation d’entrée appropriées et en encodant la sortie. Il décrit également un certain nombre d’autres mécanismes de protection que vous pouvez utiliser dans...

[Protéger contre l’injection SQL dans ASP.NET](https://msdn.microsoft.com/library/ms998271.aspx)

Cet article explique comment vous aider à protéger votre application ASP.NET contre les attaques par injection SQL. L’injection SQL peut se produire lorsqu’une application utilise une entrée pour construire des instructions SQL dynamiques ou lorsqu’elle utilise des procédures stockées pour se connecter à...

[Utiliser des expressions régulières pour contraindre les entrées dans ASP.NET](https://msdn.microsoft.com/library/ms998267.aspx)

Cet article explique comment utiliser des expressions régulières dans les applications ASP.NET pour limiter les entrées non fiables. Les expressions régulières constituent un bon moyen de valider des champs de texte tels que les noms, les adresses, les numéros de téléphone et d’autres informations utilisateur. Vous pouvez utiliser...

### <a name="code-access-security"></a>Sécurité d'accès du code

[Utiliser la sécurité d’accès du code dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998326.aspx)

Cet article explique comment sélectionner un niveau de confiance approprié pour votre application et, si nécessaire, comment créer un fichier de stratégie de sécurité d’accès du code ASP.NET personnalisé pour définir un niveau de confiance personnalisé. Vous pouvez utiliser une autre approbation de sécurité d’accès du code...

[Créer une autorisation de chiffrement personnalisée](https://msdn.microsoft.com/library/aa302362.aspx)

Cet article explique comment créer une autorisation de sécurité d’accès du code personnalisée pour contrôler l’accès par programme aux fonctionnalités de chiffrement non gérées fournies par Win32® API de protection des données (DPAPI). Utilisez cette autorisation personnalisée avec le wrapper DPAPI géré...

[Utiliser la stratégie de sécurité d’accès du code pour contraindre un assembly](https://msdn.microsoft.com/library/aa302361.aspx)

Un administrateur peut configurer la stratégie de sécurité d’accès du code pour limiter les opérations de .NET Framework code (assemblys). Dans cette procédure, vous configurez la stratégie de sécurité d’accès du code pour limiter la capacité d’un assembly à effectuer des e/s de fichier et à restreindre...

### <a name="communications-security"></a>Sécurité des communications

[Configurer SSL sur un serveur Web](https://msdn.microsoft.com/library/aa302411.aspx)

Un serveur Web doit être configuré pour SSL afin de prendre en charge les connexions HTTPS à partir des applications clientes. Cet article explique comment configurer SSL sur un serveur Web.

[Configurer des certificats clients](https://msdn.microsoft.com/library/aa302412.aspx)

IIS prend en charge l’authentification par certificat client. Cet article explique comment configurer une application Web pour exiger des certificats clients. Il montre également comment installer un certificat sur un ordinateur client et l’utiliser lors de l’appel de l’application Web.

[Utiliser IPSec pour le filtrage des ports et de l’authentification](https://msdn.microsoft.com/library/aa302366.aspx)

La sécurité du protocole Internet (IPSec) est un protocole, et non un service, qui fournit des services de chiffrement, d’intégrité et d’authentification pour le trafic réseau basé sur IP. Étant donné que IPSec fournit une protection de serveur à serveur, vous pouvez utiliser IPSec pour contrer les menaces internes...

[Utiliser IPSec pour fournir une communication sécurisée entre deux serveurs](https://msdn.microsoft.com/library/aa302413.aspx)

IPSec est une technologie fournie par Windows 2000 qui vous permet de créer des canaux chiffrés entre deux serveurs. IPSec peut être utilisé pour filtrer le trafic IP et authentifier les serveurs. Cet article explique comment configurer IPSec pour fournir un sécurisé (chiffré)...

[Utilisez SSL pour sécuriser la communication avec SQL Server](https://msdn.microsoft.com/library/aa302414.aspx)

Il est souvent vital que les applications soient en mesure de sécuriser les données transmises vers et à partir d’un serveur de base de données SQL Server. Avec SQL Server, vous pouvez utiliser SSL pour créer un canal chiffré. Cet article explique comment installer un certificat sur le serveur de base de données,...

[Appeler un service Web à l’aide de certificats clients à partir de ASP.NET 1,1](https://msdn.microsoft.com/library/aa302408.aspx)

Cet article explique comment transmettre un certificat client à un service Web pour l’authentification à partir d’une application Web ASP.NET ou d’une application Windows Forms. Vous pouvez installer le certificat client dans le magasin de l’ordinateur local ou dans le magasin de l’utilisateur. Condition

[Appeler un service Web à l’aide de SSL à partir de ASP.NET 1,1](https://msdn.microsoft.com/library/aa302409.aspx)

Le chiffrement protocole SSL (SSL) peut être utilisé pour garantir l’intégrité et la confidentialité des messages transmis à et à partir d’un service Web. Cet article explique comment utiliser SSL avec les services Web.

### <a name="cryptography"></a>Chiffrement

[Créer une bibliothèque DPAPI dans .NET 1,1](https://msdn.microsoft.com/library/aa302402.aspx)

Cet article explique comment créer une bibliothèque de classes managée qui expose les fonctionnalités DPAPI aux applications qui souhaitent chiffrer des données, par exemple, les chaînes de connexion de base de données et les informations d’identification de compte.

[Créer une bibliothèque de chiffrement dans .NET 1,1](https://msdn.microsoft.com/library/aa302405.aspx)

Cet article explique comment créer une bibliothèque de classes managée pour fournir des fonctionnalités de chiffrement pour les applications. Il permet à une application de choisir l’algorithme de chiffrement. Les algorithmes pris en charge incluent DES, Triple DES, RC2 et Rijndael.

[Stocker une chaîne de connexion chiffrée dans le registre dans ASP.NET 1,1](https://msdn.microsoft.com/library/aa302406.aspx)

Les applications peuvent choisir de stocker des données chiffrées telles que des chaînes de connexion et des informations d’identification de compte dans le Registre Windows. Cet article explique comment stocker et récupérer des chaînes chiffrées dans le registre.

[Utiliser DPAPI (magasin de l’ordinateur) à partir de ASP.NET 1,1](https://msdn.microsoft.com/library/aa302403.aspx)

Cet article explique comment utiliser DPAPI à partir d’une application Web ou d’un service Web ASP.NET pour chiffrer les données sensibles.

[Utiliser DPAPI (magasin utilisateur) à partir de ASP.NET 1,1 avec Enterprise Services](https://msdn.microsoft.com/library/aa302404.aspx)

Cet article explique comment utiliser DPAPI à partir d’une application ou d’un service Web ASP.NET pour chiffrer les données sensibles. Cet article explique comment utiliser DPAPI avec le magasin de l’utilisateur, ce qui nécessite l’utilisation d’un composant de services d’entreprise hors processus.

<a id="setup"></a>
## <a name="installation-and-setup-whitepapers"></a>Livres blancs d’installation et de configuration

Ces livres blancs fournissent des instructions pas à pas pour l’installation et la configuration de ASP.NET sur votre serveur.

[Créer un compte de service pour une application ASP.NET 2,0](https://msdn.microsoft.com/library/ms998297.aspx)

Cet article explique comment créer et configurer un compte de service personnalisé avec des privilèges minimaux pour exécuter une application Web ASP.NET. Par défaut, une application ASP.NET sur Microsoft Windows Server 2003 et IIS 6,0 s’exécute à l’aide du service réseau intégré...

[Améliorer la sécurité lors de l’hébergement de plusieurs applications dans ASP.NET 2,0](https://msdn.microsoft.com/library/aa480478.aspx)

Cet article explique comment isoler plusieurs applications les unes des autres et des ressources système partagées dans un environnement d’hébergement Web. L’environnement d’hébergement peut être un serveur Web fourni par un fournisseur de services Internet (ISP) qui héberge plusieurs...

[Utiliser la confiance moyenne dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998341.aspx)

Cet article explique comment configurer des applications Web ASP.NET pour qu’elles s’exécutent en mode de confiance moyenne. Si vous hébergez plusieurs applications sur le même serveur, vous pouvez utiliser la sécurité d’accès du code et le niveau de confiance moyen pour assurer l’isolation des applications. En définissant...

[Utiliser le compte service réseau pour accéder aux ressources dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998320.aspx)

Cet article explique comment vous pouvez utiliser le compte d’ordinateur NT AUTHORITY\Network service pour accéder aux ressources locales et réseau. Par défaut sur Windows Server 2003, les applications ASP.NET s’exécutent à l’aide de l’identité de ce compte. Il s’agit d’un minimum de privilèges...

[Utiliser la transition de protocole et la délégation avec restriction dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998355.aspx)

Cet article explique comment configurer et utiliser la transition de protocole et la délégation avec restriction pour permettre à votre application ASP.NET d’accéder aux ressources réseau tout en usurpant l’identité de l’appelant d’origine. Le système d’exploitation Microsoft® Windows® 2000...

[ASP.NET - Exécution côte à côte de .NET Framework 1.0 et 1.1](side-by-side-with-10.md "côte à côte avec 1,0")

Ce livre blanc explique comment installer .NET 1,0 et .NET 1,1 sur votre ordinateur, ce qui permet à une application Web ASP.NET de s’exécuter sur l’une ou l’autre des versions du Framework.

[ASP.NET - Accès refusé aux répertoires IIS](denied-access-to-iis-directories.md "accès refusé aux répertoires IIS")

Ce livre blanc décrit ce que vous devez faire si une demande à votre application ASP.NET retourne l’erreur «Accès refusé au répertoire *DirectoryName* . Échec du démarrage de l’analyse des modifications de répertoire.»

[Exécution d’ASP.NET 1.1 avec IIS 6.0](aspnet-and-iis6.md "ASPNET et IIS6")

Bien que Windows Server 2003 inclue à la fois IIS 6,0 et ASP.NET 1,1, ces composants sont désactivés par défaut. Ce livre blanc explique comment activer IIS 6,0 et ASP.NET 1,1, et recommande plusieurs paramètres de configuration pour tirer le meilleur...

[Correction de l’erreur « application serveur non disponible » après l’application de la mise à jour de sécurité pour Internet Explorer](ms03-32-issue.md "MS03-32-problème")

Ce document décrit le correctif qui résout un problème lié à la mise à jour de sécurité MS03-32 pour Internet Explorer qui affecte les applications ASP.NET 1,0 s’exécutant sur Windows XP professionnel.

[Créer un compte personnalisé pour exécuter ASP.NET 1,1](https://msdn.microsoft.com/library/aa302396.aspx)

Les applications Web ASP.NET s’exécutent généralement à l’aide du compte ASPNET intégré. Il peut arriver que vous souhaitiez utiliser un compte personnalisé à la place. Cet article explique comment créer un compte local doté de privilèges minimum pour exécuter des applications Web ASP.NET. ...

### <a name="configuration"></a>Configuration

[Configuration de la clé d’ordinateur dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998288.aspx)

Cet article explique comment configurer l’élément &lt;machineKey&gt; dans le fichier Web. config et montre comment configurer l’élément de&gt; &lt;machineKey pour contrôler la vérification de la falsification et le chiffrement de ViewState, les tickets d’authentification par formulaire et les cookies de rôle. ViewState est signé...

[Chiffrer les sections de configuration dans ASP.NET 2,0 à l’aide de DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)

Cet article explique comment utiliser le fournisseur de configuration protégé DPAPI (Data Protection Application Programming Interface) de Windows et l’outil ASPNET\_regiis. exe pour chiffrer les sections de vos fichiers de configuration. Vous pouvez utiliser l’outil ASPNET\_regiis. exe pour...

[Chiffrer les sections de configuration dans ASP.NET 2,0 à l’aide de RSA](https://msdn.microsoft.com/library/ms998283.aspx)

Cet article explique comment utiliser le fournisseur de configuration protégée RSA et l’outil ASPNET\_regiis. exe pour chiffrer les sections de vos fichiers de configuration. Vous pouvez utiliser l’outil ASPNET\_regiis. exe pour chiffrer des données sensibles, telles que des chaînes de connexion, conservées dans...

[Utiliser l’emprunt d’identité et la délégation dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998351.aspx)

Cet article explique comment et quand vous devez utiliser l’emprunt d’identité dans les applications ASP.NET 2,0. Par défaut, l’emprunt d’identité est désactivé et vous pouvez accéder aux ressources à l’aide de l’identité de processus de l’application Web ASP.NET. Toutefois, vous pouvez utiliser...

<a id="sql"></a>
## <a name="sql-server-whitepapers"></a>Livres blancs SQL Server

Bien que ASP.NET fonctionne avec un large éventail de bases de données, ces livres blancs se penchent spécifiquement sur la connexion des applications ASP.NET à SQL Server.

[Se connecter à SQL Server à l’aide de l’authentification SQL dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998300.aspx)

Cet article explique comment connecter une application ASP.NET de manière sécurisée à Microsoft® SQL Server™ lorsque l’authentification d’accès à la base de données utilise l’authentification SQL Native. L’authentification Windows est la méthode recommandée pour se connecter à SQL Server, car vous...

[Se connecter à SQL Server à l’aide de l’authentification Windows dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998292.aspx)

Cet article explique comment se connecter à SQL Server 2000 à l’aide d’un compte de service Windows à partir d’une application ASP.NET version 2,0. Vous devez utiliser l’authentification Windows au lieu de l’authentification SQL chaque fois que cela est possible, car vous évitez de stocker les informations d’identification dans...

[Utiliser SSL pour sécuriser la communication avec SQL Server 2000](https://msdn.microsoft.com/library/aa302414.aspx)

Il est souvent vital que les applications soient en mesure de sécuriser les données transmises vers et à partir d’un serveur de base de données SQL Server. Avec SQL Server, vous pouvez utiliser SSL pour créer un canal chiffré. Cet article explique comment installer un certificat sur le serveur de base de données,...

<a id="general"></a>
## <a name="general-whitepapers"></a>Livres blancs généraux

L’étude de cas suivante décrit le processus qui a été utilisé pour migrer les sites Web de la communauté .NET de Microsoft d’un environnement d’hébergement traditionnel vers Microsoft Azure.

[Migration des sites Web de la communauté ASP.NET et IIS.NET de Microsoft vers Microsoft Azure](https://go.microsoft.com/fwlink/?LinkId=400656)

Ces livres blancs couvrent un large éventail de sujets concernant ASP.NET.

[Utiliser le contrôle d’intégrité dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998306.aspx)

Cet article explique comment utiliser le contrôle d’intégrité pour instrumenter votre application pour un événement personnalisé. Pour créer un événement de contrôle d’état personnalisé, vous créez une classe qui dérive de System. Web. Management. WebBaseEvent, configurez la &lt;healthMonitoring&gt;...

[Implémenter la gestion des correctifs](https://msdn.microsoft.com/library/aa302364.aspx)

Cet article explique comment gérer les correctifs, notamment comment conserver un ou plusieurs serveurs à jour. Aucun logiciel supplémentaire n’est requis, à l’exception des outils disponibles en téléchargement auprès de Microsoft. Les opérations et la stratégie de sécurité doivent adopter une gestion des correctifs...

[Contrôles serveur ASP.NET pour Silverlight dans le kit de développement logiciel (SDK) Silverlight 3](https://go.microsoft.com/fwlink/?LinkId=153377)

Les contrôles serveur ASP.NET pour Silverlight (« contrôles Silverlight ASP.NET »), qui sont les contrôles ASP.NET MediaPlayer et Silverlight, ont été supprimés du kit de développement logiciel (SDK) Silverlight pour Silverlight version 3. Ce document fournit des conseils pour les développeurs qui ont travaillé avec ces ASP.NET...

[Création d’applications Web hautes performances](https://devexpress.com/act)

Découvrez comment utiliser les nouvelles fonctionnalités de la bibliothèque AJAX ASP.NET pour générer des applications Web hautes performances
