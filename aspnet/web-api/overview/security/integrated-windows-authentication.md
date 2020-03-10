---
uid: web-api/overview/security/integrated-windows-authentication
title: Authentification Windows intégrée | Microsoft Docs
author: MikeWasson
description: Décrit l’utilisation de l’authentification Windows intégrée dans API Web ASP.NET.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: e4f31f191f3c0fabff308ea5dadb0f1d9ce7d448
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621743"
---
# <a name="integrated-windows-authentication"></a><span data-ttu-id="eeb8b-103">Authentification Windows intégrée</span><span class="sxs-lookup"><span data-stu-id="eeb8b-103">Integrated Windows Authentication</span></span>

<span data-ttu-id="eeb8b-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="eeb8b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="eeb8b-105">L’authentification Windows intégrée permet aux utilisateurs de se connecter avec leurs informations d’identification Windows, à l’aide de Kerberos ou NTLM.</span><span class="sxs-lookup"><span data-stu-id="eeb8b-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="eeb8b-106">Le client envoie les informations d’identification dans l’en-tête Authorization.</span><span class="sxs-lookup"><span data-stu-id="eeb8b-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="eeb8b-107">L’authentification Windows est idéale pour un environnement intranet.</span><span class="sxs-lookup"><span data-stu-id="eeb8b-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="eeb8b-108">Pour plus d’informations, consultez [Authentification Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="eeb8b-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="eeb8b-109">Avantages</span><span class="sxs-lookup"><span data-stu-id="eeb8b-109">Advantages</span></span> | <span data-ttu-id="eeb8b-110">Inconvénients</span><span class="sxs-lookup"><span data-stu-id="eeb8b-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="eeb8b-111">-Intégré à IIS.</span><span class="sxs-lookup"><span data-stu-id="eeb8b-111">- Built into IIS.</span></span> <span data-ttu-id="eeb8b-112">-N’envoie pas les informations d’identification de l’utilisateur dans la demande.</span><span class="sxs-lookup"><span data-stu-id="eeb8b-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="eeb8b-113">-Si l’ordinateur client appartient au domaine (par exemple, une application intranet), l’utilisateur n’a pas besoin d’entrer des informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="eeb8b-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="eeb8b-114">-Non recommandé pour les applications Internet.</span><span class="sxs-lookup"><span data-stu-id="eeb8b-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="eeb8b-115">-Requiert la prise en charge de Kerberos ou NTLM dans le client.</span><span class="sxs-lookup"><span data-stu-id="eeb8b-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="eeb8b-116">-Le client doit se trouver dans le domaine Active Directory.</span><span class="sxs-lookup"><span data-stu-id="eeb8b-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="eeb8b-117">Si votre application est hébergée sur Azure et que vous disposez d’un domaine Active Directory sur site, envisagez de fédérer votre annuaire Active Directory sur site avec Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="eeb8b-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="eeb8b-118">Ainsi, les utilisateurs peuvent se connecter avec leurs informations d’identification locales, mais l’authentification est effectuée par Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eeb8b-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="eeb8b-119">Pour plus d’informations, consultez [authentification Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="eeb8b-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>

<span data-ttu-id="eeb8b-120">Pour créer une application qui utilise l’authentification Windows intégrée, sélectionnez le modèle « application intranet » dans l’Assistant projet MVC 4.</span><span class="sxs-lookup"><span data-stu-id="eeb8b-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="eeb8b-121">Ce modèle de projet place le paramètre suivant dans le fichier Web. config :</span><span class="sxs-lookup"><span data-stu-id="eeb8b-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="eeb8b-122">Côté client, l’authentification Windows intégrée fonctionne avec n’importe quel navigateur prenant en charge le schéma d’authentification [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , qui comprend la plupart des principaux navigateurs.</span><span class="sxs-lookup"><span data-stu-id="eeb8b-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="eeb8b-123">Pour les applications clientes .NET, la classe **httpclient** prend en charge l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="eeb8b-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="eeb8b-124">L’authentification Windows est vulnérable aux attaques de falsification de requête intersites (CSRF).</span><span class="sxs-lookup"><span data-stu-id="eeb8b-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="eeb8b-125">Consultez la page [prévention des attaques de falsification de requête intersites (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="eeb8b-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
