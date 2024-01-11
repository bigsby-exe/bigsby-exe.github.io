---
layout: post
title: "Surface Hub and Teams Rooms Coordinated Meetings"
date: 2022-10-10 23:00
categories: O365 Teams
tags: Teams, Surface-Hub 
---
# What is coordinated meetings 
Coordinated meetings are setting that allow multiple meeting room devices to all join and leave calls together. This is useful if you have more than one surface hub in a single room or a mix of Teams rooms devices and surface hubs.  
In these settings you can decide which of the devices will use it's mic and speakers to prevent feedback.
# Setting up Coordinated Meetings on Surface Hubs
## Intune 
Surface hubs can have coordinated meeting policies applied to them as documented in the [MS Docs](https://learn.microsoft.com/en-us/microsoftteams/rooms/surface-hub-manage-config) however due to the structure of the XML files this is only really useful if you want to maintain one policy per meeting room.  
## Setup on device 
Recently Microsoft added the ability to setup coordinated meetings via the hub settings.  
To do this open teams, press the 3 dots to the left of the user icon and go to 'Settings' (you will have to login as admin). From here there is the option to setup coordinated meetings by specifying a trusted account and selecting if you want to use the mic/camera of the current device.

# Setting up Coordinated Meetings on Teams Rooms Devices 
Similarly to a surface hub this can be deployed via policy or as is possibly easier depending on the amount of setups you have you can open the settings on the device and setup you trusted device and mic/speakers.

# Configurations
## One Way Trust
Coordinated Meetings can be configured in a one way trust, in this mode the device that trusts the other will call it into any meetings however the other device will not auto answer and will instead ring like a normal call.  
To set this up configure the trusted devices on `Device 1` to trust `Device 2` but do not setup coordinated meetings on `Device 2` at all.

## Two way trust
This is a more standard configuration where both devices trust each other, in this mode calls started on `Device 1` will ring and get auto answered on `Device 2` and when left of `Device 1` will also leave on `Device 2`.  
Something to note here is if I start a call on `Device 1` then `Device 2` will be auto joined, if I leave with `Device 2` it will be unable to remove `Device 1` from the call as the call was started by `Device 1` so ideally you would want to start and end calls on the same device.

# Important note
When testing it seems the trusted device account seems to need to have the same domain name for all devices, if this is different coordinated meetings won't work however there will be no visible errors.