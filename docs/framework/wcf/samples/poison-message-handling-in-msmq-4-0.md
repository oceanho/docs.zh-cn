---
title: "MSMQ 4.0 中的病毒消息处理 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: ec8d59e3-9937-4391-bb8c-fdaaf2cbb73e
caps.latest.revision: 18
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 18
---
# MSMQ 4.0 中的病毒消息处理
本示例演示如何在服务中执行病毒消息处理。  此示例基于[已经过事务处理的 MSMQ 绑定](../../../../docs/framework/wcf/samples/transacted-msmq-binding.md)示例。  其中使用到了 `netMsmqBinding`。  此服务是自承载控制台应用程序，通过它可以观察服务接收排队消息。  
  
 在排队通信中，客户端使用队列与服务进行通信。  更确切地说，客户端向队列发送消息。  服务从队列接收消息。  因此不必同时运行服务和客户端便可使用队列进行通信。  
  
 病毒消息是一类当服务读取消息时不能对消息进行处理，并因此终止从中读取消息的事务时从队列重复读取的消息。  在这种情况下，将再次重试消息。  如果消息出现问题，这种情况在理论上将永远继续下去。  注意，这种情况仅在您使用事务从队列中读取消息并调用服务操作时发生。  
  
 根据 MSMQ 版本，NetMsmqBinding 支持对病毒消息进行有限检测和完全检测。  在已经将消息检测为病毒后，可以通过多种方式对消息进行处理。  同样，根据 MSMQ 版本，NetMsmqBinding 支持对病毒消息进行有限处理和完全处理。  
  
 本示例演示 [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] 和 [!INCLUDE[wxp](../../../../includes/wxp-md.md)] 平台上提供的有限病毒功能和 [!INCLUDE[wv](../../../../includes/wv-md.md)] 上提供的完全病毒功能。  在这两个示例中，目的是将病毒消息从一个队列移出到另一个队列，然后由病毒消息服务对其进行处理。  
  
## MSMQ v4.0 病毒处理示例  
 在 [!INCLUDE[wv](../../../../includes/wv-md.md)] 中，MSMQ 提供一个病毒子队列功能，可以用来存储病毒消息。  本示例演示使用 [!INCLUDE[wv](../../../../includes/wv-md.md)] 处理病毒消息的最佳做法。  
  
 [!INCLUDE[wv](../../../../includes/wv-md.md)] 中的病毒消息检测非常完善。  有 3 属性可帮助检测。  <xref:System.ServiceModel.MsmqBindingBase.ReceiveRetryCount%2A> 是重新从队列中读取给定消息并将其调度到应用程序中以进行处理的次数。  当由于某一消息无法调度到应用程序或应用程序在服务操作中回滚事务，该消息返回到队列中时，即会从队列中重新读取该消息。  <xref:System.ServiceModel.MsmqBindingBase.MaxRetryCycles%2A> 是将消息移动到重试队列的次数。  当达到 <xref:System.ServiceModel.MsmqBindingBase.ReceiveRetryCount%2A> 时，即会将该消息移动到重试队列。  属性 <xref:System.ServiceModel.MsmqBindingBase.RetryCycleDelay%2A> 是将消息从重试队列移回到主队列之前的时间延迟。  <xref:System.ServiceModel.MsmqBindingBase.ReceiveRetryCount%2A> 重置为 0。  再次尝试消息。  如果读取消息的所有尝试都失败，则会将该消息标记为已中毒。  
  
 一旦将消息标记为已中毒，则将按照 <xref:System.ServiceModel.MsmqBindingBase.ReceiveErrorHandling%2A> 枚举中的设置来处理该消息。  迭代可能的值：  
  
-   Fault（默认值）：将侦听器和服务主机视为出现故障。  
  
-   Drop：将删除消息。  
  
-   Move：将消息移到病毒消息子队列中。  此值仅在 [!INCLUDE[wv](../../../../includes/wv-md.md)] 中可用。  
  
-   Reject：要通过将消息发送回发送方死信队列来拒绝消息。  此值仅在 [!INCLUDE[wv](../../../../includes/wv-md.md)] 中可用。  
  
 示例演示了如何使用 `Move` 处理病毒消息。  `Move` 可使消息移动到病毒子队列中。  
  
 服务协定是 `IOrderProcessor`，它定义了适合与队列一起使用的单向服务。  
  
```  
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IOrderProcessor  
{  
    [OperationContract(IsOneWay = true)]  
    void SubmitPurchaseOrder(PurchaseOrder po);  
}  
  
```  
  
 该服务操作显示一条指示它正在处理订单的消息。  为了演示病毒消息功能，`SubmitPurchaseOrder` 服务操作将引发异常，以便在任意一次调用服务时回滚事务。  这将导致消息被放回队列中。  并最终将消息标记为病毒。  配置设置为将病毒消息移到病毒子队列。  
  
```  
  
// Service class that implements the service contract.  
// Added code to write output to the console window.  
public class OrderProcessorService : IOrderProcessor  
{  
    static Random r = new Random(137);  
  
    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]  
    public void SubmitPurchaseOrder(PurchaseOrder po)  
    {  
  
        int randomNumber = r.Next(10);  
  
        if (randomNumber % 2 == 0)  
        {  
            Orders.Add(po);  
            Console.WriteLine("Processing {0} ", po);  
        }  
        else  
        {  
            Console.WriteLine("Aborting transaction, cannot process purchase order: " + po.PONumber);  
            Console.WriteLine();  
            throw new Exception("Cannot process purchase order: " + po.PONumber);  
        }  
    }  
  
    public static void OnServiceFaulted(object sender, EventArgs e)   
    {  
        Console.WriteLine("Service Faulted");  
    }  
  
    // Host the service within this EXE console application.  
    public static void Main()  
    {  
        // Get MSMQ queue name from app settings in configuration.  
        string queueName = ConfigurationManager.AppSettings["queueName"];  
  
        // Create the transacted MSMQ queue if necessary.  
        if (!System.Messaging.MessageQueue.Exists(queueName))  
            System.Messaging.MessageQueue.Create(queueName, true);  
  
        // Get the base address that is used to listen for WS-MetaDataExchange requests.  
        // This is useful to generate a proxy for the client.  
        string baseAddress = ConfigurationManager.AppSettings["baseAddress"];  
  
        // Create a ServiceHost for the OrderProcessorService type.  
        ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService), new Uri(baseAddress));  
  
        // Hook on to the service host faulted events.  
        serviceHost.Faulted += new EventHandler(OnServiceFaulted);  
  
        // Open the ServiceHostBase to create listeners and start listening for messages.  
        serviceHost.Open();  
  
        // The service can now be accessed.  
        Console.WriteLine("The service is ready.");  
        Console.WriteLine("Press <ENTER> to terminate service.");  
        Console.WriteLine();  
        Console.ReadLine();  
  
        if(serviceHost.State != CommunicationState.Faulted) {  
            serviceHost.Close();  
        }  
  
    }  
}  
```  
  
 服务配置包括下面的病毒消息属性：`receiveRetryCount`、`maxRetryCycles`、`retryCycleDelay` 和 `receiveErrorHandling`，如下面的配置文件所示。  
  
```  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  <appSettings>  
    <!-- Use appSetting to configure MSMQ queue name. -->  
    <add key="queueName" value=".\private$\ServiceModelSamplesPoison" />  
    <add key="baseAddress" value="http://localhost:8000/orderProcessor/poisonSample"/>  
  </appSettings>  
  <system.serviceModel>  
    <services>  
      <service   
              name="Microsoft.ServiceModel.Samples.OrderProcessorService">  
        <!-- Define NetMsmqEndpoint -->  
        <endpoint address="net.msmq://localhost/private/ServiceModelSamplesPoison"  
                  binding="netMsmqBinding"  
                  bindingConfiguration="PoisonBinding"   
                  contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />  
      </service>  
    </services>  
  
    <bindings>  
      <netMsmqBinding>  
        <binding name="PoisonBinding"   
                 receiveRetryCount="0"  
                 maxRetryCycles="1"  
                 retryCycleDelay="00:00:05"                        
                 receiveErrorHandling="Move">  
        </binding>  
      </netMsmqBinding>  
    </bindings>  
  </system.serviceModel>  
</configuration>  
```  
  
## 处理病毒消息队列中的消息  
 病毒消息服务从最终病毒消息队列中读取消息并处理这些消息。  
  
 病毒消息队列中的消息是指发送到正在处理消息的服务的消息，这可能不同于病毒消息服务终结点。  因此，当病毒消息服务从队列中读取消息时，[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 通道层可发现与终结点的不匹配之处，并且不会调度消息。  在这种情况下，消息将发送到订单处理服务，但将由病毒消息服务接收。  如果无论消息是否发送到不同的终结点都要继续接收消息，则我们必须添加 `ServiceBehavior` 来筛选地址，其中匹配标准是要匹配消息发送到的任何服务终结点。  这是成功处理从病毒消息队列中读取的消息所必需的。  
  
 病毒消息服务实现本身非常类似于服务实现。  它实现协定并处理订单。  代码示例如下所示。  
  
```  
// Service class that implements the service contract.  
// Added code to write output to the console window.  
[ServiceBehavior(AddressFilterMode=AddressFilterMode.Any)]  
public class OrderProcessorService : IOrderProcessor  
{  
    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]  
    public void SubmitPurchaseOrder(PurchaseOrder po)  
    {  
        Orders.Add(po);  
        Console.WriteLine("Processing {0} ", po);  
    }  
  
    public static void OnServiceFaulted(object sender, EventArgs e)   
    {  
        Console.WriteLine("Service Faulted...exiting app");  
        Environment.Exit(1);  
    }  
  
    // Host the service within this EXE console application.  
    public static void Main()  
    {  
  
        // Create a ServiceHost for the OrderProcessorService type.  
        ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService));  
  
        // Hook on to the service host faulted events.  
        serviceHost.Faulted += new EventHandler(OnServiceFaulted);  
  
        serviceHost.Open();  
  
        // The service can now be accessed.  
        Console.WriteLine("The poison message service is ready.");  
        Console.WriteLine("Press <ENTER> to terminate service.");  
        Console.WriteLine();  
        Console.ReadLine();  
  
        // Close the ServiceHostBase to shutdown the service.  
        if(serviceHost.State != CommunicationState.Faulted)  
        {  
            serviceHost.Close();  
        }  
    }  
  
```  
  
 与从订单队列中读取消息的订单处理服务不同，病毒消息服务从病毒子队列中读取消息。  病毒队列是主队列的子队列，其名称为“poison”，由 MSMQ 自动生成。  若要访问该队列，请提供主队列名称，后接“;”并在子队列为“poison”的情况下提供子队列名称，如下面的示例配置中所示。  
  
> [!NOTE]
>  在 MSMQ 3.0 版的示例中，病毒队列名称不是子队列，而是将消息移动到的队列。  
  
```  
  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service name="Microsoft.ServiceModel.Samples.OrderProcessorService">  
        <!-- Define NetMsmqEndpoint -->  
        <endpoint address="net.msmq://localhost/private/ServiceModelSamplesPoison;poison"  
                  binding="netMsmqBinding"  
                  contract="Microsoft.ServiceModel.Samples.IOrderProcessor" >  
        </endpoint>  
      </service>  
    </services>  
  
  </system.serviceModel>  
</configuration>  
  
```  
  
 当运行示例时，客户端、服务和病毒消息服务活动将显示在控制台窗口中。  您可以看到服务从客户端接收消息。  在每个控制台窗口中按 Enter 可以关闭服务。  
  
 服务开始运行、处理订单并随机开始终止处理。  如果消息指示服务已经处理完订单，则可以再次运行客户端来发送其他消息，直到您看到服务确实已经终止某条消息。  根据配置的病毒设置，在将消息移到最终病毒队列中之前将再次重试处理消息。  
  
```  
  
The service is ready.  
Press <ENTER> to terminate service.  
  
Processing Purchase Order: 0f063b71-93e0-42a1-aa3b-bca6c7a89546  
        Customer: somecustomer.com  
        OrderDetails  
                Order LineItem: 54 of Blue Widget @unit price: $29.99  
                Order LineItem: 890 of Red Widget @unit price: $45.89  
        Total cost of this order: $42461.56  
        Order status: Pending  
  
Processing Purchase Order: 5ef9a4fa-5a30-4175-b455-2fb1396095fa  
        Customer: somecustomer.com  
        OrderDetails  
                Order LineItem: 54 of Blue Widget @unit price: $29.99  
                Order LineItem: 890 of Red Widget @unit price: $45.89  
        Total cost of this order: $42461.56  
        Order status: Pending  
  
Aborting transaction, cannot process purchase order: 23e0b991-fbf9-4438-a0e2-20adf93a4f89  
```  
  
 启动病毒消息服务以从病毒队列中读取病毒消息。  在本示例中，病毒消息服务读取消息并处理这些消息。  您可以看到病毒消息服务读取已终止并已中毒的采购订单。  
  
```  
The service is ready.  
Press <ENTER> to terminate service.  
  
Processing Purchase Order: 23e0b991-fbf9-4438-a0e2-20adf93a4f89  
        Customer: somecustomer.com  
        OrderDetails  
                Order LineItem: 54 of Blue Widget @unit price: $29.99  
                Order LineItem: 890 of Red Widget @unit price: $45.89  
        Total cost of this order: $42461.56  
        Order status: Pending  
  
```  
  
#### 设置、生成和运行示例  
  
1.  确保已经执行了[Windows Communication Foundation 示例的一次性安装过程](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2.  如果先运行服务，则它将检查以确保队列存在。  如果队列不存在，则服务将创建一个队列。  可以先运行服务以创建队列或通过 MSMQ 队列管理器创建一个队列。  执行下面的步骤来在 Windows 2008 中创建队列。  
  
    1.  在 [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)] 中打开服务器管理器。  
  
    2.  展开**“功能”**选项卡。  
  
    3.  右击**“私有消息队列”**，然后选择**“新建”**和**“专用队列”**。  
  
    4.  选中**“事务性”**框。  
  
    5.  输入 `ServiceModelSamplesTransacted` 作为新队列的名称。  
  
3.  若要生成 C\# 或 Visual Basic .NET 版本的解决方案，请按照[生成 Windows Communication Foundation 示例](../../../../docs/framework/wcf/samples/building-the-samples.md)中的说明进行操作。  
  
4.  若要用单一计算机配置或跨计算机配置来运行示例，请更改队列名称，以映射实际的主机名而不是本地主机，并按照[运行 Windows Communication Foundation 示例](../../../../docs/framework/wcf/samples/running-the-samples.md)中的说明操作。  
  
 默认情况下对 `netMsmqBinding` 绑定传输启用了安全性。  `MsmqAuthenticationMode` 和 `MsmqProtectionLevel` 这两个属性共同确定了传输安全性的类型。  默认情况下，身份验证模式设置为 `Windows`，保护级别设置为 `Sign`。  MSMQ 必须是域的成员才可以提供身份验证和签名功能。  如果在不是域成员的计算机上运行此示例，则会接收以下错误：“用户的内部消息队列证书不存在”。  
  
#### 在加入到工作组的计算机上运行此示例  
  
1.  如果计算机不是域成员，请将身份验证模式和保护级别设置为 `None` 以禁用传输安全性，如下面的示例配置所示：  
  
    ```  
    <bindings>  
        <netMsmqBinding>  
            <binding name="TransactedBinding">  
                <security mode="None"/>  
            </binding>  
        </netMsmqBinding>  
    </bindings>  
    ```  
  
     确保通过设置终结点的 bindingConfiguration 属性将终结点与绑定关联。  
  
2.  确保在运行示例前更改 PoisonMessageServer、服务器和客户端上的配置。  
  
    > [!NOTE]
    >  将 `security mode` 设置为 `None` 等效于将 `MsmqAuthenticationMode`、`MsmqProtectionLevel` 和 `Message` 安全设置为 `None`。  
  
3.  若要使元数据交换正常工作，应当向 http 绑定注册一个 URL。  这要求服务在具有提升权限的命令窗口中运行。  否则，您将接收到异常，比如：未经处理的异常：System.ServiceModel.AddressAccessDeniedException：HTTP 无法注册 URL http:\/\/\+:8000\/ServiceModelSamples\/service\/。  进程不具有此命名空间的访问权限（有关详细信息，请参见 http:\/\/go.microsoft.com\/fwlink\/?LinkId\=70353）。  \-\-\-\> System.Net.HttpListenerException：访问被拒绝。  
  
> [!IMPORTANT]
>  您的计算机上可能已安装这些示例。  在继续操作之前，请先检查以下（默认）目录：  
>   
>  `<安装驱动器>:\WF_WCF_Samples`  
>   
>  如果此目录不存在，请访问[针对 .NET Framework 4 的 Windows Communication Foundation \(WCF\) 和 Windows Workflow Foundation \(WF\) 示例](http://go.microsoft.com/fwlink/?LinkId=150780)（可能为英文网页），下载所有 [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] 和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。  此示例位于以下目录：  
>   
>  `<安装驱动器>:\WF_WCF_Samples\WCF\Basic\Binding\Net\MSMQ\Poison\MSMQ4`  
  
## 请参阅