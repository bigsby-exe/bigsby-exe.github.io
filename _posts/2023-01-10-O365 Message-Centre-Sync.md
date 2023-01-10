---
layout: post
title: "O365 Message Centre Sync to Planner"
date: 2023-01-10 2:00
categories: O365 Planner
tags: O365, Power-Automate, Message-Centre, Planner
---
# Syncing Message Centre to Planner & Auto Tag

## Setting up sync and Group
Syncing Message Centre messages to planner is a good way to get an overview in your org of what changes are coming from Microsoft in the near future and prepare to act upon them.  
Luckily for us Microsoft have build this feature right into the [O365 Message Centre](https://admin.microsoft.com/Adminportal/Home#/MessageCenter), clicking the `Planner Sync` button will automatically create a Power Automate flow for us.

![Teams Admin Screenshot](./../../assets/img/2023/01/10/planner-sync.png)

The Planner that can be attached to a Team or Office 365 group. In my case I will create a dynamic O365 group with a dynamic membership rule to pick up users from other groups, this way management of this group will happen automatically based on the other teams.  
The rule syntax is shown below and we can add up to 50 groups this way.
```powershell
user.memberOf -any (group.objectId -in ['GROUP-GUID','GROUP-GUID','GROUP-GUID'])
```
Once we have our group setup the MS Planner can attach to this and membership will be governed by the group(s) from the rule.

## Creating a Sharepoint List
For the auto tagging in our use case we will create a Sharepoint List in the Office 365 group with the following structure
| Title | Team | 
| ----- | ---- |
| Teams | Team 1 |
| Stream | Team 2 |
| Sharepoint | Team 3 |

This will be used by our auto tagging flow to map the services to our Teams

## Auto Tagging via Power Automate flow
We should now have messages populating into our Planner, from here we want to check what Microsoft service the message is for and tag it with the appropriate coloured tag.  

1. We will start with a `When a new task is created` trigger and select our group and plan.   
![Power Automate Trigger Screenshot](./../../assets/img/2023/01/10/trigger.png)  
2. Next we will initialize a blank array called 'teams'  
3. To get the MS service in the message we will use a `Find text position` and  the search text of `]`  
4. Followed by a `Substring` that takes the output from our previous action, has a start position of '1' and for the length we will use the following expression
    > Note: The name of your text position step is used in the expression so if you have renamed it you will need to update it in the expression 
    ```
    sub(outputs('Find_text_position')?['body'],1)
    ```
    ![Power Automate Trigger Screenshot](./../../assets/img/2023/01/10/get-service.png)  

5. Some messages have multiple services so we will use a `Compose` action with the following expression to split the results on a comma
    ```
    split(outputs('Substring')?['body'],', ')
    ```
6. Next we will use a `Get Items` and get our sharepoint list we made earlier.  
7. Following this we will crete an `Apply to Each loop` where the input is the output of our compose action 
8. Inside the loop we will create a second `Apply to Each` this time the input will be the output from our Sharepoint `Get Items`.  
9. Inside the 2nd loop we will add a `Condition` to check if the 'Name' of our Sharepoint item matches the 'Current item' from the first loop 
10. Under the `If Yes` section we will add an `Append to Array` and add the Team from our sharepoint list to the array we made at the start.

11. Finally we are going to use the `Update a Task (V2)` to update task with tags  
    > For this example let's assume Team 1 is blue, Team 2 is red and Team 3 is green
    {: .prompt-info }

    For the Task ID we will use the ID from the trigger action and leave the rest default until we get to the tags.   
    For each tag we will use it's own expression this expression will check the array for a specific team name nad if found apply that tag, this way we can apply multiple tags if we need multiple teams assigned.  

    For the blue tag we will use the expression 
    ```
    contains(variables('teams'),'Team 1')
    ```
    For red the expression will be
    ```
    contains(variables('teams'),'Team 2')
    ```
    and for green it's
    ```
    contains(variables('teams'),'Team 3')
    ```
    ![Screenshot of Update a Task action](./../../assets/img/2023/01/10/tags.png)  