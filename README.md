# Entra-ID-Identity-Lab

## Summary
This is a lab created in order to gain hands-on practice & exposure within Entra ID. After the completion of my first Joiner-Mover-Leaver lab (Active-Directory-Identity-Lab) which was created to demonstrate knowledge of an on-premises environment, I wanted to ensure
that I gain some experience in an environment which is meant to simulate a cloud scenario. This project therefore demonstrates understanding in the Joiner-Mover-Leaver process inside of Entra ID by creating
a basic structure of a real company while also practicing correct privilege assignment & removal upon department assignment, transfer and eventual termination. This was performed on a personal tenant on the 
free tier, not production.

## Stack
As mentioned above this lab was created on Microsoft Entra ID free tier, no Azure subscription of any kind. Additionally this was created as a personal tenant, browser-only. 
P2 features (PIM, Access Reviews, Access Packages) are out of scope for this lab given that they are only available to those who possess a required license. 

## Environment
The environment was set up inside of the "notioneverytinggmail.on.microsoft.com" domain using the Entra ID free tier. The object count(s) consist of: 8 users, 5 groups and 0 applications. 

Group inventory includes: IT Admin, Sales, HR, DisabledUsers and Professional Accountants

## Identity Lifecycle (Joiner-Mover-Leaver)
For this specific lab a new user was created with the name "Mister Mover". This was the target user meant to demonstrate the Joiner-Mover-Leaver lifecycle process. 
Upon creation of the user the first step was to ensure that the password was reset by an admin and prompted the user to create a new password upon their next login. 

The next step was actually placing Mister Mover inside of different departments. Across the "mover" section of the lifecycle process the key step was ensuring that upon entry into a new department which Mister Mover had been relocated to, he was removed from any previous non-applicable groups. Removing non-applicable groups on transfer keeps his memberships reflecting current standing rather than accumulating every department he's ever passed through. 

And finally to close out the Joiner-Mover-Leaver lifecycle Mister Mover was stripped of any/all groups which he was placed inside of and subsequently had his account disabled. I purposely elected to disable his account and place him inside of the "DisabledUsers" group rather than deleting. The reason I did this is to ensure that his information and audit logs are still accessible even if he is no longer permitted access to running operations. 

Upon Mister Mover's account disabling and placement within the DisabledUsers group, the Joiner-Mover-Leaver lifecycle had now been successfully demonstrated and completed. 

## Privileged Role Assignment 
In addition to demonstrating the JML lifecycle with the Mister Mover user created for the exercise, I also wanted to create a user specifically meant for the assignment of the "User Administrator" role. This was done by creating a new user with the name "JohnAdministrator123" who was subsequently given the role soon after entry into the system. The assignment path = Direct and the scope was set to directory-wide. The built-in User Administrator role has a variety of different privileges including (but not limited to): creation & deletion of users, resetting passwords, managing group memberships and a variety of other administrative abilities. 

The reason for this is to compare the differences between the group membership that different users were placed in versus the assignment of a specific, privileged role to one user. During the JML lifecycle demonstration, Mister Mover was added, removed and transferred to different locations within the simulated business. However, at no point during that lifecycle was he granted additional permissions/roles. The groups were a hollow shell which he was placed inside of.

Whereas JohnAdministrator123 was given one role and therefore had administrative power which superseded all of the other example users within this exercise. This standing privilege which JohnAdministrator123 contained is the exact thing which Privileged Identity Management (PIM) exists to address and eliminate. By reviewing, assessing and auditing users on a consistent basis it allows for the discovery (and subsequent addressal) of users who have standing privilege(s) that are non-applicable to their role/department/title.   

## Deletion and Recovery Testing


## What Soft-Delete Actually Revokes


## Audit Evidence


## Known Limitations
