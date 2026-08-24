# Entra-ID-Identity-Lab

## Summary
This is a lab created in order to gain hands-on practice & exposure within Entra ID. After the completion of my first Joiner-Mover-Leaver lab [Active-Directory-Identity-Lab](https://github.com/JustaNotion/Active-Directory-Identity-Lab) which was created to demonstrate knowledge of an on-premises environment, I wanted to ensure
that I gain some experience in an environment which is meant to simulate a cloud scenario. This project therefore demonstrates understanding in the Joiner-Mover-Leaver process inside of Entra ID by creating
a basic structure of a real company while also practicing correct privilege assignment & removal upon department assignment, transfer and eventual termination. This was performed on a personal tenant on the 
free tier, not production.

## Stack
As mentioned above this lab was created on Microsoft Entra ID free tier, no Azure subscription of any kind. Additionally this was created as a personal tenant, browser-only. 
P2 features (PIM, Access Reviews, Access Packages) are out of scope for this lab given that they are only available to those who possess a required license. 

## Environment
The environment was set up inside of the "notioneverythinggmail.onmicrosoft.com" domain using the Entra ID free tier. The object count(s) consist of: 8 users, 5 groups and 0 applications. 
![The environment in regards to users](screenshots/environment-all-users.png)
![The environment in regards to tenant overview](screenshots/environment-tenant-overview.png)

Group inventory includes: IT Admin, Sales, Human Resources, DisabledUsers and Professional Accountants
![The environment meant to simulate departments which would commonly be found inside of a business](screenshots/environment-all-groups.png)

## Identity Lifecycle (Joiner-Mover-Leaver)
For this specific lab a new user was created with the name "Mister Mover". This was the target user meant to demonstrate the Joiner-Mover-Leaver lifecycle process.

Upon creation of the user the first step was to ensure that the password was reset by an admin and prompted the user to create a new password upon their next login. 

The next step was placing Mister Mover into different departments. The key step across the "mover" phase of the lifecycle was removing him from his previous groups as he entered each new department, rather than simply adding the new membership on top. Removing non-applicable groups on transfer keeps his memberships reflecting current standing rather than accumulating every department he's ever passed through. 
 ![Mister Mover being added to Human Resources](screenshots/mover-add-to-hr.png) 
 ![Mister Mover being removed from Human Resources](screenshots/mover-remove-from-hr.png)

And finally to close out the Joiner-Mover-Leaver lifecycle Mister Mover was stripped of any/all groups which he was placed inside of and subsequently had his account disabled. I purposely elected to disable his account and place him inside of the "DisabledUsers" group rather than deleting. The reason I did this is to ensure that his information and audit logs are still accessible even if he is no longer permitted access to running operations. 

![Mister Mover showing account disabled](screenshots/leaver-accountenabled-false.png)
![Another capture of Mister Mover having disabled account](screenshots/leaver-disable-activity.png) 
![Mister Mover being placed in the DisabledUsers group](screenshots/mover-moved-to-disabledusers.png)

Upon Mister Mover's account disabling and placement within the DisabledUsers group, the Joiner-Mover-Leaver lifecycle had now been successfully demonstrated and completed. 

## Privileged Role Assignment 
In addition to demonstrating the JML lifecycle with the Mister Mover user created for the exercise, I also wanted to create a user specifically meant for the assignment of the "User Administrator" role. This was done by creating a new user with the name "JohnAdministrator123" who was subsequently given the role soon after entry into the system. The assignment path = Direct and the scope was set to directory-wide. The built-in User Administrator role has a variety of different privileges including (but not limited to): creation & deletion of users, resetting passwords, managing group memberships and a variety of other administrative abilities. 

![Privileged role assigned directly](screenshots/privileged-role-assigned-direct.png)
![The scope set to directory](screenshots/privileged-role-assignments-directory-scope.png)

The reason for this is to compare the differences between the group membership that different users were placed in versus the assignment of a specific, privileged role to one user. During the JML lifecycle demonstration, Mister Mover was added, removed and transferred to different locations within the simulated business. However, at no point during that lifecycle was he granted additional permissions/roles. The groups were a hollow shell which he was placed inside of.

![Contrast between the two](screenshots/privileged-role-group-membership-only.png)

In contrast of the hollow shells, JohnAdministrator123 was given one role and therefore had administrative power which exceeded all of the other example users within this exercise. This standing privilege which JohnAdministrator123 contained is the exact thing which Privileged Identity Management (PIM) exists to address and eliminate. By reviewing, assessing and auditing users on a consistent basis it allows for the discovery (and subsequent addressal) of users who have standing privilege(s) that are non-applicable to their role/department/title.   

## Deletion and Recovery Testing
While the Mister Mover JML lifecycle ultimately concluded with account disabling and removal of any previous assignments to attain zero residual access, I did also want to experiment with purposeful deletion of groups and how membership within groups is affected. 
During the initial environment set up there was a group created entitled "Engineering", upon the creation of this group a user was subsequently placed inside of this group - "MarSue123". I wanted to verify what would happen in the event that a group was deleted with members currently present within. My primary question was "would I be required to remove the members from the group before full-deletion could be completed?" Therefore I initiated the deletion of the Engineering group and was not prompted with any requirements which needed to be met in order to soft-delete. 

![Engineering group soft-deletion](screenshots/engineering-deletiontype-softdelete.png)

I then quickly initiated (and successfully executed) the hard-deletion of the group. 
![Engineering group hard-deletion](screenshots/engineering-deletiontype-harddelete.png)

My question was answered; I did not have to remove the member(s) from within the group, the singular member (MarSue123) remained as a functioning user just without the Engineering group placement. I was unsure if this was permanent (what I now recognize as hard-deletion) or perhaps something else. After visiting the audit logs related to the entire lifespan of the Engineering group I was able to confirm that the hard-deletion execution was not automatically performed, I was the one who initiated and ultimately executed the request.
![Group Management Timeline](screenshots/audit-timeline-groupmanagement.png)

The Engineering creation and subsequent hard-deletion had cleared up any misconceptions that I had about the basic principle of deletion and recovery on Entra ID. After the process was completed I couldn't help but wonder a more specific question in regards to deletion and recovery; "I have already verified that soft-deletion & hard-deletion process are both able to be executed without removal of members present within. But if I were to soft-delete a group with members actively inside, would the members still retain their memberships even during soft-deletion? Additionally upon restoration of a soft-deleted group, would all members retain their placement within the group? Would the group be reset and emptied upon restoration?" And this curiosity is ultimately what led to the second experiment conducted as part of the deletion and recovery testing. 

I created a new, purposeful group entitled "Professional Accountants" with the description of "This group is going to be deleted immediately". I then added 3 members to the group, one of which was the target user for verifying my question - "JonnyAppleseed123". I also added one owner in order to verify if the result would affect one without the other - "MarSue123". One member (JonnyAppleseed123) and the owner (MarSue123), checked before, during and after. After adding all four users to this group I began by screenshotting all of their object IDs to ensure proper matching of the results. Given that any/all deleted Groups (and users) enter a 30 day window before permanent deletion.
![30 Day window](screenshots/deleted-users-30-day-window.png)

![Memberships before soft delete](screenshots/restore-test-user-memberships-before.png)
![Professional Accountants members before deletion](screenshots/restore-test-members-before.png)

I then initiated the process of soft-deleting the Professional Accountants group, as mentioned above in the first experiment I did not receive any sort of push-back or requirement to begin this process. With the group soft-deleted, I turned my attention to restoration. What the soft-deleted state actually did to the members' access is covered in the next section. After the restoration process completed and Professional Accountants reappeared in the group list I then checked each user of the group and received the answer that I was looking for; upon restoration of a soft-deleted group, all members and owners are placed back inside of the group respective to their original assignment. After verifying that each object ID was a correct match to the screenshots taken before deletion, this experimenting had been completed with the information that I was curious on finding out. 

![Members after restore](screenshots/restore-test-members-after.png)

![Owners after restore](screenshots/restore-test-owner-after.png)

## What Soft-Delete Actually Revokes
As mentioned above during the soft-deletion of the Professional Accountants group, neither the member nor the owner showed any membership in the group. This was verified specifically by assessing the specific user's Groups page (JonnyAppleseed123) in addition to the owner's Groups page (MarSue123). I made sure to go directly to the target user's Groups page, the portal directs you to the group's state, so I checked the user's instead.  

![Membership gone during the soft deletion of the group](screenshots/membership-gone-during-soft-delete.png)
The conclusion: Object recoverability is not continuity of entitlement. Restoring within the 30 day window grants you the object back; however, access was interrupted for the entire window. 

## Audit Evidence
- The "Core Directory" filter was used in order to distinguish actual performed actions versus portal-read related noise. Without this filter the raw auditing log would become extraordinarily tedious to go over, a mix of both performed actions in addition to a variety of different "Self-Service Group Management" logs would appear, drowning out the logs being sought after. ![Core Directory filter in use](screenshots/lifecycle-timeline-core-directory.png)
- The PasswordProfile update showing a "service principal" as the actor was surprising to me. After initiating this process I was expecting to locate these logs with the information present alongside a variety of other actions that I executed: "Initiated by (actor) notioneverything_gmail.com#EXT#@notioneverythinggmail.onmicrosoft.com" but this one attributed the change to a service principal instead; it was a reminder that the actor field records what the platform executed the change as, not necessarily who clicked the button. ![The initiated by actor which confused me for a second](screenshots/audit-password-reset-service-principal.png)
- The AccountEnabled transition from True to False on Mister Mover is captured with both old and new values visible.  ![Account disabling](screenshots/leaver-accountenabled-false.png)
- Add and remove member events likewise record old and new values, evidencing the mover path.
![Mister Mover being added to Human Resources](screenshots/mover-add-to-hr.png) 
![Mister Mover being removed from Human Resources](screenshots/mover-remove-from-hr.png)
- Two full timeline views were captured: the user-management timeline from the start of the lab to its conclusion, and the group-management timeline from the first group created through the experiments. ![Entire user management timeline](screenshots/audit-timeline-usermanagement.png) ![Entire group management timeline](screenshots/audit-timeline-groupmanagement.png)

## Known Limitations
- The biggest known limitation to mention: Groups grant nothing (No application(s), licenses, or access packages consume. Membership is a label, not entitlement)
- DisabledUsers has no policy attached
- Portal language versus platform behavior: the group deletion confirmation and success message make no mention of the 30-day recovery window, presenting deletion as final. The object is in fact recoverable; a gap between what the UI communicates and what the platform does. Both deletion paths were tested:

  ![Bulk delete confirmation — no mention of recovery](screenshots/delete-confirm-bulk-no-recovery-mention.png)
  
  ![Single-group delete confirmation from the Overview blade — same dialog](screenshots/delete-confirm-single-no-recovery-mention.png)
  
  ![Success toast — deletion presented as final](screenshots/delete-toast-no-recovery-mention.png)
  
- Nothing federated, no SSO app configured ![Reminder of the overview](screenshots/environment-tenant-overview.png)
- No P2 features (as referenced in the stack)
