---
title: "Hacer que un dominio de Internet de la compañía indique un nombre de dominio de Traffic Manager | Microsoft Docs"
description: "Este artículo le ayudará a que el nombre de dominio de la empresa indique un nombre de dominio del Administrador de tráfico."
services: traffic-manager
documentationcenter: 
author: sdwheeler
manager: carmonm
editor: 
ms.assetid: 29822946-2d45-4434-ba47-fc180a445cc3
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 16602ca37e3b62fc628ec3178e6f65e32fc3d345

---

# <a name="point-a-company-internet-domain-to-an-azure-traffic-manager-domain"></a>Hacer que un dominio de Internet de la compañía indique un dominio de Azure Traffic Manager

Cuando se crea un perfil de Traffic Manager, Azure asigna automáticamente un nombre DNS para ese perfil. Para usar un nombre de la zona DNS, cree un registro DNS CNAME que se asigne al nombre de dominio de su perfil de Traffic Manager. Puede encontrar el nombre de dominio de Traffic Manager en la sección **General** de la página Configuración del perfil de Traffic Manager.

Por ejemplo, para que el nombre www.contoso.com apunte al nombre DNS de Traffic Manager contoso.trafficmanager.net, debe crear el siguiente registro de recursos DNS:

    www.contoso.com IN CNAME contoso.trafficmanager.net

Todas las solicitudes de tráfico hacia *www.contoso.com* se redirigen a *contoso.trafficmanager.net*.

> [!IMPORTANT]
> No puede hacer que un dominio de segundo nivel como por ejemplo *contoso.com*, indique el dominio del Administrador de tráfico. Los estándares de protocolo DNS no permiten registros CNAME para nombres de dominio de segundo nivel.

## <a name="next-steps"></a>Pasos siguientes

* [Métodos de enrutamiento del Administrador de tráfico](traffic-manager-routing-methods.md)
* [Administrador de tráfico: deshabilitación, habilitación o eliminación de un perfil](disable-enable-or-delete-a-profile.md)
* [Administrador de tráfico: deshabilitación o habilitación de un extremo](disable-or-enable-an-endpoint.md)



<!--HONumber=Nov16_HO2-->


