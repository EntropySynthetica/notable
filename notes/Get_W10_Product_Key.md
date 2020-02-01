---
tags: [Windows]
title: Get_W10_Product_Key
created: '2020-01-30T20:16:16.539Z'
modified: '2020-01-30T20:41:22.580Z'
---

# Get W10 Product Key
Created Friday 15 June 2018

Via Powershell,

(Get-WmiObject -query ‘select * from SoftwareLicensingService’).OA3xOriginalProductKey

