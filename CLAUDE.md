## Description
This repository is to help manage my dietary needs including meal planning and shopping.

## Daily Goals
Protein
- 100g from lean protein sources
  - Sources included
    - chicken, beef, pork, turkey
    - beans
    - peanuts/almonds/pistachios
    - Greek yogurt
    - Cottage cheese
    - cheese stick/slices
    - egg
  - Protein should be spread out throughout the daily meals and somewhat consistent through the day
Produce
- As much as you want
- Non-Starchy Veg 
Carbs
- 100g per day
- Sources
  - Grains
    - Cereal
    - Rice
    - Bread 
  - prefer ingredients that have fiber
  - Starchy Veg
    - Potato
    - Corn
    - Peas
    - Sweet Potato
    - Squash
    - each serving sizes should be about 15g of carbs
  - Fruit
    - Serving size of 15g Carbs
    - dried fruits are concentrated so be aware
Heart Healthy Fats
Water=100oz (64 is the minimum)

## Exclusions
Foods in this section should not be included
- Fish
- Tofu
- Chicken Thighs
- Brown Rice
  - White Rice and Wild Rices are fine

## Notes
** Do not subtract fiber from carbs for totals
** Important for managing pH in the body
** Do not do Keto while on GLP-1 the meds reduce blood sugar, minimum >50g of carbs

## Shopping
We want to minimize the amount of groceries that we need to acquire.  As such we want to center the meal choices around foods which can be used more than once throughout the week.

Choose products from these stores by preference order.
1. Aldi
2. Costco
   1. only if better value or unavailable from Aldi
3. Sams Club
4. County Market

Optimize for cost per serving

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