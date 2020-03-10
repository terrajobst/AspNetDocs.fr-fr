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
# <a name="working-with-ssl-in-web-api"></a>Working with SSL in Web API (Utilisation de SSL dans l’API Web)

par [Mike Wasson](https://github.com/MikeWasson)

Plusieurs schémas d’authentification courants ne sont pas sécurisés via un HTTP simple. En particulier, l'authentification de base et l'authentification par formulaires envoient des informations d'identification non chiffrées. Pour être sécurisés, ces schémas d’authentification *doivent* utiliser SSL. En outre, les certificats clients SSL peuvent être utilisés pour authentifier les clients.

## <a name="enabling-ssl-on-the-server"></a>Activation du protocole SSL sur le serveur

Pour configurer SSL dans IIS 7 ou version ultérieure :

- Créez ou récupérez un certificat. Pour le test, vous pouvez créer un certificat auto-signé.
- Ajoutez une liaison HTTPs.

Pour plus d’informations, consultez Configuration de [SSL sur IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Pour les tests locaux, vous pouvez activer SSL dans IIS Express à partir de Visual Studio. Dans la Fenêtre Propriétés, attribuez la valeur **true**à **SSL** . Notez la valeur de l' **URL SSL**; Utilisez cette URL pour tester les connexions HTTPs.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Application de SSL dans un contrôleur d’API Web

Si vous disposez d’une liaison HTTPs et HTTP, les clients peuvent toujours utiliser le protocole HTTP pour accéder au site. Vous pouvez autoriser la disponibilité de certaines ressources par le biais du protocole HTTP, tandis que d’autres ressources nécessitent SSL. Dans ce cas, utilisez un filtre d’action pour exiger le protocole SSL pour les ressources protégées. Le code suivant montre un filtre d’authentification d’API web qui recherche SSL :

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Ajoutez ce filtre à toutes les actions de l’API web nécessitant SSL :

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>Certificats clients SSL

SSL assure l’authentification à l’aide de certificats d’infrastructure de clé publique. Le serveur doit fournir un certificat qui authentifie le serveur auprès du client. Il est moins courant pour le client de fournir un certificat au serveur, mais il s’agit d’une option pour l’authentification des clients. Pour utiliser des certificats clients avec SSL, vous devez disposer d’un moyen de distribuer des certificats signés à vos utilisateurs. Pour de nombreux types d’applications, cela ne constitue pas une bonne expérience utilisateur, mais dans certains environnements (par exemple, Enterprise), il peut être possible.

| Avantages | Inconvénients |
| --- | --- |
| -Les informations d’identification du certificat sont plus fortes que le nom d’utilisateur/mot de passe. -SSL fournit un canal sécurisé complet, avec l’authentification, l’intégrité des messages et le chiffrement des messages. | -Vous devez obtenir et gérer les certificats PKI. -La plateforme cliente doit prendre en charge les certificats clients SSL. |

Pour configurer IIS pour accepter les certificats clients, ouvrez le gestionnaire des services Internet et procédez comme suit :

1. Cliquez sur le nœud du site dans l’arborescence.
2. Double-cliquez sur la fonctionnalité **Paramètres SSL** dans le volet central.
3. Sous **certificats clients**, sélectionnez l’une des options suivantes : 

    - **Accepter**: IIS acceptera un certificat du client, mais il n’en a pas besoin.
    - **Exiger**: exiger un certificat client. (Pour activer cette option, vous devez également sélectionner « exiger SSL »)

Vous pouvez également définir ces options dans le fichier ApplicationHost. config :

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

L’indicateur **SslNegotiateCert** signifie que les services Internet (IIS) acceptent un certificat du client, mais n’en ont pas besoin (équivalent à l’option « accepter » dans le gestionnaire des services Internet). Pour exiger un certificat, définissez l’indicateur **SslRequireCert** . Pour le test, vous pouvez également définir ces options dans IIS Express, dans le fichier applicationHost local. Fichier de configuration, situé dans « Documents\IISExpress\config ».

### <a name="creating-a-client-certificate-for-testing"></a>Création d’un certificat client à des fins de test

À des fins de test, vous pouvez utiliser [Makecert. exe](/windows/desktop/SecCrypto/makecert) pour créer un certificat client. Tout d’abord, créez une autorité racine de test :

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Makecert vous invite à entrer un mot de passe pour la clé privée.

Ensuite, ajoutez le certificat au magasin « autorités de certification racines de confiance » du serveur de test, comme suit :

1. Ouvrez la console MMC.
2. Sous **fichier**, sélectionnez **Ajouter/supprimer un composant logiciel enfichable**.
3. Sélectionnez un **compte d’ordinateur**.
4. Sélectionnez **ordinateur local** et terminez l’Assistant.
5. Dans le volet de navigation, développez le nœud « autorités de certification racines de confiance ».
6. Dans le menu **action** , pointez sur **toutes les tâches**, puis cliquez sur **Importer** pour démarrer l’Assistant importation de certificat.
7. Accédez au fichier de certificat, TempCA. cer.
8. Cliquez sur **ouvrir**, puis sur **suivant** et terminez l’Assistant. (Vous serez invité à entrer de nouveau le mot de passe.)

À présent, créez un certificat client signé par le premier certificat :

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Utilisation de certificats clients dans l’API Web

Côté serveur, vous pouvez obtenir le certificat client en appelant [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) sur le message de demande. La méthode retourne la valeur null s’il n’y a aucun certificat client. Sinon, elle retourne une instance **X509Certificate2** . Utilisez cet objet pour récupérer des informations à partir du certificat, telles que l’émetteur et l’objet. Vous pouvez ensuite utiliser ces informations pour l’authentification et/ou l’autorisation.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
