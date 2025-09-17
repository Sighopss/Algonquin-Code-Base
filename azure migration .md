# Swiss Re’s Azure Migration Plan

## Case Study: Swiss Re

Swiss Re is one of the biggest names in reinsurance. Their setup had been working for years, but like any other old system, it was starting to feel a bit clunky. So they decided to move SAP S/4HANA to Azure. The goal? Make things faster, easy to manage, and less of a pain for the team involved.

## Why Move to the Cloud?

There were a few main reasons here :

- **Saving money** – On-premises servers are expensive and require constant maintenance. With Azure, you only pay for what you do  use

- **Reducing repetitive work** – Automation takes care of the boring tasks. The team can focus on more interesting projects(preventing redundancies)

- **Being ready for problems** – Cloud-based disaster recovery means that if something goes wrong, the system bounces back fast enough

- **Flexibility** – As business needs changes,  cloud infrastructure gets  easier to adapt.

Basically, they wanted IT that quietly does its job instead of constantly demanding attention which causes a trickle down effect of problems 

## Questions on their current set ups

Before moving anything, you have to understand what is already there. Some things to ask:

- What servers and operating systems are currently being  operated? How is the network structured?  

- Which apps depend on SAP S/4HANA  and how close-knit are they connected to each other exactly?

- How much data is involved and what compliance rules apply (GDPR, or financial regulations  are a good example.)  

- How are backups and disaster recovery handled today?  

- Are there any recurring performance problems or maintenance issues/mishaps

- Any custom SAP tweaks or third-party tools that need to be considered?

## RACI Matrix: Who is Doing What

| Stakeholder         | Responsible (R)             | Accountable (A)         | Consulted (C)            | Informed (I)         |
|---------------------|-----------------------------|-------------------------|--------------------------|---------------------|
| Project Manager     | Runs the project day-to-day  | Makes final calls       |                          |                     |
| IT/DevOps Team      | Sets up tech and tests it    |                         | Offers advice on architecture |                     |
| Business Units      |                             | Aligns requirements     | Shares user feedback     | Receives updates    |
| Microsoft (Azure)   |                             |                         | Offers best practices    | Informed of milestones |
| Executives          |                             | Approves big decisions  |                          | Receives reports    |
| Security/Compliance | Configures security          | Ensures compliance      | Checks risks             |                     |
| Consultants         |                             |                         | Provides SAP expertise   |                     |

## Migration Approach

I would recommend what people call “lift, tweak, and shift.” The idea is simple:

1. Move SAP S/4HANA to Azure Virtual Machines without changing too much at first.  

2. Once it is there, start using cloud tools like **Azure Site Recovery** for disaster recovery and **Azure DevOperatio s** for automation.  

Azure Migrate is handy because it helps map the current environment and spot problems before moving anything. This way, you get into the cloud quickly but can still improve and modernize things later.

## Rough Timeline

Here’s a possible schedule, roughly:

- **Weeks 1–4: Assessment** – Check everything, look for risks, understand dependencies

- **Weeks 5–8: Planning** – Design Azure setup, define roles, and run a small pilot test

- **Weeks 9–12: Migration** – Move workloads gradually, starting with non-critical ones 

- **Weeks 13–16: Testing and Tweaks** – Test well enough, fix small issues, optimize performance, and train users

- **Week 17: Go-Live**  : Switch fully to Azure, keep a close eye on things for a few weeks.

**Total time:** Around 4–5 months, give or take.

## Why Swiss Re Did This

In the end, Swiss Re needed IT that literally just  works.moving  to Azure cut costs, allowed them to scale, improved reliability, and made audits way more  simpler. Automation freed the team for more meaningful work. They wanted IT that actually helps the business, not something that constantly needs fixing and micromanagement 
