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
# <a name="aspnet-web-pages-razor-api-quick-reference"></a>Aide-mémoire de l’API pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cette page contient une liste contenant des exemples succincts des objets, propriétés et méthodes les plus couramment utilisés pour programmer des pages Web ASP.NET avec syntaxe Razor.
> 
> Les descriptions marquées avec « (v2) » ont été introduites dans pages Web ASP.NET version 2.
> 
> Pour obtenir une documentation de référence sur les API, consultez la [documentation de référence pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=208659) sur MSDN.
> 
> ## <a name="software-versions"></a>Versions de logiciels
> 
> 
> - Pages Web ASP.NET (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2 et pages Web ASP.NET 1,0 (à l’exception des fonctionnalités marquées v2).

Cette page contient des informations de référence sur les éléments suivants :

- [Classes](#Classes)
- [Données](#Data)
- [Helpers](#Helpers)
- [Validation](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Classes

### `AppState[key], AppState[index],App`

Contient des données qui peuvent être partagées par n’importe quelle page de l’application. Vous pouvez utiliser la propriété Dynamic `App` pour accéder aux mêmes données, comme dans l’exemple suivant :

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Convertit une valeur de chaîne en valeur booléenne (true/false). Retourne false ou la valeur spécifiée si la chaîne ne représente pas true/false.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Convertit une valeur de chaîne en date/heure. Retourne `DateTime.MinValue` ou la valeur spécifiée si la chaîne ne représente pas une date/heure.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Convertit une valeur de chaîne en valeur décimale. Retourne 0,0 ou la valeur spécifiée si la chaîne ne représente pas une valeur décimale.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Convertit une valeur de chaîne en valeur de type float. Retourne 0,0 ou la valeur spécifiée si la chaîne ne représente pas une valeur décimale.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Convertit une valeur de chaîne en entier. Retourne 0 ou la valeur spécifiée si la chaîne ne représente pas un entier.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Crée une URL compatible avec le navigateur à partir d’un chemin d’accès de fichier local, avec des parties de chemin d’accès supplémentaires facultatives.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Restitue la *valeur* en tant que balisage HTML au lieu de la restituer en tant que sortie codée au format html.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Retourne la valeur true si la valeur peut être convertie à partir d’une chaîne vers le type spécifié.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Retourne la valeur true si l’objet ou la variable n’a pas de valeur.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Retourne la valeur true si la demande est une publication. (Les demandes initiales sont généralement une récupération.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Spécifie le chemin d’accès d’une page de disposition à appliquer à cette page.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Contient des données partagées entre la page, les pages de disposition et les pages partielles dans la requête actuelle. Vous pouvez utiliser la propriété Dynamic `Page` pour accéder aux mêmes données, comme dans l’exemple suivant :

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Pages de disposition) Restitue le contenu d’une page de contenu qui n’est pas dans les sections nommées.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Génère le rendu d’une page de contenu à l’aide du chemin d’accès spécifié et des données supplémentaires facultatives. Vous pouvez récupérer les valeurs des paramètres supplémentaires à partir de `PageData` par position (exemple 1) ou clé (exemple 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Pages de disposition) Génère le rendu d’une section de contenu qui a un nom. Affectez à *Required* la valeur false pour rendre une section facultative.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Obtient ou définit la valeur d’un cookie HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Obtient les fichiers qui ont été téléchargés dans la requête actuelle.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Obtient les données publiées dans un formulaire (sous forme de chaînes). `Request[key]` vérifie à la fois les collections `Request.Form` et `Request.QueryString`.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Obtient les données qui ont été spécifiées dans la chaîne de requête de l’URL. `Request[key]` vérifie à la fois les collections `Request.Form` et `Request.QueryString`.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Désactive de manière sélective la validation des demandes pour un élément de formulaire, une valeur de chaîne de requête, un cookie ou une valeur d’en-tête. La validation de la demande est activée par défaut et empêche les utilisateurs de publier le balisage ou tout autre contenu potentiellement dangereux.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Ajoute un en-tête de serveur HTTP à la réponse.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Met en cache la sortie de page pendant une durée spécifiée. Vous pouvez éventuellement définir le *glissement* pour réinitialiser le délai d’attente sur chaque accès à la page et *VaryByParams* pour mettre en cache différentes versions de la page pour chaque chaîne de requête différente dans la demande de page.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Redirige la demande du navigateur vers un nouvel emplacement.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Définit le code d’état HTTP envoyé au navigateur.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Écrit le contenu des *données* dans la réponse avec un type MIME facultatif.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Écrit le contenu d’un fichier dans la réponse.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Pages de disposition) Définit une section de contenu qui a un nom.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Décode une chaîne encodée au format HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Encode une chaîne pour le rendu dans le balisage HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Retourne le chemin d’accès physique du serveur pour le chemin d’accès virtuel spécifié.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Décode le texte d’une URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Encode le texte à placer dans une URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Obtient ou définit une valeur qui existe jusqu’à ce que l’utilisateur ferme le navigateur.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Affiche une représentation sous forme de chaîne de la valeur de l’objet.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Obtient des données supplémentaires à partir de l’URL (par exemple, */MyPage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Modifie le mot de passe de l'utilisateur spécifié.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Confirme un compte à l’aide du jeton de confirmation de compte.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Crée un nouveau compte d’utilisateur avec le nom d’utilisateur et le mot de passe spécifiés. Pour exiger un jeton de confirmation, transmettez true pour *requireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Obtient l’identificateur entier pour l’utilisateur actuellement connecté.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Obtient le nom de l’utilisateur actuellement connecté.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Génère un jeton de réinitialisation de mot de passe qui peut être envoyé par courrier électronique à un utilisateur pour permettre à l’utilisateur de réinitialiser le mot de passe.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Retourne l’ID d’utilisateur à partir du nom d’utilisateur.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Retourne la valeur true si l’utilisateur actuel est connecté.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Retourne la valeur true si l’utilisateur a été confirmé (par exemple, via un e-mail de confirmation).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Retourne la valeur true si le nom de l’utilisateur actuel correspond au nom d’utilisateur spécifié.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Journalise l’utilisateur en définissant un jeton d’authentification dans le cookie.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Déconnecte l’utilisateur en supprimant le cookie de jeton d’authentification.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Si l'utilisateur n'est pas authentifié, définit l'état HTTP sur 401 (non autorisé).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Si l’utilisateur actuel n’est pas membre de l’un des rôles spécifiés, définit l’état HTTP sur 401 (non autorisé).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Si l’utilisateur actuel n’est pas l’utilisateur spécifié par le *nom d'* utilisateur, définit l’état HTTP sur 401 (non autorisé).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Si le jeton de réinitialisation du mot de passe est valide, remplace le mot de passe de l’utilisateur par le nouveau mot de passe.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Données

### `Database.Execute(SQLstatement [,parameters]`

Exécute *SQLStatement* (avec des paramètres facultatifs), tels que Insert, Delete ou Update, et retourne le nombre d’enregistrements affectés.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Retourne la colonne d’identité de la ligne la plus récemment insérée.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Ouvre le fichier de base de données spécifié ou la base de données spécifiée à l’aide d’une chaîne de connexion nommée à partir du fichier *Web. config* .

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Ouvre une base de données à l’aide de la chaîne de connexion. (Cela contraste avec `Database.Open`, qui utilise un nom de chaîne de connexion.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Interroge la base de données à l’aide de *SQLStatement* (en passant éventuellement des paramètres) et retourne les résultats sous la forme d’une collection.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Exécute *SQLStatement* (avec des paramètres facultatifs) et retourne un seul enregistrement.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Exécute *SQLStatement* (avec des paramètres facultatifs) et retourne une valeur unique.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Programmes d’assistance

### `Analytics.GetGoogleHtml(webPropertyId)`

Génère le rendu du code JavaScript Google Analytics pour l’ID spécifié.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Génère le rendu du code JavaScript StatCounter Analytics pour le projet spécifié.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Génère le rendu du code JavaScript Yahoo Analytics pour le compte spécifié.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Passe une recherche à Bing. Pour spécifier le site à rechercher et un titre pour la zone de recherche, vous pouvez définir les propriétés `Bing.SiteUrl` et `Bing.SiteTitle`. Normalement, vous définissez ces propriétés dans la page *\_AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Initialise un graphique.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Ajoute une légende à un graphique.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Ajoute une série de valeurs au graphique.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Retourne un hachage pour les données spécifiées. L’algorithme par défaut est `sha256`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Permet aux utilisateurs de Facebook d’établir une connexion avec les pages.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Génère le rendu de l’interface utilisateur pour le chargement des fichiers.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Génère le rendu de la balise Xbox Gamer spécifiée.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Génère le rendu de l’image Gravatar pour l’adresse de messagerie spécifiée.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Convertit un objet de données en une chaîne au format JavaScript Object Notation (JSON).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Convertit une chaîne d’entrée encodée JSON en objet de données que vous pouvez effectuer une itération ou une insertion dans une base de données.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Génère le rendu des liens de réseau social en utilisant le titre et l’URL facultatives spécifiés.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Associe un message d’erreur à un champ de formulaire. Utilisez le programme d’assistance `ModelState` pour accéder à ce membre.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Associe un message d’erreur à un formulaire. Utilisez le programme d’assistance `ModelState` pour accéder à ce membre.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Retourne la valeur true s’il n’y a aucune erreur de validation. Utilisez le programme d’assistance `ModelState` pour accéder à ce membre.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Génère le rendu des propriétés et des valeurs d’un objet et de tous les objets enfants.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Génère le rendu du test de vérification reCAPTCHA.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Définit des clés publiques et privées pour le service reCAPTCHA. Normalement, vous définissez ces propriétés dans la page *\_AppStart* .

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Retourne le résultat du test reCAPTCHA.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Génère le rendu des informations d’État relatives à pages Web ASP.NET.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Génère le rendu d’un flux Twitter pour l’utilisateur spécifié.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Génère le rendu d’un flux Twitter pour le texte de recherche spécifié.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Restitue un lecteur vidéo Flash pour le fichier spécifié avec une largeur et une hauteur facultatives.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Génère le rendu d’un lecteur Windows Media pour le fichier spécifié avec une largeur et une hauteur facultatives.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Restitue un lecteur Silverlight pour le fichier *. xap* spécifié avec la largeur et la hauteur requises.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Retourne l’objet spécifié par la *clé*, ou null si l’objet est introuvable.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Supprime l’objet spécifié par la *clé* du cache.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Place la *valeur* dans le cache sous le nom spécifié par la *clé*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Crée un objet `WebGrid` à l’aide des données d’une requête.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Restitue le balisage pour afficher des données dans une table HTML.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Génère le rendu d’un pagineur pour l’objet `WebGrid`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Charge une image à partir du chemin d’accès spécifié.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Ajoute l’image spécifiée sous la forme d’un filigrane.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Ajoute le texte spécifié à l’image.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Retourne l’image horizontalement ou verticalement.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Charge une image quand une image est publiée dans une page pendant le chargement d’un fichier.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Redimensionne une image.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Fait pivoter l’image vers la gauche ou vers la droite.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Enregistre l’image dans le chemin d’accès spécifié.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Définit le mot de passe du serveur SMTP. Normalement, vous définissez cette propriété dans la page *\_AppStart* .

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Envoie un e-mail.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Définit le nom du serveur SMTP. Normalement, vous définissez cette propriété dans la page *\_AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Définit le nom d’utilisateur du serveur SMTP. Normalement, vous devez définir cette propriété dans la page *\_AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Validation

### `Html.ValidationMessage(field)`

MIME Génère le rendu d’un message d’erreur de validation pour le champ spécifié.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

MIME Affiche la liste de toutes les erreurs de validation.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

MIME Inscrit un élément d’entrée utilisateur pour le type de validation spécifié.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

MIME Affiche dynamiquement les attributs de classe CSS pour la validation côté client afin que vous puissiez mettre en forme les messages d’erreur de validation. (Exige que vous référençiez les bibliothèques de script client appropriées et que vous définissiez des classes CSS.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

MIME Active la validation côté client pour le champ d’entrée utilisateur. (Exige que vous référençiez les bibliothèques de script client appropriées.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

MIME Retourne la valeur true si tous les éléments d’entrée utilisateur enregistrés pour validation contiennent des valeurs valides.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

MIME Spécifie que les utilisateurs doivent fournir une valeur pour l’élément d’entrée utilisateur.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

MIME Spécifie que les utilisateurs doivent fournir des valeurs pour chacun des éléments d’entrée utilisateur. Cette méthode ne vous permet pas de spécifier un message d’erreur personnalisé.

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

MIME Spécifie un test de validation lorsque vous utilisez la méthode `Validation.Add`.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
