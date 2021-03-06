===============
Dev Cube 简介
===============

Dev Cube 是博流提供的芯片集成开发工具，包含IOT程序下载、MCU程序下载和RF性能测试三种功能。本文档主要介绍IOT和MCU程序下载相关配置，RF性能测试请参考《射频性能测试使用手册》。

Dev Cube 提供用户下载程序的功能，并且支持时钟、flash等参数配置，用户可根据自身需求决定是否对程序进行加密、添加签名、更换程序启动时的信息文件、用户资源文件、分区表等功能配置。

具体的功能如下：

1. 支持IOT应用程序和MCU应用程序的下载

2. 支持多种型号Flash 的擦、写、读；

3. 可将各类文件下载到Flash并验证、回读；

4. 下载通讯接口支持 UART 和 JLink 两种方式。

用户可以通过 \ `Bouffalo Lab Dev Cube <https://dev.bouffalolab.com/download>`__，获取最新版本的Dev Cube。
双击解压后文件夹中的\ ``BLDevCube.exe``\，在\ ``Chip Selection``\对话框中选择对应的芯片型号，点击\ ``Finish``\进入Dev Cube主界面。

.. figure:: /bl602doc_tool/content/picture/chipselection.png
   :align: center

   芯片选择

==========
镜像组成
==========
无论是IOT程序下载还是MCU程序下载，它们的镜像组成是相同的，都如下图所示：

.. figure:: /bl602doc_tool/content/picture/tool2.png
   :align: center

   下载内容布局

如果只下载应用程序无法使芯片正常工作，必须要将引导信息下载到指定位置。引导信息包含对PLL、Boot、Flash等的配置；固件是用户自己编写的应用程序。

以单核下载为例：根据需求选择对应的参数,将对PLL、Flash等配置的信息烧录到Bootinfo Addr对应的地址中，将应用程序经过编译后的bin文件烧录到Image Addr对应的地址中。

=====================
MCU程序下载
=====================
在\ ``View``\菜单中选择MCU选项，会进入MCU程序下载界面，主要分为程序下载方式的配置、镜像参数的配置和高级镜像参数的配置。

配置程序下载方式
====================

- 配置参数包括：

   * Interface：用于选择烧录的通信接口，这里选择 Uart 进行下载
   * COM Port：当选择 UART 进行下载的时候这里选择与芯片连接的 COM 口号，可以点击 Refresh 按钮进行 COM 号的刷新
   * Uart Speed：当选择 UART 进行下载的时候，填写波特率，推荐下载频率2MHz,不宜过高
   * Chip Erase：默认设置为False，即下载时不擦除Flash
   * Xtal：用于选择板子所使用的晶振类型。

.. figure:: /bl602doc_tool/content/picture/tool5.png
   :align: center

   MCU程序下载方式选择界面

配置镜像参数
==================

- 配置参数包括：

   * Boot Source：默认为Flash
   * BootInfo Addr：Flash程序启动参数的存放地址，填写0x0
   * Image Type：默认为SingleCPU
   * Image Addr：应用程序的存放地址，建议填写0x2000或者0x2000以后的地址
   * Image File：将编译生成的bin文件路径添加到Image File 中

.. figure:: /bl602doc_tool/content/picture/tool9.png
   :align: center
   
   镜像参数选择界面

配置高级镜像参数
======================

- 当点击\ ``click here to show advanced options``\时，会展开高级镜像配置，可配置的参数包括：

   * Flash Clock：默认为XTAL
   * PLL : PLL时钟配置，默认为160M
   * CacheWayDis : 缓冲通道失能，默认为none
   * Sign : 选择是否需要ECC校验，默认为none
   * CrcIgnore : 是否需要CRC校验。当参数选择False时需要做CRC校验；参数选择True时不需要做CRC校验
   * HashIgnore : 是否需要做Hash校验。当参数选择False时需要做Hash校验；参数选择True时不需要做Hash校验
   * Encrypt : 选择加密方式，并根据AES加密方式在AES Key 和AES IV中输入对应的值。

.. figure:: /bl602doc_tool/content/picture/tool10.png
   :align: center

   高级镜像参数选择界面

下载程序
===========

- 将板子的BOOT引脚保持高电平，并且使得芯片复位，使其处于UART引导下载的状态。点击\ ``Creat&Program``\，会自动生成应用程序镜像和启动参数配置文件，出现下图log信息，程序下载成功

.. figure:: /bl602doc_tool/content/picture/tool6.png
   :align: center

   下载程序

.. note::
    若没有连接板子，只需生成应用程序镜像和启动参数配置文件，也是点击\ ``Creat&Program``\按钮

- 下载成功后，将板子的BOOT引脚保持低电平，并且使得芯片复位，使其从Flash启动。此例子以2M波特率向PC端发送字符串报文

.. figure:: /bl602doc_tool/content/picture/tool7.png
   :align: center

   log信息

=============
IOT程序下载
=============
在\ ``View``\菜单中选择IOT选项，会进入IOT程序下载界面，主要分为程序下载方式的配置和下载参数的配置。

配置程序下载方式
====================

- 配置参数包括：

   * Interface：用于选择烧录的通信接口，这里选择 Uart 进行下载
   * COM Port: 当选择 UART 进行下载的时候这里选择与芯片连接的 COM 口号，可以点击 Refresh 按钮进行 COM 号的刷新
   * Uart Rate：当选择 UART 进行下载的时候，填写波特率，推荐下载频率2MHz,不宜过高
   * Board：选择所使用的板子型号，这里选择 IoTKitA，当板子选定后，Xtal 会自动更新成与板子匹配的默认值，当然用户也是可以再次更改的
   * Xtal:选择下载时的晶振频率，如果电路板没有焊接晶振，此处应当选内部RC32M时钟源
   * Chip Erase 默认设置为False，即下载时不擦除Flash

.. figure:: /bl602doc_tool/content/picture/iot1.png
   :align: center
   
   IOT程序下载方式选择界面

配置下载参数
==================

- 配置参数包括：

   * Partition Table：使用Dev Cube目录下对应芯片型号 partition 文件夹中的分区表，默认选择2M的文件
   * Boot2 Bin:它是系统启动后运行的第一个Flash程序，负责建立BLSP安全环境，并引导主程序运行，使用Dev Cube目录下对应芯片型号 builtin_imgs 文件夹中的 Boot2 文件
   * Firmware Bin：用户编译生成的bin文件路径
   * Media/Romfs：Media和Romfs二选一，如果勾选 Media，选择的是文件，如果勾选 Romfs，则选择的是文件夹
   * MFG Bin：选择MFG文件
   * AES-Encrypt：如果使用加密功能，需要将AES-Encrypt选项选中，并在旁边的文本框中输入加密所使用的Key和IV。输入的是十六进制对应的“0”~“F”，一个Byte由两个字符构成，所以Key和IV分别要求输入32个字符。需要注意的是IV的最后8个字符（即4Bytes）必须全为0
   * Single Download Config：勾选Enable后可下载单个文件，在左侧文本框填写下载的起始地址，以0x打头

.. figure:: /bl602doc_tool/content/picture/iot2.png
   :align: center

   下载参数选择界面

下载程序
===========

- 将板子的BOOT引脚保持高电平，并且使得芯片复位，使其处于UART引导下载的状态。点击\ ``Creat&Download``\，会自动生成应用程序镜像和启动参数配置文件，出现下图log信息，程序下载成功

.. figure:: /bl602doc_tool/content/picture/iot3.png
   :align: center

   IOT下载程序

.. note::
    若没有连接板子，只需生成应用程序镜像和启动参数配置文件，也是点击\ ``Creat&Program``\按钮

- 下载成功后，将板子的BOOT引脚保持低电平，并且使得芯片复位，使其从Flash启动。此例子以2M波特率向PC端发送字符串报文

.. figure:: /bl602doc_tool/content/picture/tool7.png
   :align: center

   log信息