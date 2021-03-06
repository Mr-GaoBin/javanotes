# IT系统对接方案汇总


IT系统对接是很多企业、项目必须面对的问题；通常，多个系统之间如果完全企业自主定制开发，且有源代码、服务器的所有权，可以选择数据库直传的方式，方便快捷。如果系统之间存在权限限制或技术限制，可采用接口以保证数据的安全和对接的规范性等等，不同的场景下有不同的对接方案，以下对常用的对接方案做出汇总。

技术方案
接口
接口对接方式是比较常用，且安全规范的传输方式，一般需要根据详细需求开发定制接口，以满足系统间信息的对接。
一、传统WebService与Restful
接口一般可分为两种方式实现，一是传统web service接口，二是restful 风格的web service接口，二者区别主要由以下几点：
1. Restful Web Service的开发是面向资源的，而WebService则是面向方法。
2. Restful Web Service以Http协议作为应用协议，对资源的操作基于Http协议的几个关键方法“Get，Post，Put，Delete（204），Head，Patch，Options”，而Web Service则将方法信息封装在SOAP信封里经由Http的Post方法发送给服务端。这一区别的结果就是Restful Web Service利于缓冲（符合Web方式，利于GET缓冲），而Web Service在缓冲方面则表现出了极大的短板，因为缓冲服务器根本不知道SOAP里边的方法是不是Get，以及真实的请求资源是什么。
3.有关安全控制方面，对于基于代理服务器实现的安全控制，一般代理服务器是根据URL以及请求方法来确定该用户是否拥有相关操作权限的，很明显Restful Web Service贴近Web方式满足要求，而基于SOAP的Web Service实际的方法信息无从知晓，不具备实现安全控制的条件。
总结：WebService比较成熟，在涉及到复杂的业务逻辑，事务例如转账，用户等级划分等业务逻辑的处理上要优于Restful Service。而Restful Web Service由于是无状态的，在构建分布式应用的时候不用考虑用户Session，所以在构建分布式应用时灵活度更高，但在涉及到授权方面则略逊于前者（借助OAuth实现授权）。此外由于Restful Web Service以Http为应用协议其资源状态的转变方法有限（Http的七种方法），如果需要其他的方法只能借助已经实现方法扩展的第三方框架实现复杂操作而Web Service则可以定义自己的方法。总体来看Restful Web Service更易于构建简单的基于资源的分布式应用，而Web Service则适用于业务逻辑复杂，对系统安全性要求更高的大型企业级应用构建。

二、接口设计-共通

如果采用SOA体系架构，通过服务总线技术实现数据交换以及实现各业务子系统间、外部业务系统之间的信息共享和集成，因此SOA体系标准可作为采用的接口核心标准。主要包括：
服务目录标准
服务目录API接口格式参考国家以及关于服务目录的元数据指导规范，对于W3C UDDI v2 API结构规范，采取UDDI v2 的API的模型，定义UDDI的查询和发布服务接口，定制基于Java和SOAP的访问接口。除了基于SOAP1.2的Web Service接口方式，对于基于消息的接口采用JMS或者MQ的方式。
交换标准
基于服务的交换，采用HTTP/HTTPS作为传输协议，而其消息体存放基于SOAP1.2协议的SOAP消息格式。SOAP的消息体包括服务数据以及服务操作，服务数据和服务操作采用WSDL进行描述。
Web服务标准
用WSDL描述业务服务，将WSDL发布到UDDI用以设计/创建服务，SOAP/HTTP服务遵循WS-I Basic Profile 1.0，利用J2EE Session EJBs 实现新的业务服务，根据需求提供SOAP/HTTP or JMS and RMI/IIOP接口。
业务流程标准
使用没有扩展的标准的BPEL4WS，对于业务流程以SOAP服务形式进行访问，业务流程之间的调用通过SOAP。
数据交换安全
与外部系统对接需考虑外部访问的安全性，通过IP白名单、SSL认证等方式保证集成互访的合法性与安全性。
数据交换标准
制定适合双方系统统一的数据交换数据标准，支持对增量的数据自动进行数据同步，避免人工重复录入的工作。
2.1接口规范性设计
系统平台中的接口众多，依赖关系复杂，通过接口交换的数据与接口调用必须遵循统一的接口模型进行设计。接口模型除了遵循工程统一的数据标准和接口规范标准，实现接口规范定义的功能外，需要从数据管理、完整性管理、接口安全、接口的访问效率、性能以及可扩展性多个方面设计接口规格。

1）接口定义约定

客户端与系统平台以及系统平台间的接口消息协议，本次以基于HTTP协议的REST风格接口实现为例，如下图-接口消息协议栈示意图：



系统在http协议中传输的应用数据采用具有自解释、自包含特征的JSON数据格式，通过配置数据对象的序列化和反序列化的实现组件来实现通信数据包的编码和解码。

在接口协议中，包含接口的版本信息，通过协议版本约束服务功能规范，支持服务平台间接口协作的升级和扩展。一个服务提供者可通过版本区别同时支持多个版本的客户端，从而使得组件服务的提供者和使用者根据实际的需要，独立演进，降低系统升级的复杂度，保证系统具备灵活的扩展和持续演进的能力。

2）业务消息约定

请求消息URI中的参数采用UTF-8编码并经过URLEncode编码。

请求接口URL格式：{http|https}://{host}:{port}/

{app name}/{business component name}/{action} ；其中：

协议：HTTP REST形式接口

host：应用支撑平台交互通信服务的IP地址或域名

port：应用支撑平台交互通信服务的端口

app name：应用支撑平台交互通信服务部署的应用名称

business component name：业务组件名称

action：业务操作请求的接口名称，接口名字可配置

应答的消息体采用JSON数据格式编码，字符编码采用UTF-8。

应答消息根节点为“response”，每个响应包含固定的两个属性节点：“status”和“message”。它们分别表示操作的返回值和返回消息描述，其他的同级子节点为业务返回对象属性，根据业务类型的不同，有不同的属性名称。

当客户端支持数据压缩传输时，需要在请求的消息头的“Accept-Encoding”字段中指定压缩方式(gzip)，如消息可以被压缩传输则平台将应答的数据报文进行压缩作为应答数据返回，Content-Length为压缩后的数据长度。详细参见HTTP/1.1 RFC2616。

应答消息根节点为“response”，每个响应包含固定的两个属性节点：“status”和“message”。它们分别表示操作的返回值和返回消息描述，其他的同级子节点为业务返回对象属性，根据业务类型的不同，有不同的属性名称。

当客户端支持数据压缩传输时，需要在请求的消息头的“Accept-Encoding”字段中指定压缩方式(gzip)，如消息可以被压缩传输则平台将应答的数据报文进行压缩作为应答数据返回，Content-Length为压缩后的数据长度。详细参见HTTP/1.1 RFC2616。

3）响应码规则约定

响应结果码在响应消息的“status”属性中，相应的解释信息在响应消息的“message”属性中。解释消息为终端用户可读的消息，终端应用不需要解析可直接呈现给最终用户。响应结果码为6位数字串。根据响应类型，包括以下几类响应码。如下表中的定义。

响应码

描述

0

成功

1XXXXX

系统错误

2XXXXX

输入参数不合法错误

3XXXXX

应用级返回码，定义应用级的异常返回。

4XXXXX

正常的应用级返回码，定义特定场景的应用级返回说明。

4）数据管理

①业务数据检查

接口应提供业务数据检查功能，即对接收的数据进行合法性检查，对非法数据和错误数据则拒绝接收，以防止外来数据非法入侵，减轻应用支撑平台系统主机处理负荷。

对于接口，其业务数据检查的主要内容有以下几个方面：

•数据格式的合法性：如接收到非预期格式的数据。包括接收的数据长度，类型，开始结束标志等。

•数据来源的合法性：如接收到非授权接口的数据。

•业务类型的合法性：如接收到接口指定业务类型外的接入请求。

对于业务数据检查中解析出非法数据应提供以下几种处理方式：

•事件报警：在出现异常情况时自动报警，以便系统管理员及时进行处理。

•分析原因：在出现异常情况时，可自动分析其出错原因。如是数据来源非法和业务类型非法，本地记录并做后续管理，如是数据格式非法，分析网络传输原因或对端数据处理原因，并做相应处理。

•统计分析：定期对所有的非法记录做统计分析，分析非法数据的各种来源是否具有恶意，并做相应处理。

②数据压缩/解压

接口根据具体的需求应提供数据压缩/解压功能，以减轻网络传输压力，提高传输效率，从而使整个系统能够快速响应并发请求，高效率运行。

在使用数据压缩/解压功能时，应具体分析每一类业务的传输过程、处理过程、传输的网络介质、处理的主机系统和该类业务的并发量、峰值及对于所有业务的比例关系等，从而确定该类业务是否需要压缩/解压处理。对于传输文件的业务，必须压缩后传输，以减轻网络压力，提高传输速度。

在接口中所使用的压缩工具必须基于通用无损压缩技术，压缩算法的模型和编码必须符合标准且高效，压缩算法的工具函数必须是面向流的函数，并且提供校验检查功能。

5）完整性管理

根据业务处理和接口服务的特点，应用系统的业务主要为实时请求业务和批量传输业务。两类业务的特点分别如下：

1.实时请求业务：

(1)采用基于事务处理机制实现

(2)业务传输以数据包的方式进行

(3)对传输和处理的实时性要求很高

(4)对数据的一致性和完整性有很高的要求

(5)应保证高效地处理大量并发的请求

2.批量传输业务：

(1)业务传输主要是数据文件的形式

(2)业务接收点可并发处理大量传输，可适应高峰期的传输和处理

(3)要求传输的可靠性高

根据上述特点，完整性管理对于实时交易业务，要保证交易的完整性；对于批量传输业务，要保证数据传输的完整性。

2.2接口双方责任

1）消息发送方
遵循本接口规范中规定的验证规则，对接口数据提供相关的验证功能，保证数据的完整性、准确性；
消息发起的平台支持超时重发机制，重发次数和重发间隔可配置。
提供接口元数据信息，包括接口数据结构、实体间依赖关系、计算关系、关联关系及接口数据传输过程中的各类管理规则等信息；
提供对敏感数据的加密功能；
及时解决接口数据提供过程中数据提供方一侧出现的问题；
2）消息响应方
遵循本接口规范中规定的验证规则，对接收的数据进行验证，保证数据的完整性、准确性。
及时按照消息发送方提供的变更说明进行本系统的相关改造。
及时响应并解决接口数据接收过程中出现的问题。
3）异常处理
对接口流程调用过程中发生的异常情况，如流程异常、数据异常、会话传输异常、重发异常等，进行相应的异常处理，包括：
·对产生异常的记录生成异常记录文件。
·针对可以回收处理的异常记录，进行自动或者人工的回收处理。
·记录有关异常事件的日志，包含异常类别、发生时间、异常描述等信息。
·当接口调用异常时，根据预先配置的规则进行相关异常处理，并进行自动告警。

2.3接口可扩展性规划与设计

各个系统间的通信接口版本信息限定了各个系统平台间交互的数据协议类型、特定版本发布的系统接口功能特征、特定功能的访问参数等接口规格。通过接口协议的版本划分，为客户端升级、其他被集成系统的升级、以及系统的部署提供了较高的自由度和灵活性。

系统可根据接口请求中包含的接口协议版本实现对接口的向下兼容。系统平台可根据系统的集群策略，按协议版本分别部署，也可多版本并存部署。由于系统平台可同时支持多版本的外部系统及客户端应用访问系统，特别是新版本客户端发布时，不要求用户强制升级，也可降低强制升级安装包发布的几率。从而支持系统的客户端与系统平台分离的持续演进。

2.4接口安全性设计

为了保证系统平台的安全运行，各种集成的外部系统都应该保证其接入的安全性。

接口的安全是平台系统安全的一个重要组成部分。保证接口的自身安全，通过接口实现技术上的安全控制，做到对安全事件的“可知、可控、可预测”，是实现系统安全的一个重要基础。

根据接口连接特点与业务特色，制定专门的安全技术实施策略，保证接口的数据传输和数据处理的安全性。

系统应在接口的接入点的网络边界实施接口安全控制。

接口的安全控制在逻辑上包括：安全评估、访问控制、入侵检测、口令认证、安全审计、防(毒)恶意代码、加密等内容。

1）安全评估

安全管理人员利用网络扫描器定期(每周)/不定期(当发现新的安全漏洞时)地进行接口的漏洞扫描与风险评估。扫描对象包括接口通信服务器本身以及与之关联的交换机、防火墙等，要求通过扫描器的扫描和评估，发现能被入侵者利用的网络漏洞，并给出检测到漏洞的全面信息，包括位置、详细描述和建议改进方案，以便及时完善安全策略，降低安全风险。

安全管理人员利用系统扫描器对接口通信服务器操作系统定期(每周)/不定期(当发现新的安全漏洞时)地进行安全漏洞扫描和风险评估。在接口通信服务器操作系统上，通过依附于服务器上的扫描器代理侦测服务器内部的漏洞，包括缺少安全补丁、词典中可猜中的口令、不适当的用户权限、不正确的系统登录权限、操作系统内部是否有黑客程序驻留，安全服务配置等。系统扫描器的应用除了实现操作系统级的安全扫描和风险评估之外还需要实现文件基线控制。

接口的配置文件包括接口服务间相互协调作业的配置文件、系统平台与接口对端系统之间协调作业的配置文件，对接口服务应用的配置文件进行严格控制，并且配置文件中不应出现口令明文，对系统权限配置限制到能满足要求的最小权限，关键配置文件加密保存。为了防止对配置文件的非法修改或删除，要求对配置文件进行文件级的基线控制。

2）访问控制

访问控制主要通过防火墙控制接口对端系统与应用支撑平台之间的相互访问，避免系统间非正常访问，保证接口交互信息的可用性、完整性和保密性。访问控制除了保证接口本身的安全之外，还进一步保证应用支撑平台的安全。

为了有效抵御威胁，应采用异构的双防火墙结构，提高对防火墙安全访问控制机制的破坏难度。双防火墙在选型上采用异构方式，即采用不同生产厂家不同品牌的完全异构防火墙。同时，双防火墙中的至少一个应具有与实时入侵检测系统可进行互动的能力。当发生攻击事件或不正当访问时，实时入侵检测系统检测到相关信息，及时通知防火墙，防火墙能够自动进行动态配置，在定义的时间段内自动阻断源地址的正常访问。

系统对接口被集成系统只开放应用定义的特定端口。

采用防火墙的地址翻译功能，隐藏系统内部网络，向代理系统提供翻译后的接口通信服务器地址及端口，禁止接口对端系统对其它地址及端口的访问。

对通过/未通过防火墙的所有访问记录日志。

3）入侵检测

接口安全机制应具有入侵检测(IDS)功能，实时监控可疑连接和非法访问等安全事件。一旦发现对网络或主机的入侵行为，应报警并采取相应安全措施，包括自动阻断通信连接或者执行用户自定义的安全策略。

实施基于网络和主机的入侵检测。检测攻击行为和非法访问行为，自动中断其连接，并通知防火墙在指定时间段内阻断源地址的访问，记录日志并按不同级别报警，对重要系统文件实施自动恢复策略。

4）口令认证

对于需经接口安全控制系统对相关集成系统进行业务操作的请求，实行一次性口令认证。

为保证接口的自身安全，对接口通信服务器和其它设备的操作和管理要求采用强口令的认证机制，即采用动态的口令认证机制。

5）安全审计

为了保证接口的安全，要求对接口通信服务器的系统日志、接口应用服务器的应用日志进行实时收集、整理和统计分析，采用不同的介质存档。

6）防恶意代码或病毒

由于Internet为客户提WEB服务，因此，对于Internet接口要在网络分界点建立一个功能强大的防恶意代码系统，该系统能实时地进行基于网络的恶意代码过滤。建立集中的防恶意代码系统控制管理中心。

7）加密

为了提高接口通信信息的保密性，同时保证应用支撑平台的安全性，可以对系统平台与接口集成系统间的相关通信实施链路加密、网络加密或应用加密，保证无关人员以及无关应用不能通过网络链路监听获得关键业务信息，充分保证业务信息的安全。

三、wsdl接口设计

WSDL是一个用于精确描述Web服务的文档，WSDL文档是一个遵循WSDL-XML模式的XML文档。WSDL 文档将Web服务定义为服务访问点或端口的集合。在 WSDL 中，由于服务访问点和消息的抽象定义已从具体的服务部署或数据格式绑定中分离出来，因此可以对抽象定义进行再次使用。消息，指对交换数据的抽象描述；而端口类型，指操作的抽象集合。用于特定端口类型的具体协议和数据格式规范构成了可以再次使用的绑定。将Web访问地址与可再次使用的绑定相关联，可以定义一个端口，而端口的集合则定义为服务。

一个WSDL文档通常包含8个重要的元素，即definitions、types、import、message、portType、operation、binding、service元素。这些元素嵌套在definitions元素中，definitions是WSDL文档的根元素。

WSDL文档外层结构图示：



WSDL 服务进行交互的基本元素：

Types（消息类型）：数据类型定义的容器，它使用某种类型系统（如 XSD）。

Message（消息）：通信数据的抽象类型化定义，它由一个或者多个 part 组成。

Part：消息参数

PortType（端口类型）：特定端口类型的具体协议和数据格式规范。，它由一个或者多个 Operation组成。

Operation（操作）：对服务所支持的操作进行抽象描述，WSDL定义了四种操作：

1.单向（one-way）：端点接受信息；

3.要求-响应（solicit-response）：端点发送消息，然后接受相关消息；

4.通知（notification[2] ）：端点发送消息。

Binding：特定端口类型的具体协议和数据格式规范。

Port：定义为绑定和网络地址组合的单个端点。

Service：相关端口的集合，包括其关联的接口、操作、消息等。

外层结构里面也可能有多层结构。

四、Restful风格接口设计

4.1协议

API与用户的通信协议，通常使用HTTP/HTTPs协议(特别是web)

4.2域名

应该尽量将API部署在专用域名之下。

https://api.example.com

如果确定API很简单，不会有进一步扩展，可以考虑放在主域名下。

https://example.org/api/

4.3版本

API的版本号放入URL

https://api.example.com/v1/

另一种设计方式将版本号放在HTTP头信息中，但不如放入URL方便和直观，Github采用这种做法。

4.4路径

路径又称"终点"（endpoint），表示API的具体网址。

在RESTful架构中，每个网址代表一种资源（resource），所以网址中不能有动词，只能有名词，而且所用的名词往往与数据库的表格名对应。一般来说，数据库中的表都是同种记录的"集合"（collection），所以API中的名词也应该使用复数。

举例来说，有一个API提供动物园（zoo）的信息，还包括各种动物和雇员的信息，则它的路径应该设计成下面这样。

https://api.example.com/v1/zoos
https://api.example.com/v1/animals
https://api.example.com/v1/employees
4.5HTTP动词

对于资源的具体操作类型，由HTTP动词表示，常用的HTTP动词有下面五个（括号里是对应的SQL命令）。

GET（SELECT）：从服务器取出资源（一项或多项）。
POST（CREATE）：在服务器新建一个资源。
PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
DELETE（DELETE）：从服务器删除资源。
两个不常用的HTTP动词：

HEAD：获取资源的元数据。
OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。
举例如下：

GET /zoos：列出所有动物园
POST /zoos：新建一个动物园
GET /zoos/ID：获取某个指定动物园的信息
PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）
PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）
DELETE /zoos/ID：删除某个动物园
GET /zoos/ID/animals：列出某个指定动物园的所有动物
DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物
4.6过滤信息

如果记录数量很多，服务器不可能都将它们返回给用户。API应该提供参数，过滤返回结果，常见的参数如下：

?limit=10：指定返回记录的数量
?offset=10：指定返回记录的开始位置。
?page=2&per_page=100：指定第几页，以及每页的记录数。
?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
?animal_type_id=1：指定筛选条件
参数设计允许存在冗余，即允许API路径和URL参数偶尔有重复。比如，GET /zoo/ID/animals 与 GET /animals?zoo_id=ID 的含义是相同的。

4.7状态码

服务器向用户返回的状态码和提示信息，常见的有以下一些（方括号中是该状态码对应的HTTP动词）。

 200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。

201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。

202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）

204 NO CONTENT - [DELETE]：用户删除数据成功。

400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。

401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。

403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。

404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。

406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。

410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。

422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。

500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。

4.8错误处理

如果状态码是4xx，就应该向用户返回出错信息。一般来说，返回的信息中将error作为键名，出错信息作为键值即可。

{

    error: "Invalid API key"

}

4.9返回结果

针对不同操作，服务器向用户返回的结果应该符合以下规范：

GET /collection：返回资源对象的列表（数组）

GET /collection/resource：返回单个资源对象

POST /collection：返回新生成的资源对象

PUT /collection/resource：返回完整的资源对象

PATCH /collection/resource：返回完整的资源对象

DELETE /collection/resource：返回一个空文档

4.10Hypermedia API

RESTful API最好做到Hypermedia，即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。

比如，当用户向api.example.com的根目录发出请求，会得到如下文档：

{"link": {

      "rel":   "collection https://www.example.com/zoos",
    
      "href":  "https://api.example.com/zoos",
    
      "title": "List of zoos",
    
      "type":  "application/vnd.yourformat+json"
    
    }}

文档中有一个link属性，用户读取这个属性就知道下一步该调用什么API了。rel表示这个API与当前网址的关系（collection关系，并给出该collection的网址），href表示API的路径，title表示API的标题，type表示返回类型。

Hypermedia API的设计被称为HATEOAS。Github的API采用次设计方式，访问api.github.com会得到一个所有可用API的网址列表。

    {
    
      "current_user_url": "https://api.github.com/user",
    
      "authorizations_url": "https://api.github.com/authorizations",
    
      // ...
    
    }

如果想获取当前用户的信息，应该去访问api.github.com/user，然后就得到了下面结果：

 

    {
    
      "message": "Requires authentication",
    
      "documentation_url": "https://developer.github.com/v3"
    
    }

上面代码表示，服务器给出了提示信息，以及文档的网址。

4.11其它

1）API的身份认证应该使用OAuth 2.0框架。

2）服务器返回的数据格式，应该尽量使用JSON，避免使用XML。

消息服务传输
Java消息服务（Java Message Service）是message数据传输的典型的实现方式。系统A和系统B通过一个消息服务器进行数据交换。系统A发送消息到消息服务器，如果系统B订 阅系统A发送过来的消息，消息服务器会消息推送给B。双方约定消息格式即可。目前市场上有很多开源的jms消息中间件，比如 kafka，ActiveMQ, OpenJMS。



一、消息服务设计

目前需要导入一个大文件到系统A，系统A保存文件信息，而文件里面的明细信息需要导入到系统B进行分析，当系统B分析完成之后，需要把分析结果通知系统A。



1）系统A先保存文件到文件服务器。

2）系统A 通过webservice 调用系统B提供的服务器，把需要处理的文件名发送到系统B。由于文件很大，需要处理很长时间，所以B不及时处理文件，而是保存需要处理的文件名到数据库，通过后台定时调度机制去处理。所以B接收请求成功，立刻返回系统A成功。

3）系统B定时查询数据库记录，通过记录查找文件路径，找到文件进行处理。这个过程很长。

4）系统B处理完成之后发送消息给系统A，告知系统A文件处理完成。

5）系统A 接收到系统B请求来的消息，进行展示任务结果。

二、消息服务优缺点

消息服务优点

1）由于jms定义了规范，有很多的开源的消息中间件可以选择，而且比较通用。接入起来相对也比较简单

2）通过消息方式比较灵活，可以采取同步，异步，可靠性的消息处理，消息中间件也可以独立出来部署。

消息服务缺点

1）学习jms相关的基础知识，消息中间件的具体配置，以及实现的细节对于开发人员来说还是有一点学习成本的

2）在大数据量的情况下，消息可能会产生积压，导致消息延迟，消息丢失，甚至消息中间件崩溃。

文件共享
对于大数据量的交互，采用文件的交互方式是最佳方式之一。

一、文件共享设计

系统A和系统B约定文件服务器地址，文件命名规则,文件内容格式等内容，通过上传文件到文件服务器进行数据交互。



最典型的应用场景是批量处理数据：例如系统A把今天12点之前把要处理的数据生成到一个文件，系统B第二天凌晨1点进行处理，处理完成之后，把处理 结果生成到一个文件，系统A 12点在进行结果处理。这种状况经常发生在A是事物处理型系统，对响应要求比较高，不适合做数据分析型的工作，而系统B是后台系统，对处理能力要求比较 高，适合做批量任务系统。

以上只是说明通过文件方式的数据交互，实际情况B完成任务之后，可能通过socket的方式通知A，不一定是通过文件方式。

二、文件共享优缺点

优点：

1 在数据量大的情况下，可以通过文件传输，不会超时，不占用网络带宽。

2 方案简单，避免了网络传输，网络协议相关的概念。

缺点：

1 不太适合做实时类的业务

2 必须有共同的文件服务器，文件服务器这里面存在风险。因为文件可能被篡改，删除，或者存在泄密等。

3 必须约定文件数据的格式，当改变文件格式的时候，需要各个系统都同步做修改。

socket传输
Socket方式是最简单的交互方式，适用于c/s 交互模式，一台客户机，一台服务器。

一、socket传输设计

服务器提供服务，通过ip地址和端口进行服务访问。而客户机通过连接服务器指定的端口进行消息交互。其中传输协议可以 是tcp/UDP 协议。而服务器和约定了请求报文格式和响应报文格式。如下图所示：



目前常用的http调用，java远程调用，webserivces 都是采用的这种方式，只不过不同的就是传输协议以及报文格式。

二、socket传输优缺点

优点：

1）易于编程，目前java提供了多种框架，屏蔽了底层通信细节以及数据传输转换细节。

2）容易控制权限。通过传输层协议https，加密传输的数据，使得安全性提高

3）通用性比较强，无论客户端是.net架构，java，python 都是可以的。尤其是webservice规范，使得服务变得通用

缺点：

1）服务器和客户端必须同时工作，当服务器端不可用的时候，整个数据交互是不可进行。

2）当传输数据量比较大的时候，严重占用网络带宽，可能导致连接超时。使得在数据量交互的时候，服务变的很不可靠。

数据库传输
一、数据库传输设计

系统A和系统B通过连接同一个数据库服务器的同一张表进行数据交换。当系统A请求系统B处理数据的时候，系统A Insert一条数据，系统B select 系统A插入的数据进行处理。



二、数据库传输优缺点

优点：

1）相比文件方式传输来说，因为使用的同一个数据库，交互更加简单。

2）由于数据库提供相当做的操作，比如更新，回滚等。交互方式比较灵活,而且通过数据库的事务机制，可以做成可靠性的数据交换。

缺点：

1）当连接B的系统越来越多的时候，由于数据库的连接池是有限的，导致每个系统分配到的连接不会很多，当系统越来越多的时候，可能导致无可用的数据库连接

2）一般情况，来自两个不同公司的系统，不太会开放自己的数据库给对方连接，因为这样会有安全性影响

数据爬取
数据爬取可根据不同环境做不同方案，C/S模式可用数据抓包工具进行抓包数据，B/S模式可定制开发网络爬虫实现数据爬取。获取到的数据传输到指定位置，再进行使用处理。不过爬取的数据获取方式比较“非主流”，并且存在安全问题及对服务器的压力，不建议使用此种方式。

原文链接：https://blog.csdn.net/qq_36632174/article/details/113735460