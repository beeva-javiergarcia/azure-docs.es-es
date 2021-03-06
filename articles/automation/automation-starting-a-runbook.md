---
title: Inicio de un runbook en Automatización de Azure | Microsoft Docs
description: Resume los distintos métodos que pueden usarse para iniciar un runbook de Automatización de Azure y proporciona detalles sobre cómo utilizar el Portal de Azure y Windows PowerShell.
services: automation
documentationcenter: ''
author: mgoedtel
manager: jwhit
editor: tysonn

ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/08/2016
ms.author: magoedte;bwren

---
# <a name="starting-a-runbook-in-azure-automation"></a>Inicio de un runbook en Automatización de Azure
La tabla siguiente le ayudará a determinar el método para iniciar el runbook de Automatización de Azure que sea más adecuado a su escenario concreto. Este artículo incluye detalles acerca de cómo iniciar un runbook con el portal de Azure y Windows PowerShell. En otra documentación a la que puede acceder desde los siguientes vínculos se proporcionan detalles sobre los otros métodos.

| **MÉTODO** | **CARACTERÍSTICAS** |
| --- | --- |
| [Portal de Azure](#starting-a-runbook-with-the-azure-portal) |<li>Método más sencillo con la interfaz de usuario interactiva.<br> <li>Forma para proporcionar valores de parámetro simples.<br> <li>Seguimiento sencillo del estado del trabajo.<br> <li>Acceso autenticado con inicio de sesión de Azure. |
| [Windows PowerShell](https://msdn.microsoft.com/library/dn690259.aspx) |<li>Permite llamar desde la línea de comandos con los cmdlets de Windows PowerShell.<br> <li>Puede incluirse en una solución automatizada con varios pasos.<br> <li>La solicitud se autentica con el certificado o el usuario de OAuth principal/servicio principal.<br> <li>Permite proporcionar valores de parámetro simples y complejos.<br> <li>Realizar el seguimiento del estado del trabajo.<br> <li>Responder a la necesidad del cliente para admitir los cmdlets de PowerShell. |
| [API de Automatización de Azure](https://msdn.microsoft.com/library/azure/mt662285.aspx) |<li>El método más flexible, pero también más complejo.<br> <li>Permite llamar desde cualquier código personalizado que pueda realizar solicitudes HTTP.<br> <li>La solicitud se autentica con el certificado o el usuario de OAuth principal/servicio principal.<br> <li>Permite proporcionar valores de parámetro simples y complejos.<br> <li>Realizar el seguimiento del estado del trabajo. |
| [Webhooks](automation-webhooks.md) |<li>Permite iniciar el runbook desde una única solicitud HTTP.<br> <li>Autenticado con el token de seguridad de la dirección URL.<br> <li>Cliente no puede invalidar los valores de parámetro especificados al crear webhook. El runbook puede definir un único parámetro que se rellena con los detalles de la solicitud HTTP.<br> <li>No tiene capacidad de realizar un seguimiento del estado del trabajo a través de la dirección URL de webhook. |
| [Respuesta a una alerta de Azure](../log-analytics/log-analytics-alerts.md) |<li>Permite iniciar un runbook en respuesta a una alerta de Azure.<br> <li>Permite configurar webhook para el runbook y vincular a la alerta.<br> <li>Autenticado con el token de seguridad de la dirección URL.<br> <li>Actualmente solo admite la alerta para métricas. |
| [Programación](automation-schedules.md) |<li>Permite iniciar automáticamente el runbook en una programación cada hora, diaria o semanal.<br> <li>Permite manipular la programación a través del portal de Azure, los cmdlets de PowerShell o la API de Azure.<br> <li>Permite proporcionar valores de parámetro que se usarán con la programación. |
| [Desde otro Runbook](automation-child-runbooks.md) |<li>Permite usar un runbook como una actividad en otro runbook.<br> <li>Es útil para la funcionalidad utilizada por varios runbooks.<br> <li>Permite proporcionar valores de parámetro a un runbook secundario y usar la salida de un runbook principal. |

La siguiente imagen ilustra el proceso paso a paso detallado en el ciclo de vida de un runbook. En ella figuran las distintas formas de iniciar un runbook en Automatización de Azure, los componentes requeridos para que Hybrid Runbook Worker ejecute runbooks de Automatización de Azure y las interacciones entre los distintos componentes. Para obtener información sobre cómo ejecutar runbooks de Automation en su centro de datos, consulte [Trabajos híbridos de runbook](automation-hybrid-runbook-worker.md)

![Arquitectura de runbook](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="starting-a-runbook-with-the-azure-portal"></a>Inicio de un runbook con el Portal de Azure
1. En el Portal de Azure, seleccione **Automatización** y, a continuación, haga clic en el nombre de una cuenta de Automatización.
2. Seleccione la pestaña **Runbooks** .
3. Seleccione un runbook y, a continuación, haga clic en **Iniciar**.
4. Si el runbook tiene parámetros, se le pedirá que proporcione los valores de cada parámetro con un cuadro de texto. Consulte [Parámetros de runbook](#Runbook-parameters) a continuación para obtener más información sobre los parámetros.
5. Seleccione **Ver trabajo** junto al mensaje del runbook **inicial** o seleccione la pestaña **Trabajos** para que el runbook vea el estado del trabajo de runbook.

## <a name="starting-a-runbook-with-the-azure-portal"></a>Inicio de un runbook con el Portal de Azure
1. En la cuenta de automatización, haga clic en la parte **Runbooks** para abrir la hoja **Runbooks**.
2. Haga clic en un runbook para abrir su hoja **Runbook** .
3. Haga clic en **Iniciar**.
4. Si el runbook no tiene parámetros, se le pedirá que confirme si desea iniciarlo. Si el runbook tiene parámetros, se abrirá la hoja **Iniciar Runbook** para que pueda proporcionar los valores de parámetros. Consulte [Parámetros de runbook](#Runbook-parameters) a continuación para obtener más información sobre los parámetros.
5. La hoja **Trabajo** se abre para que pueda seguir el estado del trabajo.

## <a name="starting-a-runbook-with-windows-powershell"></a>Inicio de un runbook con Windows PowerShell
Puede utilizar [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) para iniciar un runbook con Windows PowerShell. El código de ejemplo siguiente inicia un runbook llamado Test-Runbook.

```
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

Start-AzureRmAutomationRunbook devuelve un objeto de trabajo que puede usar para realizar un seguimiento de su estado una vez que se inicia el runbook. Después, puede utilizar este objeto de trabajo con [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) para determinar el estado del trabajo y con [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) para obtener su salida. El código de ejemplo siguiente inicia un runbook llamado Test-Runbook, espera hasta que se ha completado y después muestra la salida.

```
$runbookName = "Test-Runbook"
$ResourceGroup = "ResourceGroup01"
$AutomationAcct = "MyAutomationAccount"

$job = Start-AzureRmAutomationRunbook –AutomationAccountName $AutomationAcct -Name $runbookName -ResourceGroupName $ResourceGroup

$doLoop = $true
While ($doLoop) {
   $job = Get-AzureRmAutomationJob –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup
   $status = $job.Status
   $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzureRmAutomationJobOutput –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup –Stream Output
```

Si el runbook requiere parámetros, debe proporcionarlos como una [tabla hash](http://technet.microsoft.com/library/hh847780.aspx) donde la clave de la tabla hash coincide con el nombre del parámetro y el valor es el valor del parámetro. En el ejemplo siguiente se muestra cómo iniciar un runbook con dos parámetros de cadena denominados FirstName y LastName, un número entero denominado RepeatCount y un parámetro booleano denominado Show. Para obtener información adicional sobre los parámetros, consulte [Parámetros de runbook](#Runbook-parameters) a continuación.

```
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a>Parámetros de runbook
Cuando se inicia un runbook desde el Portal de Azure o Windows PowerShell, la instrucción se envía a través del servicio web de Automatización de Azure. Este servicio no admite parámetros con tipos de datos complejos. Si necesita proporcionar un valor para un parámetro complejo, se debe llamar en línea desde otro runbook, tal como se describe en [Runbooks secundarios en la Automatización de Azure](automation-child-runbooks.md).

El servicio web de Automatización de Azure proporciona una funcionalidad especial para parámetros mediante ciertos tipos de datos, tal como se describe en las secciones siguientes.

### <a name="named-values"></a>Valores con nombre
Si el parámetro tiene el tipo de datos [object], puede usar el siguiente formato JSON para enviar una lista de valores con nombre: *{"Nombre1":Valor1, "Nombre2":Valor2, "Nombre3":Valor3}*. Estos valores deben ser tipos simples. El runbook recibirá el parámetro como un [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) con propiedades que se corresponden a cada valor con nombre.

Considere el siguiente runbook de prueba que acepta un parámetro denominado user.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][object]$user
   )
    $userObject = $user | ConvertFrom-JSON
    if ($userObject.Show) {
        foreach ($i in 1..$userObject.RepeatCount) {
            $userObject.FirstName
            $userObject.LastName
        }
    }
}
```

El texto siguiente podría utilizarse para el parámetro de usuario.

```
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

Esta acción devuelve la siguiente salida:

```
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a>Matrices
Si el parámetro es una matriz como [array] o [string], puede usar entonces el siguiente formato JSON para enviarle una lista de valores: *[Value1,Value2,Value3]*. Estos valores deben ser tipos simples.

Considere el siguiente runbook de prueba que acepta un parámetro denominado *user*.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][array]$user
   )
    if ($user[3]) {
        foreach ($i in 1..$user[2]) {
            $ user[0]
            $ user[1]
        }
    }
}
```

El texto siguiente podría utilizarse para el parámetro de usuario.

```
["Joe","Smith",2,true]
```

Esta acción devuelve la siguiente salida:

```
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a>Credenciales
Si el parámetro es de tipo de datos **PSCredential**, puede proporcionar el nombre de un [recurso de credenciales](automation-credentials.md)de Automatización de Azure. El runbook recuperará la credencial con el nombre que especifique.

Considere el siguiente runbook de prueba que acepta un parámetro denominado credential.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

Podría utilizar el siguiente texto para el parámetro de usuario suponiendo que ha habido un recurso de credenciales denominado *My Credential*.

```
My Credential
```

Suponiendo que el nombre de usuario de la credencial era *jsmith*, esto da como resultado la salida siguiente.

```
jsmith
```

## <a name="next-steps"></a>Pasos siguientes
* La arquitectura de runbook de este artículo proporciona información general sobre la administración de recursos en Azure y entornos locales con Hybrid Runbook Worker.  Para obtener información sobre cómo ejecutar runbooks de Automation en su centro de datos, consulte [Trabajos híbridos de runbook](automation-hybrid-runbook-worker.md).
* Para aprender más sobre la creación runbooks modulares para usarse por otros runbooks para funciones específicas o comunes, consulte [Runbooks secundarios](automation-child-runbooks.md).

<!--HONumber=Oct16_HO2-->


