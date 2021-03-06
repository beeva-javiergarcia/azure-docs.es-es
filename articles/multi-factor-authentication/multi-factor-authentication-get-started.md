---
title: Nube frente a servidor de Azure MFA | Microsoft Docs
description: "Elija la solución de seguridad para la autenticación multifactor que sea más adecuada para su situación contestando a dos preguntas: qué estoy tratando de asegurar y dónde están ubicados mis usuarios.  A continuación, elija la nube, Servidor MFA o AD FS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: ec2270ea-13d7-4ebc-8a00-fa75ce6c746d
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/14/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 72347099d980f2ca73f39f984787197e1f87e45a


---
# <a name="choose-the-azure-multifactor-authentication-solution-for-you"></a>Elección de la solución de Azure Multi-Factor Authentication más adecuada
Puesto que existen varios modelos de Azure Multi-Factor Authentication (MFA), se debe responder primero a algunas preguntas para descubrir cuál es el más adecuado para usar.  Estas preguntas son:

* [Qué es lo que quiero proteger](#what-am-i-trying-to-secure)
* [Dónde se encuentran los usuarios](#where-are-the-users-located)
* [¿Qué características necesito?](#what-featured-do-i-need)

Las siguientes secciones proporcionan instrucciones para determinar cada una de estas respuestas.

## <a name="what-am-i-trying-to-secure"></a>¿Qué es lo que quiero proteger?
Para determinar la solución correcta de verificación en dos pasos, en primer lugar, hay que responder a la pregunta de qué es lo que está intentando proteger con un segundo método de autenticación.  ¿Es una aplicación que está en Azure?  ¿O un sistema de acceso remoto?  Mediante la determinación de lo que estamos intentando proteger, encontraremos la respuesta a la pregunta de dónde hay que habilitar Multi-Factor Authentication.  

| ¿Qué intenta proteger? | Multi-Factor Authentication en la nube | Servidor Multi-Factor Authentication |
| --- |:---:|:---:|
| Aplicaciones propias de Microsoft |● |● |
| Aplicaciones de SaaS en la galería de aplicaciones |● |● |
| Aplicaciones de IIS que se publican a través del proxy de aplicación de Azure AD |● |● |
| Aplicaciones de IIS que no se publican a través del proxy de aplicación de Azure AD | |● |
| Acceso remoto como VPN, RDG | |● |

## <a name="where-are-the-users-located"></a>Dónde se encuentran los usuarios
A continuación, en función del lugar en que se encuentren los usuarios, podemos determinar la solución correcta que se debe utilizar, ya sea en la nube o de forma local mediante Servidor MFA.

| Ubicación del usuario | Multi-Factor Authentication en la nube | Servidor Multi-Factor Authentication |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| Azure AD y AD local mediante la federación con AD FS |● |● |
| Azure AD y AD local a través de DirSync, Azure AD Sync, Azure AD Connect - sin sincronización de contraseñas |● |● |
| Azure AD y AD local a través de DirSync, Azure AD Sync, Azure AD Connect - con sincronización de contraseñas |● | |
| Active Directory local | |● |

## <a name="what-features-do-i-need"></a>¿Qué características necesito?
La tabla siguiente es una comparación de las características que están disponibles en Multi-Factor Authentication en la nube con las del Servidor Multi-Factor Authentication.

| Multi-Factor Authentication en la nube | Servidor Multi-Factor Authentication |
| --- |:---:|:---:|
| Notificación de aplicación móvil como segundo factor |● |
| Código de comprobación de aplicación móvil como segundo factor |● |
| Llamada de teléfono como segundo factor |● |
| SMS unidireccional como segundo factor |● |
| SMS bidireccional como segundo factor | |
| Tokens de hardware como segundo factor | |
| Contraseñas de aplicación para los clientes que no son compatibles con MFA |● |
| Control de administración sobre los métodos de autenticación |● |
| Modo de PIN | |
| Alerta de fraude |● |
| Informes de MFA |● |
| Omisión por única vez | |
| Saludos personalizados para las llamadas de teléfono |● |
| Identificador de llamada personalizable para llamadas telefónicas |● |
| IP de confianza |● |
| Recordar MFA para dispositivos de confianza |● |
| Acceso condicional |● |
| Memoria caché | |

Ahora que hemos determinado cuál vamos a usar: la autenticación multifactor de nube o el Servidor MFA local, ya podemos comenzar a configurar y usar Azure Multi-Factor Authentication. **Seleccione el icono que representa su opción**

<center>




[![Nube](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Página de proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>



<!--HONumber=Nov16_HO2-->


