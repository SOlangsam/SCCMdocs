---
title: "Enable or Disable a Software Metering Rule | Microsoft Docs"
ms.custom: ""
ms.date: "09/20/2016"
ms.prod: "configuration-manager"
ms.reviewer: ""
ms.suite: ""
ms.technology:
  - "configmgr-other"
ms.tgt_pltfrm: ""
ms.topic: "article"
applies_to:
  - "System Center Configuration Manager (current branch)"
ms.assetid: 13812f78-4627-4427-a32c-7c8bfe64a307
caps.latest.revision: 7
author: "shill-ms"
ms.author: "v-suhill"
manager: "mbaldwin"
---
# How to Enable or Disable a Software Metering Rule
You enable or disable a software metering rule, in System Center Configuration Manager, by loading the instance of the software metering rule that is identified by the software metering rule ID and then setting the Enabled value.  

### To enable or disable a software metering rule  

1.  Set up a connection to the SMS Provider.  

2.  Load the software metering rule object by using the [SMS_MeteredProductRule](assetId:///SMS_MeteredProductRule?qualifyHint=False&autoUpgrade=True) class and a known software metering rule ID.  

3.  Set the [Enabled](assetId:///Enabled?qualifyHint=False&autoUpgrade=True) property to `true` or `false`.  

## Example  
 The following example method shows how to enable or disable a software metering rule by loading the instance of the software metering rule that is identified by the software metering rule ID and setting the assetId:///Enabled?qualifyHint=False&autoUpgrade=True property.  

> [!IMPORTANT]
>  The rule ID corresponds to the value that is stored in the property [RuleID](assetId:///RuleID?qualifyHint=False&autoUpgrade=True). The Configuration Manager console displays a **Rule ID** column, which actually corresponds to the value that is stored in the property [SecurityID](assetId:///SecurityID?qualifyHint=False&autoUpgrade=True).  

 For information about calling the sample code, see [Calling Configuration Manager Code Snippets](../../develop/core/understand/calling-code-snippets.md).  

```vbs  

' Enable or disable a software metering rule.  
 Sub EnableDisableSoftwareMeteringRule(connection,          _  
                                      existingSWMRuleID,    _  
                                      enableDisableSWMRule)                           

    ' Get an existing software metering rule to enable or disable.   
    Set existingSWMRule = connection.Get("SMS_MeteredProductRule.RuleID='" & existingSWMRuleID & "'")    

    ' Get file name for output.  
    fileName = existingSWMRule.FileName  

    ' Enable or disable the rule.  
    existingSWMRule.Enabled = enableDisableSWMRule  

    ' Save the new rule and properties.  
    existingSWMRule.Put_   

    ' Output a success message.  
    Wscript.Echo "SWM rule ID:    " & existingSWMRuleID  
    Wscript.Echo "Rule name:      " & fileName  
    Wscript.Echo "Set enabled to: " & enableDisableSWMRule  

 End Sub  
```  

```c#  

public void EnableDisableSoftwareMeteringRule(WqlConnectionManager connection,  
                                              string existingSWMRuleID,  
                                              bool enableDisableSWMRule)  
{  
    try  
    {  
        // Get the specific software metering rule to enable or disable.  
        IResultObject existingSWMRule = connection.GetInstance(@"SMS_MeteredProductRule.RuleID='" + existingSWMRuleID + "'");  

        // Get rule name for output message.  
        string productName = existingSWMRule["ProductName"].StringValue;  

        // Set the software metering rule.  
        existingSWMRule["Enabled"].BooleanValue = enableDisableSWMRule;  

        // Save changes.  
        existingSWMRule.Put();  

        // Output a success message.  
        Console.WriteLine("SWM rule ID: " + existingSWMRuleID);  
        Console.WriteLine("Rule name: " + productName);  
        Console.WriteLine("Set enabled to: " + enableDisableSWMRule);  
    }  

    catch (SmsException ex)  
    {  
        Console.WriteLine("Failed to modify software metering rule. Error: " + ex.Message);  
        throw;  
    }  
}  

```  

 The example method has the following parameters:  

||||  
|-|-|-|  
|Parameter|Type|Description|  
|`connection`|-   Managed: [WqlConnectionManager](assetId:///WqlConnectionManager?qualifyHint=False&autoUpgrade=True)<br />-   VBScript: [SWbemServices](assetId:///SWbemServices?qualifyHint=False&autoUpgrade=True)|A valid connection to the SMS Provider.|  
|`existingSWMRuleID`|-   Managed: `String`<br />-   VBScript: `String`|Identifies a specific software metering rule. In this case, identifies the specific software metering rule that will be enabled or disabled.|  
|`enableDisableSWMRule`|-   Managed: `Boolean`<br />-   VBScript: `Boolean`|Enables or disables the software metering rule.<br /><br /> `true` - Enabled<br /><br /> `false` - Disabled|  

## Compiling the Code  
 This C# example requires:  

### Namespaces  
 System  

 System.Collections.Generic  

 System.Text  

 Microsoft.ConfigurationManagement.ManagementProvider  

 Microsoft.ConfigurationManagement.ManagementProvider.WqlQueryEngine  

### Assembly  
 adminui.wqlqueryengine  

 microsoft.configurationmanagement.managementprovider  

## Robust Programming  
 For more information about error handling, see [About Configuration Manager Errors](../../develop/core/understand/about-configuration-manager-errors.md).  

## .NET Framework Security  
 For more information about securing Configuration Manager applications, see [Securing Configuration Manager Applications](../../develop/core/understand/securing-configuration-manager-applications.md).  

## See Also  
 [System Center Configuration Manager Software Development Kit](../../develop/core/misc/system-center-configuration-manager-sdk.md)   
 [Configuration Manager Software Metering](../../develop/apps/software-metering.md)   
 [SMS_MeteredProductRule Server WMI Class](../../develop/reference/apps/sms_meteredproductrule-server-wmi-class.md)