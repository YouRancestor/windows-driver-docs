---
title: Magnetic stripe reader events
description: Describes the events that are passed from the device driver to the Point of Service (POS) API layer by using ReadFile.
ms.assetid: '48ce9fcb-5ea2-4045-92df-990d94e5e98b'
ms.author: windowsdriverdev
ms.date: 9/7/2018
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
ms.localizationpriority: medium
---

# Magnetic stripe reader events

This section describes the events that are passed from the device driver to the Point of Service (POS) API layer by using [ReadFile](https://docs.microsoft.com/windows/desktop/api/fileapi/nf-fileapi-readfile). This section focuses on events that are specific to magnetic stripe readers (MSRs).

## In this section

[MagneticStripeReaderDataReceived](magneticstripereaderdatareceived.md)  
Occurs after a successful scan event.

[MagneticStripeReaderErrorOccured](magneticstripereadererroroccured.md)  
Occurs when there is an MSR error, such as a scanning error.
