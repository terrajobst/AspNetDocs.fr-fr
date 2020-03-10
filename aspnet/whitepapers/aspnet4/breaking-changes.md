---
uid: whitepapers/aspnet4/breaking-changes
title: Modifications avec rupture ASP.NET 4 | Microsoft Docs
author: rick-anderson
description: Ce document décrit les modifications apportées à la version 4 du .NET Framework qui peuvent potentiellement affecter les applications qui ont été créées à l’aide de...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 8ccad3b40a723c92a3164de082e1f94577141008
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546248"
---
# <a name="aspnet-4-breaking-changes"></a>Changements importants dans ASP.NET 4

> Ce document décrit les modifications apportées à la version 4 du .NET Framework qui peuvent potentiellement affecter les applications qui ont été créées à l’aide de versions antérieures, y compris les versions bêta 1 et bêta 2 de ASP.NET 4.
> 
> [Téléchargez ce livre blanc](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)

<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Contenu

[Paramètre ControlRenderingCompatibilityVersion dans le fichier Web. config](#0.1__Toc256770141 "_Toc256770141")  
[Modifications de ClientIDMode](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode et UrlEncode encodent désormais des guillemets simples](#0.1__Toc256770143 "_Toc256770143")  
[L’analyseur de page ASP.NET (. aspx) est plus strict](#0.1__Toc256770144 "_Toc256770144")  
[Fichiers de définition de navigateur mis à jour](#0.1__Toc256770145 "_Toc256770145")  
[System. Web. mobile. dll supprimé du fichier de configuration Web racine](#0.1__Toc256770146 "_Toc256770146")  
[Validation de la demande ASP.NET](#0.1__Toc256770147 "_Toc256770147")  
[L’algorithme de hachage par défaut est désormais HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Erreurs de configuration liées à la nouvelle configuration racine ASP.NET 4](#0.1__Toc256770149 "_Toc256770149")  
[Les applications enfants ASP.NET 4 ne parviennent pas à démarrer quand elles se trouvent sous ASP.NET 2,0 ou ASP.NET 3,5](#0.1__Toc256770150 "_Toc256770150")  
[Les sites Web ASP.NET 4 ne démarrent pas sur les ordinateurs où SharePoint est installé](#0.1__Toc256770151 "_Toc256770151")  
[La propriété HttpRequest. FilePath n’intègre plus les valeurs PathInfo](#0.1__Toc256770152 "_Toc256770152")  
[Les applications ASP.NET 2,0 peuvent générer des erreurs HttpException qui référencent EURL. axd](#0.1__Toc256770153 "_Toc256770153")  
[Les gestionnaires d’événements peuvent ne pas être déclenchés dans un document par défaut en mode intégré IIS 7 ou IIS 7,5](#0.1__Toc256770154 "_Toc256770154")  
[Modifications apportées à l’implémentation de la sécurité d’accès du code (CAS) ASP.NET](#0.1__Toc256770155 "_Toc256770155")  
[MembershipUser et d’autres types dans l’espace de noms System. Web. Security ont été déplacés](#0.1__Toc256770156 "_Toc256770156")  
[Modifications de la mise en cache de sortie à modifier \* en-tête HTTP](#0.1__Toc256770157 "_Toc256770157")  
[Les types System. Web. Security pour Passport sont obsolètes](#0.1__Toc256770158 "_Toc256770158")  
[La propriété MenuItem. PopOutImageUrl ne parvient pas à afficher une image dans ASP.NET 4](#0.1__Toc256770159 "_Toc256770159")  
[Menu. StaticPopOutImageUrl et menu. DynamicPopOutImageUrl ne pas afficher les images lorsque les chemins d’accès contiennent des barres obliques inverses](#0.1__Toc256770160 "_Toc256770160")  
[AVERTISSEMENT](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Paramètre ControlRenderingCompatibilityVersion dans le fichier Web. config

Les contrôles ASP.NET ont été modifiés dans le .NET Framework version 4 afin de vous permettre de spécifier plus précisément comment ils restituent le balisage. Dans les versions précédentes du .NET Framework, certains contrôles ont émis un balisage que vous n’aviez pas de moyen de désactiver. Par défaut, ASP.NET 4 ce type de balisage n’est plus généré.

Si vous utilisez Visual Studio 2010 pour mettre à niveau votre application à partir de ASP.NET 2,0 ou ASP.NET 3,5, l’outil ajoute automatiquement un paramètre au fichier `Web.config` qui conserve le rendu hérité. Toutefois, si vous mettez à niveau une application en changeant le pool d’applications dans IIS pour cibler le .NET Framework 4, ASP.NET utilise le nouveau mode de rendu par défaut. Pour désactiver le nouveau mode de rendu, ajoutez le paramètre suivant dans le fichier `Web.config` :

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Les principales modifications apportées par le nouveau comportement sont les suivantes :

- Les contrôles **image** et **ImageButton** n’affichent plus d’attribut `border="0"`.
- La classe **BaseValidator** et les contrôles de validation qui dérivent de celle-ci n’affichent plus de texte rouge par défaut.
- Le contrôle **HtmlForm** ne rend pas un attribut de **nom** .
- Le contrôle de **table** n’affiche plus d’attribut `border="0"`.
- Les contrôles qui ne sont pas conçus pour une entrée utilisateur (par exemple, le contrôle **label** ) n’affichent plus l’attribut `disabled="disabled"` si leur propriété **Enabled** a la valeur **false** (ou s’ils héritent ce paramètre d’un contrôle conteneur).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>Modifications de ClientIDMode

Le paramètre **ClientIDMode** dans ASP.net 4 vous permet de spécifier comment ASP.NET génère l’attribut **ID** pour les éléments HTML. Dans les versions précédentes de ASP.NET, le comportement par défaut était équivalent au paramètre **AutoID** de **ClientIDMode**. Toutefois, le paramètre par défaut est désormais **prévisible**.

Si vous utilisez Visual Studio 2010 pour mettre à niveau votre application à partir de ASP.NET 2,0 ou ASP.NET 3,5, l’outil ajoute automatiquement un paramètre au fichier `Web.config` qui conserve le comportement des versions antérieures du .NET Framework. Toutefois, si vous mettez à niveau une application en changeant le pool d’applications dans IIS pour cibler le .NET Framework 4, ASP.NET utilise le nouveau mode par défaut. Pour désactiver le nouveau mode ID client, ajoutez le paramètre suivant dans le fichier `Web.config` :

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode et UrlEncode encodent désormais des guillemets simples

Dans ASP.NET 4, les méthodes **HtmlEncode** et **UrlEncode** des classes **HttpUtility** et **HttpServerUtility** ont été mises à jour pour encoder le caractère guillemet simple (') comme suit :

- La méthode **HtmlEncode** encode les instances du guillemet simple en tant que'.
- La méthode **UrlEncode** encode les instances du guillemet simple sous la forme %27.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>L’analyseur de page ASP.NET (. aspx) est plus strict

L’analyseur de page pour les pages ASP.NET (fichiers`.aspx`) et les contrôles utilisateur (fichiers`.ascx`) est plus strict dans ASP.NET 4 et rejette davantage d’instances de balisage non valides. Par exemple, les deux extraits de code suivants s’analysent correctement dans les versions antérieures de ASP.NET, mais génèrent désormais des erreurs d’analyse dans ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Notez le point-virgule non valide à la fin de la balise **HiddenField** .

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Notez l’attribut de **style** non fermé qui s’exécute dans l’attribut **CssClass** .

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Fichiers de définition de navigateur mis à jour

Les fichiers de définition de navigateur ont été mis à jour pour inclure des informations sur les navigateurs et les appareils récemment sortis et mis à jour. Des navigateurs et appareils anciens (comme Netscape Navigator) ont été supprimés tandis que de nouveaux navigateurs et appareils (comme Google Chrome et Apple iPhone) ont été ajoutés.

Si votre application contient des définitions de navigateur personnalisées qui héritent de l’une des définitions de navigateur supprimées, une erreur s’affiche. Par exemple, si le dossier `App_Browsers` contient une définition de navigateur qui hérite de la définition du navigateur IE2, vous recevrez le message d’erreur de configuration suivant :

- Le navigateur ou l’élément de passerelle avec l’ID « IE2 » est introuvable.

> [!NOTE]
> L’objet **HttpBrowserCapabilities** (qui est exposé par la propriété **Request. Browser** de la page) est piloté par les fichiers de définitions de navigateur. Par conséquent, les informations retournées par l’accès à une propriété de cet objet dans ASP.NET 4 peuvent être différentes de celles retournées dans une version antérieure de ASP.NET.

Vous pouvez restaurer les anciens fichiers de définition de navigateur en copiant les fichiers de définition de navigateur à partir du dossier suivant :

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Copiez les fichiers dans le dossier `\CONFIG\Browsers` correspondant pour ASP.NET 4. Après avoir copié les fichiers, exécutez l’outil en ligne de commande ASPNET\_RegBrowsers. exe.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System. Web. mobile. dll supprimé du fichier de configuration Web racine

Dans les versions précédentes de ASP.NET, une référence à l’assembly System. Web. mobile. dll était incluse dans le fichier de `Web.config` racine dans la section **assemblys** sous. Afin d’améliorer les performances, la référence à cet assembly a été supprimée.

L’assembly System. Web. mobile. dll est inclus dans ASP.NET 4, mais il est déconseillé. Si vous souhaitez utiliser des types de l’assembly System. Web. mobile. dll, ajoutez une référence à cet assembly dans le fichier de `Web.config` racine ou dans un fichier d' `Web.config` d’application. Par exemple, si vous souhaitez utiliser l’un des contrôles mobiles ASP.NET (déconseillés), vous devez ajouter une référence à l’assembly System. Web. mobile. dll dans le fichier `Web.config`.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Validation de la demande ASP.NET

La fonctionnalité de validation de la demande dans ASP.NET fournit un certain niveau de protection par défaut contre les attaques de script entre sites (XSS). Dans les versions précédentes de ASP.NET, la validation de la demande était activée par défaut. Toutefois, il s’applique uniquement aux pages ASP.NET (fichiers`.aspx` et à leurs fichiers de classe) et uniquement lorsque ces pages étaient en cours d’exécution.

Dans ASP.NET 4, par défaut, la validation de la demande est activée pour toutes les demandes, car elle est activée avant la phase **beginRequest** d’une requête http. Par conséquent, la validation de la demande s’applique aux demandes pour toutes les ressources ASP.NET, pas seulement aux demandes de page. aspx. Cela comprend les requêtes telles que les appels de service Web et les gestionnaires HTTP personnalisés. La validation de la demande est également active lorsque les modules HTTP personnalisés lisent le contenu d’une requête HTTP.

Par conséquent, les erreurs de validation de la demande peuvent maintenant se produire pour les requêtes qui n’ont pas déclenché d’erreurs précédemment. Pour rétablir le comportement de la fonctionnalité de validation de demande ASP.NET 2,0, ajoutez le paramètre suivant dans le fichier `Web.config` :

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Toutefois, nous vous recommandons d’analyser les erreurs de validation de la demande pour déterminer si les gestionnaires, les modules ou tout autre code personnalisé existants peuvent accéder à des entrées HTTP potentiellement dangereuses qui pourraient être des vecteurs d’attaque XSS.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>L’algorithme de hachage par défaut est désormais HMACSHA256

ASP.NET utilise des algorithmes de chiffrement et de hachage pour sécuriser des données telles que les cookies d’authentification par formulaire et l’état d’affichage. Par défaut, ASP.NET 4 utilise désormais l’algorithme HMACSHA256 pour les opérations de hachage sur les cookies et l’état d’affichage. Les versions antérieures de ASP.NET utilisaient l’ancien algorithme HMACSHA1.

Vos applications peuvent être affectées si vous exécutez des environnements mixtes ASP.NET 2.0/ASP. NET 4 où les données telles que les cookies d’authentification par formulaire doivent fonctionner avec les versions across.NET Framework. Pour configurer une application Web ASP.NET 4 afin d’utiliser l’ancien algorithme HMACSHA1, ajoutez le paramètre suivant dans le fichier `Web.config` :

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Erreurs de configuration liées à la nouvelle configuration racine ASP.NET 4

Les fichiers de configuration racine (le fichier `machine.config` et le fichier `Web.config` racine) pour le .NET Framework 4 (et par conséquent ASP.NET 4) ont été mis à jour pour inclure la plupart des informations de configuration de réutilisabilité de ASP.NET 3,5 qui se trouvent dans les fichiers de `Web.config` de l’application. En raison de la complexité des systèmes de configuration IIS 7 et IIS 7,5 gérés, l’exécution d’applications ASP.NET 3,5 sous ASP.NET 4 et sous IIS 7 et IIS 7,5 peut entraîner des erreurs de configuration ASP.NET ou IIS.

Nous vous recommandons de mettre à niveau les applications ASP.NET 3,5 vers ASP.NET 4 en utilisant les outils de mise à niveau de projet dans Visual Studio 2010, si possible. Visual Studio 2010 modifie automatiquement le fichier `Web.config` de l’application ASP.NET 3,5 pour qu’il contienne les paramètres appropriés pour ASP.NET 4.

Toutefois, il s’agit d’un scénario pris en charge pour exécuter des applications ASP.NET 3,5 à l’aide de l' .NET Framework 4 sans recompilation. Dans ce cas, vous devrez peut-être modifier manuellement le fichier `Web.config` de l’application avant d’exécuter l’application sous le .NET Framework 4 et sous IIS 7 ou IIS 7,5.

Les deux sections suivantes décrivent les modifications que vous devrez peut-être apporter pour différentes combinaisons de logiciels.

**Windows Vista SP1 ou Windows Server 2008 SP1, où ni le correctif logiciel KB958854 ni SP2 ne sont installés.** Dans cette configuration, le système de configuration IIS 7 fusionne de manière incorrecte la configuration managée d’une application en comparant le fichier de `Web.config` au niveau de l’application avec les fichiers de `machine.config` ASP.NET 2,0. Pour cette raison, les fichiers de `Web.config` au niveau de l’application du .NET Framework 3,5 ou version ultérieure doivent avoir une définition de section de configuration **System. Web. extensions** (l’élément) afin de ne pas provoquer d’échec de validation IIS 7.

Toutefois, les entrées de fichiers `Web.config` au niveau de l’application modifiées manuellement qui ne correspondent pas exactement aux définitions de la section de configuration d’origine qui ont été introduites avec Visual Studio 2008 entraînent des erreurs de configuration ASP.NET. (Les entrées de configuration par défaut générées par Visual Studio 2008 fonctionnent correctement.) Un problème courant est que les fichiers `Web.config` modifiés manuellement ignorent les attributs de configuration **allowDefinition** et **RequirePermission** qui se trouvent dans les différentes définitions de section de configuration. Cela provoque une incompatibilité entre la section de configuration abrégée dans les fichiers de `Web.config` au niveau de l’application et la définition complète dans le fichier de `machine.config` ASP.NET 4. Par conséquent, au moment de l’exécution, le système de configuration ASP.NET 4 lève une erreur de configuration.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2 et Windows Vista SP1 et Windows Server 2008 SP1 sur lesquels le correctif logiciel KB958854 est installé.**

Dans ce scénario, le système de configuration natif IIS 7 et IIS 7,5 retourne une erreur de configuration, car il effectue une comparaison de texte sur l’attribut de **type** défini pour un gestionnaire de section de configuration managée. Étant donné que tous les fichiers `Web.config` générés par Visual Studio 2008 et Visual Studio 2008 SP1 ont « 3,5 » dans la chaîne de type pour le système. les gestionnaires de section de configuration **Web. extensions** (et associés), et étant donné que le fichier de `machine.config` ASP.net 4 a « 4,0 » dans l’attribut de **type** pour les mêmes gestionnaires de section de configuration, les applications générées dans Visual Studio 2008 ou Visual Studio 2008 SP1 échouent toujours à la validation de la configuration dans iis 7 et IIS 7,5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Résolution de ces problèmes

La solution de contournement pour le premier scénario consiste à mettre à jour le fichier `Web.config` au niveau de l’application en incluant le texte de configuration réutilisable à partir d’un fichier `Web.config` qui a été généré automatiquement par Visual Studio 2008.

Une autre solution de contournement pour le premier scénario consiste à installer le Service Pack 2 pour Vista ou Windows Server 2008 sur votre ordinateur ou à installer le correctif logiciel KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) pour résoudre le comportement incorrect de la fusion et de la configuration du système de configuration IIS. Toutefois, une fois que vous avez effectué l’une de ces actions, votre application rencontrera probablement une erreur de configuration en raison du problème décrit pour le deuxième scénario.

La solution de contournement pour le deuxième scénario consiste à supprimer ou commenter toutes les définitions de section de configuration **System. Web. extensions** et les définitions de groupe de section de configuration à partir du fichier de `Web.config` au niveau de l’application. Ces définitions se trouvent généralement en haut du fichier de `Web.config` au niveau de l’application et peuvent être identifiées par l’élément **configSections** et ses enfants.

Pour les deux scénarios, il est recommandé de supprimer également manuellement la section **System. CodeDom** , bien que cela ne soit pas obligatoire.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>Les applications enfants ASP.NET 4 ne parviennent pas à démarrer quand elles se trouvent sous ASP.NET 2,0 ou ASP.NET 3,5

Les applications ASP.NET 4 configurées comme enfants d’applications qui exécutent des versions antérieures d’ASP.NET risquent de ne pas démarrer en raison d’erreurs de configuration ou de compilation. L’exemple suivant illustre une structure de répertoires pour une application affectée.

`/parentwebapp` (configuré pour utiliser ASP.NET 2,0 ou ASP.NET 3,5)  
`/childwebapp` (configuré pour utiliser ASP.NET 4)

L’application dans le dossier `childwebapp` ne peut pas démarrer sur IIS 7 ou IIS 7,5 et signale une erreur de configuration. Le texte de l’erreur inclura un message similaire à ce qui suit :

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

Sur IIS 6, l’application dans le dossier `childwebapp` ne démarre pas non plus, mais signale une erreur différente. Par exemple, le texte d’erreur peut indiquer les éléments suivants :

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Ces scénarios se produisent parce que les informations de configuration de l’application parente dans le dossier `parentwebapp` font partie de la hiérarchie des informations de configuration qui détermine les paramètres de configuration fusionnés finaux qui sont utilisés par l’application Web enfant dans le dossier `childwebapp`. Selon que l’application Web ASP.NET 4 s’exécute sur IIS 7 (ou IIS 7,5) ou sur IIS 6, le système de configuration IIS ou le système de compilation ASP.NET 4 renvoie une erreur.

Les étapes que vous devez suivre pour résoudre ce problème et faire en sorte que l’application enfant ASP.NET 4 fonctionne varient selon que l’application ASP.NET 4 s’exécute sur IIS 6 ou sur IIS 7 (ou IIS 7,5).

### <a name="step-1-iis-7-or-iis-75-only"></a>Étape 1 (IIS 7 ou IIS 7,5 uniquement)

Cette étape est nécessaire uniquement sur les systèmes d’exploitation qui exécutent IIS 7 ou IIS 7,5, y compris Windows Vista, Windows Server 2008, Windows 7 et Windows Server 2008 R2.

Déplacez la définition **configSections** dans le fichier `Web.config` de l’application parente (l’application qui exécute ASP.NET 2,0 ou ASP.net 3,5) dans le fichier racine `Web.config` pour the.NET Framework 2,0. Le système de configuration Native IIS 7 et IIS 7,5 analyse l’élément **configSections** lorsqu’il fusionne la hiérarchie des fichiers de configuration. Le déplacement de la définition **configSections** du fichier `Web.config` de l’application Web parent vers le fichier `Web.config` racine masque effectivement l’élément du processus de fusion de configuration qui se produit pour l’application ASP.net 4 enfant.

Sur un système d’exploitation 32 bits ou pour les pools d’applications 32 bits, le fichier `Web.config` racine pour ASP.NET 2,0 et ASP.NET 3,5 se trouve normalement dans le dossier suivant :

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

Sur un système d’exploitation 64 bits ou pour les pools d’applications 64 bits, le fichier `Web.config` racine pour ASP.NET 2,0 et ASP.NET 3,5 se trouve normalement dans le dossier suivant :

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Si vous exécutez des applications Web 32 bits et 64 bits sur un ordinateur 64 bits, vous devez déplacer l’élément **configSections** vers le haut dans les fichiers de `Web.config` racine pour les systèmes 32 bits et 64 bits.

Lorsque vous placez l’élément **configSections** dans le fichier racine `Web.config`, collez la section juste après l’élément de **configuration** . L’exemple suivant montre à quoi doit ressembler la partie supérieure du fichier `Web.config` racine lorsque vous avez terminé de déplacer les éléments.

> [!NOTE]
> Dans l’exemple suivant, les lignes ont été encapsulées pour des raisons de lisibilité.

[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Étape 2 (toutes les versions d’IIS)

Cette étape est requise si l’application Web enfant ASP.NET 4 s’exécute sur IIS 6 ou sur IIS 7 (ou IIS 7,5).

Dans le fichier `Web.config` de l’application Web parente qui exécute ASP.NET 2 ou ASP.NET 3,5, ajoutez une balise d' **emplacement** qui spécifie explicitement (à la fois pour les systèmes de configuration IIS et ASP.net) que les entrées de configuration s’appliquent uniquement à l’application Web parente. L’exemple suivant illustre la syntaxe de l’élément **location** à ajouter :

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

L’exemple suivant montre comment la balise d' **emplacement** est utilisée pour encapsuler toutes les sections de configuration à partir de la section **appSettings** et se termine par la section **System. webServer** .

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Une fois les étapes 1 et 2 terminées, les applications Web enfants ASP.NET 4 démarrent sans erreur.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>Les sites Web ASP.NET 4 ne démarrent pas sur les ordinateurs où SharePoint est installé

Les serveurs Web qui exécutent SharePoint ont un fichier `Web.config` qui est déployé à la racine d’un site Web SharePoint (par exemple, `c:\inetpub\wwwroot\web.config` pour le site Web par défaut). Dans ce fichier `Web.config`, SharePoint définit un niveau de confiance partielle personnalisé nommé WSS\_minimal.

Si vous essayez d’exécuter un site Web ASP.NET 4 qui est déployé en tant qu’enfant de ce type de site Web SharePoint, l’erreur suivante s’affiche :

`Could not find permission set named 'ASP.NET'.`

Cette erreur se produit parce que l’infrastructure de sécurité d’accès du code (CAS) de ASP.NET 4 recherche un jeu d’autorisations nommé ASP.NET. Toutefois, le fichier de configuration de confiance partielle référencé par WSS\_minimal ne contient aucun jeu d’autorisations portant ce nom.

Actuellement, il n’existe pas de version de SharePoint disponible compatible avec ASP.NET. Par conséquent, vous ne devez pas essayer d’exécuter un site Web ASP.NET 4 en tant que site enfant sous les sites Web SharePoint.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>La propriété HttpRequest. FilePath n’intègre plus les valeurs PathInfo

Les versions précédentes de ASP.NET incluaient une valeur **PathInfo** dans la valeur retournée à partir de différentes propriétés liées au chemin d’accès de fichier, notamment **HttpRequest. FilePath**, **HttpRequest. AppRelativeCurrentExecutionFilePath**et **HttpRequest. CurrentExecutionFilePath**. ASP.NET 4 n’ajoute plus la valeur **PathInfo** dans les valeurs de retour de ces propriétés. Au lieu de cela, les informations **PathInfo** sont disponibles dans **HttpRequest. PathInfo**. Par exemple, imaginez le fragment d’URL suivant :

`/testapp/Action.mvc/SomeAction`

Dans les versions antérieures de ASP.NET, les propriétés **HttpRequest** ont les valeurs suivantes :

**HttpRequest. FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest. PathInfo**: (vide)

Dans ASP.NET 4, les propriétés **HttpRequest** ont à la place les valeurs suivantes :

**HttpRequest. FilePath**: `/testapp/Action.mvc`

**HttpRequest. PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>Les applications ASP.NET 2,0 peuvent générer des erreurs HttpException qui référencent EURL. axd

Une fois ASP.NET 4 activé sur IIS 6, les applications ASP.NET 2.0 qui s’exécutent sur IIS 6 (dans Windows Server 2003 ou Windows Server 2003 R2) risquent de générer des erreurs comme la suivante :

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Cette erreur se produit car lorsque ASP.NET détecte qu’un site Web est configuré pour utiliser ASP.NET 4, un composant natif de ASP.NET 4 transmet une URL sans extension à la partie managée de ASP.NET en vue d’un traitement supplémentaire. Toutefois, si les répertoires virtuels situés sous un site Web ASP.NET 4 sont configurés pour utiliser ASP.NET 2,0, le traitement de l’URL sans extension de cette façon génère une URL modifiée qui contient la chaîne « EURL. axd ». Cette URL modifiée est ensuite envoyée à l’application ASP.NET 2,0. ASP.NET 2,0 ne peut pas reconnaître le format « EURL. axd ». Par conséquent, ASP.NET 2,0 tente de trouver un fichier nommé `eurl.axd` et de l’exécuter. Étant donné qu’aucun fichier de ce type n’existe, la requête échoue avec une exception **HttpException** .

Vous pouvez contourner ce problème à l’aide de l’une des options suivantes.

### <a name="option-1"></a>Option 1 :

Si ASP.NET 4 n’est pas requis pour exécuter le site Web, remappez le site pour utiliser ASP.NET 2,0 à la place.

### <a name="option-2"></a>Option 2 :

Si ASP.NET 4 est requis pour exécuter le site Web, déplacez les répertoires virtuels ASP.NET 2,0 enfants vers un autre site Web mappé à ASP.NET 2,0.

### <a name="option-3"></a>Option 3

S’il n’est pas pratique de remapper le site Web à ASP.NET 2,0 ou de modifier l’emplacement d’un répertoire virtuel, désactivez explicitement le traitement des URL sans extension dans ASP.NET 4. Procédez comme suit :

1. Dans le Registre Windows, ouvrez le nœud suivant :

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Créez une nouvelle valeur **DWORD** nommée **EnableExtensionlessUrls**.
2. Affectez à **EnableExtensionlessUrls** la valeur 0. Cela désactive le comportement des URL sans extension.
3. Enregistrez la valeur de Registre et fermez l’éditeur du Registre.
4. Exécutez l’outil en ligne de commande **IISReset** , ce qui amène IIS à lire la nouvelle valeur de registre.

> [!NOTE]
> L’affectation de la valeur 1 à **EnableExtensionlessUrls** active le comportement des URL sans extension. Il s’agit du paramètre par défaut si aucune valeur n’est spécifiée.

<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Les gestionnaires d’événements peuvent ne pas être déclenchés dans un document par défaut en mode intégré IIS 7 ou IIS 7,5

ASP.NET 4 comprend des modifications qui modifient la façon dont l’attribut **action** de l’élément de **formulaire** HTML est rendu quand une URL sans extension correspond à un document par défaut. Un exemple de résolution d’URL sans extension vers un document par défaut serait [http://contoso.com/](http://contoso.com/), provoquant une demande d' [http://contoso.com/Default.aspx](http://contoso.com/Default.aspx).

ASP.NET 4 affiche désormais la valeur d’attribut **action** de l’élément de **formulaire** HTML sous la forme d’une chaîne vide quand une demande est effectuée à une URL sans extension à laquelle un document par défaut est mappé. Par exemple, dans les versions antérieures de ASP.NET, une demande de [http://contoso.com](http://contoso.com) entraînerait une demande de `Default.aspx`. Dans ce document, la balise **Form** d’ouverture serait affichée comme dans l’exemple suivant :

`<form action="Default.aspx" />`

Dans ASP.NET 4, une demande de [http://contoso.com](http://contoso.com) entraîne également une demande à `Default.aspx`. Toutefois, ASP.NET affiche maintenant la balise **Form** d’ouverture html comme dans l’exemple suivant :

`<form action="" />`

Cette différence dans la façon dont l’attribut d' **action** est rendu peut entraîner des modifications subtiles dans le mode de traitement d’une publication de formulaire par IIS et ASP.net. Lorsque l’attribut **action** est une chaîne vide, l’objet **DefaultDocumentModule** IIS crée une demande enfant pour `Default.aspx`. Dans la plupart des cas, cette demande enfant est transparente pour le code de l’application, et la page `Default.aspx` s’exécute normalement.

Toutefois, en raison d’une interaction potentielle entre le code managé et le mode intégré IIS 7 ou IIS 7.5, les pages .aspx managées peuvent cesser de fonctionner correctement pendant la demande enfant. Si les conditions suivantes sont remplies, la demande enfant à un `Default.aspx` document génère une erreur ou un comportement inattendu :

1. Une page. aspx est envoyée au navigateur avec l’attribut **action** de l’élément de **formulaire** défini sur «».
2. Le formulaire est publié dans ASP.NET.
3. Un module HTTP managé lit une partie du corps de l’entité. Par exemple, un module lit **Request. Form** ou **Request. params**. Le corps d’entité de la demande POST est ainsi lu dans la mémoire managée. Le corps d’entité n’est donc plus disponible pour les modules de code natif qui s’exécutent en mode intégré IIS 7 ou IIS 7.5.
4. L’objet **DEFAULTDOCUMENTMODULE** IIS finit par s’exécuter et crée une demande enfant dans le document `Default.aspx`. Toutefois, étant donné que le corps d’entité a déjà été lu par une partie du code managé, aucun corps d’entité n’est disponible pour un envoi à la demande enfant.
5. Lorsque le pipeline HTTP s’exécute pour la demande enfant, le gestionnaire des fichiers `.aspx` s’exécute pendant la phase d’exécution du gestionnaire.
6. Étant donné qu’il n’y a pas de corps d’entité, il n’existe aucune variable de formulaire ni aucun État d’affichage, et par conséquent, aucune information n’est disponible pour le gestionnaire de page. aspx pour déterminer l’événement (le cas échéant) supposé être déclenché. Par conséquent, aucun des gestionnaires d’événements de publication pour la page .aspx concernée ne s’exécute.

Vous pouvez contourner ce comportement des manières suivantes :

- Identifiez le module HTTP qui accède au corps d’entité de la requête pendant les demandes de documents par défaut et déterminez s’il peut être configuré pour s’exécuter uniquement pour les demandes managées. En mode intégré pour IIS 7 et IIS 7,5, les modules HTTP peuvent être marqués pour s’exécuter uniquement pour les demandes managées en ajoutant l’attribut suivant à l’entrée **System. webServer/modules** du module :

- `precondition="managedHandler"`

- Ce paramètre désactive le module pour les demandes qu’IIS 7 et IIS 7,5 déterminent comme des demandes qui ne sont pas gérées. Pour les demandes de document par défaut, la première demande est une URL sans extension. Par conséquent, IIS n’exécute aucun module managé marqué avec une condition préalable du gestionnaire géré pendant le traitement de la requête initiale. Par conséquent, les modules managés ne lisent pas accidentellement le corps de l’entité et le corps de l’entité est donc toujours disponible et est transmis à la demande enfant et au document par défaut.

- Si les modules HTTP problématiques doivent s’exécuter pour toutes les requêtes (pour les fichiers statiques, pour les URL sans extension qui se résolvent en objet **DefaultDocumentModule** , pour les demandes managées, etc.), modifiez les pages. aspx affectées en définissant explicitement la propriété **action** du contrôle **System. Web. UI. HtmlControls. HtmlForm** de la page sur une chaîne non vide. Par exemple, si le document par défaut est `Default.aspx`, modifiez le code de la page pour définir explicitement la propriété **action** du contrôle **HtmlForm** sur « default. aspx ».

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Modifications apportées à l’implémentation de la sécurité d’accès du code (CAS) ASP.NET

ASP.NET 2,0 et, par extension, les fonctionnalités ASP.NET qui ont été ajoutées dans 3,5, utilisent le modèle de sécurité d’accès du code (CAS) .NET Framework 1,1 et 2,0. Toutefois, l’implémentation de CAS dans ASP.NET 4 a été considérablement revue. Par conséquent, les applications ASP.NET de confiance partielle qui reposent sur du code de confiance s’exécutant dans le Global Assembly Cache (GAC) peuvent échouer avec différentes exceptions de sécurité. Les applications de confiance partielle qui reposent sur des modifications approfondies à la stratégie CAS d’ordinateur peuvent également échouer avec des exceptions de sécurité.

Vous pouvez rétablir le comportement des applications ASP.NET 4 de confiance partielle sur le comportement de ASP.NET 1,1 et 2,0 à l’aide du nouvel attribut **LegacyCasModel** de l’élément de configuration **Trust** , comme indiqué dans l’exemple suivant :

`<trust level= "Medium" legacyCasModel="true" />`

Lorsque vous revenez au modèle CAS hérité, les anciens comportements d’autorité de certification suivants sont activés :

- La stratégie CAS de l’ordinateur est respectée.
- Plusieurs jeux d’autorisations différents dans un même domaine d’application sont autorisés.
- Les assertions d’autorisation explicites ne sont pas requises pour les assemblys dans le GAC qui sont appelés lorsque seul ASP.NET ou autre .NET Framework code se trouve sur la pile.

Un scénario ne peut pas être rétabli dans le .NET Framework 4 : les applications de confiance partielle non Web ne peuvent plus appeler certaines API dans System. Web. dll et System. Web. extensions. dll. Dans les versions précédentes de la .NET Framework, il était possible pour les applications de confiance partielle non Web d’accorder explicitement des autorisations **AspNetHostingPermission** . Ces applications peuvent ensuite utiliser **System. Web. HttpUtility**, les types dans les espaces de noms **System. Web. ClientServices.\*** et les types liés à l’appartenance, aux rôles et aux profils. L’appel de ces types à partir d’applications de confiance partielle non Web n’est plus pris en charge dans le .NET Framework 4.

> [!NOTE]
> Les fonctionnalités **HtmlEncode** et **HtmlDecode** de la classe **System. Web. HttpUtility** ont été déplacées vers la nouvelle classe **system .net. WebUtility** 4 .NET Framework 4. Si c’était la seule fonctionnalité ASP.NET utilisée, modifiez le code de l’application pour utiliser la nouvelle classe **WebUtility** à la place.

Voici un résumé de haut niveau des modifications apportées à l’implémentation des autorités de certification par défaut dans ASP.NET 4 :

- Les domaines d’application ASP.NET sont désormais des domaines d’application homogènes. Seuls les ensembles d’autorisations de confiance partielle et de confiance totale sont disponibles dans un domaine d’application.
- Les jeux d’autorisations de confiance partielle ASP.NET sont indépendants de toute stratégie CAS au niveau de l’entreprise, de l’ordinateur ou de l’utilisateur.
- Les assemblys ASP.NET inclus dans 3,5 et 3,5 SP1 ont été convertis pour utiliser le modèle de transparence .NET Framework 4.
- L’utilisation de l’attribut ASP.NET **AspNetHostingPermission** a été considérablement réduite. La plupart des instances de cet attribut ont été supprimées des API ASP.NET publiques.
- Les assemblys compilés dynamiquement qui sont créés par les fournisseurs de build ASP.NET ont été mis à jour pour marquer explicitement les assemblys comme étant transparents.
- Tous les assemblys ASP.NET sont maintenant marqués de sorte que l’attribut APTCA soit respecté uniquement dans les environnements d’hébergement Web. Les environnements d’hébergement non Web partiellement approuvés comme ClickOnce ne pourront pas effectuer d’appel dans des assemblys ASP.NET.

Pour plus d’informations sur le nouveau modèle de sécurité d’accès du code ASP.NET 4, consultez [utilisation de la sécurité d’accès du code dans les Applications ASP.net](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) sur le site Web MSDN.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>MembershipUser et d’autres types dans l’espace de noms System. Web. Security ont été déplacés

Certains types qui sont utilisés dans l’appartenance à ASP.NET ont été déplacés de `System.Web.dll` vers le nouvel assembly System. Web. ApplicationServices. dll. Les types ont été déplacés pour résoudre des dépendances de couches architecturales entre des types dans le client et dans des références SKU .NET Framework étendues.

Les projets de site Web n’ont pas de problème suite au déplacement de ces types, car System. Web. ApplicationServices. dll a été ajouté à la liste des assemblys référencés qui est utilisée par défaut par le système de compilation ASP.NET. Si vous mettez à niveau un projet de site Web créé à l’aide d’une version antérieure de ASP.NET à ASP.NET 4 en l’ouvrant dans Visual Studio 2010, le projet se compilera sans erreurs.

De même, si vous mettez à niveau un projet d’application Web créé dans une version antérieure de ASP.NET vers ASP.NET 4 en l’ouvrant dans Visual Studio 2010, le processus de mise à niveau ajoute une référence à System. Web. ApplicationServices. dll au projet. Par conséquent, les projets d’application Web mis à niveau seront également compilés sans erreurs.

Les fichiers compilés (binaires) qui ont été créés à l’aide des versions antérieures de ASP.NET s’exécuteront également sans erreur sur ASP.NET 4, même si les types d’appartenance ont été déplacés vers un assembly différent. Les informations de transfert de type ont été ajoutées à la version ASP.NET 4 de `System.Web.dll` qui achemine automatiquement les références au moment de l’exécution pour ces types vers le nouvel emplacement pour les types.

Toutefois, les bibliothèques de classes qui utilisent des types d’appartenance spécifiques et qui ont été mis à niveau à partir de versions antérieures de ASP.NET ne peuvent pas être compilées lorsqu’elles sont utilisées dans un projet ASP.NET 4. Par exemple, un projet de bibliothèque de classes peut ne pas réussir à compiler et signaler une erreur telle que la suivante :

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Vous pouvez contourner ce problème en ajoutant une référence dans votre projet de bibliothèque de classes à System. Web. ApplicationServices. dll.

La liste suivante répertorie les types *System. Web. Security* qui ont été déplacés de `System.Web.dll` vers System. Web. ApplicationServices. dll :

- *System. Web. Security. MembershipCreateStatus*
- *System. Web. Security. Membership. CreateUserException*
- *System. Web. Security. MembershipPasswordException*
- *System. Web. Security. MembershipPasswordFormat*
- *System. Web. Security. MembershipProvider*
- *System. Web. Security. MembershipProviderCollection*
- *System. Web. Security. MembershipUser*
- *System. Web. Security. MembershipUserCollection*
- *System. Web. Security. MembershipValidatePasswordEventHandler*
- *System. Web. Security. ValidatePasswordEventArgs*
- *System. Web. Security. RoleProvider*
- <a id="0.1_a"></a>*System. Web. Configuration. MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>Modifications de la mise en cache de sortie à modifier \* en-tête HTTP

Dans ASP.NET 1,0, un bogue a provoqué des pages mises en cache qui spécifiaient `Location="ServerAndClient"` en tant que paramètre OUTPUT-cache pour émettre un en-tête HTTP `Vary:*` dans la réponse. Les navigateurs clients ne mettaient donc jamais en cache la page localement.

Dans ASP.NET 1,1, la méthode **System. Web. HttpCachePolicy. SetOmitVaryStar** a été ajoutée, ce que vous pouvez appeler pour supprimer l’en-tête `Vary:*`. Cette méthode a été choisie, car la modification de l’en-tête HTTP émis a été considérée comme une modification avec rupture à ce moment-là. Toutefois, les développeurs ont été confondus par le comportement dans ASP.NET, et les rapports de bogues suggèrent que les développeurs n’ont pas conscience du comportement **SetOmitVaryStar** existant.

Dans ASP.NET 4, la décision a été prise de résoudre le problème racine. L’en-tête HTTP `Vary:*` n’est plus émis à partir des réponses qui spécifient la directive suivante :

`<%@OutputCache Location="ServerAndClient" %>`

Par conséquent, **SetOmitVaryStar** n’est plus nécessaire pour supprimer l’en-tête `Vary:*`.

Dans les applications qui spécifient `Location="ServerAndClient"` dans la directive **@ OutputCache** sur une page, vous voyez maintenant le comportement impliqué par le nom de la valeur de l’attribut d' **emplacement** , c’est-à-dire que les pages seront mises en cache dans le navigateur sans que vous ayez besoin d’appeler la méthode **SetOmitVaryStar** .

Si des pages de votre application doivent émettre des `Vary:*`, appelez la méthode **AppendHeader** , comme dans l’exemple suivant :

`HttpResponse.AppendHeader("Vary","*");`

Vous pouvez également modifier la valeur de l’attribut **emplacement** de la mise en cache de sortie en « serveur ».

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Les types System. Web. Security pour Passport sont obsolètes

La prise en charge de Passport intégrée à ASP.NET 2,0 a été obsolète et n’est pas prise en charge depuis quelques années en raison des modifications apportées à Passport (désormais LiveID). Par conséquent, les cinq types liés à Passport dans **System. Web. Security** sont désormais marqués avec l’attribut **ObsoleteAttribute** .

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>La propriété MenuItem. PopOutImageUrl ne parvient pas à afficher une image dans ASP.NET 4

Dans ASP.NET 3,5, la propriété *MenuItem. PopOutImageUrl* vous permet de spécifier l’URL d’une image qui est affichée dans un élément de menu pour indiquer que l’élément de menu a un sous-menu dynamique. L’exemple suivant montre comment spécifier cette propriété dans le balisage dans ASP.NET 3,5.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

En raison d’un changement de conception dans ASP.NET 4, aucune sortie n’est restituée pour le *PopOutImageUrl* si la propriété est définie pour la classe *MenuItem* . Au lieu de cela, vous devez spécifier une URL d’image directement dans le contrôle de *menu* à l’aide de la propriété *StaticPopOutImageUrl* ou de la propriété *DynamicPopOutImageUrl* . Quand vous utilisez un menu statique, la propriété *menu. StaticPopOutImageUrl* spécifie l’URL d’une image qui est affichée pour indiquer que l’élément de menu statique a un sous-menu, comme illustré dans l’exemple suivant :

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Si vous travaillez avec un menu dynamique, vous utilisez la propriété *menu. DynamicPopOutImageUrl* pour spécifier l’URL d’une image qui indique qu’un élément de menu dynamique a un sous-menu. L’exemple suivant est semblable au précédent, mais montre comment définir la propriété *DynamicPopOutImageUrl* pour un menu dynamique.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Si la propriété *menu. DynamicPopOutImageUrl* n’est pas définie et que la propriété *menu. DynamicEnableDefaultPopOutImage* est définie sur *false*, aucune image ne s’affiche. De même, si la propriété *StaticPopOutImageUrl* n’est pas définie et que la propriété *StaticEnableDefaultPopOutImage* est définie sur *false*, aucune image ne s’affiche.

Lorsque vous définissez les chemins d’accès de ces propriétés, utilisez une barre oblique (/) au lieu d’une barre oblique inverse (\). Pour plus d’informations, consultez [menu. StaticPopOutImageUrl et menu. DynamicPopOutImageUrl ne pas afficher les images lorsque les chemins d’accès contiennent des barres obliques inverses](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu. StaticPopOutImageUrl_and_Menu.") ailleurs dans ce document.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu. StaticPopOutImageUrl et menu. DynamicPopOutImageUrl ne pas afficher les images lorsque les chemins d’accès contiennent des barres obliques inverses

Dans ASP.NET 4, les images que vous spécifiez à l’aide des propriétés *menu. StaticPopOutImageUrl* et *menu. DynamicPopOutImageUrl* ne sont pas restituées si le chemin d’accès contient des points d’ancrage (\). Il s’agit d’une modification par rapport aux versions antérieures de ASP.NET.

L’exemple de balisage de contrôle de *menu* suivant montre le jeu de propriétés *StaticPopOutImageUrl* à l’aide d’un chemin d’accès qui contient une barre oblique inverse. Dans ASP.NET 4, l’image spécifiée dans la propriété n’est pas restituée.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Pour corriger ce problème, modifiez les valeurs de chemin d’accès spécifiées dans les propriétés *StaticPopOutImageUrl* et *DynamicPopOutImageUrl* pour qu’elles utilisent des barres obliques (/). L’exemple suivant illustre cette modification :

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Notez que les applications qui ont été migrées à partir de versions antérieures de ASP.NET vers ASP.NET 4 peuvent également être affectées, car la propriété *MenuItem. PopOutImageUrl* a été modifiée. Pour plus d’informations, consultez [la propriété MenuItem. PopOutImageUrl ne parvient pas à afficher une image dans ASP.net 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem. PopOutImageUrl_Propert") ailleurs dans ce document.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Exclusion de responsabilité

Ce document est une version préliminaire et peut être modifié substantiellement avant le lancement de la mise en production commerciale finale du logiciel qu’il décrit.

Les informations contenues dans ce document correspondent à la connaissance que Microsoft Corporation possède des problèmes abordés à la date de la publication. Microsoft devant répondre à des conditions de marché qui évoluent, ce document ne doit pas être considéré comme un engagement de sa part, et Microsoft ne peut pas garantir l’exactitude des informations présentées à la date de la publication.

Ce livre blanc est fourni à titre d'information uniquement. MICROSOFT NE FOURNIT AUCUNE GARANTIE, EXPRESSE, IMPLICITE OU LÉGALE, QUANT AUX INFORMATIONS CONTENUES DANS CE DOCUMENT.

L'utilisateur est tenu d'observer la réglementation relative aux droits d'auteur applicable dans son pays. Aucune partie de ce document ne peut être reproduite, stockée ou introduite dans un système de restitution, ou transmise à quelque fin ou par quelque moyen que ce soit (électronique, mécanique, photocopie, enregistrement ou autre) sans la permission expresse et écrite de Microsoft Corporation.

Microsoft peut détenir des brevets, avoir déposé des demandes d'enregistrement de brevets ou être titulaire de marques, droits d'auteur ou autres droits de propriété intellectuelle portant sur tout ou partie des éléments qui font l'objet du présent document. Sauf stipulation expresse contraire d'un contrat de licence écrit de Microsoft, la fourniture de ce document n'a pas pour effet de vous concéder une licence sur ces brevets, marques, droits d'auteur ou autres droits de propriété intellectuelle.

Sauf mention contraire, les exemples de sociétés, d’organisations, de produits, de noms de domaine, d’adresses de messagerie, de logos, de personnes, de lieux et d’événements mentionnés dans le présent document sont fictifs et ne sont pas associés à une société, une organisation, un produit, un nom de domaine, une adresse de messagerie réels une adresse, un logo, une personne, un lieu ou un événement est intentionnel ou doit être déduit.

© 2010 Microsoft Corporation. Tous droits réservés.

Microsoft et Windows sont soit des marques déposées de Microsoft Corporation, soit des marques de Microsoft Corporation aux États-Unis d'Amérique et/ou dans d'autres pays.

Les noms des sociétés et des produits mentionnés dans le présent document peuvent être des marques de leurs propriétaires respectifs.
