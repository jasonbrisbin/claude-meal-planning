## Welcome Message
At the start of every new conversation, display a status summary:
1. Check the most recent file in `meal_plans/` and show its date
2. Show today's date and whether a meal plan exists for the current week
3. List available commands: `/project:meal-plan` — generate a new weekly meal plan
Keep it brief (5-6 lines max).

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

## Microsoft To-Do (via Zapier MCP)
- Used for grocery/shopping lists
- **Find a Task** requires a `title` parameter — it cannot list all tasks in a list without a search term
- **Create Task** works without restrictions — can specify list, title, due date, importance, notes
- **Complete Task** available for marking tasks done
- **API Request (Beta)** — raw Microsoft Graph API calls; use for updating tasks (e.g., PATCH to change importance, title, etc.)
  - Endpoint pattern: `https://graph.microsoft.com/v1.0/me/todo/lists/{list-id}/tasks/{task-id}`
  - Content-Type header is forced in Zapier config (no need to pass it)
  - Body should be JSON (e.g., `{"importance": "high"}`)
  - Batch endpoint: `POST https://graph.microsoft.com/v1.0/$batch` — up to 4 requests per batch (20 causes 429 throttling)
  - To list all tasks use API Request with GET: `https://graph.microsoft.com/v1.0/me/todo/lists/{list-id}/tasks?$top=200` (paginate with `$skip=200`)

### List Access Rules
- **Groceries** — READ-ONLY. Never create, update, or delete tasks here.
- **Claude** — READ-WRITE. Use this list for meal planning grocery lists.

### Lists
- Groceries, Claude (and others not used by this project)

## Environment
You are working with a Windows host.  Commands and scripts should use powershell when possible.