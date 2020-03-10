---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: Aide-mémoire sur l’API pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Cette page contient une liste contenant des exemples succincts des objets, propriétés et méthodes les plus couramment utilisés pour programmer des pages Web ASP.NET avec syntaxe Razor.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: e010307fc0576e8b003fbfe665cae77618d9c9a5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574332"
---
# <a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="052a5-103">Aide-mémoire de l’API pages Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="052a5-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>

<span data-ttu-id="052a5-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="052a5-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="052a5-105">Cette page contient une liste contenant des exemples succincts des objets, propriétés et méthodes les plus couramment utilisés pour programmer des pages Web ASP.NET avec syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="052a5-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="052a5-106">Les descriptions marquées avec « (v2) » ont été introduites dans pages Web ASP.NET version 2.</span><span class="sxs-lookup"><span data-stu-id="052a5-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="052a5-107">Pour obtenir une documentation de référence sur les API, consultez la [documentation de référence pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=208659) sur MSDN.</span><span class="sxs-lookup"><span data-stu-id="052a5-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="052a5-108">Versions de logiciels</span><span class="sxs-lookup"><span data-stu-id="052a5-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="052a5-109">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="052a5-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="052a5-110">Ce didacticiel fonctionne également avec pages Web ASP.NET 2 et pages Web ASP.NET 1,0 (à l’exception des fonctionnalités marquées v2).</span><span class="sxs-lookup"><span data-stu-id="052a5-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>

<span data-ttu-id="052a5-111">Cette page contient des informations de référence sur les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="052a5-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="052a5-112">Classes</span><span class="sxs-lookup"><span data-stu-id="052a5-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="052a5-113">Données</span><span class="sxs-lookup"><span data-stu-id="052a5-113">Data</span></span>](#Data)
- [<span data-ttu-id="052a5-114">Helpers</span><span class="sxs-lookup"><span data-stu-id="052a5-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="052a5-115">Validation</span><span class="sxs-lookup"><span data-stu-id="052a5-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="052a5-116">Classes</span><span class="sxs-lookup"><span data-stu-id="052a5-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="052a5-117">Contient des données qui peuvent être partagées par n’importe quelle page de l’application.</span><span class="sxs-lookup"><span data-stu-id="052a5-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="052a5-118">Vous pouvez utiliser la propriété Dynamic `App` pour accéder aux mêmes données, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="052a5-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="052a5-119">Convertit une valeur de chaîne en valeur booléenne (true/false).</span><span class="sxs-lookup"><span data-stu-id="052a5-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="052a5-120">Retourne false ou la valeur spécifiée si la chaîne ne représente pas true/false.</span><span class="sxs-lookup"><span data-stu-id="052a5-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="052a5-121">Convertit une valeur de chaîne en date/heure.</span><span class="sxs-lookup"><span data-stu-id="052a5-121">Converts a string value to date/time.</span></span> <span data-ttu-id="052a5-122">Retourne `DateTime.MinValue` ou la valeur spécifiée si la chaîne ne représente pas une date/heure.</span><span class="sxs-lookup"><span data-stu-id="052a5-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="052a5-123">Convertit une valeur de chaîne en valeur décimale.</span><span class="sxs-lookup"><span data-stu-id="052a5-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="052a5-124">Retourne 0,0 ou la valeur spécifiée si la chaîne ne représente pas une valeur décimale.</span><span class="sxs-lookup"><span data-stu-id="052a5-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="052a5-125">Convertit une valeur de chaîne en valeur de type float.</span><span class="sxs-lookup"><span data-stu-id="052a5-125">Converts a string value to a float.</span></span> <span data-ttu-id="052a5-126">Retourne 0,0 ou la valeur spécifiée si la chaîne ne représente pas une valeur décimale.</span><span class="sxs-lookup"><span data-stu-id="052a5-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="052a5-127">Convertit une valeur de chaîne en entier.</span><span class="sxs-lookup"><span data-stu-id="052a5-127">Converts a string value to an integer.</span></span> <span data-ttu-id="052a5-128">Retourne 0 ou la valeur spécifiée si la chaîne ne représente pas un entier.</span><span class="sxs-lookup"><span data-stu-id="052a5-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="052a5-129">Crée une URL compatible avec le navigateur à partir d’un chemin d’accès de fichier local, avec des parties de chemin d’accès supplémentaires facultatives.</span><span class="sxs-lookup"><span data-stu-id="052a5-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="052a5-130">Restitue la *valeur* en tant que balisage HTML au lieu de la restituer en tant que sortie codée au format html.</span><span class="sxs-lookup"><span data-stu-id="052a5-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="052a5-131">Retourne la valeur true si la valeur peut être convertie à partir d’une chaîne vers le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="052a5-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="052a5-132">Retourne la valeur true si l’objet ou la variable n’a pas de valeur.</span><span class="sxs-lookup"><span data-stu-id="052a5-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="052a5-133">Retourne la valeur true si la demande est une publication.</span><span class="sxs-lookup"><span data-stu-id="052a5-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="052a5-134">(Les demandes initiales sont généralement une récupération.)</span><span class="sxs-lookup"><span data-stu-id="052a5-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="052a5-135">Spécifie le chemin d’accès d’une page de disposition à appliquer à cette page.</span><span class="sxs-lookup"><span data-stu-id="052a5-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="052a5-136">Contient des données partagées entre la page, les pages de disposition et les pages partielles dans la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="052a5-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="052a5-137">Vous pouvez utiliser la propriété Dynamic `Page` pour accéder aux mêmes données, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="052a5-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="052a5-138">(Pages de disposition) Restitue le contenu d’une page de contenu qui n’est pas dans les sections nommées.</span><span class="sxs-lookup"><span data-stu-id="052a5-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="052a5-139">Génère le rendu d’une page de contenu à l’aide du chemin d’accès spécifié et des données supplémentaires facultatives.</span><span class="sxs-lookup"><span data-stu-id="052a5-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="052a5-140">Vous pouvez récupérer les valeurs des paramètres supplémentaires à partir de `PageData` par position (exemple 1) ou clé (exemple 2).</span><span class="sxs-lookup"><span data-stu-id="052a5-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="052a5-141">(Pages de disposition) Génère le rendu d’une section de contenu qui a un nom.</span><span class="sxs-lookup"><span data-stu-id="052a5-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="052a5-142">Affectez à *Required* la valeur false pour rendre une section facultative.</span><span class="sxs-lookup"><span data-stu-id="052a5-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="052a5-143">Obtient ou définit la valeur d’un cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="052a5-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="052a5-144">Obtient les fichiers qui ont été téléchargés dans la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="052a5-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="052a5-145">Obtient les données publiées dans un formulaire (sous forme de chaînes).</span><span class="sxs-lookup"><span data-stu-id="052a5-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="052a5-146">`Request[key]` vérifie à la fois les collections `Request.Form` et `Request.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="052a5-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="052a5-147">Obtient les données qui ont été spécifiées dans la chaîne de requête de l’URL.</span><span class="sxs-lookup"><span data-stu-id="052a5-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="052a5-148">`Request[key]` vérifie à la fois les collections `Request.Form` et `Request.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="052a5-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="052a5-149">Désactive de manière sélective la validation des demandes pour un élément de formulaire, une valeur de chaîne de requête, un cookie ou une valeur d’en-tête.</span><span class="sxs-lookup"><span data-stu-id="052a5-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="052a5-150">La validation de la demande est activée par défaut et empêche les utilisateurs de publier le balisage ou tout autre contenu potentiellement dangereux.</span><span class="sxs-lookup"><span data-stu-id="052a5-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="052a5-151">Ajoute un en-tête de serveur HTTP à la réponse.</span><span class="sxs-lookup"><span data-stu-id="052a5-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="052a5-152">Met en cache la sortie de page pendant une durée spécifiée.</span><span class="sxs-lookup"><span data-stu-id="052a5-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="052a5-153">Vous pouvez éventuellement définir le *glissement* pour réinitialiser le délai d’attente sur chaque accès à la page et *VaryByParams* pour mettre en cache différentes versions de la page pour chaque chaîne de requête différente dans la demande de page.</span><span class="sxs-lookup"><span data-stu-id="052a5-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="052a5-154">Redirige la demande du navigateur vers un nouvel emplacement.</span><span class="sxs-lookup"><span data-stu-id="052a5-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="052a5-155">Définit le code d’état HTTP envoyé au navigateur.</span><span class="sxs-lookup"><span data-stu-id="052a5-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="052a5-156">Écrit le contenu des *données* dans la réponse avec un type MIME facultatif.</span><span class="sxs-lookup"><span data-stu-id="052a5-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="052a5-157">Écrit le contenu d’un fichier dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="052a5-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="052a5-158">(Pages de disposition) Définit une section de contenu qui a un nom.</span><span class="sxs-lookup"><span data-stu-id="052a5-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="052a5-159">Décode une chaîne encodée au format HTML.</span><span class="sxs-lookup"><span data-stu-id="052a5-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="052a5-160">Encode une chaîne pour le rendu dans le balisage HTML.</span><span class="sxs-lookup"><span data-stu-id="052a5-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="052a5-161">Retourne le chemin d’accès physique du serveur pour le chemin d’accès virtuel spécifié.</span><span class="sxs-lookup"><span data-stu-id="052a5-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="052a5-162">Décode le texte d’une URL.</span><span class="sxs-lookup"><span data-stu-id="052a5-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="052a5-163">Encode le texte à placer dans une URL.</span><span class="sxs-lookup"><span data-stu-id="052a5-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="052a5-164">Obtient ou définit une valeur qui existe jusqu’à ce que l’utilisateur ferme le navigateur.</span><span class="sxs-lookup"><span data-stu-id="052a5-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="052a5-165">Affiche une représentation sous forme de chaîne de la valeur de l’objet.</span><span class="sxs-lookup"><span data-stu-id="052a5-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="052a5-166">Obtient des données supplémentaires à partir de l’URL (par exemple, */MyPage/ExtraData*).</span><span class="sxs-lookup"><span data-stu-id="052a5-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="052a5-167">Modifie le mot de passe de l'utilisateur spécifié.</span><span class="sxs-lookup"><span data-stu-id="052a5-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="052a5-168">Confirme un compte à l’aide du jeton de confirmation de compte.</span><span class="sxs-lookup"><span data-stu-id="052a5-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="052a5-169">Crée un nouveau compte d’utilisateur avec le nom d’utilisateur et le mot de passe spécifiés.</span><span class="sxs-lookup"><span data-stu-id="052a5-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="052a5-170">Pour exiger un jeton de confirmation, transmettez true pour *requireConfirmationToken.*</span><span class="sxs-lookup"><span data-stu-id="052a5-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="052a5-171">Obtient l’identificateur entier pour l’utilisateur actuellement connecté.</span><span class="sxs-lookup"><span data-stu-id="052a5-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="052a5-172">Obtient le nom de l’utilisateur actuellement connecté.</span><span class="sxs-lookup"><span data-stu-id="052a5-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="052a5-173">Génère un jeton de réinitialisation de mot de passe qui peut être envoyé par courrier électronique à un utilisateur pour permettre à l’utilisateur de réinitialiser le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="052a5-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="052a5-174">Retourne l’ID d’utilisateur à partir du nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="052a5-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="052a5-175">Retourne la valeur true si l’utilisateur actuel est connecté.</span><span class="sxs-lookup"><span data-stu-id="052a5-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="052a5-176">Retourne la valeur true si l’utilisateur a été confirmé (par exemple, via un e-mail de confirmation).</span><span class="sxs-lookup"><span data-stu-id="052a5-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="052a5-177">Retourne la valeur true si le nom de l’utilisateur actuel correspond au nom d’utilisateur spécifié.</span><span class="sxs-lookup"><span data-stu-id="052a5-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="052a5-178">Journalise l’utilisateur en définissant un jeton d’authentification dans le cookie.</span><span class="sxs-lookup"><span data-stu-id="052a5-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="052a5-179">Déconnecte l’utilisateur en supprimant le cookie de jeton d’authentification.</span><span class="sxs-lookup"><span data-stu-id="052a5-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="052a5-180">Si l'utilisateur n'est pas authentifié, définit l'état HTTP sur 401 (non autorisé).</span><span class="sxs-lookup"><span data-stu-id="052a5-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="052a5-181">Si l’utilisateur actuel n’est pas membre de l’un des rôles spécifiés, définit l’état HTTP sur 401 (non autorisé).</span><span class="sxs-lookup"><span data-stu-id="052a5-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="052a5-182">Si l’utilisateur actuel n’est pas l’utilisateur spécifié par le *nom d'* utilisateur, définit l’état HTTP sur 401 (non autorisé).</span><span class="sxs-lookup"><span data-stu-id="052a5-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="052a5-183">Si le jeton de réinitialisation du mot de passe est valide, remplace le mot de passe de l’utilisateur par le nouveau mot de passe.</span><span class="sxs-lookup"><span data-stu-id="052a5-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="052a5-184">Données</span><span class="sxs-lookup"><span data-stu-id="052a5-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="052a5-185">Exécute *SQLStatement* (avec des paramètres facultatifs), tels que Insert, Delete ou Update, et retourne le nombre d’enregistrements affectés.</span><span class="sxs-lookup"><span data-stu-id="052a5-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="052a5-186">Retourne la colonne d’identité de la ligne la plus récemment insérée.</span><span class="sxs-lookup"><span data-stu-id="052a5-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="052a5-187">Ouvre le fichier de base de données spécifié ou la base de données spécifiée à l’aide d’une chaîne de connexion nommée à partir du fichier *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="052a5-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="052a5-188">Ouvre une base de données à l’aide de la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="052a5-188">Opens a database using the connection string.</span></span> <span data-ttu-id="052a5-189">(Cela contraste avec `Database.Open`, qui utilise un nom de chaîne de connexion.)</span><span class="sxs-lookup"><span data-stu-id="052a5-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="052a5-190">Interroge la base de données à l’aide de *SQLStatement* (en passant éventuellement des paramètres) et retourne les résultats sous la forme d’une collection.</span><span class="sxs-lookup"><span data-stu-id="052a5-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="052a5-191">Exécute *SQLStatement* (avec des paramètres facultatifs) et retourne un seul enregistrement.</span><span class="sxs-lookup"><span data-stu-id="052a5-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="052a5-192">Exécute *SQLStatement* (avec des paramètres facultatifs) et retourne une valeur unique.</span><span class="sxs-lookup"><span data-stu-id="052a5-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="052a5-193">Programmes d’assistance</span><span class="sxs-lookup"><span data-stu-id="052a5-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="052a5-194">Génère le rendu du code JavaScript Google Analytics pour l’ID spécifié.</span><span class="sxs-lookup"><span data-stu-id="052a5-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="052a5-195">Génère le rendu du code JavaScript StatCounter Analytics pour le projet spécifié.</span><span class="sxs-lookup"><span data-stu-id="052a5-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="052a5-196">Génère le rendu du code JavaScript Yahoo Analytics pour le compte spécifié.</span><span class="sxs-lookup"><span data-stu-id="052a5-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="052a5-197">Passe une recherche à Bing.</span><span class="sxs-lookup"><span data-stu-id="052a5-197">Passes a search to Bing.</span></span> <span data-ttu-id="052a5-198">Pour spécifier le site à rechercher et un titre pour la zone de recherche, vous pouvez définir les propriétés `Bing.SiteUrl` et `Bing.SiteTitle`.</span><span class="sxs-lookup"><span data-stu-id="052a5-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="052a5-199">Normalement, vous définissez ces propriétés dans la page *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="052a5-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="052a5-200">Initialise un graphique.</span><span class="sxs-lookup"><span data-stu-id="052a5-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="052a5-201">Ajoute une légende à un graphique.</span><span class="sxs-lookup"><span data-stu-id="052a5-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="052a5-202">Ajoute une série de valeurs au graphique.</span><span class="sxs-lookup"><span data-stu-id="052a5-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="052a5-203">Retourne un hachage pour les données spécifiées.</span><span class="sxs-lookup"><span data-stu-id="052a5-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="052a5-204">L’algorithme par défaut est `sha256`.</span><span class="sxs-lookup"><span data-stu-id="052a5-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="052a5-205">Permet aux utilisateurs de Facebook d’établir une connexion avec les pages.</span><span class="sxs-lookup"><span data-stu-id="052a5-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="052a5-206">Génère le rendu de l’interface utilisateur pour le chargement des fichiers.</span><span class="sxs-lookup"><span data-stu-id="052a5-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="052a5-207">Génère le rendu de la balise Xbox Gamer spécifiée.</span><span class="sxs-lookup"><span data-stu-id="052a5-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="052a5-208">Génère le rendu de l’image Gravatar pour l’adresse de messagerie spécifiée.</span><span class="sxs-lookup"><span data-stu-id="052a5-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="052a5-209">Convertit un objet de données en une chaîne au format JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="052a5-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="052a5-210">Convertit une chaîne d’entrée encodée JSON en objet de données que vous pouvez effectuer une itération ou une insertion dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="052a5-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="052a5-211">Génère le rendu des liens de réseau social en utilisant le titre et l’URL facultatives spécifiés.</span><span class="sxs-lookup"><span data-stu-id="052a5-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="052a5-212">Associe un message d’erreur à un champ de formulaire.</span><span class="sxs-lookup"><span data-stu-id="052a5-212">Associates an error message with a form field.</span></span> <span data-ttu-id="052a5-213">Utilisez le programme d’assistance `ModelState` pour accéder à ce membre.</span><span class="sxs-lookup"><span data-stu-id="052a5-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="052a5-214">Associe un message d’erreur à un formulaire.</span><span class="sxs-lookup"><span data-stu-id="052a5-214">Associates an error message with a form.</span></span> <span data-ttu-id="052a5-215">Utilisez le programme d’assistance `ModelState` pour accéder à ce membre.</span><span class="sxs-lookup"><span data-stu-id="052a5-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="052a5-216">Retourne la valeur true s’il n’y a aucune erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="052a5-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="052a5-217">Utilisez le programme d’assistance `ModelState` pour accéder à ce membre.</span><span class="sxs-lookup"><span data-stu-id="052a5-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="052a5-218">Génère le rendu des propriétés et des valeurs d’un objet et de tous les objets enfants.</span><span class="sxs-lookup"><span data-stu-id="052a5-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="052a5-219">Génère le rendu du test de vérification reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="052a5-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="052a5-220">Définit des clés publiques et privées pour le service reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="052a5-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="052a5-221">Normalement, vous définissez ces propriétés dans la page *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="052a5-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="052a5-222">Retourne le résultat du test reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="052a5-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="052a5-223">Génère le rendu des informations d’État relatives à pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="052a5-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="052a5-224">Génère le rendu d’un flux Twitter pour l’utilisateur spécifié.</span><span class="sxs-lookup"><span data-stu-id="052a5-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="052a5-225">Génère le rendu d’un flux Twitter pour le texte de recherche spécifié.</span><span class="sxs-lookup"><span data-stu-id="052a5-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="052a5-226">Restitue un lecteur vidéo Flash pour le fichier spécifié avec une largeur et une hauteur facultatives.</span><span class="sxs-lookup"><span data-stu-id="052a5-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="052a5-227">Génère le rendu d’un lecteur Windows Media pour le fichier spécifié avec une largeur et une hauteur facultatives.</span><span class="sxs-lookup"><span data-stu-id="052a5-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="052a5-228">Restitue un lecteur Silverlight pour le fichier *. xap* spécifié avec la largeur et la hauteur requises.</span><span class="sxs-lookup"><span data-stu-id="052a5-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="052a5-229">Retourne l’objet spécifié par la *clé*, ou null si l’objet est introuvable.</span><span class="sxs-lookup"><span data-stu-id="052a5-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="052a5-230">Supprime l’objet spécifié par la *clé* du cache.</span><span class="sxs-lookup"><span data-stu-id="052a5-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="052a5-231">Place la *valeur* dans le cache sous le nom spécifié par la *clé*.</span><span class="sxs-lookup"><span data-stu-id="052a5-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="052a5-232">Crée un objet `WebGrid` à l’aide des données d’une requête.</span><span class="sxs-lookup"><span data-stu-id="052a5-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="052a5-233">Restitue le balisage pour afficher des données dans une table HTML.</span><span class="sxs-lookup"><span data-stu-id="052a5-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="052a5-234">Génère le rendu d’un pagineur pour l’objet `WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="052a5-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="052a5-235">Charge une image à partir du chemin d’accès spécifié.</span><span class="sxs-lookup"><span data-stu-id="052a5-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="052a5-236">Ajoute l’image spécifiée sous la forme d’un filigrane.</span><span class="sxs-lookup"><span data-stu-id="052a5-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="052a5-237">Ajoute le texte spécifié à l’image.</span><span class="sxs-lookup"><span data-stu-id="052a5-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="052a5-238">Retourne l’image horizontalement ou verticalement.</span><span class="sxs-lookup"><span data-stu-id="052a5-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="052a5-239">Charge une image quand une image est publiée dans une page pendant le chargement d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="052a5-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="052a5-240">Redimensionne une image.</span><span class="sxs-lookup"><span data-stu-id="052a5-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="052a5-241">Fait pivoter l’image vers la gauche ou vers la droite.</span><span class="sxs-lookup"><span data-stu-id="052a5-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="052a5-242">Enregistre l’image dans le chemin d’accès spécifié.</span><span class="sxs-lookup"><span data-stu-id="052a5-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="052a5-243">Définit le mot de passe du serveur SMTP.</span><span class="sxs-lookup"><span data-stu-id="052a5-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="052a5-244">Normalement, vous définissez cette propriété dans la page *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="052a5-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="052a5-245">Envoie un e-mail.</span><span class="sxs-lookup"><span data-stu-id="052a5-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="052a5-246">Définit le nom du serveur SMTP.</span><span class="sxs-lookup"><span data-stu-id="052a5-246">Sets the SMTP server name.</span></span> <span data-ttu-id="052a5-247">Normalement, vous définissez cette propriété dans la page *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="052a5-247">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="052a5-248">Définit le nom d’utilisateur du serveur SMTP.</span><span class="sxs-lookup"><span data-stu-id="052a5-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="052a5-249">Normalement, vous devez définir cette propriété dans la page *\_AppStart* .</span><span class="sxs-lookup"><span data-stu-id="052a5-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="052a5-250">Validation</span><span class="sxs-lookup"><span data-stu-id="052a5-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="052a5-251">MIME Génère le rendu d’un message d’erreur de validation pour le champ spécifié.</span><span class="sxs-lookup"><span data-stu-id="052a5-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="052a5-252">MIME Affiche la liste de toutes les erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="052a5-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="052a5-253">MIME Inscrit un élément d’entrée utilisateur pour le type de validation spécifié.</span><span class="sxs-lookup"><span data-stu-id="052a5-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="052a5-254">MIME Affiche dynamiquement les attributs de classe CSS pour la validation côté client afin que vous puissiez mettre en forme les messages d’erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="052a5-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="052a5-255">(Exige que vous référençiez les bibliothèques de script client appropriées et que vous définissiez des classes CSS.)</span><span class="sxs-lookup"><span data-stu-id="052a5-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="052a5-256">MIME Active la validation côté client pour le champ d’entrée utilisateur.</span><span class="sxs-lookup"><span data-stu-id="052a5-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="052a5-257">(Exige que vous référençiez les bibliothèques de script client appropriées.)</span><span class="sxs-lookup"><span data-stu-id="052a5-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="052a5-258">MIME Retourne la valeur true si tous les éléments d’entrée utilisateur enregistrés pour validation contiennent des valeurs valides.</span><span class="sxs-lookup"><span data-stu-id="052a5-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="052a5-259">MIME Spécifie que les utilisateurs doivent fournir une valeur pour l’élément d’entrée utilisateur.</span><span class="sxs-lookup"><span data-stu-id="052a5-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="052a5-260">MIME Spécifie que les utilisateurs doivent fournir des valeurs pour chacun des éléments d’entrée utilisateur.</span><span class="sxs-lookup"><span data-stu-id="052a5-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="052a5-261">Cette méthode ne vous permet pas de spécifier un message d’erreur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="052a5-261">This method does not let you specify a custom error message.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

<span data-ttu-id="052a5-262">MIME Spécifie un test de validation lorsque vous utilisez la méthode `Validation.Add`.</span><span class="sxs-lookup"><span data-stu-id="052a5-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
