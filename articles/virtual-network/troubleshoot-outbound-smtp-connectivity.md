---
title: Troubleshoot outbound SMTP connectivity in Azure | Microsoft Docs
description: Learn the recommended method for sending email and how to troubleshoot problems with outbound SMTP connectivity in Azure.
services: virtual-network
author: genlin
manager: dcscontentpm
editor: ''

ms.service: virtual-network
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/28/2021
ms.author: genli
---


# Troubleshoot outbound SMTP connectivity problems in Azure

Outbound email messages that are sent directly to external domains (such as outlook.com and gmail.com) from a virtual machine (VM) are made available only to certain subscription types in Microsoft Azure. 

> [!IMPORTANT]
> For all examples below, the process applies *only* to Virtual Machines & VM Scale Sets resources (`Microsoft.Compute/virtualMachines` & `Microsoft.Compute/virtualMachineScaleSets`). Sending email on Port 25 is unsupported from all Azure Platform-as-a-Service (PaaS) resources, including [Azure App Service](https://azure.microsoft.com/services/app-service) and [Azure Functions](https://azure.microsoft.com/services/functions).

## Recommended method of sending email
We recommend you use authenticated SMTP relay services to send email from Azure VMs or from Azure App Service. (These relay services typically connect through TCP port 587, but they support other ports.) These services are used to maintain IP and domain reputation to minimize the possibility that external domains reject your messages or put them to the SPAM folder. [SendGrid](https://sendgrid.com/partners/azure/) is one such SMTP relay service, but there are others. You might also have an authenticated SMTP relay service on your on-premises servers.

Using these email delivery services isn't restricted in Azure, regardless of the subscription type.

## Enterprise Agreement
For VMs that are deployed in standard Enterprise Agreement subscriptions, the outbound SMTP connections on TCP port 25 will not be blocked. However, there is no guarantee that external domains will accept the incoming emails from the VMs. If your emails are rejected or filtered by the external domains, you should contact the email service providers of the external domains to resolve the problems. These problems are not covered by Azure support.

For Enterprise Dev/Test subscriptions, port 25 is blocked by default.
It is possible to have this block removed. To request to have the block removed, go to the **Cannot send email (SMTP-Port 25)** section of the **Diagnose and Solve** blade in the Azure Virtual Network resource in the Azure portal and open a support request.

After the subscription is exempted from this block and the VMs are stopped and restarted, all VMs in that subscription are exempted going forward. The exemption applies only to the subscription requested and only to VM traffic that is routed directly to the internet. 

## All Other Subscription Types 

The Azure platform will block outbound SMTP connections on TCP port 25 for deployed VMs.  This is to ensure better security for Microsoft partners and customers, protect Microsoft’s Azure platform, and conform to industry standards. 

If you're using these subscription types, we encourage you to use an authenticated SMTP relay service, as outlined earlier in this article. 

## Changing subscription type

If you change your subscription type from Enterprise Agreement to another type of subscription, changes to your deployments may result in outbound SMTP being blocked.  


## Need help? Contact support

If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your problem resolved quickly. Use this issue type: **Technical** > **Virtual Network** > **Cannot send email (SMTP/Port 25)**.
