---
layout: post
title: "Setting up a Power Platform Test Environment"
date: 2022-06-29 20:00
categories: O365 Power-Platform
tags: Power-Platform 
---

## Table of Contents
1. [Setup O365 Tenant](#setting-up-your-o365-tenancy)
2. [Setup fake users and content](#setting-up-fake-users-sharepoint-sites-and-activity)
3. [Optional Setup](#optional-setup)
    1. [Adding a custom domain](#custom-domain)
    2. [Branding](#branding)
4. [Setup Power Platform environment](#setting-up-the-default-power-platform-environment)


## Setting up your O365 tenancy
First step in getting a power platform environment is having an office 365 tenancy.
This can be setup for free for development by using the [Microsoft 365 Developer Program](https://developer.microsoft.com/en-us/microsoft-365/dev-program)

You will need to attach payment details but these won't be charged unless you utilize Azure premium services.

## Setting up fake users, SharePoint sites and activity
Setting up users an activity is relatively easy and will likely be useful for future tests especially if you want to test permission based actions. 

To do this go to the [MS Developer Program Dashboard](https://developer.microsoft.com/en-us/microsoft-365/profile) with your regular Microsoft account used to setup the dev program and not the onmicrosoft.com account and run the automated tasks to set users, mail & events and SharePoint sites, you can run one or all 3 it's up to you.  
There is currently no charge for this action. 

## Optional setup
You should now have a fully setup O365 tenancy with 25 users, emails, activity and SharePoint sites. 

From here there are a few optional extras you can setup. 

- ### Custom Domain
    If you have your own a custom domain you can set this up by logging into [portal.azure.com](https://portal.azure.com/) with the onmicrosoft.com email setup with the tenant & going to `Azure Active Directory` and then to `Custom domain names` (or go [here](https://portal.azure.com/#view/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/~/Domains)) press `Add a custom domain` and follow the steps to setup your DNS records.  

    > Note: You will still keep the onmicrosft.com domain however you can set your custom as default.

    Once setup and set as default you can go into [`Users`](https://portal.azure.com/#view/Microsoft_AAD_UsersAndTenants/UserManagementMenuBlade/~/AllUsers) go to each user press `Edit Properties` and change the `User principal name` and `Email` fields to use your new domain.

- ### Branding
    Another recommended config I would suggest is setting the branding, this will make it easy and quick to differentiate your test tenant from any real tenants you may use.  

    To set this up go to [admin.microsoft.com](https://admin.microsoft.com/) press `Show all` on the sidebar menu and go to `Settings` > `Org Settings` > `Organization Profile` > `Custom Themes` (or go [here](https://admin.microsoft.com/Adminportal/Home#/Settings/OrganizationProfile/:/Settings/L1/CustomThemes)), from here you can edit the default theme and under logos add an image file to use and under colors setup your brand colors  
    
    Next go to [portal.azure.com](https://portal.azure.com) and go to `Company Branding` and from here you can set a banner logo and Sign-in background.

    Once setup you will see your tenant icon on the top rail of most pages including power platform apps. 

## Setting up the default power platform environment

Now you have setup a O365 Tenant you should have a default Power Platform environment, you can view and modify the settings by going to [admin.powerplatform.microsoft.com/environments](https://admin.powerplatform.microsoft.com/environments)

To setup a Teams Power Platform environment go to [admin.teams.microsoft.com](https://admin.teams.microsoft.com/) drop down `Teams` from the sidebar and click `Manage Teams` from here you can add a team and add your user to it.
Now open Teams either the desktop client or in a browser go to [teams.microsoft.com](https://teams.microsoft.com/) press the 3 dots from the left side rail and go to `More Apps`, from here install the Power Apps app and create an app in the newly created team, this will generate the environment automatically and we will now be able to see it if we go back to the Power Platform admin center.