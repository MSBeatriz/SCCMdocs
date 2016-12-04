---
title: "Set the PXE Service Point Response to PXE Requests | Microsoft Docs"
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
ms.assetid: 27015eb9-af08-4882-aa3a-53dac2d6ec24
caps.latest.revision: 11
author: "shill-ms"
ms.author: "v-suhill"
manager: "mbaldwin"
---
# How to Set the PXE Service Point Response to PXE Requests
In System Center Configuration Manager, you set the distribution point response to incoming PXE requests by setting the [IsActive](assetId:///IsActive?qualifyHint=False&autoUpgrade=True) embedded property.  

 assetId:///IsActive?qualifyHint=False&autoUpgrade=True has the following possible values.  

|Value|Description|  
|-----------|-----------------|  
|0|The distribution point does not respond to PXE requests.|  
|1|The distribution service point responds to requests.|  

### To set the distribution point response to PXE requests  

1.  Set up a connection to the SMS Provider. For more information, see [About the SMS Provider in Configuration Manager](../../develop/core/understand/about-the-sms-provider-in-configuration-manager.md).  

2.  Make a connection to the distribution point instance with PXE enabled.  

3.  Get the embedded properties.  

4.  Update the assetId:///IsActive?qualifyHint=False&autoUpgrade=True embedded property.  

5.  Commit the changes to the site control file.  

## Example  
 The following example method sets the response for a PXE request based on the supplied `String` value (`allowResponse`).  

 For information about calling the sample code, see [Calling Configuration Manager Code Snippets](../../develop/core/understand/calling-code-snippets.md).  

```c#  
public void SetAllowResponse(WqlConnectionManager connection,                                  string siteCode,                                  string serverName,                                  string allowResponse){    try    {        //Connect to distribution point instance.                        IResultObject siteRole = connection.GetInstance("SMS_SCI_SysResUse.FileType=2,ItemName=\"[\\\"Display=\\\\\\\\" + serverName + "\\\\\\\"]MSWNET:[\\\"SMS_SITE=" + siteCode + "\\\"]\\\\\\\\" + serverName + "\\\\,SMS Distribution Point\",ItemType=\"System Resource Usage\",SiteCode=" + "\"" + siteCode + "\"");        // Create temporary copy of the embedded properties.        Dictionary<string, IResultObject> embeddedProperties = siteRole.EmbeddedProperties;        // Enumerate through the embedded properties and makes changes as needed.        foreach (KeyValuePair<string, IResultObject> kvp in siteRole.EmbeddedProperties)        {            // Setting: IsActive            if (kvp.Value.PropertyList["PropertyName"] == "IsActive")            {                // Get current property value.                Console.WriteLine();                Console.WriteLine("Property: {0}", kvp.Value.PropertyList["PropertyName"]);                Console.WriteLine("Current value: {0}", kvp.Value.PropertyList["Value"]);                // Change value.                embeddedProperties["IsActive"]["Value"].StringValue = allowResponse;                Console.WriteLine("Setting the {0} value to {1}.", kvp.Value.PropertyList["PropertyName"], allowResponse);            }        }        // Store the settings that have changed.        siteRole.EmbeddedProperties = embeddedProperties;        // Save the settings.         siteRole.Put();    }    catch (SmsException ex)    {        Console.WriteLine();        Console.WriteLine("Failed. Error: " + ex.InnerException.Message);    }}  
```  

 The example method has the following parameters:  

||||  
|-|-|-|  
|Parameter|Type|Description|  
|`connection`|Managed: [WqlConnectionManager](assetId:///WqlConnectionManager?qualifyHint=False&autoUpgrade=True)|A valid connection to the SMS Provider.|  
|`siteCode`|Managed: `String`|The Configuration Manager site code.|  
|`serverName`|Managed: `String`|The server name. For example, `“SERVER1.DOMAIN1.COM��?`.|  
|allowResponse|Managed: `String`|The value to set whether the distribution point will respond to PXE requests.<br /><br /> -   0 - The distribution point does not respond to PXE requests.<br />-   1 - The PXE service point responds to requests from unknown computers.|  

## Compiling the Code  
 The C# example has the following compilation requirements:  

### Namespaces  
 System  

 System.Collections.Generic  

 System.Text  

 Microsoft.ConfigurationManagement.ManagementProvider  

 Microsoft.ConfigurationManagement.ManagementProvider.WqlQueryEngine  

### Assembly  
 microsoft.configurationmanagement.managementprovider  

 adminui.wqlqueryengine  

## Robust Programming  
 For more information about error handling, see [About Configuration Manager Errors](../../develop/core/understand/about-configuration-manager-errors.md).  

## .NET Framework Security  
 For more information about securing Configuration Manager applications, see [Securing Configuration Manager Applications](../../develop/core/understand/securing-configuration-manager-applications.md).  

## See Also  
 [About Operating System Deployment Site Role Configuration](../../develop/osd/about-operating-system-deployment-site-role-configuration.md)   
 [Configuration Manager Operating System Deployment](../../develop/osd/operating-system-deployment.md)   
 [Configuration Manager Programming Fundamentals](../../develop/core/understand/configuration-manager-programming-fundamentals.md)   
 [Operating System Deployment Site Role Configuration](../../develop/osd/operating-system-deployment-site-role-configuration.md)   
 [How to Read and Write to the Configuration Manager Site Control File by Using Managed Code](../../develop/core/understand/how-to-read-and-write-to-the-site-control-file-by-using-managed-code.md)   
 [How to Read and Write to the Configuration Manager Site Control File by Using WMI](../../develop/core/understand/how-to-read-and-write-to-the-site-control-file-by-using-wmi.md)