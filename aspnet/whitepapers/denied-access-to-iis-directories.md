---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET a refusé l’accès aux répertoires IIS | Microsoft Docs
author: rick-anderson
description: Ce livre blanc décrit ce que vous devez faire si une demande à votre application ASP.NET retourne l’erreur «Accès refusé au répertoire DirectoryName. Échec de la tentative...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: a3a53aa88abbe1bcaaea7d691406800c8f9b988b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638501"
---
# <a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="55a3c-104">ASP.NET - Accès refusé aux répertoires IIS</span><span class="sxs-lookup"><span data-stu-id="55a3c-104">ASP.NET Denied Access to IIS Directories</span></span>

> <span data-ttu-id="55a3c-105">Ce livre blanc décrit ce que vous devez faire si une demande à votre application ASP.NET retourne l’erreur «Accès refusé au répertoire *DirectoryName* .</span><span class="sxs-lookup"><span data-stu-id="55a3c-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="55a3c-106">Échec du démarrage de l’analyse des modifications de répertoire.»</span><span class="sxs-lookup"><span data-stu-id="55a3c-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="55a3c-107">S’applique à ASP.NET 1,0 et ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="55a3c-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>

<span data-ttu-id="55a3c-108">ASP.NET v1 RTM s’exécute désormais à l’aide d’un compte Windows avec moins de privilèges, inscrit en tant que compte « ASPNET » sur un ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="55a3c-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="55a3c-109">Sur certains systèmes verrouillés, ce compte ne peut pas par défaut avoir accès en lecture à la sécurité des répertoires de contenu d’un site Web, du répertoire racine de l’application ou du répertoire racine du site Web.</span><span class="sxs-lookup"><span data-stu-id="55a3c-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="55a3c-110">Dans ce cas, vous recevrez l’erreur suivante lors de la demande de pages d’une application Web donnée :</span><span class="sxs-lookup"><span data-stu-id="55a3c-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="55a3c-111">Pour résoudre ce problème, vous devez modifier les autorisations de sécurité sur les répertoires appropriés.</span><span class="sxs-lookup"><span data-stu-id="55a3c-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="55a3c-112">Plus précisément, ASP.NET requiert un accès en lecture, en exécution et en liste pour le compte ASPNET pour la racine du site Web (par exemple : c:\inetpub\wwwroot ou tout autre répertoire de site que vous avez peut-être configuré dans IIS), le répertoire de contenu et le répertoire racine de l’application pour surveiller les modifications apportées au fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="55a3c-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="55a3c-113">La racine de l’application correspond au chemin d’accès au dossier associé au répertoire virtuel de l’application dans l’outil d’administration IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="55a3c-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="55a3c-114">Par exemple, considérez la hiérarchie d’application suivante sous le dossier Wwwroot.</span><span class="sxs-lookup"><span data-stu-id="55a3c-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="55a3c-115">Pour cet exemple, le compte ASPNET a besoin des autorisations de lecture définies ci-dessus pour le contenu des répertoires MyApp et wwwroot.</span><span class="sxs-lookup"><span data-stu-id="55a3c-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="55a3c-116">Une seule ACL héritée sur le dossier racine peut également être utilisée pour les deux répertoires s’ils sont imbriqués.</span><span class="sxs-lookup"><span data-stu-id="55a3c-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="55a3c-117">Pour ajouter des autorisations à un répertoire, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="55a3c-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="55a3c-118">À l’aide de l’Explorateur Windows, accédez au répertoire</span><span class="sxs-lookup"><span data-stu-id="55a3c-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="55a3c-119">Cliquez avec le bouton droit sur le dossier Directory, puis choisissez « Propriétés ».</span><span class="sxs-lookup"><span data-stu-id="55a3c-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="55a3c-120">Accédez à l’onglet « sécurité » dans la boîte de dialogue des propriétés.</span><span class="sxs-lookup"><span data-stu-id="55a3c-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="55a3c-121">Cliquez sur le bouton « Ajouter », puis entrez le nom de l’ordinateur suivi du nom du compte ASPNET.</span><span class="sxs-lookup"><span data-stu-id="55a3c-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="55a3c-122">Par exemple, sur un ordinateur nommé « WebDev », vous devez entrer webdev\ASPNET et appuyer sur « OK ».</span><span class="sxs-lookup"><span data-stu-id="55a3c-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="55a3c-123">Vérifiez que les cases à cocher « lire &amp; exécuter », « afficher le contenu du dossier » et « lecture » sont activées dans le compte ASPNET.</span><span class="sxs-lookup"><span data-stu-id="55a3c-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="55a3c-124">Appuyez sur OK pour fermer la boîte de dialogue et enregistrer les modifications.</span><span class="sxs-lookup"><span data-stu-id="55a3c-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="55a3c-125">Si vous le souhaitez, ces modifications peuvent être automatisées à l’aide de scripts ou de l’outil « Cacls. exe » fourni avec Windows.</span><span class="sxs-lookup"><span data-stu-id="55a3c-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="55a3c-126">Pour plus d’informations sur le compte ASPNET, consultez le [document FAQ](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="55a3c-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="55a3c-127">Si une application Web donnée s’appuie sur des autorisations d’écriture ou de modification d’un dossier ou d’un fichier particulier, cette opération peut être accordée en suivant la même procédure et en activant les cases à cocher « écrire » et/ou « modifier ».</span><span class="sxs-lookup"><span data-stu-id="55a3c-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="55a3c-128">Sur les ordinateurs qui autorisent tout le monde ou le groupe d’utilisateurs à accéder en lecture à ces répertoires (qui est la configuration par défaut), aucun problème ne se produit et les étapes ci-dessus ne sont pas requises.</span><span class="sxs-lookup"><span data-stu-id="55a3c-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
