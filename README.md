# power-platform-learning
### Power BI Service Terminology
![power bi terminology](assets/power-bi-terminology.png)

## Power Apps
- **Workspaces**: Is more for collaborating with the team
- **Apps vs Workspaces**: Any change on Workspace will be directly reflected for anyone in Workspace. But apps will reflect the latest version when we publish it.

### Dataverse
- Solutions are like packages to move from one environment to another. E.g when we create PROD environment, we create a solution in dev environment, put things need to copy inside solution and then copy then to production.
- Managed solutions is like locked solution. We can't modify components inside it. It's good for PROD or test environment. When we export a solution, we can define whether it gets exported as managed or unmanaged. But for unmanaged, we can modify components. Also, when we delete the unmanaged solution, only the container (the solution itself) gets deleted, not the inside.
- When we create a table in Dataverse, we can define Ownership:
  - Oragnisation: Then the table will be controlled by oraganisation
  - User or Team: Then data and records in user level
- "Business Roles" is useful when we want to defines some rules on table. Like if field value was "Unkown", change it to something else, if hide it, or don't let to be shown, etc.
- Realtime Workflow and actions are for make the platform do something (like send email, change something, so on) when something happened (like the row changed, record assigned to someone, so on)
  - Find out both these options in: Choose a Solution > New > Automation > Process. 
  - Workflows needs to be attached to a table. Workflow doesn't run on background, you can see it. This is the reason the platform recommends you to use Power Automate instead
  - Actions can be not attached to a table. They run whenever trigger them. Action creates a message whenever gets trigerred. This message can have input and output 
- DataFlaws: Is for get data from a source (like spreadsheet, Azure, so on), Transform it (in Power Query) and then export it.