---
title: Características y configuración del servicio de sincronización de Azure AD Connect | Microsoft Docs
description: Describe las características del servicio de sincronización de Azure AD Connect.
services: active-directory
documentationcenter: ''
author: andkjell
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: andkjell;markvi

---
# <a name="azure-ad-connect-sync-service-features"></a>Características del servicio de sincronización de Azure AD Connect
La característica de sincronización de Azure AD Connect tiene dos componentes:

* El componente local denominado **sincronización de Azure AD Connect**, también conocido como **motor de sincronización**.
* El servicio que se encuentra en Azure AD, también conocido como **servicio de sincronización de Azure AD Connect**

En este tema se explica cómo funcionan las siguientes características del **servicio de sincronización de Azure AD Connect** y cómo puede configurarlas mediante Windows PowerShell.

Estas opciones se configuran mediante el [módulo Azure Active Directory para Windows PowerShell](http://aka.ms/aadposh). Descárguelo e instálelo por separado desde Azure AD Connect. Los cmdlets descritos en este tema se introdujeron con la [versión de marzo de 2016 (compilación 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Si no tiene los cmdlets que se documentan en este tema o estos no generan el mismo resultado, asegúrese de que ejecuta la versión más reciente.

Para ver la configuración en el directorio de Azure AD, ejecute `Get-MsolDirSyncFeatures`.  
![Resultado de Get-MsolDirSyncFeatures](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Muchas de estas opciones solo se pueden cambiar mediante Azure AD Connect.

Se pueden configurar las siguientes opciones en `Set-MsolDirSyncFeature`:

| DirSyncFeature | Comentario |
| --- | --- |
| [DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) |Permite poner un atributo en cuarentena si es un duplicado de otro objeto en lugar de consignar un error en todo el objeto durante la exportación. |
| [EnableSoftMatchOnUpn](#userprincipalname-soft-match) |Permite que los objetos se unan a userPrincipalName además de la dirección SMTP principal. |
| [SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) |Permite que el motor de sincronización actualice el atributo userPrincipalName para los usuarios administrados y con licencia (no federados). |

Después de habilitar una característica, no se puede volver a deshabilitar.

> [!NOTE]
> Desde el 24 de agosto de 2016, la característica *Resistencia de atributos duplicados* está habilitada de forma predeterminada para nuevos directorios de Azure AD. Esta característica también se implantará y habilitará en directorios creados antes de esta fecha. Recibirá una notificación por correo electrónico cuando el directorio esté próximo a habilitar esta característica.
> 
> 

Las siguientes opciones se configuran mediante Azure AD Connect y no se pueden modificar por `Set-MsolDirSyncFeature`:

| DirSyncFeature | Comentario |
| --- | --- |
| DeviceWriteback |[Azure AD Connect: habilitación de la escritura diferida de dispositivos](active-directory-aadconnect-feature-device-writeback.md) |
| DirectoryExtensions |[Sincronización de Azure AD Connect: Extensiones de directorio](active-directory-aadconnectsync-feature-directory-extensions.md) |
| PasswordSync |[Implementación de la sincronización de contraseña mediante la sincronización de Azure AD Connect](active-directory-aadconnectsync-implement-password-synchronization.md) |
| UnifiedGroupWriteback |[Versión preliminar: reescritura de grupos](active-directory-aadconnect-feature-preview.md#group-writeback) |
| UserWriteback |No se admite actualmente. |

## <a name="duplicate-attribute-resiliency"></a>Resistencia de atributos duplicados
En lugar de no aprovisionar objetos con atributos UPN/proxyAddresses duplicados, el atributo duplicado se "pone en cuarentena" y se le asigna un valor temporal. Cuando se resuelve el conflicto, el UPN temporal se cambia por el valor correcto automáticamente. Este comportamiento se puede habilitar para UPN y proxyAddress por separado. Para obtener más información, consulte [Sincronización de identidades y resistencia de atributos duplicados](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>Coincidencia parcial de UserPrincipalName
Cuando esta característica está habilitada, se activa la coincidencia parcial en UPN, así como la [dirección SMTP principal](https://support.microsoft.com/kb/2641663), que siempre está habilitada. La coincidencia parcial se utiliza para hacer coincidir los usuarios de Azure AD en la nube con los locales.

Si necesita que coincidan las cuentas de AD locales con las cuentas existentes creadas en la nube y no está utilizando Exchange Online, esta característica resulta útil. En este escenario, normalmente no tiene una razón para establecer el atributo de SMTP en la nube.

Esta característica está activa de forma predeterminada para los directorios de Azure AD recién creados. Puede ver si esta característica está habilitada en su caso ejecutando lo siguiente:  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Si esta característica no está habilitada para el directorio de Azure AD, puede habilitarla ejecutando lo siguiente:  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>Sincronización de las actualizaciones de userPrincipalName
Históricamente, las actualizaciones para el atributo UserPrincipalName con el servicio de sincronización desde una instancia local han estado bloqueadas, a menos que se cumplieran las siguientes condiciones:

* El usuario está administrado (no federado).
* Al usuario no se le ha asignado una licencia.

Para obtener más información, consulte [Nombres de usuario en Office 365, Azure o Intune no coinciden con el UPN local o el identificador de inicio de sesión alternativo](https://support.microsoft.com/kb/2523192).

Esta característica permite al motor de sincronización actualizar el valor de userPrincipalName cuando se modifica de manera local y se usa la sincronización de contraseñas. Si utiliza la federación, no se admite esta característica.

Esta característica está activa de forma predeterminada para los directorios de Azure AD recién creados. Puede ver si esta característica está habilitada en su caso ejecutando lo siguiente:  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Si esta característica no está habilitada para el directorio de Azure AD, puede habilitarla ejecutando lo siguiente:  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Después de habilitar esta característica, los valores existentes de userPrincipalName permanecerán tal cual. En el próximo cambio del atributo userPrincipalName en la instancia local, la sincronización diferencial normal de los usuarios actualizará el UPN.  

## <a name="see-also"></a>Otras referencias
* [Sincronización de Azure AD Connect](active-directory-aadconnectsync-whatis.md)
* [Integración de las identidades locales con Azure Active Directory](active-directory-aadconnect.md).

<!--HONumber=Oct16_HO2-->


