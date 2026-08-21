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
The environment was set up inside of the "notioneverytinggmail.on.microsoft.com" domain using the Entra ID free tier. The object count(s) consist of: 9 users, 5 groups and 0 applications. 
(insert screenshot(s) here)
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
While the Mister Mover JML lifecycle ultimately concluded with account disabling and removal of any previous assignments to attain zero residual access, I did also want to experiment with purposeful deletion groups and how membership within groups are effected. 
During the initial environment set up there was a group created entitled "Engineering", upon the creation of this group a user was subsequently placed inside of this group - "MarSue123". I wanted to verify what would happen in the event that a group was deleted with members currently present within. My primary question was "would I be required to remove the members from the group before full-deletion could be completed?" Therefore I initiated the deletion of the Engineering group and was not prompted with any requirements which needed to be met in order to soft-delete. I then quickly initiated (and successfully executed) the hard-deletion of the group. My question was answered; I did not have to remove the member(s) from within the group, the singular member (MarSue123) remained as a functioning user just without the Engineering group placement. Some other things noted during this experiment were the dialog that I encountered which mentioned "the group will be deleted immediately" upon initiation of the soft-deletion. I was unsure if this was permanent (what I now recognize as hard-deletion) or perhaps something else. After visiting the audit logs related to the entire lifespan of the Engineering group I was able to confirm that the hard-deletion execution was not automatically performed, I was the one who initiated and ultimately executed the request, therefore clearing up the dialog I encountered. 

The Engineering creation and subsequent hard-deletion had cleared up any misconceptions that I had about the basic principle of deletion and recovery on Entra ID. After the process was completed I couldn't help but wonder a more specific question in regards to deletion and recovery; "I have already verified that soft-deletion & hard-deletion process are both able to be executed without removal of members present within. But if I were to soft-delete a group with members actively inside, would the members still retain their memberships even during soft-deletion? Additionally upon restoration of a soft-deleted group, would all members retain their placement within the group? Would the group be reset and emptied upon restoration?" And this curiosity is ultimately what led to the second experiment conducted as part of the deletion and recovery testing. 

I created a new, purposeful group entitled "Professional Accountants" with the description of "This group is going to be deleted immediately". I then added 3 members to the group, one of which is the target user for verifying my question - "JonnyAppleseed123". I also added one owner in order to verify if the result would affect one without the other - "MarSue123". One member (JonnyAppleseed123) and the owner (MarSue123), checked before, during and after. After adding all four users to this group I began by screenshotting all of their object IDs to ensure proper matching of the results. Given that any/all deleted Groups (and users) enter a 30 day window before permanent deletion, I then initiated the process of soft-deleting the Professional Accountants group, as mentioned above in the first experiment I did not receive any sort of push-back or requirement to begin this process. With the group soft-deleted, I turned my attention to restoration. What the soft-deleted state actually did to the members' access is covered in the next section. After the restoration process completed and Professional Accountants was back in the group list I then checked each user of the group and received the answer that I was looking for; upon restoration of a soft-deleted group, all members and owners are placed back inside of the group respective to their original assignment. After verifying that each object ID was a correct match to the screenshots taken before deletion, this experimenting had been completed with the information that I was curious on finding out. 

## What Soft-Delete Actually Revokes
As mentioned above during the soft-deletion of the Professional Accountants group, none of the members/owners displayed as still being placed within the group. This was verified specifically by assessing the specific user's Groups page (JonnyAppleseed123) in addition to the owner's Groups page (MarSue123) and noting that both did not have a placement within the Professional Accountants group. I made sure to go directly to the target users Groups page as I was encountering a different piece of information as the portal kept pointing towards the group's state. 

The conclusion: Object recoverability is not continuity of entitlement. Restoring within the 30 day window grants you the object back; however, access was interrupted for the entire window. 

## Audit Evidence
- The "Core Directory" filter was used in order to distinguish actual performed actions versus portal-read related noise. Without this filter the raw auditing log would become extraordinarily tedious to go over, a mix of both performed actions in addition to a variety of different "Self-Service Group Management" logs would appear, drowning out the logs being sought after. 
- The PasswordProfile update showing a "service principal" as the actor was surprising to me. After initiating this process I was expecting to locate these logs with the information present alongside a variety of other actions that I executed: "Initiated by (actor) notioneverything_gmail.com#EXT#@notioneverythinggmail.onmicrosoft.com"
- Here is the log from Mister Mover which displays the "AccountEnabled: True" being transitioned to "False" with both old and new values visible.
- Add/Remove member events with old and new values; the mover path, evidenced. 

## Known Limitations
The biggest known limitation to mention: Groups grant nothing (No application(s), licenses, or access package consumes them. Membership is a label, not entitlement)
DisabledUsers has no policy attached
Nothing federated, no SSO app configured
No P2 features (as referenced in the stack)
Documentation contradiction: As referenced in Deletion and Recovery Testing, the "delete confirmation page" says the group will be deleted immediately. My experiment disproved this. 
