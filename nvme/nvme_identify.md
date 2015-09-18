
NVMe协议Identify详解
==============================


>摘要
>----
>

目录
----
*   [概览](#overview)
*   [Identify Structure](#id_struct)
    *   [Device Identify Structure](#dev_id)
    *   [Namespace Identify Structure](#ns_id)
*   [设备端处理](#overview)
*   [主机端处理](#overview)
***

<h2 id="overview">概览</h2>

<h2 id="id_struct">Identify Structure</h2>
<h3 id="dev_id">Identify Controller Data Structure</h3>


*   [VID](#id_vid)
*   [SSVID](#id_ssvid)
*   [SN](#id_sn)


|[字节] |O/M|缩写|解释|
|:------|---|----|:---|
|[01:00]|M|<h4 id="id_vid">VID</h4><br>|PCI Vendor ID设备厂商ID，此ID由PCI SIG组织分配|
|[03:02]|M|<h4 id="id_ssvid">SSVID</h4>|PCI Subsystem Vendor ID<br>ID由PCI SIG组织分配|
|[23:04]|M|<h4 id="id_sn">SN</h4><br>|Serail Number<br>序列号，由厂商定义的ASCII码字符串|
|[63:24]|M|MN|Model Number<br>型号。由厂商定义的ASCII码字符串|
|[71:64]|M|FR|Firmware Revision<br>固件版本号。当前使用的固件版本号（ASCII码字符串）|
|[72]|M|RAB|Recommended Arbitration Burst<br>推荐的突发仲裁长度,即控制器每次从SQ队列中取出最大命令个数|
|[75:73]|M|IEEE|IEEE OUI Identify<br>控制器厂商的OUI(Organization Unique Identifier,组织唯一标识符)|
|[76]|O|CMIC|Controller Multi-Path I/O and Namespace Sharing Capabilities<br>多路径IO和Namespace共享标志位.其中:<br>**bit[7:3]** : 保留<br>**bit[2]** : 置1表示控制器与一个SR-IOV虚拟功能绑定.清0表示控制器为一个PCI功能设备<br>**bit[1]** : 置1表示NVM子系统可能包含2个或多个控制器.清0表示NVM子系统只一个控制器<br>**bit[0]** : 置1表示NVM子系统可能包含2个或多个PCIE端口,清0表示NVM子系统只有一个PCIE端口|
|[77]|M|MDTS|Maxmun Data Transfer Size<br>主机与控制之间的最大数据传输长度。主机发送命令时，相关的数据长度不应超过此值，否则命令会被控制器中止，并返回错误状态`Invalid Field in Command`，此值存储的是以2为底的对数值，单位是内存页大小，0表示传输长度不受限制。其它值如n，则表示2^n个内存页大小的数据长度。<br>使用SGL位存储桶时，当SGL描述的是存储目标区时，其长度应计算在内，而作为描述源数据时则不用计算在内。|
|[79:78]|M|CNTLID|Controller ID<br>控制器ID。|
|[83:80]|M|VER|Version<br>NVME协议版本信息。1.2版及后续版本此字段值应为非零值。|
|[87:84]|M|RTD3R|RTD3 Resume Latency<br>从`Runtime D3`状态恢复需要的典型延迟时间，单位是毫秒(ms)，1.2版及后续版本此字段值应为非零值。|
|[91:88]|M|RTD3E|RTD3 Entry Latency<br>进入`Runtime D3`状态需要的典型延迟时间，单位是毫秒(ms)，1.2版及后续版本此字段值应为非零值。|
|[95:92]|M|OAES|Optional Asynchronous Event Supported<br>标示位，标志是否支持异步事件。只有被使能的异步事情，控制器才能发送异步事件。其中:<br>**bit[31:9]** : 保留<br>**bit[8]** : 置1表示支持`Namespace Attribute Changed`事件<br>**bit[7:0]** : 保留|
|[239:96]|||Reserved|
|[255:240]|||Refor to NVMe Management Interface Specification|
|[257:256]|M|OACS|Optional Admin Command Support<br>是否支持可选的管理命令，以下各位置1表示支持，清0表示不支持<br>**bit[15:14]** : 保留<br>**bit[3]** : `Namespace Management`和`Namespace Attachment`命令<br>**bit[2]** : `Firmware Commit`和`Firmware Image Download`命令<br>**bit[1]** : `Format NVM`<br>**bit[0]** : `Security Send`和`Security Receive`|
|[258]|M|ACL|Abort Command Limit<br>控制器一次可同时处理的`Abort`命令的最大值。此值为0's based值，推荐的最小值为4个`Abort`命令|
|[259]|M|AERL|Asynchronous Event Request Limit<br>控制器可同时处理下列命令的最大个数：`Asynchronous Event Request`命令。为0's based值，推荐的最小值为4条命令|
|[260]|M|FRMW|Firmware Updates<br>标识固件升级功能。<br>**bit[7:5]** : 保留<br>**bit[4]** : 置1表示支持不用reset即可激活固件<br>**bit[3:1]** : 支持的固件存储槽个数。最小值为1最大值为7<br>**bit[0]** : 置1表示第一个固件存储槽为只读。|
|[261]|M|LPA|Log Page Attributes<br>`Get Log Page`命令相关的可靠属性标志位。具体如下:<br>**bit[7:2]** : 保留<br>**bit[1]** : 置1支持`Command Effects log page`<br>**bit[0]** : 置1支持每个Namespace上的`SMART/Health Infomation Log Page`|
|[262]|M|ELPE|Error Log Page Entries<br>控制器中存储`Error Information log entry`的个数。0'based 值。|
|[263]|M|NPSS|Number of Power States Support<br>能支持多少种电源功耗状态。0'based 值。电源状态从0开始，顺序增加。控制器最少支持一个电源状态（状态0），最多可支持31个附加电源状态（总共32个状态）|
|[264]|M|AVSCC|Admin Vendor Specific Command Configuration<br>配置自定义管理命令。<br>**bit[7:0]** : 保留<br>**bit[0]** : 置1表示`Admin Vendor Specific Command`使用[规定的格式](#t1)。否则使用自定义格式。|
|[265]|O|APSTA|Autonomous Power State Transition Attributes<br>自动电源状态切换属性。<br>**bit[7:0]** : 保留<br>**bit[0]** : 置1表示支持自动电源状态切换|
|[267:266]|M|WCTEMP|Warning Composite Temperature Threshold<br>复合温度报警阈值。此字段为最小值，当前值可通过`SMART/Health Information log`获得，达到报警阈值时，控制器应立即采取补偿措施（降温或降低工作负荷），控制器应努力做到复合温度值始终低于此值。字段为0时表示没有报警阈值。1.2兼容或更高版本此字段应为非零值，推荐值为0x0157|
|[269:268]|M|CCTEMP|Critical Composite Temperature Threshold<br>复合温度临界阈值。超过此温度时，可能会发生数据丢失，设备自动关闭，性能严重下降或永久性损害，此时控制器应停止操作。0表示没有临界阈值。1.2兼容或更高版本此字段应为非零值|
|[271:270]|O|MTFA|Maximun Time for Firmware Activation<br>控制器停止处理命令激活固件映像所花费的时间，若控制器支持不用复位就可激活固件的功能，此字段有效。字段值以100ms为单位，0值则表示时间不确定。|
|[275:272]|O|HMPRE|Host Memory Buffer Preferred Size<br>`Host Memory Buffer`功能所需主机分配内存大小首选值。字段值以4KB为单位。此值应大于`HMMIN`，非零值表示支持`Host Memory Buffer`功能，否则不支持。|
|[279:276]|O|HMMIN|Host Memory Buffer Minimum Size<br>`Host Memory Buffer`功能所需主机分配内存大小最小值，字段值以4KB为单位。若为0表示可分配任意大小，最大为`HMPRE`|
|[295:280]|O|TNVMCAP|Total NVM Capacity<br>NVM子系统存储总长度，字节为单位。支持`Namespace Management`和`Namespace Attachment commands`时此字段也必须被支持。|
|[311:296]|O|UNVMCAP|Unallocated NVM Capacity<br>NVM子系统未被分配的存储总长度，字节为单位。支持`Namespace Management`和`Namespace Attachment commands`时此字段也必须被支持。|
|[315:312]|O|RPMBS|Replay Protected Memory Block Support<br>是否支持一个或多个`Replay Protected Memory Block`,具体定义如下:<br>**bit[31:24]** : 0's based 值，512B为单位的字段值。表示`Security Send`或`Security Receive`命令读写的大小。<br>**bit[23:16]** : 总长度。即每个RPMB的长度。单位为128KB，0's based值。 <br>**bit[15:06]** : 保留<br>**bit[05:03]** : 授权方式。目前只00b有效，为`HMAC SHA-256`，其它值保留<br>**bit[02:00]** : RPMB的个数。0值表示不支持`Replay Protected Memory Blocks`。若为非零值，控制器要同时支持命令`Security Send`和`Security Receive`|
|[511:316]|||Reserved|
|[512]|M|SQES|Submission Queue Entry Size<br>SQ Entry的大小。其中:<br>**bit[7:4]** : NVM命令支持的最大SQ Entry长度。此值为以2底的对数值，推荐值为6，即2^6=64个字节。控制器若需要实现专用的扩展功能，可设定一个更大的值。<br>**bit[3:0]** : NVM命令支持的最小SQ Entry长度，此值为以2底的对数值，设定值须为6，即2^6=64个字节。|
|[513]|M|CQES|Completion Queue Entry Size<br>CQ Entry的大小。其中:<br>**bit[7:4]** : NVM命令支持的最大CQ Entry长度。此值为以2底的对数值，推荐值为4，即2^4=16个字节。控制器若需要实现专用的扩展功能，可设定一个更大的值。<br>**bit[3:0]** : NVM命令支持的最小CQ Entry长度，此值为以2底的对数值，设定值须为4，即2^4=16个字节。|
|[515:514]|||Reserved||
|[519:516]|M|NN|Number of Namespace<br>Namespace个数。|
|[521:520]|M|ONCS|Optional NVM Command Support<br>标识是否支持可选的NVM命令。以下各BIT位，置1表示支持所列命令。<br>**bit[15:6]** : 保留<br>**bit[5]** : Resevation命令.`Reservation Report`, `Reservation Register`, `Reservation Acquire`,和`Reservation Release`<br>**bit[4]** : `Set Features`命令的`Save`字段。`Get Features`命令的`Select`字段<br>**bit[3]** : `Write Zeroes`命令<br>**bit[2]** : `Dataset Management`命令<br>**bit[1]** : `Write Uncorrectable`命令<br>**bit[0]** : `Compare`命令|
|[523:522]|M|FUSES|Fused Operation Support<br>是否支持Fused操作。<br>**bit[15:1]** : 保留<br>**bit[0]** : 值1表示控制器支持`Compare`和`Write`融合（fused）操作|
|[524]|M|FNA|Format NVM Attributes<br>格式化NVM相关的属性。其中<br>**bit[7:3]** : 保留<br>**bit[2]** : 置1表示支持加密擦除<br>**bit[1]** : 置1表示加密擦除会作用于所有Namespace，而对某一特定的Namespace的用户数据进行擦除则会影响到所有Namespace的数据<br>**bit[0]** : 置表示对任一Namespace进行格式化操作会对所有Namespace作格式化操作|
|[525]|M|VWC|Volatile Write Cache<br>是否实现有易失性写Cache。<br>**bit[7:1]** : 保留<br>**bit[0]** : 置1表示实现有`Volatile Write Cache`<br>`Set Features`命令可控制是否开启指定的`Volatile Write Cache`，主机可发送`Flush`命令将Cache内的数据Flush到介质。若未开启`Volatile Write Cache`，`Flush`命令会成功返回，但不会执行任何操作|
|[527:526]|M|AWUN|Atomic Write Unit Normal<br>所有Namespace空间下，能够保证的原子写操作的长度。0's based值，单位为逻辑块。若`Identify Namespace data structure`中NAWUN的值比此值大，则以NAWUN为准。若一条写命令的数据长度小于或等于`AWUN`的值，则可保证此命令的原子操作特性，否则不能保证。<br>`AWUN`=0xFFFF时，所有命令都具有操作原子性（因为命令的最大长度就是此值）。推荐的最小值为128KB，即`AWUN`=255(当LBA大小为512B时)。|
|[529:528]|M|AWUPF|Atomic Write Unit Power Fail<br>在掉电或发生错误时，能够保证的原子写操作的长度，作用域为所有NVM Namespace空间。若`Identify Namespace data structure`中NAWUPF的值比此值大，则以NAWUPF为准。|
|[530]|M|NVSCC|NVM Vendor Specific Command Configuration<br>控制NVM自定义命令的处理配置。<br>**bit[7:1]** : 保留<br>**bit[0]** : 置1则使用[规定的格式](#)，否则使用自定义格式。|
|[531]|||Reserved|
|[533:532]|O|ACWU|Atomic Compare & Write Unit<br>0's based值，单位为逻辑块。`Compare`和`Write`融合操作的情况下，能够保证的原子写操作的长度。若`Identify Namespace data structure`中NACWU的值比此值大，则以NACWU为准。支持`Compare`和`Write`融合操作时，此字段也必须被同时支持，否则此字段应置为0。|
|[535:534]|M||Reserved|
|[539:536]|O|SGLS|SGL Support<br>标志是否支持SGL，其中:<br>**bit[31:19]** : 保留<br>**bit[18]** : 置1表示数据或元数据SGL长度可以比总数据长，否则应等于总数据长度。<br>**bit[17]** : 置1表示支持字节对齐物理连续的缓冲区<br>**bit[16]** : 置1表示支持`SGL Bit Bucket`描述符<br>**bit[15:01]** : 保留<br>**bit[00]** : 置1表示NVM命令集支持使用SGL(包括`SGL Data Block`描述符，`SGL Segment`描述符和`SGL Last Segment`描述符)。|
|[703:540]|M||Reserved|
|[2047:704]|||Reserved|
|[2079:2048]|M|PSD0|Power State 0 Descriptor<br>电源状态0描述符，格式详见[Power State Descriptor](#)|
|[2111:2080]|O|PSD1|Power State 1 Descriptor|
|[2143:2112]|O|PSD2|Power State 2 Descriptor|
|...|...|...|...|
|[3071:3040]|O|PSD31|Power State 31 Descriptor|
|[4095:3072]|O|VS|Vendor Specific<br>自定义|

6.2 Power State Descriptor Data Structure 
----
|[比特位]|缩写|解释|
|:------|---|:---|
|[255:184]||Reserved|
|[183:182]|APS|Active Power Scale。<br>`Active Power`的粒度值，其中:<br>**00b** : 不使用 <br>**01b** : 0.0001W<br>**10b** : 0.01W<br>**11b** : 保留|
|[181:179]||Reserved|
|[178:176]|APW|Active Power Workload<br>本电源状态下，计算最大功耗所使用的负载值。|
|[175:160]|ACTP|Active Power<br>本电源状态下，负载为`APW`时，运行10s的平均功耗。`APS`*`ACTP`值即为单位为瓦的功耗值。`ACTP`=0时则报告无效|
|[159:152]||Reserved|
|[151:150]|IPS|Idle Power Scale<br>`Idle Power`的粒度值,定义与`APS`类似|
|[149:144]||Reserved|
|[143:128]|IDLP|Idle Power<br>本电源状态下，IDLE状态(无挂起的命令需处理，寄存器访问和后台进程的状态)时，运行30s的平均功耗。需在进入IDLE状态后10s开始测试。`IPS`*`IDLP`值即为单位为瓦的功耗值。`IDLP`=0时则报告无效|


<h3 id="ns_id">Namespace Identify Structure</h3>

`Identify`命令用于读取描述NVM子系统的相关信息。
`Identify`命令使用`PRP Entry 1`,`PRP Entry 2`和`Dword10`，其它未使用的字段保留。释义如下：

|字段    |位 |携带的信息|说明|
|:------|---|---------|:---|
|PRP 1|[63:00]|PRP Entry 1|第一个PRP Entry，指向缓冲区的起始地址。|
|PRP 2|[63:00]|PRP Entry 2|第二个PRP Entry，使用规则参考SQ Entry的定义|
|Dword10|[31:16]|Controller Identifier (CNTID)|控制器ID。有些`Identify`类型操作会用到此字段，若没用到则此字段清0。|
|Dword10|[15:08]|Reserved||
|Dword10|[07:00]|Controller or Namespace Structure (CNS)|此字段指定何种信息返回给主机|
不同的CNS(Controller or Namespace Structure)值，控制器会返回不同的信息，定义如下：
|CNS值|返回的信息类型|
|---|--|
|00h|返回CDW1.NSID指定的Namespace的`Identify Namespace data structure`。若指定的Namespace未被激活，则返回数据全部清0。<br>若控制器支持`Namespace Management`，当CDW1.NSID值为0xFFFFFFFF时，控制器返回的`Identify Namespace data structure`里只包含所有Namespace共有的功能。|
|01h|返回`Identify Controller data structure`|
|02h|返回Namespace ID的列表(最多1024个)。这些ID对应的Namespace需具有以下特点:(1)已经激活并附加到控制器(2)ID值大于CDW1.NSID|
|03h-0Fh|保留|
|10h|返回Namespace ID的列表(最多1024个)。这些ID对应的Namespace需具有以下特点:(1)出现在控制器里，可能被附加也可能没有(2)ID值大于CDW1.NSID|
|11h|返回CDW1.NSID指定的Namespace的`Identify Namespace data structure`。若Namespace不存在则返回错误状态`Invalid Namespace or Format`|
|12h|返回控制器ID的列表(最多2047个)，列表中的ID大于或等于CDW10.CNTID，列表中列出的控制器应已经附加了CDW1.NSID指定的Namespace|
|13h|返回控制器ID的列表(最多2047个)，列表中的ID大于或等于CDW10.CNTID|
|14h-1Fh|保留|
|20h-FFh|保留|


<h3 id="id_sample">举例</h3>
<h4 id="id_sample">QEMU</h4>
<h4 id="id_sample">Intel 750</h4>


<h2 id="">设备端处理</h2>


Intel P

<h2 id="">主机端处理</h2>



www.qemu.org

www.nvmexpress.org