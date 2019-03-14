---
title: Tag Helpers intégrés d’ASP.NET Core
author: pkellner
description: Découvrez comment les Tag Helpers intégrés d’ASP.NET Core augmentent votre productivité.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/Index
---

# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="1ee78-103">Tag Helpers intégrés d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ee78-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="1ee78-104">Par [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="1ee78-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="1ee78-105">Pour avoir une vue d’ensemble des Tag Helpers, consultez <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="1ee78-105">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

> [!NOTE]
> <span data-ttu-id="1ee78-106">Il existe des Tag Helpers intégrés qui ne sont pas décrits dans la documentation.</span><span class="sxs-lookup"><span data-stu-id="1ee78-106">There are built-in Tag Helpers which aren't described in the documentation.</span></span> <span data-ttu-id="1ee78-107">Ces Tag Helpers sont utilisés en interne par le moteur d’affichage [Razor](xref:mvc/views/razor) .</span><span class="sxs-lookup"><span data-stu-id="1ee78-107">These Tag Helpers are used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="1ee78-108">Cela inclut notamment un Tag Helper pour le caractère `~` (tilde), qui développe le chemin racine du site web.</span><span class="sxs-lookup"><span data-stu-id="1ee78-108">This includes a Tag Helper for the `~` (tilde) character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="1ee78-109">Tag Helpers intégrés d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ee78-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="1ee78-110">**[Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1ee78-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="1ee78-111">**[Tag Helper de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1ee78-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="1ee78-112">**[Tag Helper de cache distribué](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1ee78-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="1ee78-113">**[Tag Helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1ee78-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="1ee78-114">**[Tag Helper Form](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1ee78-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="1ee78-115">**[Tag Helper d’image](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1ee78-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="1ee78-116">**[Tag Helper Input](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1ee78-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="1ee78-117">**[Tag Helper Label](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1ee78-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="1ee78-118">**[Tag Helper Partial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1ee78-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="1ee78-119">**[Tag Helper Select](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1ee78-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="1ee78-120">**[Tag Helper Textarea](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1ee78-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="1ee78-121">**[Tag Helper de message de validation](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1ee78-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="1ee78-122">**[Tag Helper de résumé de validation](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1ee78-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ee78-123">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="1ee78-123">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/th-components>
