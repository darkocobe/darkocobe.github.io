---
title: CA Design DOs and DON’T'S
author: Darko Todoroski
date: 2020-02-05 20:55:00 +0800
categories: [Azure, Conditional Access]
tags: [design, conditional access policy, guide]
pin: true
---

These days Azure AD Conditional Policies are used by almost every organization present on one of the three Microsoft clouds (Azure, M365, Dynamics 365). I recommend this lengthy blog post for everyone that has work to do with Conditional Access policies. No matter if you are a cloud architect, cloud engineer, or cloud support engineer, you will extend your knowledge and make proper decisions when you design your conditional access policies.

Below I share essential information that you need to take into consideration during your conditional access project. Even if you already have a CA policy design or implemented them, you can find this article useful and adjust some of your practices.

I divided the blog post into four sections (Plan, Dsign, Build, Operate). It is crucial to read every section of it and follow them in order during your project. Throughout the article, I use the CA abbreviation for Conditional Access.

## Plan

Every project starts with a plan. I know it sounds like a cliché, but having a plan is critical when you start designing conditional access policies. This plan can help you to make proper decisions and sometimes can save your day as well.

You need to involve the correct people that will work, participate and make decisions in this project. I've seen many examples where organizations implemented the CA policies ad-hoc because the IT department got an order to do so. But this approach most of the time can lead to disruption. Depends on the policy scope, you need to involve your architects (security, network), CISO, support desk, etc.

Once you have good people around you, you will need to collect and document their requirements. Everyone can share their opinion, and putting that on paper is crucial for you. You will need these requirements to make your design. You can have conditions that can be conflicting with each other. These conditions can cause one or the other department to have an impact. That is why for you, as an owner of the CA policies critical to understand, document, and guide the rest of the organization during the design and implement policies that will achieve everyone's requirements. Later on, in another blog post, I will show you an example of how you can use the requirements and translate them into the design.

During this stage of the project, you will organize pilot groups to test each planned policy. It is critical to choose your pilot groups accurately. I've seen many bad examples where organizations will pilot solutions within their IT organization, and once the pilot is successful, they will continue rolling out throughout the organization. This approach is deficient and has a lot of potential for failure. You can't test some scenarios within the IT organization. Imagine that you have an application used only within the HR department, and you want to protect it using CA policies. In this case, piloting within the IT organization will give you negative feedback.

Instead, what I recommend is to build an organization-wide pilot program (ex. eat your dog food members). Inform your organization about your internal testing program and let them subscribe to it. You want to keep your participants enthusiastic about your pilot projects. You can establish an award system where every quarter in a lottery can win cool gadgets (headphones, phone, tablet, etc.) Collecting participant's feedback is equally important. It can be through an internal survey system or via telemetry. It is essential to communicate with this group honestly, on time, and be present when they need help and support as best as you can. Otherwise can lead to many frustrations, and people will avoid being members of this group no matter the prize. :) You can use this pilot group of people for many other projects.

## Design

## Build

Finally, once you put all this on paper, have a good plan and design, it is time to start implementing it. As a general rule, you should never enable CA policies directly to the whole organization from the beginning.

You should always start testing policies with the What-If tool and simulate the impact on your organization. Note: You need to be aware that What-If evaluates only the enabled (On) or enabled in (Report only) policies. In this case, if you want to use the tool on a smaller user scope, I would recommend to scope the CA policy to one test user and simulate the results for that test user to see the outcome before you go to the next step.

Implementing CA policies is so easy that almost anyone can do it. The tricky part is when you start messing around without knowing what you do. And then things get trickier. That is why skipping the previous project phases is not an option. You don't want to implement policies that can have a bad end-user experience and destruct your project. Bad end-user experience can be a blocker of the project. Especially when you hit the C level in your organization, and they feel the awful user experience themselves. This approach can lead to frustrations and poor decisions. It is your responsibility to turn your end-users into advocates of your project.

I hope you remember the Microsoft Windows as a Service update strategy. If you do not remember, you can read more details about it here. This strategy is not the topic of our blog post. Instead, we are going to use a similar approach to push our CA policies into production.
Once you familiarize yourself with this approach, you probably found the picture below. This picture shows how Microsoft is deploying Windows as a Service in separate Rings.

** picture*

The strategy here is to organize your pilot participants in Ring groups (Azure AD Cloud groups) to test your policies. So, design a couple of Rings, preferably not more than three. Of course, this depends on the size of your organization, but don't overcomplicate.  Once you design your deployment rings and inform pilot participants, you can use this Windows as a service idea to push the policies into production. You start with Ring 1 (a small set of users throughout the organization) to test your CA policy. Further, based on the feedback (telemetry and survey results) you proceed with extensive scope in Ring 2, Ring 3, and as a final step, you deploy to all users in production. Now you have a mechanism that will give you an idea of how the policies are working in your organization, do you have an impact, and if you do, it is easy to roll back with just removing the Ring group from the policy scope.

To collect feedback, you can use two techniques. One is to organize a survey. You can use Microsoft Forms which comes for free for every M365 customer. Ask your pilot users to provide their feedback not just about technical challenges but as well about their experience.
The other technique is to collect telemetry data from the AAD logs and analyze it.
I would suggest using both in combination. Why? You want to know what is happening in the backend (from the logs), but same time you want to recognize the user experience. If something is looking satisfactory at the backend (policies properly applied and working) does not mean the user experience is acceptable as well. I firmly believe that when you implement security measures, people need to understand your point and have a good experience. This way, you will get their support in the next policy implementation or future project.

Telemetry data and dashboards that will present your rings can help you to spot problems early, even sometimes before the users. Spotting problems in your dashboard (for example people can't authenticate, or pass the policy for some reason) will play a major part in this phase. Once you spot an issue, you can react with a rollback or fix your policy, and end-users don't suffer. Using this approach, they will even more appreciate your effort because they will feel your support and flexibility to help immediately. Do not forget the communication and support for these pilot groups from the Planning phase.
Once the telemetry data and the user's feedback are positive, you can move forward to the next ring or pushing the policy to all users with confidence.

## Operate

Many organizations are ignoring this critical phase. Once implementation is over, it is crucial to manage and operate this solution as well. I've seen many examples where people want to show me their policies, and they wonder who created the rest of the policies, why policies are changed and not configured as they thought, etc.

This challenge brings the RBAC to the table. It is critical to have control over who can create, modify, delete CA policies. As you remember, we said CA policies are your Identity firewall. You don't want everyone to be able to change your configuration. You don't want someone adding an exception to avoid policies applied. Good start the is the built-in role "????". This role will allow you to limit the people responsible for managing CA policies. I highly recommend you to use this role via PIM and with an approval process. This way will make sure that no-one will abuse the responsibilities and modify your policies.

On top of that detecting changes, play a giant part in the game. Every change should be controlled and properly planned. Azure AD stores audit logs for changes in CA. You need to keep these logs for a longer period so you can have full visibility. By default, Azure AD keeps the audit logs for 30 days only.

Once you transfer your sign-in and audit logs to your Log Analytics workspace, you can analyze the CA policies using the built-in Conditional Acess Workbook. Microsoft provides this workbook default, and you can extend it with your scenarios. You can implement a workbook that presents changes in CA policies. As well you should notify your admins if someone performed an unauthorized adjustment.

CA policies are critical for protecting your cloud regardless of Azure infrastructure, M365, or Dynamics. Keep the management and monitoring tight and have control a hundred percent of the time.
If you are serious about Infrastructure as Code, you can use M365 DSC for keeping your policies as code. This way, you can keep your CA configuration in the desired state. If you detect changes, you can audit or overwrite them. You can investigate more about M365 by going to the M365 DSC GitHub page.

### Business Continuity and Disaster Recovery (BCDR)

Disruption can happen and will happen for sure. The problem is that Murphy always kicks in when you do not plan. Murphy can be your friend, but only if you are prepared to welcome him. In any other situation, will be against you. When we discuss business continuity and disaster recovery, you need to be aware that this is a shared responsibility between Microsoft and you. Microsoft will take care of the service availability and disaster recovery, and you need to take care of your part backing up your CA policies.

It is critical to pay attention to this stage because there is no undo button when you modify policies. Microsoft doesn't perform a backup of your policy configuration as well. Therefore you can't call Microsoft and ask them to restore the backup from yesterday. :)

Many people not paying attention. I visit organizations daily, and I must say that almost 90% of them do not have any procedure for backing up their CA policy configuration. I've seen many backup examples like word documents with screenshots, excel files with configuration settings, and many other options. I do not say that this is wrong, but it can be very inflexible, especially when you need to change any of those or restore all of them. Keeping these files up to date rarely happens. There are many community efforts to create solutions for CA policy documentation, and my preference is code. :)

Therefore I suggest documenting all CA policies as code. And all changes that you perform always should go via code. This way will give you the opportunity even in the worse situations when by accident someone deletes all the policies with just one click to recreate/reenable all of them. The code approach will help with the Ring structure as well. The actual implementation steps will come in another blog post.
