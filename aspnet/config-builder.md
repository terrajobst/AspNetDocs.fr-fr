---
uid: config-builder
title: Générateurs de configuration pour ASP.NET
author: rick-anderson
description: Découvrez comment obtenir des données de configuration à partir de sources autres que des valeurs Web. config à partir de sources externes.
ms.author: riande
ms.date: 10/29/2018
msc.type: content
ms.openlocfilehash: 5299d9ab057c3096773955a7461e77a80673ebfe
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74586763"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="46989-103">Générateurs de configuration pour ASP.NET</span><span class="sxs-lookup"><span data-stu-id="46989-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="46989-104">Par [Stephen Molloy](https://github.com/StephenMolloy) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="46989-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="46989-105">Les générateurs de configuration fournissent un mécanisme moderne et agile permettant aux applications ASP.NET d’obtenir des valeurs de configuration à partir de sources externes.</span><span class="sxs-lookup"><span data-stu-id="46989-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="46989-106">Générateurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="46989-106">Configuration builders:</span></span>

* <span data-ttu-id="46989-107">Sont disponibles dans .NET Framework 4.7.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="46989-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="46989-108">Fournir un mécanisme flexible pour la lecture des valeurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="46989-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="46989-109">Traitez certains des besoins de base des applications lorsqu’ils se déplacent dans un environnement de conteneur et de Cloud.</span><span class="sxs-lookup"><span data-stu-id="46989-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="46989-110">Peut être utilisé pour améliorer la protection des données de configuration en dessinant à partir de sources précédemment indisponibles (par exemple, des Azure Key Vault et des variables d’environnement) dans le système de configuration .NET.</span><span class="sxs-lookup"><span data-stu-id="46989-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="46989-111">Générateurs de configuration de clé/valeur</span><span class="sxs-lookup"><span data-stu-id="46989-111">Key/value configuration builders</span></span>

<span data-ttu-id="46989-112">Un scénario courant qui peut être géré par les générateurs de configuration consiste à fournir un mécanisme de remplacement de clé/valeur de base pour les sections de configuration qui suivent un modèle clé/valeur.</span><span class="sxs-lookup"><span data-stu-id="46989-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="46989-113">Le concept de .NET Framework de ConfigurationBuilders n’est pas limité aux sections ou modèles de configuration spécifiques.</span><span class="sxs-lookup"><span data-stu-id="46989-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="46989-114">Toutefois, un grand nombre des générateurs de configuration de `Microsoft.Configuration.ConfigurationBuilders` ([GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) fonctionnent dans le modèle clé/valeur.</span><span class="sxs-lookup"><span data-stu-id="46989-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="46989-115">Paramètres des générateurs de configuration de clé/valeur</span><span class="sxs-lookup"><span data-stu-id="46989-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="46989-116">Les paramètres suivants s’appliquent à tous les générateurs de configuration de clé/valeur dans `Microsoft.Configuration.ConfigurationBuilders`.</span><span class="sxs-lookup"><span data-stu-id="46989-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="46989-117">Mode</span><span class="sxs-lookup"><span data-stu-id="46989-117">Mode</span></span>

<span data-ttu-id="46989-118">Les générateurs de configuration utilisent une source externe d’informations de clé/valeur pour remplir les éléments clé/valeur sélectionnés du système de configuration.</span><span class="sxs-lookup"><span data-stu-id="46989-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="46989-119">Plus précisément, les sections `<appSettings/>` et `<connectionStrings/>` reçoivent un traitement spécial des générateurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="46989-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="46989-120">Les générateurs fonctionnent en trois modes :</span><span class="sxs-lookup"><span data-stu-id="46989-120">The builders work in three modes:</span></span>

* <span data-ttu-id="46989-121">`Strict` : mode par défaut.</span><span class="sxs-lookup"><span data-stu-id="46989-121">`Strict` - The default mode.</span></span> <span data-ttu-id="46989-122">Dans ce mode, le générateur de configuration fonctionne uniquement sur des sections de configuration de clé/valeur bien connues.</span><span class="sxs-lookup"><span data-stu-id="46989-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="46989-123">`Strict` mode énumère chaque clé dans la section.</span><span class="sxs-lookup"><span data-stu-id="46989-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="46989-124">Si une clé correspondante est trouvée dans la source externe :</span><span class="sxs-lookup"><span data-stu-id="46989-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="46989-125">Les générateurs de configuration remplacent la valeur de la section de configuration obtenue par la valeur de la source externe.</span><span class="sxs-lookup"><span data-stu-id="46989-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="46989-126">`Greedy` : ce mode est étroitement lié au mode de `Strict`.</span><span class="sxs-lookup"><span data-stu-id="46989-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="46989-127">Plutôt que d’être limitées aux clés qui existent déjà dans la configuration d’origine :</span><span class="sxs-lookup"><span data-stu-id="46989-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="46989-128">Les générateurs de configuration ajoutent toutes les paires clé/valeur de la source externe dans la section de configuration résultante.</span><span class="sxs-lookup"><span data-stu-id="46989-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="46989-129">`Expand`-opère sur le XML brut avant qu’il ne soit analysé dans un objet de section de configuration.</span><span class="sxs-lookup"><span data-stu-id="46989-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="46989-130">Il peut être considéré comme une expansion des jetons dans une chaîne.</span><span class="sxs-lookup"><span data-stu-id="46989-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="46989-131">Toute partie de la chaîne XML brute qui correspond au modèle `${token}` est candidate à l’expansion de jeton.</span><span class="sxs-lookup"><span data-stu-id="46989-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="46989-132">Si aucune valeur correspondante n’est trouvée dans la source externe, le jeton n’est pas modifié.</span><span class="sxs-lookup"><span data-stu-id="46989-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="46989-133">Dans ce mode, les générateurs ne sont pas limités aux sections `<appSettings/>` et `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="46989-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="46989-134">Le balisage suivant de *Web. config* active le [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) en mode `Strict` :</span><span class="sxs-lookup"><span data-stu-id="46989-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="46989-135">Le code suivant lit le `<appSettings/>` et `<connectionStrings/>` indiqué dans le fichier *Web. config* précédent :</span><span class="sxs-lookup"><span data-stu-id="46989-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="46989-136">Le code précédent définit les valeurs de propriété sur :</span><span class="sxs-lookup"><span data-stu-id="46989-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="46989-137">Valeurs du fichier *Web. config* si les clés ne sont pas définies dans les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="46989-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="46989-138">Valeurs de la variable d’environnement, si elles sont définies.</span><span class="sxs-lookup"><span data-stu-id="46989-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="46989-139">Par exemple, `ServiceID` contient :</span><span class="sxs-lookup"><span data-stu-id="46989-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="46989-140">« ServiceID value from Web. config », si la variable d’environnement `ServiceID` n’est pas définie.</span><span class="sxs-lookup"><span data-stu-id="46989-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="46989-141">Valeur de la variable d’environnement `ServiceID`, si elle est définie.</span><span class="sxs-lookup"><span data-stu-id="46989-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="46989-142">L’illustration suivante montre le `<appSettings/>` clés/valeurs du fichier *Web. config* précédent défini dans l’éditeur d’environnement :</span><span class="sxs-lookup"><span data-stu-id="46989-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![éditeur d’environnement](config-builder/static/env.png)

<span data-ttu-id="46989-144">Remarque : vous devrez peut-être quitter et redémarrer Visual Studio pour voir les modifications apportées aux variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="46989-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="46989-145">Gestion des préfixes</span><span class="sxs-lookup"><span data-stu-id="46989-145">Prefix handling</span></span>

<span data-ttu-id="46989-146">Les préfixes de clé peuvent simplifier la définition des clés, car :</span><span class="sxs-lookup"><span data-stu-id="46989-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="46989-147">La configuration .NET Framework est complexe et imbriquée.</span><span class="sxs-lookup"><span data-stu-id="46989-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="46989-148">Les sources de clé/valeur externes sont généralement de base et plates par nature.</span><span class="sxs-lookup"><span data-stu-id="46989-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="46989-149">Par exemple, les variables d’environnement ne sont pas imbriquées.</span><span class="sxs-lookup"><span data-stu-id="46989-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="46989-150">Utilisez l’une des approches suivantes pour injecter à la fois `<appSettings/>` et `<connectionStrings/>` dans la configuration via des variables d’environnement :</span><span class="sxs-lookup"><span data-stu-id="46989-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="46989-151">Avec la `EnvironmentConfigBuilder` en mode de `Strict` par défaut et les noms de clé appropriés dans le fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="46989-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="46989-152">Le code et le balisage précédents adoptent cette approche.</span><span class="sxs-lookup"><span data-stu-id="46989-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="46989-153">À l’aide de cette approche, vous **ne pouvez pas** avoir des clés portant le même nom dans `<appSettings/>` et `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="46989-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="46989-154">Utilisez deux `EnvironmentConfigBuilder`s en mode `Greedy` avec des préfixes et des `stripPrefix`distincts.</span><span class="sxs-lookup"><span data-stu-id="46989-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="46989-155">Avec cette approche, l’application peut lire `<appSettings/>` et `<connectionStrings/>` sans avoir besoin de mettre à jour le fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="46989-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="46989-156">La section suivante, [stripPrefix](#stripprefix), montre comment procéder.</span><span class="sxs-lookup"><span data-stu-id="46989-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="46989-157">Utilisez deux `EnvironmentConfigBuilder`s en mode `Greedy` avec des préfixes distincts.</span><span class="sxs-lookup"><span data-stu-id="46989-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="46989-158">Avec cette approche, vous ne pouvez pas avoir de noms de clé en double, car les noms de clé doivent différer par le préfixe.</span><span class="sxs-lookup"><span data-stu-id="46989-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="46989-159">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="46989-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="46989-160">Avec la balise précédente, la même source de clé/valeur plate peut être utilisée pour remplir la configuration de deux sections différentes.</span><span class="sxs-lookup"><span data-stu-id="46989-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="46989-161">L’illustration suivante montre le `<appSettings/>` et `<connectionStrings/>` clés/valeurs du fichier *Web. config* précédent défini dans l’éditeur d’environnement :</span><span class="sxs-lookup"><span data-stu-id="46989-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![éditeur d’environnement](config-builder/static/prefix.png)

<span data-ttu-id="46989-163">Le code suivant lit les clés/valeurs de `<appSettings/>` et `<connectionStrings/>` contenues dans le fichier *Web. config* précédent :</span><span class="sxs-lookup"><span data-stu-id="46989-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="46989-164">Le code précédent définit les valeurs de propriété sur :</span><span class="sxs-lookup"><span data-stu-id="46989-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="46989-165">Valeurs du fichier *Web. config* si les clés ne sont pas définies dans les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="46989-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="46989-166">Valeurs de la variable d’environnement, si elles sont définies.</span><span class="sxs-lookup"><span data-stu-id="46989-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="46989-167">Par exemple, en utilisant le fichier *Web. config* précédent, les clés/valeurs dans l’image précédente de l’éditeur d’environnement et le code précédent, les valeurs suivantes sont définies :</span><span class="sxs-lookup"><span data-stu-id="46989-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="46989-168">Clé</span><span class="sxs-lookup"><span data-stu-id="46989-168">Key</span></span>              | <span data-ttu-id="46989-169">Value</span><span class="sxs-lookup"><span data-stu-id="46989-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="46989-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="46989-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="46989-171">AppSetting_ServiceID à partir de variables env</span><span class="sxs-lookup"><span data-stu-id="46989-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="46989-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="46989-172">AppSetting_default</span></span>            | <span data-ttu-id="46989-173">Valeur AppSetting_default de env</span><span class="sxs-lookup"><span data-stu-id="46989-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="46989-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="46989-174">ConnStr_default</span></span>         | <span data-ttu-id="46989-175">ConnStr_default Val de env</span><span class="sxs-lookup"><span data-stu-id="46989-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="46989-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="46989-176">stripPrefix</span></span>

<span data-ttu-id="46989-177">`stripPrefix`: valeur booléenne, la valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="46989-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="46989-178">Le balisage XML précédent sépare les paramètres d’application des chaînes de connexion, mais requiert toutes les clés du fichier *Web. config* pour utiliser le préfixe spécifié.</span><span class="sxs-lookup"><span data-stu-id="46989-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="46989-179">Par exemple, le préfixe `AppSetting` doit être ajouté à la clé de `ServiceID` (« AppSetting_ServiceID »).</span><span class="sxs-lookup"><span data-stu-id="46989-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="46989-180">Avec `stripPrefix`, le préfixe n’est pas utilisé dans le fichier *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="46989-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="46989-181">Le préfixe est requis dans la source du générateur de configuration (par exemple, dans l’environnement). Nous pensons que la plupart des développeurs utiliseront `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="46989-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="46989-182">En général, les applications suppriment le préfixe.</span><span class="sxs-lookup"><span data-stu-id="46989-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="46989-183">Le *fichier Web. config* suivant supprime le préfixe :</span><span class="sxs-lookup"><span data-stu-id="46989-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="46989-184">Dans le fichier *Web. config* précédent, la clé de `default` se trouve à la fois dans les `<appSettings/>` et `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="46989-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="46989-185">L’illustration suivante montre le `<appSettings/>` et `<connectionStrings/>` clés/valeurs du fichier *Web. config* précédent défini dans l’éditeur d’environnement :</span><span class="sxs-lookup"><span data-stu-id="46989-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![éditeur d’environnement](config-builder/static/prefix.png)

<span data-ttu-id="46989-187">Le code suivant lit les clés/valeurs de `<appSettings/>` et `<connectionStrings/>` contenues dans le fichier *Web. config* précédent :</span><span class="sxs-lookup"><span data-stu-id="46989-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="46989-188">Le code précédent définit les valeurs de propriété sur :</span><span class="sxs-lookup"><span data-stu-id="46989-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="46989-189">Valeurs du fichier *Web. config* si les clés ne sont pas définies dans les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="46989-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="46989-190">Valeurs de la variable d’environnement, si elles sont définies.</span><span class="sxs-lookup"><span data-stu-id="46989-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="46989-191">Par exemple, en utilisant le fichier *Web. config* précédent, les clés/valeurs dans l’image précédente de l’éditeur d’environnement et le code précédent, les valeurs suivantes sont définies :</span><span class="sxs-lookup"><span data-stu-id="46989-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="46989-192">Clé</span><span class="sxs-lookup"><span data-stu-id="46989-192">Key</span></span>              | <span data-ttu-id="46989-193">Value</span><span class="sxs-lookup"><span data-stu-id="46989-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="46989-194">ServiceID</span><span class="sxs-lookup"><span data-stu-id="46989-194">ServiceID</span></span>           | <span data-ttu-id="46989-195">AppSetting_ServiceID à partir de variables env</span><span class="sxs-lookup"><span data-stu-id="46989-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="46989-196">default</span><span class="sxs-lookup"><span data-stu-id="46989-196">default</span></span>            | <span data-ttu-id="46989-197">Valeur AppSetting_default de env</span><span class="sxs-lookup"><span data-stu-id="46989-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="46989-198">default</span><span class="sxs-lookup"><span data-stu-id="46989-198">default</span></span>         | <span data-ttu-id="46989-199">ConnStr_default Val de env</span><span class="sxs-lookup"><span data-stu-id="46989-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="46989-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="46989-200">tokenPattern</span></span>

<span data-ttu-id="46989-201">`tokenPattern`: chaîne, la valeur par défaut est `@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="46989-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="46989-202">Le comportement `Expand` des générateurs recherche les jetons qui ressemblent à `${token}`dans le code XML brut.</span><span class="sxs-lookup"><span data-stu-id="46989-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="46989-203">La recherche s’effectue avec l’expression régulière par défaut `@"\$\{(\w+)\}"`.</span><span class="sxs-lookup"><span data-stu-id="46989-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="46989-204">Le jeu de caractères qui correspond à `\w` est plus strict que XML et de nombreuses sources de configuration le permettent.</span><span class="sxs-lookup"><span data-stu-id="46989-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="46989-205">Utilisez `tokenPattern` lorsque plus de caractères que `@"\$\{(\w+)\}"` sont requis dans le nom du jeton.</span><span class="sxs-lookup"><span data-stu-id="46989-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="46989-206">`tokenPattern`: chaîne :</span><span class="sxs-lookup"><span data-stu-id="46989-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="46989-207">Permet aux développeurs de modifier l’expression régulière utilisée pour la correspondance des jetons.</span><span class="sxs-lookup"><span data-stu-id="46989-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="46989-208">Aucune validation n’est effectuée pour s’assurer qu’il s’agit d’une expression régulière bien formée et non dangereuse.</span><span class="sxs-lookup"><span data-stu-id="46989-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="46989-209">Il doit contenir un groupe de capture.</span><span class="sxs-lookup"><span data-stu-id="46989-209">It must contain a capture group.</span></span> <span data-ttu-id="46989-210">L’expression régulière entière doit correspondre à l’intégralité du jeton.</span><span class="sxs-lookup"><span data-stu-id="46989-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="46989-211">La première capture doit être le nom du jeton à rechercher dans la source de configuration.</span><span class="sxs-lookup"><span data-stu-id="46989-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="46989-212">Générateurs de configuration dans Microsoft. Configuration. ConfigurationBuilders</span><span class="sxs-lookup"><span data-stu-id="46989-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="46989-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="46989-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="46989-214">[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="46989-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="46989-215">Est le plus simple des générateurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="46989-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="46989-216">Lit les valeurs de l’environnement.</span><span class="sxs-lookup"><span data-stu-id="46989-216">Reads values from the environment.</span></span>
* <span data-ttu-id="46989-217">N’a pas d’options de configuration supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="46989-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="46989-218">La valeur de l’attribut `name` est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="46989-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="46989-219">**Remarque :** Dans un environnement de conteneur Windows, les variables définies au moment de l’exécution sont injectées uniquement dans l’environnement de processus EntryPoint.</span><span class="sxs-lookup"><span data-stu-id="46989-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="46989-220">Les applications qui s’exécutent en tant que service ou processus sans point d’entrée ne récupèrent pas ces variables, sauf si elles sont injectées par le biais d’un mécanisme dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="46989-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="46989-221">Pour [IIS](https://github.com/Microsoft/iis-docker/pull/41)/les conteneurs basés sur [ASP.net](https://github.com/Microsoft/aspnet-docker), la version actuelle de [servicemonitor. exe](https://github.com/Microsoft/iis-docker/pull/41) gère uniquement cette fonction dans *DefaultAppPool* .</span><span class="sxs-lookup"><span data-stu-id="46989-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="46989-222">D’autres variantes de conteneur basées sur Windows peuvent avoir besoin de développer leur propre mécanisme d’injection pour les processus non EntryPoint.</span><span class="sxs-lookup"><span data-stu-id="46989-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="46989-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="46989-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="46989-224">Ne stockez jamais les mots de passe, les chaînes de connexion sensibles ou d’autres données sensibles dans le code source.</span><span class="sxs-lookup"><span data-stu-id="46989-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="46989-225">Les secrets de production ne doivent pas être utilisés à des fins de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="46989-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="46989-226">Ce générateur de configuration offre une fonctionnalité similaire à [ASP.net Core gestionnaire de secret](/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="46989-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="46989-227">Le [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) peut être utilisé dans .NET Framework projets, mais un fichier de secrets doit être spécifié.</span><span class="sxs-lookup"><span data-stu-id="46989-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="46989-228">Vous pouvez également définir la propriété `UserSecretsId` dans le fichier projet et créer le fichier de secrets bruts à l’emplacement correct pour la lecture.</span><span class="sxs-lookup"><span data-stu-id="46989-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="46989-229">Pour conserver les dépendances externes de votre projet, le fichier de secret est au format XML.</span><span class="sxs-lookup"><span data-stu-id="46989-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="46989-230">La mise en forme XML est un détail d’implémentation et le format ne doit pas être basé sur.</span><span class="sxs-lookup"><span data-stu-id="46989-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="46989-231">Si vous avez besoin de partager un fichier *secrets. JSON* avec des projets .net Core, envisagez d’utiliser [SimpleJsonConfigBuilder](#simplejsonconfigbuilder).</span><span class="sxs-lookup"><span data-stu-id="46989-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfigbuilder).</span></span> <span data-ttu-id="46989-232">Le format de `SimpleJsonConfigBuilder` pour .NET Core doit également être considéré comme un détail d’implémentation susceptible d’être modifié.</span><span class="sxs-lookup"><span data-stu-id="46989-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="46989-233">Attributs de configuration pour `UserSecretsConfigBuilder`:</span><span class="sxs-lookup"><span data-stu-id="46989-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="46989-234">`userSecretsId`-il s’agit de la méthode recommandée pour identifier un fichier de secrets XML.</span><span class="sxs-lookup"><span data-stu-id="46989-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="46989-235">Il fonctionne de la même façon que .NET Core, qui utilise une propriété de projet `UserSecretsId` pour stocker cet identificateur.</span><span class="sxs-lookup"><span data-stu-id="46989-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="46989-236">La chaîne doit être unique, il n’est pas nécessaire qu’il s’agit d’un GUID.</span><span class="sxs-lookup"><span data-stu-id="46989-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="46989-237">Avec cet attribut, le `UserSecretsConfigBuilder` Rechercher dans un emplacement local connu (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) un fichier de secrets appartenant à cet identificateur.</span><span class="sxs-lookup"><span data-stu-id="46989-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="46989-238">`userSecretsFile` : attribut facultatif spécifiant le fichier contenant les secrets.</span><span class="sxs-lookup"><span data-stu-id="46989-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="46989-239">Le caractère `~` peut être utilisé au début pour référencer la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="46989-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="46989-240">Cet attribut ou l’attribut `userSecretsId` est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="46989-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="46989-241">Si les deux sont spécifiés, `userSecretsFile` est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="46989-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="46989-242">`optional`: Boolean, valeur par défaut `true`-empêche une exception si le fichier de secrets est introuvable.</span><span class="sxs-lookup"><span data-stu-id="46989-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="46989-243">La valeur de l’attribut `name` est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="46989-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="46989-244">Le format du fichier de secrets est le suivant :</span><span class="sxs-lookup"><span data-stu-id="46989-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="46989-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="46989-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="46989-246">[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) lit les valeurs stockées dans le [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="46989-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="46989-247">`vaultName` est requis (nom du coffre ou URI du coffre).</span><span class="sxs-lookup"><span data-stu-id="46989-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="46989-248">Les autres attributs permettent de contrôler le coffre auquel se connecter, mais ne sont nécessaires que si l’application ne s’exécute pas dans un environnement qui fonctionne avec `Microsoft.Azure.Services.AppAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="46989-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="46989-249">La bibliothèque d’authentification des services Azure est utilisée pour récupérer automatiquement les informations de connexion à partir de l’environnement d’exécution, si possible.</span><span class="sxs-lookup"><span data-stu-id="46989-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="46989-250">Vous pouvez remplacer automatiquement les informations de connexion en fournissant une chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="46989-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="46989-251">`vaultName`-requis si `uri` n’est pas fourni.</span><span class="sxs-lookup"><span data-stu-id="46989-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="46989-252">Spécifie le nom du coffre dans votre abonnement Azure à partir duquel lire les paires clé/valeur.</span><span class="sxs-lookup"><span data-stu-id="46989-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="46989-253">`connectionString`-une chaîne de connexion utilisable par [le paramètre azureservicetokenprovider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="46989-253">`connectionString` - A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* <span data-ttu-id="46989-254">`uri` : se connecte à d’autres fournisseurs Key Vault avec la valeur de `uri` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="46989-254">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="46989-255">S’il n’est pas spécifié, Azure (`vaultName`) est le fournisseur du coffre.</span><span class="sxs-lookup"><span data-stu-id="46989-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="46989-256">`version`-Azure Key Vault fournit une fonctionnalité de contrôle de version pour les secrets.</span><span class="sxs-lookup"><span data-stu-id="46989-256">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="46989-257">Si `version` est spécifié, le générateur récupère uniquement les secrets correspondant à cette version.</span><span class="sxs-lookup"><span data-stu-id="46989-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="46989-258">`preloadSecretNames` : par défaut, ce générateur interroge **tous les** noms de clé dans le coffre de clés lors de son initialisation.</span><span class="sxs-lookup"><span data-stu-id="46989-258">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="46989-259">Pour empêcher la lecture de toutes les valeurs de clés, affectez à cet attribut la valeur `false`.</span><span class="sxs-lookup"><span data-stu-id="46989-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="46989-260">L’affectation de la valeur `false` lit les secrets un par un.</span><span class="sxs-lookup"><span data-stu-id="46989-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="46989-261">La lecture d’un secret à la fois peut être utile si le coffre autorise un accès « obtenir », mais pas un accès « liste ».</span><span class="sxs-lookup"><span data-stu-id="46989-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="46989-262">**Remarque :** Lors de l’utilisation du mode `Greedy`, `preloadSecretNames` doit être `true` (valeur par défaut).</span><span class="sxs-lookup"><span data-stu-id="46989-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="46989-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="46989-263">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="46989-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) est un générateur de configuration de base qui utilise les fichiers d’un répertoire comme source de valeurs.</span><span class="sxs-lookup"><span data-stu-id="46989-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="46989-265">Le nom d’un fichier est la clé et le contenu est la valeur.</span><span class="sxs-lookup"><span data-stu-id="46989-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="46989-266">Ce générateur de configuration peut être utile lors de l’exécution dans un environnement de conteneurs orchestrés.</span><span class="sxs-lookup"><span data-stu-id="46989-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="46989-267">Les systèmes tels que Dockr essaim et Kubernetes fournissent `secrets` à leurs conteneurs Windows orchestrés dans cette clé par fichier.</span><span class="sxs-lookup"><span data-stu-id="46989-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="46989-268">Détails de l’attribut :</span><span class="sxs-lookup"><span data-stu-id="46989-268">Attribute details:</span></span>

* <span data-ttu-id="46989-269">`directoryPath` - Obligatoire.</span><span class="sxs-lookup"><span data-stu-id="46989-269">`directoryPath` - Required.</span></span> <span data-ttu-id="46989-270">Spécifie un chemin d’accès pour rechercher des valeurs.</span><span class="sxs-lookup"><span data-stu-id="46989-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="46989-271">Docker pour Windows les secrets sont stockés dans le répertoire *C:\ProgramData\Docker\secrets* par défaut.</span><span class="sxs-lookup"><span data-stu-id="46989-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="46989-272">`ignorePrefix`-les fichiers qui commencent par ce préfixe sont exclus.</span><span class="sxs-lookup"><span data-stu-id="46989-272">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="46989-273">La valeur par défaut est « ignore ».</span><span class="sxs-lookup"><span data-stu-id="46989-273">Defaults to "ignore.".</span></span>
* <span data-ttu-id="46989-274">`keyDelimiter`-la valeur par défaut est `null`.</span><span class="sxs-lookup"><span data-stu-id="46989-274">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="46989-275">S’il est spécifié, le générateur de configuration parcourt plusieurs niveaux de l’annuaire, en générant des noms de clés avec ce délimiteur.</span><span class="sxs-lookup"><span data-stu-id="46989-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="46989-276">Si cette valeur est `null`, le générateur de configuration recherche uniquement le niveau supérieur de l’annuaire.</span><span class="sxs-lookup"><span data-stu-id="46989-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="46989-277">`optional`-la valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="46989-277">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="46989-278">Spécifie si le générateur de configuration doit provoquer des erreurs si le répertoire source n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="46989-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="46989-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="46989-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="46989-280">Ne stockez jamais les mots de passe, les chaînes de connexion sensibles ou d’autres données sensibles dans le code source.</span><span class="sxs-lookup"><span data-stu-id="46989-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="46989-281">Les secrets de production ne doivent pas être utilisés à des fins de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="46989-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="46989-282">Les projets .NET Core utilisent fréquemment des fichiers JSON pour la configuration.</span><span class="sxs-lookup"><span data-stu-id="46989-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="46989-283">[SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) Builder permet d’utiliser les fichiers JSON .net core dans le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="46989-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="46989-284">Ce générateur de configuration fournit un mappage de base à partir d’une source de clé/valeur plate dans des zones clé/valeur spécifiques de .NET Framework Configuration.</span><span class="sxs-lookup"><span data-stu-id="46989-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="46989-285">Ce générateur de configuration ne fournit **pas** de configurations hiérarchiques.</span><span class="sxs-lookup"><span data-stu-id="46989-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="46989-286">Le fichier de stockage JSON est similaire à un dictionnaire, et non à un objet hiérarchique complexe.</span><span class="sxs-lookup"><span data-stu-id="46989-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="46989-287">Un fichier hiérarchique à plusieurs niveaux peut être utilisé.</span><span class="sxs-lookup"><span data-stu-id="46989-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="46989-288">Ce fournisseur `flatten`la profondeur en ajoutant le nom de la propriété à chaque niveau à l’aide de `:` comme délimiteur.</span><span class="sxs-lookup"><span data-stu-id="46989-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="46989-289">Détails de l’attribut :</span><span class="sxs-lookup"><span data-stu-id="46989-289">Attribute details:</span></span>

* <span data-ttu-id="46989-290">`jsonFile` - Obligatoire.</span><span class="sxs-lookup"><span data-stu-id="46989-290">`jsonFile` - Required.</span></span> <span data-ttu-id="46989-291">Spécifie le fichier JSON à partir duquel effectuer la lecture.</span><span class="sxs-lookup"><span data-stu-id="46989-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="46989-292">Le caractère `~` peut être utilisé au début pour référencer la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="46989-292">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="46989-293">`optional`-booléen, la valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="46989-293">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="46989-294">Empêche de lever des exceptions si le fichier JSON est introuvable.</span><span class="sxs-lookup"><span data-stu-id="46989-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="46989-295">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="46989-295">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="46989-296">`Flat` est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="46989-296">`Flat` is the default.</span></span> <span data-ttu-id="46989-297">Lorsque `jsonMode` est `Flat`, le fichier JSON est une source de clé/valeur unique.</span><span class="sxs-lookup"><span data-stu-id="46989-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="46989-298">Les `EnvironmentConfigBuilder` et `AzureKeyVaultConfigBuilder` sont également des sources de clé/valeur à plat unique.</span><span class="sxs-lookup"><span data-stu-id="46989-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="46989-299">Lorsque le `SimpleJsonConfigBuilder` est configuré en mode `Sectional` :</span><span class="sxs-lookup"><span data-stu-id="46989-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="46989-300">Le fichier JSON est divisé de façon conceptuelle juste au niveau supérieur en plusieurs dictionnaires.</span><span class="sxs-lookup"><span data-stu-id="46989-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="46989-301">Chacun des dictionnaires est appliqué uniquement à la section de configuration qui correspond au nom de propriété de niveau supérieur associé.</span><span class="sxs-lookup"><span data-stu-id="46989-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="46989-302">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="46989-302">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="46989-303">Implémentation d’un générateur de configuration de clé/valeur personnalisé</span><span class="sxs-lookup"><span data-stu-id="46989-303">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="46989-304">Si les générateurs de configuration ne répondent pas à vos besoins, vous pouvez en écrire un personnalisé.</span><span class="sxs-lookup"><span data-stu-id="46989-304">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="46989-305">La classe de base `KeyValueConfigBuilder` gère les modes de substitution et la plupart des problèmes de préfixe.</span><span class="sxs-lookup"><span data-stu-id="46989-305">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="46989-306">Un projet d’implémentation a besoin uniquement des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="46989-306">An implementing project need only:</span></span>

* <span data-ttu-id="46989-307">Héritez de la classe de base et implémentez une source de base de paires clé/valeur via le `GetValue` et `GetAllValues`:</span><span class="sxs-lookup"><span data-stu-id="46989-307">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="46989-308">Ajoutez [Microsoft. Configuration. ConfigurationBuilders. base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) au projet.</span><span class="sxs-lookup"><span data-stu-id="46989-308">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="46989-309">La classe de base `KeyValueConfigBuilder` fournit une grande partie du travail et un comportement cohérent entre les générateurs de configuration de clé/valeur.</span><span class="sxs-lookup"><span data-stu-id="46989-309">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="46989-310">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="46989-310">Additional resources</span></span>

* [<span data-ttu-id="46989-311">Référentiel GitHub générateurs de configuration</span><span class="sxs-lookup"><span data-stu-id="46989-311">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="46989-312">Authentification de service à service pour Azure Key Vault à l’aide de .NET</span><span class="sxs-lookup"><span data-stu-id="46989-312">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
