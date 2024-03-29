---
layout: post
title: "Restricting Entra Groups with Administrative Units"
date: 2024-01-11 12:00
categories: Entra Security Administration
tags: Administrative Units Access Control Entra
---

# Restricting Entra Groups with Administrative Units

Often, you may need to create a user or group in Entra that different teams can manage, without granting them broad access to modify all users or groups. For example, you might want the applications team to modify the 'applications' group, while the service desk should be able to add or remove people from these groups without the ability to delete or rename them. To achieve this, we can utilize [Administrative Units (AUs)](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/administrative-units) in Entra. AUs allow us to set up distinct groups of administrators and define their specific permissions for certain objects.

## Understanding Administrative Units in Entra

Before we dive into the practical applications, let's first understand what Administrative Units are.

### What are Administrative Units?

Administrative Units are a feature in Entra that allow for the segregation of directory objects, such as users and groups, into separate units. This segregation enables you to delegate administrative tasks to different admins  without granting them full administrative rights across the entire directory.

### Why Use Administrative Units?

- **Enhanced Security**: By limiting the scope of what each admin can control, the risk of unauthorized or accidental changes to critical parts of the directory is minimized.
- **Organizational Efficiency**: AUs allow for a more organized and manageable structure, especially in larger organizations.
- **Customized Control**: Administrative roles can be customised to specific needs of different departments or teams.

## Implementing Administrative Units

To setup AUs 
1. Go to [entra.microsoft.com](https://entra.microsoft.com/#view/Microsoft_AAD_IAM/AdminUnitManagementBlade) 
2. Under "Identity" section, click "Show More" and go to "Admin Units"
3. Create an Admin Unit, in this example we will name it "Apps Team"
4. Consider using the [Restrictive Management](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/admin-units-restricted-management) option if you want to limit the management of objects within the AU exclusively to AU admins. This means that even Group Admins will not be able to modify these groups, which can be useful in certain scenarios.
5. Click "Next" and select the role you would like the admin to have (this will be the role they get on objects within the AU). In my case I will select "Group Admin" and then select the users who will have this role.  

Now we have an AU setup we can apply it to a group this will mean that although ordinarily Alice does not have access to modify the group for this specific group she will have all the permissions of Group Admin.  

To assign an AU to a group:
1. Go to [entra.microsoft.com](https://entra.microsoft.com/#view/Microsoft_AAD_IAM/GroupsManagementMenuBlade/~/AllGroups/menuId/AllGroups)
2. Navigate to "Groups" and then "All Groups."
3. Find the group you want to manage and click on "Administrative units." Here, you can assign the "Apps Team" AU we created earlier.
4. After assigning, it may take up to 60 seconds or so for the changes to replicate. Once done, Alice should now have control over this group without being listed as the Owner or being a member of the group.

## Restrictive Management
In the previous section, I mentioned Restrictive Management and its role in limiting access to certain groups. This feature is particularly useful when you want to prevent globally assigned Group Admins from editing specific groups. It's also beneficial in scenarios where admins should have access to most groups, except a few select ones.

To implement this:

1. Assign the Group Admin role to all relevant admins as usual. This grants them broad access to most groups.
2. Then, set up Administrative Units (AUs) for the specific groups you want to restrict access to. By applying Restrictive Management to these AUs, you can ensure that only designated admins within these units can manage the groups in question.
3. This approach allows for a more granular control of group management, ensuring that certain groups remain under tighter control as per your requirements.


