---
title: Información general sobre la terminología de Service Fabric | Microsoft Docs
description: Descripción sobre la terminología de Service Fabric. Describe la terminología y los conceptos clave que se emplean en el resto de la documentación.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: chackdan;subramar

ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/25/2016
ms.author: ryanwi

---
# Información general sobre la terminología de Service Fabric
Service Fabric es una plataforma de sistemas distribuidos que facilita el empaquetamiento, la implementación y la administración de microservicios escalables y confiables. En este tema se describe la terminología que se usa en Service Fabric para comprender los términos que se usan en la documentación.

## Conceptos de infraestructura
**Clúster**: conjunto de máquinas físicas o virtuales conectadas a la red en las que se implementan y administran los microservicios. Los clústeres pueden escalar a miles de equipos.

**Nodo**: cada una de las máquinas físicas o virtuales que forman parte de un clúster se denominan "nodo". A cada nodo se le asigna un nombre de nodo (una cadena). Los nodos tienen características como las propiedades de colocación. Cada máquina física o virtual tiene un servicio de Windows de inicio automático, `FabricHost.exe`, que empieza a ejecutarse en el arranque y luego inicia dos ejecutables: `Fabric.exe` y `FabricGateway.exe`. Estos dos ejecutables conforman el nodo. En los escenarios de prueba, se pueden hospedar varios nodos en un solo equipo y máquina virtual mediante la ejecución de varias instancias de `Fabric.exe` y `FabricGateway.exe`.

## Conceptos de las aplicaciones
**Tipo de aplicación**: el nombre o versión asignados a una colección de tipos de servicios. Esta información se define en un archivo `ApplicationManifest.xml`, insertado en un directorio del paquete de aplicación, que luego se copia en el almacén de imágenes del clúster de Service Fabric. Después puede crear una aplicación con nombre de este tipo de aplicación en el clúster.

Para más información, consulte el artículo [Modelar una aplicación en Service Fabric](service-fabric-application-model.md).

**Paquete de aplicación**: un directorio del disco que contiene el archivo `ApplicationManifest.xml` del tipo de aplicación. Hace referencia a los paquetes de servicios de cada uno de los tipos de servicios que conforman el tipo de aplicación. Los archivos en el directorio del paquete de aplicación se copian en el almacén de imágenes del clúster de Service Fabric. Por ejemplo, un paquete de aplicación de un tipo de aplicación de correo electrónico puede contener referencias a un paquete de servicios de cola, un paquete de servicios de front-end y un paquete de servicios de base de datos.

**Aplicación con nombre**: una vez que un paquete de aplicación se copia en el almacén de imágenes, crea una instancia de la aplicación en el clúster especificando el tipo de aplicación del paquete de aplicación (mediante su nombre o versión). A cada instancia del tipo de aplicación se le asigna un nombre de URI similar al siguiente: `"fabric:/MyNamedApp"`. En un clúster se pueden crear varias aplicaciones con nombre de un único tipo de aplicación. También se pueden crear aplicaciones con nombre de diferentes tipos de aplicación. Cada aplicación con nombre se administra de manera independiente y tiene versiones independientes.

**Tipo de servicio**: el nombre o versión asignados a los paquetes de código, paquetes de datos y paquetes de configuración de un servicio. Se define en un archivo `ServiceManifest.xml`, insertado en un directorio del paquete de servicio, al que después hace referencia un archivo `ApplicationManifest.xml` del paquete de aplicación. En el clúster, después de crear una aplicación con nombre se pude crear un servicio con nombre desde uno de los tipos de servicio del tipo de aplicación. El archivo `ServiceManifest.xml` del tipo de servicio describe el servicio.

Para más información, consulte el artículo [Modelar una aplicación en Service Fabric](service-fabric-application-model.md).

Existen dos tipos de servicios:

* **Sin estado:** los servicios sin estado se usan cuando el estado persistente del servicio se almacena en un servicio de almacenamiento externo como Almacenamiento de Azure, Base de datos SQL de Azure o Azure DocumentDB. Utilice un servicio sin estado en el caso de que el servicio no tenga almacenamiento persistente. Por ejemplo, un servicio de calculadora en el que se pasan valores al servicio, se realizan cálculos con dichos valores y se devuelve un resultado.
* **Con estado:** los servicios con estado se usan se desea que Service Fabric administre el estado del servicio a través de sus modelos de programación Reliable Collections o Reliable Actors. Especifique el número de particiones por las que desea distribuir el estado (a efectos de escalabilidad) al crear un servicio con nombre. Especifique también cuántas veces debe replicarse su estado en todos los nodos (para lograr una mayor confiabilidad). Todos los servicios con nombre tienen una única réplica principal y varias réplicas secundarias. Puede modificar el estado de su servicio con nombre escribiendo en la réplica principal. Tras ello, Service Fabric replica este estado en todas las réplicas secundarias, manteniendo el estado sincronizado. Service Fabric detecta automáticamente cuándo se produce un error de una réplica principal y, como respuesta, promueve una de las réplicas secundarias existentes para que ocupe el puesto de réplica principal. Seguidamente, Service Fabric crea una nueva réplica secundaria.

**Paquete de servicio**: un directorio del disco que contiene el archivo `ServiceManifest.xml` del tipo de servicio. Este archivo hace referencia al código, a los datos estáticos y a los paquetes de configuración del tipo de servicio. El archivo `ApplicationManifest.xml` del tipo de aplicación hace referencia a los archivos del directorio del paquete de servicio. Por ejemplo, un paquete de servicio puede hacer referencia al código, a los datos estáticos y a los paquetes de configuración que conforman un servicio de base de datos.

**Servicio con nombre**: después de crear una aplicación con nombre puede crear una instancia de uno de sus tipos de servicio en el clúster mediante la especificación del tipo de servicio (con su nombre o versión). A cada instancia del tipo de servicio se le asigna un nombre de URI cuyo ámbito es el URI de su aplicación con nombre. Por ejemplo, si crea un servicio con nombre "MyDatabase" en la aplicación con nombre "MyNamedApp", el URI será similar al siguiente: `"fabric:/MyNamedApp/MyDatabase"`. En una aplicación con nombre, se pueden crear varios servicios con nombre. Todos los servicios con nombre pueden tener su propio esquema de partición y cuentas de instancia o réplica.

**Paquete de código**: un directorio del disco que contiene archivos ejecutables (normalmente EXE o DLL) del tipo de servicio. El archivo `ServiceManifest.xml` del tipo de servicio hace referencia a los archivos del directorio del paquete de código. Cuando se crea un servicio con nombre, el paquete de código se copia en los nodos seleccionados para la ejecución de dicho servicio con nombre. A continuación, el código comienza a ejecutarse. Hay dos tipos de ejecutables de paquetes de código:

* **Ejecutables de invitado**: ejecutables que se ejecutan tal cual en sistema operativo host (Windows o Linux). Es decir, estos ejecutables no se vinculan ni hacen referencia a los archivos de tiempo de ejecución de Service Fabric y, por consiguiente, no usan ninguno de los modelos de programación de Service Fabric. Estos ejecutables no pueden utilizar algunas características de Service Fabric, como el servicio de nomenclatura para la detección de puntos de conexión. Los ejecutables de invitado no pueden informar de métricas de carga específicas de cada instancia del servicio.
* **Ejecutables de host de servicios**: son ejecutables que utilizan los modelos de programación de Service Fabric mediante la vinculación a los archivos del sistema en tiempo de ejecución de Service Fabric, lo que habilita las características de Service Fabric. Por ejemplo, una instancia del servicio con nombre puede registrar puntos de conexión con en el servicio de nombres de Service Fabric y también puede notificar métricas de carga.

**Paquete de datos**: un directorio del disco que contiene los archivos de datos de solo lectura estáticos del tipo de servicio (normalmente archivos fotográficos, de sonido y de vídeo). El archivo `ServiceManifest.xml` del tipo de servicio hace referencia a los archivos del directorio del paquete de datos. Cuando se crea un servicio con nombre, el paquete de datos se copia en los nodos seleccionados para la ejecución de dicho servicio con nombre. El código empieza a ejecutarse, tras lo cual se habilita el acceso a los archivos de datos.

**Paquete de configuración**: un directorio del disco que contiene los archivos de configuración de solo lectura estáticos del tipo de servicio (normalmente archivos de texto). El archivo `ServiceManifest.xml` del tipo de servicio hace referencia a los archivos del directorio del paquete de configuración. Cuando se crea un servicio con nombre, los archivos del paquete de configuración se copian en los nodos seleccionados para la ejecución de dicho servicio con nombre. Seguidamente, el código empieza a ejecutarse, tras lo cual se habilita el acceso a los archivos de configuración.

**Esquema de partición**: al crear un servicio con nombre, especifique un esquema de partición. Los servicios con grandes cantidades de estado dividen los datos entre las particiones que los distribuyen entre los nodos del clúster. Esto permite el escalado del estado del servicio con nombre. Dentro de una partición, los servicios con nombre sin estado tienen instancias, mientras que los servicios con nombre con estado tienen réplicas. Normalmente, los servicios con nombre sin estado solo tienen una partición, ya que no tienen un estado interno. Las instancias de la partición se proporcionan para disponibilidad; si se produce un error en una instancia, otras instancias seguirán operando con normalidad y luego Service Fabric creará una nueva instancia. Los servicios con nombre con estado mantienen su estado dentro de las réplicas y cada partición tiene su propio conjunto de réplicas en los que todos los estados se mantienen sincronizados. Si se produce algún error en una réplica, Service Fabric creará una nueva réplica a partir de las réplicas existentes.

Para obtener más información, consulte el artículo [Partición de Reliable Services de Service Fabric](service-fabric-concepts-partitioning.md).

## Servicios del sistema
Hay servicios del sistema que se crean en cada clúster que proporcionan las funciones de la plataforma de Service Fabric.

**Servicio de nomenclatura**: servicio presente en cada clúster de Service Fabric, que resuelve los nombres de servicio como una ubicación del clúster. Usted administra las propiedades y los nombres de servicio de manera similar a un servicio de nombres de dominio (DNS) para el clúster. Los clientes pueden comunicarse de forma segura con cualquier nodo del clúster mediante el servicio de nomenclatura para resolver el nombre de un servicio y su ubicación. Los clientes obtienen la dirección IP real de la máquina y el puerto en el que se esté ejecutando. Puede desarrollar servicios y clientes capaces de resolver la ubicación de red actual aunque las aplicaciones se muevan dentro del clúster (por ejemplo, debido a errores, equilibrio de recursos o el ajuste del tamaño del clúster).

Lea [Conexión y comunicación con servicios en Service Fabric](service-fabric-connect-and-communicate-with-services.md) para obtener más información sobre las API de comunicación de clientes y servicios compatibles con el servicio de nomenclatura.

**Servicio de almacenamiento de imágenes**: cada clúster de Service Fabric tiene un servicio de almacenamiento de imágenes donde se guardan los paquetes de aplicación implementados y con versión. Copie un paquete de aplicación en el almacén de imágenes y luego registre el tipo de aplicación en dicho paquete. Después de aprovisionar el tipo de aplicación, cree una aplicación con nombre a partir de él. El registro de un tipo de aplicación del servicio de almacenamiento de imágenes se puede anular una vez eliminadas todas sus aplicaciones con nombre.

Para obtener más información sobre la implementación en el almacén de imágenes, consulte el artículo [Implementación de una aplicación](service-fabric-deploy-remove-applications.md).

## Modelos de programación integrados
Hay modelos de programación de .NET Framework disponibles para generar servicios de Service Fabric:

**Reliable Services**: una API para generar servicios con y sin estado. Los servicios con estado almacenan su estado en Reliable Collections (como un diccionario o una cola). También le permiten de conectar una amplia variedad de pilas de comunicación, como la API de web y Windows Communication Foundation (WCF).

**Reliable Actors**: una API para crear objetos con y sin estado a través del modelo de programación de actor virtual. Este modelo puede resultar útil cuando hay muchas unidades independientes de cálculo/estado. Dado que este modelo utiliza un modelo de subprocesos basado en turnos, es conveniente evitar usar código que llame a otros actores o servicios, ya que un actor individual no puede procesar otras solicitudes entrantes hasta que se completen todas sus solicitudes salientes.

Para obtener más información, consulte el artículo [Elección de un marco para su servicio](service-fabric-choose-framework.md).

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Pasos siguientes
Para más información acerca de Service Fabric:

* [Información general de Service Fabric](service-fabric-overview.md)
* [¿Por qué usar un enfoque de microservicios para crear aplicaciones?](service-fabric-overview-microservices.md)
* [Escenarios de aplicación](service-fabric-application-scenarios.md)

<!---HONumber=AcomDC_0831_2016-->