#### <a name="to-create-public-endpoints-on-the-virtual-device"></a>Para crear puntos de conexión públicos en el dispositivo virtual
1. Inicie sesión en el portal clásico de Azure.

* Haga clic en **Máquinas virtuales**y, a continuación, seleccione la máquina virtual que se utiliza como dispositivo virtual.
* Haga clic en **Extremos**. En la página **Extremos** aparecen todos los extremos para la máquina virtual.
* Haga clic en **Agregar**. Aparecerá el cuadro de diálogo **Add Endpoint** . Haga clic en la flecha para continuar.
* Para el **Nombre**, escriba el siguiente nombre para el punto de conexión: **WinRMHttps**.
* En **Protocolo**, especifique **TCP**.
* En **Puerto público**, escriba los números de puerto que desea usar para la conexión.
* Para el **puerto privado**, escriba **5986**.
* Haga clic en la marca de verificación para crear el extremo.

Una vez creado el punto de conexión, puede ver sus detalles para determinar la dirección IP virtual (VIP) pública. Anote esta dirección.



<!--HONumber=Nov16_HO2-->


