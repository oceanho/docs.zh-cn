---
title: "&lt;basicHttpBinding&gt; 的 &lt;message&gt;"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 51cdd329-6461-471a-8747-56c2299b61e5
caps.latest.revision: "23"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.openlocfilehash: a27ef2568f507ea431fb882f4a7ad5e4f7a06785
ms.sourcegitcommit: ce279f2d7fe2220e6ea0a25a8a7a5370ddf8d9f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2017
---
# <a name="ltmessagegt-of-ltbasichttpbindinggt"></a>&lt;basicHttpBinding&gt; 的 &lt;message&gt;
定义的消息级安全性设置[ \<basicHttpBinding >](../../../../../docs/framework/configure-apps/file-schema/wcf/basichttpbinding.md)。  
  
 \<系统。ServiceModel >  
\<绑定 >  
\<basicHttpBinding >  
\<绑定 >  
\<安全 >  
\<消息 >  
  
## <a name="syntax"></a>语法  
  
```xml  
<message   
   algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"  
      clientCredentialType="UserName/Certificate"/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  
 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|特性|描述|  
|---------------|-----------------|  
|algorithmSuite|设置消息加密和密钥包装算法。 此属性类型为 <xref:System.ServiceModel.Security.SecurityAlgorithmSuite>，用于指定算法和密钥大小。 这些算法映射到安全策略语言 (WS-SecurityPolicy) 规范中指定的算法。<br /><br /> 默认值为 `Basic256`。|  
|clientCredentialType|指定要在使用基于消息的安全性执行客户端身份验证时使用的凭据类型。 默认值为 `UserName`。|  
  
## <a name="clientcredentialtype-attribute"></a>clientCredentialType 属性  
  
|值|描述|  
|-----------|-----------------|  
|UserName|-要求客户端在使用 UserName 凭据向服务器进行身份验证。 此凭据需要指定使用[ \<c a t e >](../../../../../docs/framework/configure-apps/file-schema/wcf/clientcredentials.md)。<br />-   [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)]不支持发送密码摘要，也使用派生密钥的密码并使用这样的密钥来提供消息安全性。 因此，在使用 UserName 凭据时，[!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] 将确保传输的安全性。 对于 `basicHttpBinding`，这要求设置一个 SSL 通道。|  
|证书|要求使用证书向服务器对客户端进行身份验证。 在这种情况下，客户端凭据需要指定使用[ \<c a t e >](../../../../../docs/framework/configure-apps/file-schema/wcf/clientcredentials.md)和[ \<t i a l >](../../../../../docs/framework/configure-apps/file-schema/wcf/clientcertificate-of-servicecredentials.md)。 此外，在使用消息安全模式时，需要向客户端提供服务证书。 在这种情况下需要使用指定服务凭据<xref:System.ServiceModel.Description.ClientCredentials>类或`ClientCredentials`行为元素并指定服务证书使用[ \<serviceCertificate >](../../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md)。|  
  
### <a name="child-elements"></a>子元素  
 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<安全 >](../../../../../docs/framework/configure-apps/file-schema/wcf/security-of-basichttpbinding.md)|定义的安全功能[ \<basicHttpBinding >](../../../../../docs/framework/configure-apps/file-schema/wcf/basichttpbinding.md)。|  
  
## <a name="example"></a>示例  
 此示例演示如何实现使用 basicHttpBinding 和消息安全性的应用程序。 在下面的服务配置示例中，终结点定义将指定 basicHttpBinding 并引用名为 `Binding1` 的绑定配置。 服务用于向客户端验证自己身份的证书是在配置文件的 `behaviors` 节中 `serviceCredentials` 元素的下面设置的。 应用于证书（客户端使用该证书向服务验证自己的身份）的验证模式也是在 `behaviors` 节中 `clientCertificate` 元素的下面设置的。  
  
 在客户端配置文件中指定同样的绑定和安全详细信息。  
  
```xml  
<system.serviceModel>  
    <services>  
      <service name="Microsoft.ServiceModel.Samples.CalculatorService"  
               behaviorConfiguration="CalculatorServiceBehavior">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>  
          </baseAddresses>  
        </host>  
        <!-- this endpoint is exposed at the base address provided by host: http://localhost:8000/ServiceModelSamples/service  -->  
        <endpoint address=""  
                  binding="basicHttpBinding"  
                  bindingConfiguration="Binding1"   
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
        <!-- the mex endpoint is exposed at http://localhost:8000/ServiceModelSamples/service/mex -->  
        <endpoint address="mex"  
                  binding="mexHttpBinding"  
                  contract="IMetadataExchange" />  
      </service>  
    </services>  
  
    <bindings>  
      <basicHttpBinding>  
        <!--   
        This configuration defines the SecurityMode as Message and   
        the clientCredentialType as Certificate.  
        -->  
        <binding name="Binding1" >  
          <security mode = "Message">  
            <message clientCredentialType="Certificate"/>  
          </security>  
        </binding>  
      </basicHttpBinding>  
    </bindings>  
  
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="CalculatorServiceBehavior">  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <!--  
        The serviceCredentials behavior allows one to define a service certificate.  
        A service certificate is used by a client to authenticate the service and provide message protection.  
        This configuration references the "localhost" certificate installed during the setup instructions.  
        -->  
          <serviceCredentials>  
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />  
            <clientCertificate>  
              <!--   
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
            is in the user's Trusted People store, then it will be trusted without performing a  
            validation of the certificate's issuer chain. This setting is used here for convenience so that the   
            sample can be run without having to have certificates issued by a certification authority (CA).  
            This setting is less secure than the default, ChainTrust. The security implications of this   
            setting should be carefully considered before using PeerOrChainTrust in production code.   
            -->  
              <authentication certificateValidationMode="PeerOrChainTrust" />  
            </clientCertificate>  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
</system.serviceModel>  
```  
  
## <a name="see-also"></a>另请参阅  
 <xref:System.ServiceModel.BasicHttpMessageSecurity>  
 <xref:System.ServiceModel.Configuration.BasicHttpSecurityElement.Message%2A>  
 <xref:System.ServiceModel.BasicHttpSecurity.Message%2A>  
 <xref:System.ServiceModel.Configuration.BasicHttpMessageSecurityElement>  
 [保护服务和客户端](../../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)  
 [绑定](../../../../../docs/framework/wcf/bindings.md)  
 [配置系统提供的绑定](../../../../../docs/framework/wcf/feature-details/configuring-system-provided-bindings.md)  
 [使用绑定来配置 Windows Communication Foundation 服务和客户端](http://msdn.microsoft.com/en-us/bd8b277b-932f-472f-a42a-b02bb5257dfb)  
 [\<绑定 >](../../../../../docs/framework/misc/binding.md)
