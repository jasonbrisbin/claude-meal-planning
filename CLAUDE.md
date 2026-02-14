## Welcome Message
At the start of every new conversation, display a status summary:
1. Check the most recent file in `meal_plans/` and show its date
2. Show today's date and whether a meal plan exists for the current week
3. Check MCP server readiness (run these in parallel):
   - **Google Calendar**: call `list_calendars` — report ✅ if it returns results, ❌ if it errors
   - **Microsoft To Do (n8n)**: call `Get_many_lists_in_Microsoft_To_Do` — report ✅ if it returns results, ❌ if it errors
4. List available commands: `/project:meal-plan` — generate a new weekly meal plan
Keep it brief (7-8 lines max).

## Description
This repository is to help manage my dietary needs including meal planning and shopping.

## Dietary Guidelines
See [dietary guidelines](docs/dietary-guidelines.md) for nutritional targets, allowed/excluded foods, and important dietary notes. Always consult this file when creating meal plans.

## Shopping
See [shopping guidelines](docs/shopping-guidelines.md) for store preferences and shopping strategy.

## Scheduling
Add the meal plan to the corresponding date on my Google Calendar for "Family".  All times are in Central time zone.
- When creating calendar events, always include the timezone offset (e.g., `-06:00` for CST) in ISO timestamps to avoid UTC interpretation.
- Breakfast
  - Monday-Friday at 7am
  - Saturday-Sunday at 9am
- Lunch
  - Monday-Friday 12pm
  - Saturday-Seunday 1pm
- Dinner at 5pm daily

## File Storage
- `meal_plans/` - Weekly meal plans, named `YYYY-MM-DD.md` (using the start date of the week)
- `recipes/` - Recipe ideas to incorporate into meal plans
- `docs/` - Project documentation and setup guides
- `articles/` - Articles describing the solution I am creating which can be posted to social media like LinkedIn.

## Microsoft To-Do (via n8n MCP)
- Used for grocery/shopping lists
- **Get many tasks** (`Get_many_tasks_in_Microsoft_To_Do`) — list tasks on a given list
- **Create a task** (`Create_a_task_in_Microsoft_To_Do`) — create tasks with title, due date, importance, notes
- **Update a task** (`Update_a_task_in_Microsoft_To_Do`) — update task properties (e.g., importance)
- **Delete a task** (`Delete_a_task_in_Microsoft_To_Do`) — remove tasks
- **Get many lists** (`Get_many_lists_in_Microsoft_To_Do`) — list all To-Do lists

### List Access Rules
- **Groceries** — READ-ONLY. Never create, update, or delete tasks here.
- **Claude** — READ-WRITE. Use this list for meal planning grocery lists.

### Lists
- Groceries, Claude (and others not used by this project)

## Workflow Diagrams
When the `/meal-plan` command (`.claude/commands/meal-plan.md`) is modified, you **must** update both workflow diagrams to reflect the changes:
- `docs/meal-plan-workflow.md` (Mermaid version)
- `docs/meal-plan-workflow.html` (HTML/CSS Material Design version)

## Environment
You are working with a Windows host.  Commands and scripts should use powershell when possible.