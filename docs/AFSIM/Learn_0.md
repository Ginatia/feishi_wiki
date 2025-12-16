# WSF

## 应用程序、仿真和场景对象及扩展

WSF 应用程序必须实例化三个基本对象才能执行 WSF 模拟：

- WsfApplication - WSF 应用程序的监控对象
- WsfScenario - 定义要模拟的对象的属性和初始条件。这通常由一个或多个文件填充。
- WsfSimulation （或其衍生版本之一）- 执行场景。

这些对象均允许定义扩展，从而扩展 WSF 的功能。它们包括：

- WsfApplicationExtension
- WsfScenarioExtension
- WsfSimulationExtension


## 仿真输入

场景对象（ WsfScenario ）通过文件或其他来源的输入进行填充。

WSF 实现了一个名为 UtInput 的输入流抽象概念。该类提供了许多从流中提取数据的方法。WSF 提供了 UtInput 的两个具体实现：

- UtInputFile - 从文本文件读取的输入流（类似于 C++ 的 'ifstream' 对象）。

- UtInputString - 从 C++ 字符串（例如 C++ 'strstream' 或 'stringstream' 对象）读取的输入流。

### 对象输入

WSF 中几乎所有主要对象都实现了该方法：

`bool ProcessInput(UtInput& aInput);`

通过此方法，进行对象属性的脚本输入。


## WSF 模拟的主要对象类

### Platforms  平台

平台代表汽车、卡车、飞机、卫星、人、建筑物等事物。它充当容器，其他物体都附着在其上。

WsfPlatform 是平台的基类。通常情况下，您无需继承此类，因为它实际上只是一个容器。平台的所有功能都由其组成部分定义。

### Movers 机动器

移动装置作为其所连接的平台的推进系统。

它负责维持平台的运动状态（例如：位置、姿态、速度、加速度）。

需要注意的是，平台本身并非“空中平台”、“地面平台”或“卫星”。

它之所以具备这些平台的属性，是因为它连接了相应类型的移动装置。

WsfMover 是所有移动器实现的基类。

您可以继承它来提供自己的移动器，也可以使用 WSF 提供的实现之一：

- WsfAirMover
- WsfGroundMover
- WsfSpaceMover


### Sensors  传感器

传感器对象为平台提供“感知”功能（可以把它们想象成平台的眼睛、耳朵和鼻子）。

WsfSensor 是所有传感器实现的基类。您可以继承它来提供自己的传感器实现，也可以使用 WSF 提供的实现：

- WsfPassiveSensor
- WsfRadarSensor

### Communications  通讯

通信对象为平台之间相互通信提供了机制。

WsfComm 是所有通信实现的基础类。您可以继承它来提供自己的实现，也可以使用 WSF 提供的实现之一：

- WsfCommRcvr
- WsfCommXmtr
- WsfRadioRcvr
- WsfRadioXmtr

### Processors  处理器

处理器为平台提供认知能力。这些能力可以代表由有机体（大脑）或无机体（计算机）方式提供的决策过程。处理器可以：

- 接收来自同一平台上另一个处理器的消息。
- 接收来自同一平台上的传感器的消息。
- 通过通信接收器接收来自其他平台的消息。
- 定期调用。
- 向同一平台上的其他处理器发送消息。
- 通过通信发射器向其他平台发送消息。
- 调用操作或更改平台或其组成部分的状态。


WsfProcessor 是所有处理器实现的基类。您可以继承它来提供自己的处理器，也可以使用 WSF 提供的实现之一：

- WsfDelayProcessor
- WsfMessageProcessor
- WsfTrackProcessor

### Signatures 特征

平台具有一个名为“特征”的“属性”，它表示平台对位于其一定角度的传感器的可见性 。

WsfSignature 对象是特征的容器。目前已实现以下几种特征：

- WsfRadarSig

作为特征实现的一部分，存在一个名为“特征状态”的属性。实际上，它允许根据平台的配置使用不同的特征表。


### Aerodynamics Properties  空气动力学特性

WsfAero 实现了空气动力学特性。未来可能会进行扩展。

### Fuel Consumption  燃油消耗

WsfFuel 采用了一种非常简单的燃油消耗模型。未来将会扩展该模型，提供更多选项。

### Zones  区域

区域是指可用于决策过程的多边形或圆形区域。

### Command Chain  指挥链

命令链代表了指挥官-对等节点-下级节点的结构，用于将消息定向到各个平台。

平台通过添加一个 WsfCommandChain 对象成为命令链的成员，该对象包含命令链的名称和发出命令的平台的名称。

根据平台的角色，它可以属于多个命令链。

## Platforms and Platform indices. 平台索引

平台在模拟过程中可能会被销毁。

因此，对象（例如模拟观察者、其他平台等）不应保留指向其他平台的指针，因为无法保证该平台始终存在。

应该保留的是“平台索引”。

当一个平台被添加到仿真中时，它会被分配一个唯一的平台索引，该索引在仿真期间不会被重新分配（注意：平台索引只有在调用 WsfSimulation::Initialize 函数时才会分配）。

可以通过以下方式检索活动平台的平台索引：

`unsigned int platformIndex = platformPtr->GetIndex();`

给定平台索引，可以通过以下方式检索关联平台的地址：

`WsfPlatform* WsfSimulation::GetPlatformByIndex(platformIndex)`


