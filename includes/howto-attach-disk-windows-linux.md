


## Conectar un disco vacío
El acoplamiento de un disco vacío supone un método sencillo de agregar un disco de datos, porque Azure crea el archivo .vhd y lo guarda en la cuenta de almacenamiento.

1. Haga clic en **Máquinas virtuales** y, a continuación, seleccione la VM correspondiente.
2. En la barra de comandos, haga clic en **Conectar** y luego en **Conectar disco vacío**.

    ![Conectar un disco vacío](./media/howto-attach-disk-windows-linux/AttachEmptyDisk.png)

1. Aparecerá el cuadro de diálogo **Conectar un disco vacío**.

    ![Conectar un nuevo disco vacío](./media/howto-attach-disk-windows-linux/AttachEmptyDetail.png)


    Haga lo siguiente:

    - En **Nombre de archivo**, acepte el nombre predeterminado o escriba otro para el archivo .vhd. El disco de datos usa un nombre generado automáticamente, incluso si escribe otro nombre para el archivo .vhd

    - Escriba el **Tamaño (GB)** del disco de datos.

    - Haga clic en la marca de verificación para continuar.

1. Una vez creado y conectado el disco de datos, este aparece en el panel de la VM.
   
   ![Disco de datos vacío conectado correctamente](./media/howto-attach-disk-windows-linux/AttachEmptySuccess.png)

> [!NOTE]
> Después de agregar un nuevo disco de datos, debe iniciar sesión en la VM e inicializar el disco para que se pueda usar.
> 
> 

## Acoplamiento de un disco existente
El acoplamiento de un disco existente requiere que disponga de un .vhd disponible en la cuenta de almacenamiento. Use el cmdlet [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) para cargar el archivo .vhd a la cuenta de almacenamiento. Una vez que se haya creado y cargado el archivo .vhd, puede acoplarlo a una VM.

1. Haga clic en **Máquinas virtuales** y, a continuación, seleccione la máquina virtual correspondiente.
2. En la barra de comandos, haga clic en **Conectar** y luego elija **Conectar disco**.

    ![Acoplar disco de datos](./media/howto-attach-disk-windows-linux/AttachExistingDisk.png)


1. Seleccione el disco de datos y haga clic en la marca de verificación para acoplarlo.
   
    ![Especificar la información del disco de datos](./media/howto-attach-disk-windows-linux/AttachExistingDetail.png)
2. Una vez conectado el disco de datos, este aparece en el panel de la VM.

    ![Disco de datos conectado correctamente](./media/howto-attach-disk-windows-linux/AttachExistingSuccess.png)

<!---HONumber=AcomDC_0525_2016-->