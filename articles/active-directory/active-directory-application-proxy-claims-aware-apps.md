---
title: Trabajar con las aplicaciones para notificaciones en Proxy de aplicación
description: Explica cómo poner en funcionamiento el proxy de la aplicación de Azure AD.
services: active-directory
documentationcenter: ''
author: kgremban
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2016
ms.author: kgremban

---
# Trabajar con las aplicaciones para notificaciones en Proxy de aplicación
Las aplicaciones para notificaciones realizan una redirección al servicio de token de seguridad (STS), que a su vez solicita las credenciales del usuario a cambio de un token antes de redirigir al usuario a la aplicación. Para permitir que el proxy de aplicación funcione con estas redirecciones, deben realizarse los siguientes pasos.

## Requisitos previos
Antes de realizar este procedimiento, asegúrese de que el STS al que redirige la aplicación para notificaciones está disponible fuera de la red local.

## Configuración del Portal de Azure clásico
1. Publique la aplicación según las instrucciones de [Publicar aplicaciones con el proxy de aplicación](active-directory-application-proxy-publish.md).
2. En la lista de aplicaciones, seleccione la aplicación para notificaciones y haga clic en **Configurar**.
3. Si elige **Acceso directo** como su **Método de autenticación previa**, asegúrese de seleccionar **HTTPS** como su esquema **Dirección URL externa**.
4. Si elige **Azure Active Directory** como **Método de autenticación previa**, seleccione **Ninguno** como **Método de autenticación interno**.

## Configuración de AD FS
1. Abra Administración de AD FS.
2. Vaya a **Relying Party Trusts** (Relaciones de confianza para usuario autenticado), haga clic con el botón derecho en la aplicación que va a publicar con el proxy de aplicación y seleccione **Propiedades**. ![Relaciones de confianza para usuario autenticado: haga clic con el botón derecho en el nombre de la aplicación - captura de pantalla](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  
3. En la pestaña **Extremos**, en **Tipo de extremo**, seleccione **WS-Federation**.
4. En **Trusted URL** (Dirección URL de confianza), escriba la dirección URL que indicó en Proxy de aplicación en **Dirección URL externa** y haga clic en **Aceptar**. ![Agregar un extremo: establezca el valor de Dirección URL de confianza - captura de pantalla](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## Consulte también
* [Publicar aplicaciones con Proxy de aplicación](active-directory-application-proxy-publish.md)
* [Habilitar el inicio de sesión único](active-directory-application-proxy-sso-using-kcd.md)
* [Solucionar los problemas que tiene con el Proxy de aplicación](active-directory-application-proxy-troubleshoot.md)
* [Habilitación de las aplicaciones de cliente nativo para interactuar con el proxy de aplicación](active-directory-application-proxy-native-client.md)

Para ver las últimas noticias y actualizaciones, consulte el [Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/) (Blog de Proxy de aplicación).

<!---HONumber=AcomDC_0622_2016-->