---
Description: In this topic you'll use the USB Kernel-Mode Driver template provided with Microsoft Visual Studio Professional 2012 to write a simple kernel-mode driver framework (KMDF)-based client driver.
title: How to write your first USB client driver (KMDF)
---

# How to write your first USB client driver (KMDF)


In this topic you'll use the **USB Kernel-Mode Driver** template provided with Microsoft Visual Studio Professional 2012 to write a simple kernel-mode driver framework (KMDF)-based client driver. After building and installing the client driver, you'll view the client driver in **Device Manager** and view the driver output in a debugger.

For an explanation about the source code generated by the template, see [Understanding the KMDF template code for a USB client driver](understanding-the-kmdf-template-code-for-usb.md).

### Prerequisites

For developing, debugging, and installing a kernel-mode driver, you need two computers:

-   A host computer running Windows 7 or a later version of the Windows operating system. The host computer is your development environment, where you write and debug your driver.
-   A target computer running Windows Vista or a later version of Windows. The target computer has the kernel-mode driver that you want to debug.

Before you begin, make sure that you meet the following requirements:

**Software requirements**

-   Your host computer hosts your development environment and has Visual Studio Professional 2012.
-   Your host computer has the latest Windows Driver Kit (WDK) for Windows 8. The kit include headers, libraries, tools, documentation, and the debugging tools required to develop, build, and debug a KMDF driver. To get the latest version of the WDK, see [How to Get the WDK](http://www.microsoft.com/whdc/DevTools/WDK/WDKpkg.mspx).
-   Your host computer has the latest version of debugging tools for Windows. You can get the latest version from the WDK or you can [Download and Install Debugging Tools for Windows](http://msdn.microsoft.com/windows/hardware/gg463009.aspx).
-   Your target computer is running Windows Vista or a later version of Windows.
-   Your host and target computers are configured for kernel debugging. For more information, see [Setting Up a Network Connection in Visual Studio](https://msdn.microsoft.com/library/windows/hardware/hh439353).

**Hardware requirements**

Get a USB device for which you will be writing the client driver. In most cases, you are provided with a USB device and its hardware specification. The specification describes device capabilities and the supported vendor commands. Use the specification to determine the functionality of the USB driver and the related design decisions.

If you are new to USB driver development, use the OSR USB FX2 learning kit to study USB samples included with the WDK. You can get the learning kit from [OSR Online](http://www.osronline.com/). It contains the USB FX2 device and all the required hardware specifications to implement a client driver.

You can also get a Microsoft USB Test Tool (MUTT) devices. MUTT hardware can be purchased from [JJG Technologies](http://jjgtechnologies.com/mutt.md). The device does not have installed firmware installed. To install firmware, download the MUTT software package from [this Web site](http://msdn.microsoft.com/windows/hardware/jj590752) and run MUTTUtil.exe. For more information, see the documentation included with the package.

**Recommended reading**

-   [Concepts for All Driver Developers](https://msdn.microsoft.com/library/windows/hardware/ff554731)
-   [Device nodes and device stacks](https://msdn.microsoft.com/library/windows/hardware/ff554721)
-   [Getting started with Windows drivers](https://msdn.microsoft.com/library/windows/hardware/ff554690)
-   [Kernel-Mode Driver Framework](https://msdn.microsoft.com/library/windows/hardware/ff557565)
-   *Developing Drivers with Windows Driver Foundation*, written by Penny Orwick and Guy Smith. For more information, see [Developing Drivers with WDF](http://msdn.microsoft.com/windows/hardware/gg463318).

Instructions
------------

### <a href="" id="generate-the-kmdf-driver-code-by-using-the--visual-studio-professional-2012---usb-driver-template"></a>Step 1: Generate the KMDF driver code by using the Visual Studio Professional 2012 USB driver template

For instructions about generating KMDF driver code, see the steps in [Writing a KMDF driver based on a template](https://msdn.microsoft.com/library/windows/hardware/hh439654).

**For USB-specific code, select the following options in Visual Studio Professional 2012**

1.  In the **New Project** dialog box, in the left pane, select **USB.**
2.  In the middle pane, select **USB Kernel-Mode Driver**.

The following screen shot shows **New Project** dialog box for the **USB Kernel-Mode Driver** template.

![visual studio new project options](images/kmdf-tmpl.png)

This topic assumes that the name of the Visual Studio project is "MyUSBDriver\_". It contains the following files:

| Files                      | Description                                                                                                          |
|----------------------------|----------------------------------------------------------------------------------------------------------------------|
| Public.h                   | Provides common declarations shared by the client driver and user applications that communicate with the USB device. |
| *&lt;Project name&gt;*.inf | Contains information required to install the client driver on the target computer.                                   |
| Trace.h                    | Declares tracing functions and macros.                                                                               |
| Driver.h; Driver.c         | Declares and defines driver entry points and event callback routines.                                                |
| Device.h; Device.c         | Declares and defines event callback routine for the prepare-hardware event.                                          |
| Queue.h; Queue.c           | Declares and defines an event callback routine for the event raised by the framework's queue object.                 |

 

### <a href="" id="modify-the-inf-file-to-add-information-about-your-device"></a>Step 2: Modify the INF file to add information about your device

Before you build the driver, you must modify the template INF file with information about your device, specifically the hardware ID string.

In **Solution Explorer**, under **Driver Files**, double-click the INF file.

In the INF file you can provide information such as the manufacturer and provider name, the device setup class, and so on. One piece of information that you must provide is the hardware identifier of your device.

To provide the hardware ID string:

1.  Attach your USB device to your host computer and let Windows enumerate the device.
2.  Open **Device Manager** and open properties for your device.
3.  On the **Details** tab, select **Hardward Ids** under **Property.**

    The hardware ID for the device is displayed in the list box. Right-click and copy the hardware ID string.

4.  Replace USB\\VID\_vvvv&PID\_pppp in the following line with your hardware ID string.

    `[Standard.NT$ARCH$] %MyUSBDriver_.DeviceDesc%=MyUSBDriver__Device, USB\VID_vvvv&PID_pppp`

### <a href="" id="build-the-usb-client-driver-code"></a>Step 3: Build the USB client driver code

**To build your driver**

1.  Open the driver project or solution in Visual Studio Professional 2012
2.  Right-click the solution in the **Solution Explorer** and select **Configuration Manager**.
3.  From the **Configuration Manager**, select the **Active Solution Configuration** (for example, **Windows 8 Debug** or **Windows 8 Release**) and the **Active Solution Platform** (for example, Win32) that correspond to the type of build you're interested in.
4.  From the **Build** menu, click **Build Solution**.

For more information, see [Building a Driver](https://msdn.microsoft.com/windows-drivers/develop/building_a_driver).

### <a href="" id="configure-a-computer-for-testing-and-debugging"></a>Step 4: Configure a computer for testing and debugging

To test and debug a driver, you run the debugger on the host computer and the driver on the target computer. So far, you have used Visual Studio on the host computer to build a driver. Next you need to configure a target computer. To configure a target computer, follow the instructions in [Provision a computer for driver deployment and testing](https://msdn.microsoft.com/library/windows/hardware/dn745909).

### <a href="" id="enable-tracing-for-kernel-debugging"></a>Step 5: Enable tracing for kernel debugging

The template code contains several trace messages (TraceEvents) that can help you track function calls. All functions in the source code contain trace messages that mark the entry and exit of a routine. For errors, the trace message contains the error code and a meaningful string. Because WPP tracing is enabled for your driver project, the PDB symbol file created during the build process contains trace message formatting instructions. If you configure the host and target computers for WPP tracing, your driver can send trace messages to a file or the debugger.

**To configure your host computer for WPP tracing**

1.  Create trace message format (TMF) files by extracting trace message formatting instructions from the PDB symbol file.

    You can use Tracepdb.exe to create TMF files. The tool is located in the *&lt;install folder&gt;*Windows Kits\\8.0\\bin\\*&lt;architecture&gt;* folder of the WDK. The following command creates TMF files for the driver project.

    **tracepdb -f \[PDBFiles\] -p \[TMFDirectory\]**

    The **-f** option specifies the location and the name of the PDB symbol file. The **-p** option specifies the location for the TMF files that are created by Tracepdb. For more information, see [**Tracepdb Commands**](https://msdn.microsoft.com/library/windows/hardware/ff553043).

    At the specified location you'll see three files (one per .c file in the project). They are given GUID file names.

2.  In the debugger, type the following commands:
    1.  **.load Wmitrace**

        Loads the Wmitrace.dll extension.

    2.  **.chain**

        Verify that the debugger extension is loaded.

    3.  **!wmitrace.searchpath +***&lt;TMF file location&gt;*

        Add the location of the TMF files to the debugger extension's search path.

        The output resembles this:

        `Trace Format search path is: 'C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE;c:\drivers\tmf'`

**To configure your target computer for WPP tracing**

1.  Make sure you have the Tracelog tool on your target computer. The tool is located in the *&lt;install\_folder&gt;*Windows Kits\\8.0\\Tools\\*&lt;arch&gt;* folder of the WDK. For more information, see [**Tracelog Command Syntax**](https://msdn.microsoft.com/library/windows/hardware/ff553012).
2.  Open a **Command Window** and run as administrator.
3.  Type the following command:

    **tracelog -start MyTrace -guid \#c918ee71-68c7-4140-8f7d-c907abbcb05d -flag 0xFFFF -level 7-rt -kd**

    The command starts a trace session named MyTrace.

    The **guid** argument specifies the GUID of the trace provider, which is the client driver. You can get the GUID from Trace.h in the Visual Studio Professional 2012 project. As another option, you can type the following command and specify the GUID in a .guid file. The file contains the GUID in hyphen format:

    **tracelog -start MyTrace -guid c:\\drivers\\Provider.guid -flag 0xFFFF -level 7-rt -kd**

    You can stop the trace session by typing the following command:

    **tracelog -stop MyTrace**

### <a href="" id="deploy-the-driver-on-the-target-computer"></a>Step 6: Deploy the driver on the target computer

1.  In the **Solution Explorer** window, right click the *&lt;project name&gt;***Package** , and choose **Properties**.
2.  In the left pane, navigate to **Configuration Properties &gt; Driver Install &gt; Deployment**.
3.  Check Enable deployment, and check Import into driver store.
4.  For **Remote Computer Name**, specify the name of the target computer.
5.  Select **Install and Verify**.
6.  Click **Ok**.
7.  On the **Debug** menu, choose **Start Debugging**, or press **F5** on the keyboard.

**Note**  Do *not* specify the hardware ID of your device under **Hardware ID Driver Update**. The hardware ID must be specified only in your driver's information (INF) file.

 

For more information about deploying the driver to the target system in Visual Studio Professional 2012, see [Deploying a Driver to a Test Computer](https://msdn.microsoft.com/windows-drivers/develop/deploying_a_driver_to_a_test_computer).

You can also manually install the driver on the target computer by using Device Manager. If you want to install the driver from a command prompt, these utilities are available:

-   [PnPUtil](https://msdn.microsoft.com/library/windows/hardware/ff550419)

    This tool comes with the Windows. It is in Windows\\System32. You can use this utility to add the driver to the driver store.

    ``` syntax
    C:\>pnputil /a m:\MyDriver_.inf
    Microsoft PnP Utility

    Processing inf : MyDriver_.inf
    Driver package added successfully.
    Published name : oem22.inf
    ```

    For more information, see [PnPUtil Examples](https://msdn.microsoft.com/library/windows/hardware/ff550428).

-   [**DevCon Update**](https://msdn.microsoft.com/library/windows/hardware/ff544832)

    This tool comes with the WDK. You can use it to install and update drivers.

    ``` syntax
    devcon update c:\windows\inf\MyDriver_.inf USB\VID_0547&PID_1002\5&34B08D76&0&6
    ```

### <a href="" id="view-the-driver-in-device-manager"></a>Step 7: View the driver in Device Manager

1.  Enter the following command to open **Device Manager**:

    **devmgmt**

2.  Verify that **Device Manager** shows a node for the following node:

    **Samples**

    **MyUSBDriver\_Device**

### <a href="" id="view-the-output-in-the-debugger"></a>Step 8: View the output in the debugger

Visual Studio first displays progress in the **Output** window. Then it opens the **Debugger Immediate Window**. Verify that trace messages appear in the debugger on the host computer. The output should look like this, where "MyUSBDriver\_" is the name of the driver module:

```
[3]0004.0054::00/00/0000-00:00:00.000 [MyUSBDriver_]MyUSBDriver_EvtDriverContextCleanup Entry
[1]0004.0054::00/00/0000-00:00:00.000 [MyUSBDriver_]MyUSBDriver_EvtDriverDeviceAdd Entry
[1]0004.0054::00/00/0000-00:00:00.000 [MyUSBDriver_]MyUSBDriver_EvtDriverDeviceAdd Exit
[0]0004.0054::00/00/0000-00:00:00.000 [MyUSBDriver_]DriverEntry Entry
[0]0004.0054::00/00/0000-00:00:00.000 [MyUSBDriver_]DriverEntry Exit
```

## Related topics


[Understanding the KMDF template code for a USB client driver](understanding-the-kmdf-template-code-for-usb.md)

[Getting started with USB client driver development](getting-started-with-usb-client-driver-development.md)

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Busbcon\buses%5D:%20How%20to%20write%20your%20first%20USB%20client%20driver%20%28KMDF%29%20%20RELEASE:%20%281/26/2017%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")



