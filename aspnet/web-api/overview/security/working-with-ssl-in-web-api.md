---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Utilisation de SSL dans l’API Web | Microsoft Docs
author: MikeWasson
description: Montre comment utiliser SSL avec API Web ASP.NET, y compris l’utilisation de certificats clients SSL.
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 31589b3713b1f1a9b98d12906bfef81f8bf5e3f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598636"
---
# <a name="working-with-ssl-in-web-api"></a><span data-ttu-id="b7861-103">Working with SSL in Web API (Utilisation de SSL dans l’API Web)</span><span class="sxs-lookup"><span data-stu-id="b7861-103">Working with SSL in Web API</span></span>

<span data-ttu-id="b7861-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b7861-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b7861-105">Plusieurs schémas d’authentification courants ne sont pas sécurisés via un HTTP simple.</span><span class="sxs-lookup"><span data-stu-id="b7861-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="b7861-106">En particulier, l'authentification de base et l'authentification par formulaires envoient des informations d'identification non chiffrées.</span><span class="sxs-lookup"><span data-stu-id="b7861-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="b7861-107">Pour être sécurisés, ces schémas d’authentification *doivent* utiliser SSL.</span><span class="sxs-lookup"><span data-stu-id="b7861-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="b7861-108">En outre, les certificats clients SSL peuvent être utilisés pour authentifier les clients.</span><span class="sxs-lookup"><span data-stu-id="b7861-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="b7861-109">Activation du protocole SSL sur le serveur</span><span class="sxs-lookup"><span data-stu-id="b7861-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="b7861-110">Pour configurer SSL dans IIS 7 ou version ultérieure :</span><span class="sxs-lookup"><span data-stu-id="b7861-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="b7861-111">Créez ou récupérez un certificat.</span><span class="sxs-lookup"><span data-stu-id="b7861-111">Create or get a certificate.</span></span> <span data-ttu-id="b7861-112">Pour le test, vous pouvez créer un certificat auto-signé.</span><span class="sxs-lookup"><span data-stu-id="b7861-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="b7861-113">Ajoutez une liaison HTTPs.</span><span class="sxs-lookup"><span data-stu-id="b7861-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="b7861-114">Pour plus d’informations, consultez Configuration de [SSL sur IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="b7861-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="b7861-115">Pour les tests locaux, vous pouvez activer SSL dans IIS Express à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b7861-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="b7861-116">Dans la Fenêtre Propriétés, attribuez la valeur **true**à **SSL** .</span><span class="sxs-lookup"><span data-stu-id="b7861-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="b7861-117">Notez la valeur de l' **URL SSL**; Utilisez cette URL pour tester les connexions HTTPs.</span><span class="sxs-lookup"><span data-stu-id="b7861-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="b7861-118">Application de SSL dans un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="b7861-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="b7861-119">Si vous disposez d’une liaison HTTPs et HTTP, les clients peuvent toujours utiliser le protocole HTTP pour accéder au site.</span><span class="sxs-lookup"><span data-stu-id="b7861-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="b7861-120">Vous pouvez autoriser la disponibilité de certaines ressources par le biais du protocole HTTP, tandis que d’autres ressources nécessitent SSL.</span><span class="sxs-lookup"><span data-stu-id="b7861-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="b7861-121">Dans ce cas, utilisez un filtre d’action pour exiger le protocole SSL pour les ressources protégées.</span><span class="sxs-lookup"><span data-stu-id="b7861-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="b7861-122">Le code suivant montre un filtre d’authentification d’API web qui recherche SSL :</span><span class="sxs-lookup"><span data-stu-id="b7861-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="b7861-123">Ajoutez ce filtre à toutes les actions de l’API web nécessitant SSL :</span><span class="sxs-lookup"><span data-stu-id="b7861-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="b7861-124">Certificats clients SSL</span><span class="sxs-lookup"><span data-stu-id="b7861-124">SSL Client Certificates</span></span>

<span data-ttu-id="b7861-125">SSL assure l’authentification à l’aide de certificats d’infrastructure de clé publique.</span><span class="sxs-lookup"><span data-stu-id="b7861-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="b7861-126">Le serveur doit fournir un certificat qui authentifie le serveur auprès du client.</span><span class="sxs-lookup"><span data-stu-id="b7861-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="b7861-127">Il est moins courant pour le client de fournir un certificat au serveur, mais il s’agit d’une option pour l’authentification des clients.</span><span class="sxs-lookup"><span data-stu-id="b7861-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="b7861-128">Pour utiliser des certificats clients avec SSL, vous devez disposer d’un moyen de distribuer des certificats signés à vos utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b7861-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="b7861-129">Pour de nombreux types d’applications, cela ne constitue pas une bonne expérience utilisateur, mais dans certains environnements (par exemple, Enterprise), il peut être possible.</span><span class="sxs-lookup"><span data-stu-id="b7861-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="b7861-130">Avantages</span><span class="sxs-lookup"><span data-stu-id="b7861-130">Advantages</span></span> | <span data-ttu-id="b7861-131">Inconvénients</span><span class="sxs-lookup"><span data-stu-id="b7861-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="b7861-132">-Les informations d’identification du certificat sont plus fortes que le nom d’utilisateur/mot de passe.</span><span class="sxs-lookup"><span data-stu-id="b7861-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="b7861-133">-SSL fournit un canal sécurisé complet, avec l’authentification, l’intégrité des messages et le chiffrement des messages.</span><span class="sxs-lookup"><span data-stu-id="b7861-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="b7861-134">-Vous devez obtenir et gérer les certificats PKI.</span><span class="sxs-lookup"><span data-stu-id="b7861-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="b7861-135">-La plateforme cliente doit prendre en charge les certificats clients SSL.</span><span class="sxs-lookup"><span data-stu-id="b7861-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="b7861-136">Pour configurer IIS pour accepter les certificats clients, ouvrez le gestionnaire des services Internet et procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="b7861-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="b7861-137">Cliquez sur le nœud du site dans l’arborescence.</span><span class="sxs-lookup"><span data-stu-id="b7861-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="b7861-138">Double-cliquez sur la fonctionnalité **Paramètres SSL** dans le volet central.</span><span class="sxs-lookup"><span data-stu-id="b7861-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="b7861-139">Sous **certificats clients**, sélectionnez l’une des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="b7861-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="b7861-140">**Accepter**: IIS acceptera un certificat du client, mais il n’en a pas besoin.</span><span class="sxs-lookup"><span data-stu-id="b7861-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="b7861-141">**Exiger**: exiger un certificat client.</span><span class="sxs-lookup"><span data-stu-id="b7861-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="b7861-142">(Pour activer cette option, vous devez également sélectionner « exiger SSL »)</span><span class="sxs-lookup"><span data-stu-id="b7861-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="b7861-143">Vous pouvez également définir ces options dans le fichier ApplicationHost. config :</span><span class="sxs-lookup"><span data-stu-id="b7861-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="b7861-144">L’indicateur **SslNegotiateCert** signifie que les services Internet (IIS) acceptent un certificat du client, mais n’en ont pas besoin (équivalent à l’option « accepter » dans le gestionnaire des services Internet).</span><span class="sxs-lookup"><span data-stu-id="b7861-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="b7861-145">Pour exiger un certificat, définissez l’indicateur **SslRequireCert** .</span><span class="sxs-lookup"><span data-stu-id="b7861-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="b7861-146">Pour le test, vous pouvez également définir ces options dans IIS Express, dans le fichier applicationHost local. Fichier de configuration, situé dans « Documents\IISExpress\config ».</span><span class="sxs-lookup"><span data-stu-id="b7861-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="b7861-147">Création d’un certificat client à des fins de test</span><span class="sxs-lookup"><span data-stu-id="b7861-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="b7861-148">À des fins de test, vous pouvez utiliser [Makecert. exe](/windows/desktop/SecCrypto/makecert) pour créer un certificat client.</span><span class="sxs-lookup"><span data-stu-id="b7861-148">For testing purposes, you can use [MakeCert.exe](/windows/desktop/SecCrypto/makecert) to create a client certificate.</span></span> <span data-ttu-id="b7861-149">Tout d’abord, créez une autorité racine de test :</span><span class="sxs-lookup"><span data-stu-id="b7861-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="b7861-150">Makecert vous invite à entrer un mot de passe pour la clé privée.</span><span class="sxs-lookup"><span data-stu-id="b7861-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="b7861-151">Ensuite, ajoutez le certificat au magasin « autorités de certification racines de confiance » du serveur de test, comme suit :</span><span class="sxs-lookup"><span data-stu-id="b7861-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="b7861-152">Ouvrez la console MMC.</span><span class="sxs-lookup"><span data-stu-id="b7861-152">Open MMC.</span></span>
2. <span data-ttu-id="b7861-153">Sous **fichier**, sélectionnez **Ajouter/supprimer un composant logiciel enfichable**.</span><span class="sxs-lookup"><span data-stu-id="b7861-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="b7861-154">Sélectionnez un **compte d’ordinateur**.</span><span class="sxs-lookup"><span data-stu-id="b7861-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="b7861-155">Sélectionnez **ordinateur local** et terminez l’Assistant.</span><span class="sxs-lookup"><span data-stu-id="b7861-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="b7861-156">Dans le volet de navigation, développez le nœud « autorités de certification racines de confiance ».</span><span class="sxs-lookup"><span data-stu-id="b7861-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="b7861-157">Dans le menu **action** , pointez sur **toutes les tâches**, puis cliquez sur **Importer** pour démarrer l’Assistant importation de certificat.</span><span class="sxs-lookup"><span data-stu-id="b7861-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="b7861-158">Accédez au fichier de certificat, TempCA. cer.</span><span class="sxs-lookup"><span data-stu-id="b7861-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="b7861-159">Cliquez sur **ouvrir**, puis sur **suivant** et terminez l’Assistant.</span><span class="sxs-lookup"><span data-stu-id="b7861-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="b7861-160">(Vous serez invité à entrer de nouveau le mot de passe.)</span><span class="sxs-lookup"><span data-stu-id="b7861-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="b7861-161">À présent, créez un certificat client signé par le premier certificat :</span><span class="sxs-lookup"><span data-stu-id="b7861-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="b7861-162">Utilisation de certificats clients dans l’API Web</span><span class="sxs-lookup"><span data-stu-id="b7861-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="b7861-163">Côté serveur, vous pouvez obtenir le certificat client en appelant [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) sur le message de demande.</span><span class="sxs-lookup"><span data-stu-id="b7861-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="b7861-164">La méthode retourne la valeur null s’il n’y a aucun certificat client.</span><span class="sxs-lookup"><span data-stu-id="b7861-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="b7861-165">Sinon, elle retourne une instance **X509Certificate2** .</span><span class="sxs-lookup"><span data-stu-id="b7861-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="b7861-166">Utilisez cet objet pour récupérer des informations à partir du certificat, telles que l’émetteur et l’objet.</span><span class="sxs-lookup"><span data-stu-id="b7861-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="b7861-167">Vous pouvez ensuite utiliser ces informations pour l’authentification et/ou l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="b7861-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
