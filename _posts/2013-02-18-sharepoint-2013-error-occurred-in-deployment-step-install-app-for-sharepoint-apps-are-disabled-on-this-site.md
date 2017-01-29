---
layout: post
title: "SharePoint 2013 Error occurred in deployment step 'Install app for sharepoint' apps are disabled on this site"
date: 2015-03-06 -0800
comments: true
tags: [Apps are disabled on this site, Error occurred in deployment step 'Install app for SharePoint', sharepoint 2013 app deployment, sharepoint app deployment error, sharepoint app helloworld]
redirect_from:
  - index.php/sharepoint-2013-error-occurred-in-deployment-step-install-app-for-sharepoint-apps-are-disabled-on-this-site/
---

I was just deploying my first app in an on premises sharepoint installation and deploying failed with following error in the output

Error occurred in deployment step ‘Install app for SharePoint': Apps are disabled on this site

I got to know that this is something expected as in sharepoint 2013 you have to create isolated app domain for development of apps, which can be created by following powershell script as taken from a technet article.


```powershell

net start spadminv4
net start sptimerv4
Set-SPAppDomain "your app domain here"
Get-SPServiceInstance | where{$_.GetType().Name -eq "AppManagementServiceInstance" -or $_.GetType().Name -eq "SPSubscriptionSettingsServiceInstance"} | Start-SPServiceInstance
Get-SPServiceInstance | where{$_.GetType().Name -eq "AppManagementServiceInstance" -or $_.GetType().Name -eq "SPSubscriptionSettingsServiceInstance"}
$account = Get-SPManagedAccount "your managed account here" 
$appPoolSubSvc = New-SPServiceApplicationPool -Name SettingsServiceAppPool -Account $account
$appPoolAppSvc = New-SPServiceApplicationPool -Name AppServiceAppPool -Account $account
$appSubSvc = New-SPSubscriptionSettingsServiceApplication –ApplicationPool $appPoolSubSvc –Name SettingsServiceApp –DatabaseName SettingsServiceDB 
$proxySubSvc = New-SPSubscriptionSettingsServiceApplicationProxy –ServiceApplication $appSubSvc
$appAppSvc = New-SPAppManagementServiceApplication -ApplicationPool $appPoolAppSvc -Name AppServiceApp -DatabaseName AppServiceDB
$proxyAppSvc = New-SPAppManagementServiceApplicationProxy -ServiceApplication $appAppSvc
Set-SPAppSiteSubscriptionName -Name "app" -Confirm:$false

```


**Important Note**
please replace the place holders “your app domain here” and “your managed account here” with appropriate values , app domain can be any arbitarary app domain like apps.myorg.com as sharepoint adds this in hosts file so no dns entry is required this is something to take care of in production. In managed account specify a managed account if you have not created one you can create one in central administration in security options.