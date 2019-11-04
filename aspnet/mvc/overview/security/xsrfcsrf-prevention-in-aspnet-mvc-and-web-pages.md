---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: Prévention de XSRF/CSRF dans ASP.NET MVC et les pages Web | Microsoft Docs
author: Rick-Anderson
description: La falsification de requête intersites (également appelée XSRF ou CSRF) est une attaque contre les applications hébergées sur le Web, ce qui permet à un site Web malveillant d’influencer l’interaction...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 6fcfcda5b95e5844f7d357ac0cbb6d1fd2e215ac
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445767"
---
# <a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Prévention de XSRF/CSRF dans ASP.NET MVC et les pages web

par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> La falsification de requête intersites (également appelée XSRF ou CSRF) est une attaque contre les applications hébergées sur le Web, ce qui permet à un site Web malveillant d’influencer l’interaction entre un navigateur client et un site Web approuvé par ce navigateur. Ces attaques sont rendues possibles, car les navigateurs Web enverront automatiquement des jetons d’authentification à chaque demande adressée à un site Web. L’exemple canonique est un cookie d’authentification, tel qu’ASP. Ticket d’authentification par formulaire du réseau. Toutefois, les sites Web qui utilisent un mécanisme d’authentification persistant (tel que l’authentification Windows, de base, etc.) peuvent être ciblés par ces attaques.
> 
> Une attaque XSRF est distincte d’une attaque par hameçonnage. Les attaques par hameçonnage nécessitent une interaction de la victime. Dans une attaque par hameçonnage, un site Web malveillant imite le site Web cible et la victime est trompeur en fournissant des informations sensibles à la personne malveillante. Dans une attaque XSRF, il n’y a souvent aucune interaction nécessaire de la part de la victime. Au lieu de cela, le pirate s’appuie sur le navigateur pour envoyer automatiquement tous les cookies pertinents au site Web de destination.
> 
> Pour plus d’informations, consultez le projet OWASP ( [Open Web Application Security](https://www.owasp.org/index.php/Main_Page)) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).

## <a name="anatomy-of-an-attack"></a>Anatomie d’une attaque

Pour parcourir une attaque XSRF, envisagez un utilisateur qui souhaite effectuer des transactions bancaires en ligne. Cet utilisateur accède pour la première fois à WoodgroveBank.com et se connecte, à partir duquel l’en-tête de réponse contiendra son cookie d’authentification :

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Étant donné que le cookie d’authentification est un cookie de session, il est automatiquement supprimé par le navigateur lorsque le processus de navigation s’arrête. Toutefois, jusqu’à ce moment-là, le navigateur inclura automatiquement le cookie avec chaque demande à WoodgroveBank.com. L’utilisateur souhaite maintenant transférer $1000 vers un autre compte. il remplit donc un formulaire sur le site bancaire et le navigateur envoie cette demande au serveur :

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Étant donné que cette opération a un effet secondaire (elle initie une transaction monétaire), le site bancaire a choisi d’exiger un HTTP postal afin de lancer cette opération. Le serveur lit le jeton d’authentification à partir de la demande, recherche le numéro de compte de l’utilisateur actuel, vérifie qu’il y a suffisamment de fonds, puis initialise la transaction dans le compte de destination.

La Banque en ligne est terminée, l’utilisateur quitte le site bancaire et consulte d’autres sites sur le Web. L’un de ces sites (fabrikam.com) comprend le balisage suivant sur une page incorporée dans un &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Ce qui amène ensuite le navigateur à effectuer cette requête :

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

L’attaquant exploite le fait que l’utilisateur peut toujours disposer d’un jeton d’authentification valide pour le site Web cible et qu’il utilise un petit extrait de code JavaScript pour faire en sorte que le navigateur effectue automatiquement une publication HTTP sur le site cible. Si le jeton d’authentification est toujours valide, le site bancaire initie un transfert de $250 vers le compte du choix de l’attaquant.

### <a name="ineffective-mitigations"></a>Atténuations inefficaces

Il est intéressant de noter que dans le scénario ci-dessus, le fait que WoodgroveBank.com était accessible via SSL et avait un cookie d’authentification SSL uniquement insuffisant pour contrecarrer l’attaque. L’attaquant est en mesure de spécifier le [schéma d’URI](http://en.wikipedia.org/wiki/URI_scheme) (https) dans son &lt;&gt; formulaire, et le navigateur continue à envoyer des cookies non expirés au site cible tant que ces cookies sont cohérents avec le schéma d’URI de la cible prévue.

Il est possible de faire en sorte que l’utilisateur ne puisse pas visiter les sites non approuvés, car la visite de sites de confiance uniquement permet de rester en ligne en toute sécurité. Il y a de la vérité, mais malheureusement ce Conseil n’est pas toujours pratique. Peut-être que l’utilisateur « approuve » le site de News local ConsolidatedMessenger. ConsolidatedMessenger.com et accède à ce site à la place, mais ce site présente une vulnérabilité XSS qui permet à une personne malveillante d’injecter le même extrait de code qui s’exécutait sur fabrikam.com.

Vous pouvez vérifier que les demandes entrantes comportent un [en-tête Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) référençant votre domaine. Cela permet d’arrêter les demandes involontairement envoyées à partir d’un domaine tiers. Toutefois, certaines personnes désactivent l’en-tête Referer de leur navigateur pour des raisons de confidentialité, et les attaquants peuvent parfois usurper cet en-tête si certains logiciels non sécurisés sont installés sur la victime. La vérification de l' [en-tête Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) n’est pas considérée comme une approche sécurisée pour empêcher les attaques XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Atténuations du XSRF d’exécution de la pile Web

Le runtime ASP.NET Web Stack utilise une variante du [modèle de jeton du synchronisateur](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#synchronizer-token-pattern) pour se défendre contre les attaques XSRF. La forme générale du modèle de jeton du synchronisateur est que deux jetons anti-XSRF sont envoyés au serveur avec chaque HTTP postérieur (en plus du jeton d’authentification) : un jeton en tant que cookie et l’autre en tant que valeur de formulaire. Les valeurs de jeton générées par le runtime ASP.NET ne sont pas déterministes ou prévisibles par une personne malveillante. Lors de l’envoi des jetons, le serveur autorise la requête à continuer uniquement si les deux jetons passent un contrôle de comparaison.

Le *jeton de session* de vérification de demande XSRF est stocké en tant que cookie HTTP et contient actuellement les informations suivantes dans sa charge utile :

- Un jeton de sécurité, qui se compose d’un identificateur 128 bits aléatoire.   
 L’illustration suivante montre le jeton de session de vérification de demande XSRF affiché avec les outils de développement F12 d’Internet Explorer : (Notez qu’il s’agit de l’implémentation actuelle et qu’elle est sujette à modification.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

Le *jeton de champ* est stocké en tant que `<input type="hidden" />` et contient les informations suivantes dans sa charge utile :

- Nom d’utilisateur de l’utilisateur connecté (s’il est authentifié).
- Toutes les données supplémentaires fournies par un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Les charges utiles des jetons anti-XSRF sont chiffrées et signées. vous ne pouvez donc pas afficher le nom d’utilisateur lorsque vous utilisez des outils pour examiner les jetons. Lorsque l’application Web cible ASP.NET 4,0, les services de chiffrement sont fournis par la routine [machineKey. Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) . Lorsque l’application Web cible ASP.NET 4,5 ou une version ultérieure, les services de chiffrement sont fournis par la routine [machineKey. Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) , qui offre de meilleures performances, une meilleure extensibilité et une plus grande sécurité. Pour plus d’informations, consultez les billets de blog suivants :

- [Améliorations du chiffrement dans ASP.NET 4,5, PT. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Améliorations du chiffrement dans ASP.NET 4,5, PT. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Améliorations du chiffrement dans ASP.NET 4,5, PT. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Génération des jetons

Pour générer les jetons anti-XSRF, appelez la méthode [@Html.AntiForgeryToken](https://msdn.microsoft.com/library/dd470175.aspx) à partir d’une vue MVC ou @AntiForgery.GetHtml() à partir d’une page Razor. Le Runtime effectue ensuite les étapes suivantes :

1. Si la requête HTTP actuelle contient déjà un jeton de session anti-XSRF (le cookie anti-XSRF \_\_RequestVerificationToken), le jeton de sécurité est extrait de celui-ci. Si la requête HTTP ne contient pas de jeton de session anti-XSRF ou si l’extraction du jeton de sécurité échoue, un nouveau jeton anti-XSRF aléatoire est généré.
2. Un jeton de champ anti-XSRF est généré à l’aide du jeton de sécurité de l’étape (1) ci-dessus et de l’identité de l’utilisateur actuellement connecté. (Pour plus d’informations sur la détermination de l’identité de l’utilisateur, consultez la section **[scénarios avec prise en charge spéciale](#_Scenarios_with_special)** ci-dessous.) En outre, si un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) est configuré, le runtime appelle sa méthode [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) et inclut la chaîne retournée dans le jeton de champ. (Pour plus d’informations, consultez la section **[configuration et extensibilité](#_Configuration_and_extensibility)** .)
3. Si un nouveau jeton anti-XSRF a été généré à l’étape (1), un nouveau jeton de session sera créé pour le contenir et sera ajouté à la collection de cookies HTTP sortants. Le jeton de champ de l’étape (2) sera encapsulé dans un élément `<input type="hidden" />`, et ce balisage HTML sera la valeur de retour de `Html.AntiForgeryToken()` ou `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Validation des jetons

Pour valider les jetons anti-XSRF entrants, le développeur comprend un attribut [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) sur son action ou contrôleur MVC, ou elle appelle `@AntiForgery.Validate()` à partir de sa page Razor. Le Runtime effectue les étapes suivantes :

1. Le jeton de session entrant et le jeton de champ sont lus et le jeton anti-XSRF extrait de chaque. Les jetons anti-XSRF doivent être identiques par étape (2) dans la routine de génération.
2. Si l’utilisateur actuel est authentifié, son nom d’utilisateur est comparé au nom d’utilisateur stocké dans le jeton de champ. Les noms d’utilisateur doivent correspondre.
3. Si un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) est configuré, le runtime appelle sa méthode *ValidateAdditionalData* . La méthode doit retourner la valeur booléenne *true*.

Si la validation est réussie, la demande est autorisée à se poursuivre. Si la validation échoue, le Framework lève une *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Conditions d’échec

À compter du runtime ASP.NET Web Stack v2, les *HttpAntiForgeryException* qui sont levées pendant la validation contiennent des informations détaillées sur ce qui s’est produit. Les conditions d’échec actuellement définies sont les suivantes :

- Le jeton de session ou le jeton de formulaire n’est pas présent dans la demande.
- Le jeton de session ou le jeton de formulaire est illisible. La cause la plus probable est qu’une batterie de serveurs exécutant des versions incompatibles du runtime ASP.NET Web Stack ou d’une batterie de serveurs dans laquelle l’élément &lt;machineKey&gt; dans Web. config diffère entre les ordinateurs. Vous pouvez utiliser un outil tel que Fiddler pour forcer cette exception en falsifiant un jeton anti-XSRF.
- Le jeton de session et le jeton de champ ont été permutés.
- Le jeton de session et le jeton de champ contiennent des jetons de sécurité incompatibles.
- Le nom d’utilisateur incorporé dans le jeton de champ ne correspond pas au nom d’utilisateur de l’utilisateur connecté actuel.
- La méthode *[IAntiForgeryAdditionalDataProvider. ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* a retourné la *valeur false*.

Les fonctionnalités anti-XSRF peuvent également effectuer des vérifications supplémentaires lors de la génération ou de la validation des jetons, et les défaillances pendant ces vérifications peuvent entraîner la levée d’exceptions. Pour plus d’informations, consultez les sections relative à l’authentification et à la **[configuration et](#_Configuration_and_extensibility)** [l’authentification basée sur WIF/ACS/claims](#_WIF_ACS) .

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Scénarios avec prise en charge spéciale

### <a name="anonymous-authentication"></a>Authentification anonyme

Le système anti-XSRF contient une prise en charge spéciale des utilisateurs anonymes, où « Anonymous » est défini comme utilisateur où la propriété *IIdentity. IsAuthenticated* retourne la *valeur false*. Les scénarios incluent la fourniture d’une protection XSRF à la page de connexion (avant l’authentification de l’utilisateur) et les schémas d’authentification personnalisés, où l’application utilise un mécanisme autre que *IIdentity* pour identifier les utilisateurs.

Pour prendre en charge ces scénarios, rappelez-vous que les jetons de session et de champ sont joints par un jeton de sécurité, qui est un identificateur opaque généré de manière aléatoire 128 bits. Ce jeton de sécurité est utilisé pour suivre la session d’un utilisateur individuel lorsqu’il navigue sur le site, de sorte qu’il sert efficacement à un identificateur anonyme. Une chaîne vide est utilisée à la place du nom d’utilisateur pour les routines de génération et de validation décrites ci-dessus.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF/ACS/authentification basée sur les revendications

Normalement, les classes *IIdentity* intégrées à l' .NET Framework ont la propriété qui *IIdentity.Name* est suffisante pour identifier de manière unique un utilisateur particulier dans une application particulière. Par exemple, *FormsIdentity.Name* retourne le nom d’utilisateur stocké dans la base de données d’appartenance (qui est unique pour toutes les applications en fonction de cette base de données), *WindowsIdentity.Name* retourne l’identité qualifiée du domaine de l’utilisateur, et ainsi de suite. Ces systèmes fournissent non seulement une authentification ; ils *identifient* également les utilisateurs pour une application.

L’authentification basée sur les revendications, en revanche, ne nécessite pas nécessairement l’identification d’un utilisateur particulier. Au lieu de cela, les types *ClaimsPrincipal* et *ClaimsIdentity* sont associés à un ensemble d’instances de *revendication* , où les revendications individuelles peuvent être « is 18 + years of Age » ou « is administrateur » à autre chose. Étant donné que l’utilisateur n’a pas nécessairement été identifié, le runtime ne peut pas utiliser la propriété *ClaimsIdentity.Name* comme identificateur unique pour cet utilisateur particulier. L’équipe a vu des exemples concrets où *ClaimsIdentity.Name* retourne *null*, retourne un nom convivial ou retourne une chaîne qui n’est pas appropriée pour une utilisation en tant qu’identificateur unique pour l’utilisateur.

La plupart des déploiements qui utilisent l’authentification basée sur les revendications utilisent [Azure Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) en particulier. ACS permet au développeur de configurer des *fournisseurs d’identité* individuels (tels que ADFS, le fournisseur de comptes Microsoft, les fournisseurs OpenID comme Yahoo !, etc.) et les fournisseurs d’identité retournent des *identificateurs de nom*. Ces identificateurs de noms peuvent contenir des informations d’identification personnelle (PII) comme une adresse de messagerie, ou peuvent être rendues anonymes comme un identificateur personnel privé (PPID). Quel que soit le Tuple (fournisseur d’identité, identificateur de nom) suffisamment utilisé comme jeton de suivi approprié pour un utilisateur particulier pendant qu’il parcourt le site, le runtime ASP.NET Web Stack peut utiliser le tuple à la place du nom d’utilisateur lors de la génération et validation des jetons de champ anti-XSRF. Les URI particuliers pour le fournisseur d’identité et l’identificateur de nom sont les suivants :

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(pour plus d’informations, consultez cette [page de document](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) des services ACS.)

Lors de la génération ou de la validation d’un jeton, le runtime ASP.NET Web Stack effectue une liaison d’essai avec les types :

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (pour le kit de développement logiciel (SDK) WIF.)
- `System.Security.Claims.ClaimsIdentity` (pour .NET 4,5).

Si ces types existent, et si le *IIIIdentity* de l’utilisateur actuel implémente ou sous-classe l’un de ces types, la fonction anti-XSRF utilise le Tuple (fournisseur d’identité, identificateur de nom) à la place du nom d’utilisateur lors de la génération et de la validation des jetons. Si aucun tuple de ce type n’est présent, la requête échoue avec une erreur décrivant au développeur comment configurer le système anti-XSRF pour comprendre le mécanisme d’authentification basé sur les revendications en cours d’utilisation. Pour plus d’informations, consultez la section **[configuration et extensibilité](#_Configuration_and_extensibility)** .

### <a name="oauth--openid-authentication"></a>Authentification OAuth/OpenID

Enfin, la fonction anti-XSRF offre une prise en charge spéciale des applications qui utilisent l’authentification OAuth ou OpenID. Cette prise en charge est basée sur une méthode heuristique : si le *IIdentity.Name* actuel commence par http://ou https://, les comparaisons de noms d’utilisateur seront effectuées à l’aide d’un comparateur ordinal plutôt que du comparateur OrdinalIgnoreCase par défaut.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Configuration et extensibilité

Parfois, les développeurs peuvent souhaiter un contrôle plus étroit sur les comportements de génération et de validation anti-XSRF. Par exemple, il est possible que le comportement par défaut des applications MVC et Web pages d’ajout automatique de cookies HTTP à la réponse ne soit pas souhaitable et que le développeur souhaite conserver les jetons ailleurs. Il existe deux API pour vous aider :

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

La méthode *GetTokens* prend comme entrée un jeton de session de vérification de demande XSRF existant (qui peut être null) et produit comme sortie un nouveau jeton de session de vérification de demande XSRF et un jeton de champ. Les jetons sont simplement des chaînes opaques sans décoration. la valeur *formToken* ne sera pas incluse dans un wrapper dans une balise d’entrée de&gt; &lt;. La valeur *newCookieToken* peut être null ; Si cela se produit, la valeur *oldCookieToken* est toujours valide et aucun nouveau cookie de réponse n’a besoin d’être défini. L’appelant de *GetTokens* est responsable de la persistance des cookies de réponse nécessaires ou de la génération de tout balisage nécessaire. la méthode *GetTokens* elle-même ne modifie pas la réponse en tant qu’effet secondaire. La méthode *Validate* prend les jetons de session et de champ entrants et exécute la logique de validation mentionnée ci-dessus.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

Le développeur peut configurer le système anti-XSRF à partir du démarrage de l’application\_. La configuration est programmée. Les propriétés du type *AntiForgeryConfig* statique sont décrites ci-dessous. La plupart des utilisateurs qui utilisent des revendications doivent définir la propriété UniqueClaimTypeIdentifier.

| **Property** | **Description** |
| --- | --- |
| **AdditionalDataProvider** | [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) qui fournit des données supplémentaires lors de la génération du jeton et qui consomme des données supplémentaires lors de la validation du jeton. La valeur par défaut est *null*. Pour plus d’informations, consultez la section [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) . |
| **CookieName** | Chaîne qui fournit le nom du cookie HTTP utilisé pour stocker le jeton de session anti-XSRF. Si cette valeur n’est pas définie, un nom est généré automatiquement en fonction du chemin d’accès virtuel déployé de l’application. La valeur par défaut est *null*. |
| **RequireSsl** | Valeur booléenne qui détermine si les jetons anti-XSRF doivent être soumis sur un canal sécurisé SSL. Si cette valeur est *true*, tous les cookies générés automatiquement ont l’indicateur « Secure » défini, et les API anti-XSRF lèvent si elles sont appelées à partir d’une demande qui n’est pas envoyée via SSL. La valeur par défaut est *false*. |
| **SuppressIdentityHeuristicChecks** | Valeur booléenne qui détermine si le système anti-XSRF doit désactiver sa prise en charge des identités basées sur les revendications. Si cette valeur est *true*, le système suppose que *IIdentity.Name* est approprié pour une utilisation en tant qu’identificateur unique pour chaque utilisateur et ne tente pas d’effectuer une opération spéciale *IClaimsIdentity* ou *CLCLAIMSIDENTITY* comme décrit dans le [WIF/ACS/ section authentification basée sur les revendications](#_WIF_ACS) . La valeur par défaut est `false`. |
| **UniqueClaimTypeIdentifier** | Chaîne qui indique le type de revendication approprié pour une utilisation en tant qu’identificateur unique pour chaque utilisateur. Si cette valeur est définie et que le *IIdentity* actuel est basé sur les revendications, le système tente d’extraire une revendication du type spécifié par *UniqueClaimTypeIdentifier*, et la valeur correspondante est utilisée à la place du nom d’utilisateur de l’utilisateur lorsque génération du jeton de champ. Si le type de revendication est introuvable, le système ne parvient pas à effectuer la requête. La valeur par défaut est *null*, ce qui indique que le système doit utiliser le Tuple (fournisseur d’identité, identificateur de nom) comme décrit précédemment à la place du nom d’utilisateur de l’utilisateur. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

Le type *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* permet aux développeurs d’étendre le comportement du système anti-XSRF en arrondissant les données supplémentaires dans chaque jeton. La méthode *GetAdditionalData* est appelée chaque fois qu’un jeton de champ est généré, et la valeur de retour est incorporée dans le jeton généré. Un responsable de l’implémentation peut retourner un horodateur, une valeur à usage unique ou toute autre valeur qu’il souhaite de cette méthode.

De même, la méthode *ValidateAdditionalData* est appelée chaque fois qu’un jeton de champ est validé et la chaîne « données supplémentaires » incorporée dans le jeton est transmise à la méthode. La routine de validation peut implémenter un délai d’expiration (en vérifiant l’heure actuelle par rapport à l’heure de la création du jeton), une routine de vérification de la valeur à usage unique ou toute autre logique souhaitée.

## <a name="design-decisions-and-security-considerations"></a>Décisions de conception et considérations relatives à la sécurité

Le jeton de sécurité qui lie les jetons de session et de champ est techniquement nécessaire uniquement lorsque vous tentez de protéger des utilisateurs anonymes/non authentifiés contre les attaques XSRF. Lorsque l’utilisateur est authentifié, le jeton d’authentification lui-même (soumis de manière présumée sous la forme d’un cookie) peut être utilisé comme moitié d’une paire de jetons de synchronisateur. Toutefois, il existe des scénarios valides pour la protection des pages de connexion atteintes par des utilisateurs non authentifiés, et la logique anti-XSRF a été simplifiée par la génération et la validation systématiques du jeton de sécurité, même pour les utilisateurs authentifiés. Elle offre également une protection supplémentaire au cas où un jeton de champ serait compromis par un pirate, car la définition ou la découverte du jeton de session serait un autre obstacle à la surmonter par l’attaquant.

Les développeurs doivent faire attention lorsque plusieurs applications sont hébergées dans un domaine unique. Par exemple, même si *example1.cloudapp.net* et *example2.cloudapp.net* sont des hôtes différents, il existe une relation d’approbation implicite entre tous les ordinateurs hôtes dans le domaine *\*. cloudapp.net* . Cette relation d’approbation implicite [permet aux hôtes potentiellement non fiables d’affecter les cookies de l’autre](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (les stratégies de même origine qui régissent les demandes Ajax ne s’appliquent pas nécessairement aux cookies http). Le runtime ASP.NET Web Stack fournit une certaine atténuation dans le fait que le nom d’utilisateur est incorporé dans le jeton de champ. par conséquent, même si un sous-domaine malveillant est en mesure de remplacer un jeton de session, il ne pourra pas générer un jeton de champ valide pour l’utilisateur. Toutefois, en cas d’hébergement dans ce type d’environnement, les routines anti-XSRF intégrées ne peuvent toujours pas se défendre contre le détournement de session ou les XSRF de connexion.

Actuellement, les routines anti-XSRF ne se protègent pas contre [le détournement](https://www.owasp.org/index.php/Clickjacking). Les applications qui souhaitent se défendre contre le détournement peuvent facilement le faire en envoyant un en-tête X-Frame-options : SAMEORIGIN avec chaque réponse. Cet en-tête est pris en charge par tous les navigateurs récents. Pour plus d’informations, consultez le [blog IE](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), le [blog SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx)et [OWASP](https://www.owasp.org/index.php/Clickjacking). Le runtime ASP.NET Web Stack peut, dans une version ultérieure, faire en sorte que les programmes d’assistance anti-XSRF MVC et Web pages définissent automatiquement cet en-tête afin que les applications soient automatiquement protégées contre cette attaque.

Les développeurs Web doivent continuer à s’assurer que leur site ne soit pas vulnérable aux attaques XSS. Les attaques XSS sont très puissantes et une exploitation réussie interrompt également les défenses du runtime Web Stack ASP.NET contre les attaques XSRF.

## <a name="acknowledgment"></a>Accusé

[@LeviBroderick](https://twitter.com/LeviBroderick), qui a écrit une grande partie du code de sécurité ASP.net, il s’agit de l’essentiel de ces informations.
