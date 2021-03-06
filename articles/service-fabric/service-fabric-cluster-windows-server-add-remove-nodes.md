---
title: Incorporación o eliminación de un clúster de Service Fabric independiente | Microsoft Docs
description: Obtenga información sobre cómo agregar o quitar nodos en un clúster de Azure Service Fabric en una máquina virtual o física con Windows Server, la que podría estar en el entorno local o en alguna nube.
services: service-fabric
documentationcenter: .net
author: dsk-2015
manager: timlt
editor: ''

ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2016
ms.author: dkshir;chackdan

---
# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Incorporación o eliminación de nodos de un clúster de Service Fabric independiente con Windows Server
Una vez que [cree su clúster de Service Fabric independiente en máquinas de Windows Server](service-fabric-cluster-creation-for-windows-server.md), puede que las necesidades empresariales cambien y que deba agregar o quitar varios nodos del clúster. En este artículo, se muestran los pasos detallados para lograr este objetivo.

## <a name="add-nodes-to-your-cluster"></a>Incorporación de nodos al clúster
1. Prepare la VM o la máquina que desea agregar al clúster con los pasos que se indican en la sección [Preparar las máquinas para cumplir los requisitos previos de la implementación del clúster](service-fabric-cluster-creation-for-windows-server.md#preparemachines) .
2. Planee a qué dominio de error y de actualización va a agregar esta máquina o VM.
3. Abra una conexión de Escritorio remoto (RDP) en la máquina o VM que desea agregar al clúster.
4. Copie o [descargue el paquete independiente de Service Fabric para Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) en esta máquina o VM y descomprímalo.
5. Ejecute PowerShell como administrador y vaya a la ubicación del paquete no descomprimido.
6. Ejecute el script *AddNode.ps1* de PowerShell con los parámetros que describen el nuevo nodo que se va a agregar. En el ejemplo siguiente se agrega un nuevo nodo denominado VM5, con el tipo NodeType0 y la dirección IP 182.17.34.52, en UD1 y FD1. *ExistingClusterConnectionEndPoint* es un punto de conexión para un nodo que ya está presente en el clúster existente. Para este punto de conexión, puede elegir la dirección IP de *cualquier* nodo del clúster.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1 -AcceptEULA

```

## <a name="remove-nodes-from-your-cluster"></a>Eliminación de nodos del clúster
1. Según el nivel de confiabilidad elegido para el clúster, no podrá quitar los primeros nodos de n (3/5/7/9) del tipo de nodo principal
2. No se admite la ejecución del comando RemoveNode en un clúster de desarrollo.
3. Abra una conexión de Escritorio remoto (RDP) en la máquina o máquina virtual que desea quitar del clúster.
4. Copie o [descargue el paquete independiente de Service Fabric para Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) y descomprima el paquete en esta VM o máquina.
5. Ejecute PowerShell como administrador y vaya a la ubicación del paquete no descomprimido.
6. Ejecute el script *RemoveNode.ps1* de PowerShell. En el ejemplo siguiente se quita el nodo actual del clúster. *ExistingClusterConnectionEndPoint* es un punto de conexión para un nodo que ya está presente en el clúster existente. Para este punto de conexión, puede elegir la dirección IP de *cualquier* **otro nodo** del clúster.

```
.\RemoveNode.ps1 -ExistingClusterConnectionEndPoint 182.17.34.50:19000
```

Defecto conocido que se corregirá en la próxima versión. Incluso después de quitar un nodo, este sigue apareciendo como inactivo en las consultas y SFX. 

## <a name="next-steps"></a>Pasos siguientes
* [Opciones de configuración de clústeres de Windows independientes](service-fabric-cluster-manifest.md)
* [Proteger un clúster independiente en Windows mediante la seguridad de Windows](service-fabric-windows-cluster-windows-security.md)
* [Protección de un clúster de Windows independiente mediante certificados](service-fabric-windows-cluster-x509-security.md)
* [Creación de un clúster de Service Fabric independiente con VM de Azure con Windows](service-fabric-cluster-creation-with-windows-azure-vms.md)

<!--HONumber=Oct16_HO2-->


