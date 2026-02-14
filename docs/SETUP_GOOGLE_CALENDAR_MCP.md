# Google Calendar MCP Server Setup Guide

This documents the actual setup used for this project, using the Python-based MCP server at `C:\Users\jbris\git\calendar-mcp\`.

## Step 1: Create Google OAuth Credentials

1. **Go to Google Cloud Console**
   - Visit: https://console.cloud.google.com/
   - Sign in with your Google account

2. **Create a New Project** (or use existing)
   - Click "Select a project" > "New Project"
   - Name: "Claude Meal Planner"
   - Click "Create"

3. **Enable Google Calendar API**
   - Go to: https://console.cloud.google.com/apis/library
   - Search for "Google Calendar API"
   - Click on it and press "Enable"

4. **Configure OAuth Consent Screen**
   - Go to: https://console.cloud.google.com/apis/credentials/consent
   - Choose "External"
   - Fill in required fields:
     - App name: "Claude Meal Planner"
     - User support email: your email
     - Developer contact: your email
   - Add Scopes: search for `calendar`, add `.../auth/calendar`
   - **Add Test Users**: add your Google email address (e.g., `jbrisbin@gmail.com`)
   - Click "Save and Continue"

5. **Create the OAuth Client**
   - Go to: https://console.cloud.google.com/apis/credentials
   - Click "Create Credentials" > "OAuth client ID"
   - Application type: **Desktop app**
   - Name: "Envvars" (or your choice)
   - Click "Create"
   - **Copy the Client ID and Client Secret** (you'll need them for the `.env` file)

## Step 2: Configure the MCP Server

1. **Create the `.env` file** in the calendar-mcp project directory:

   ```
   cd C:\Users\jbris\git\calendar-mcp
   cp example.env .env
   ```

2. **Edit `.env`** and add your credentials:

   ```env
   GOOGLE_CLIENT_ID='your-client-id-here'
   GOOGLE_CLIENT_SECRET='your-client-secret-here'
   TOKEN_FILE_PATH='.gcp-saved-tokens.json'
   OAUTH_CALLBACK_PORT=8080
   CALENDAR_SCOPES='https://www.googleapis.com/auth/calendar'
   ```

3. **Install Python dependencies**:

   ```
   pip install -r requirements.txt
   pip install mcp
   pip install pydantic[email]
   ```

   Note: `mcp` and `pydantic[email]` are not listed in `requirements.txt` but are required.

## Step 3: Run Initial Authentication

Run the server manually once to complete the OAuth flow:

```
cd C:\Users\jbris\git\calendar-mcp
python run_server.py
```

- A browser window will open for Google sign-in
- Sign in and grant calendar permissions
- You may see an "unverified app" warning - click "Continue" (normal for test apps)
- After authentication, tokens are saved to `.gcp-saved-tokens.json`
- You can stop the server (Ctrl+C) after authentication succeeds

## Step 4: Configure Claude Code MCP Settings

1. **Create `.mcp.json`** in the project root (`C:\Users\jbris\git\menu\.mcp.json`):

   ```json
   {
     "mcpServers": {
       "google-calendar": {
         "type": "stdio",
         "command": "python",
         "args": [
           "C:\\Users\\jbris\\git\\calendar-mcp\\run_server.py"
         ]
       }
     }
   }
   ```

   Note: The project-level `.mcp.json` is what the VSCode extension picks up. The global `~/.claude/mcp.json` may also work but was less reliable in our experience.

2. **Restart VSCode** completely for the MCP server to load.

## Step 5: Verify the Connection

Once restarted, ask Claude to list your calendars. You should see your calendars including "Family".

## Troubleshooting

**MCP tools not appearing after restart**
- Check that `mcp` Python package is installed: `python -c "import mcp; print('OK')"`
- Check that `pydantic[email]` is installed: `pip install pydantic[email]`
- Verify `.mcp.json` exists in the project root (not just `~/.claude/mcp.json`)
- Check the calendar MCP log: `C:\Users\jbris\git\calendar-mcp\calendar_mcp.log`

**Error: "Access blocked" during OAuth**
- Add your email as a test user in the OAuth consent screen
- Go to Google Cloud Console > OAuth consent screen > Test users > Add Users

**Event times are wrong (off by 6 hours)**
- Always include timezone offset in ISO timestamps (e.g., `2026-02-16T07:00:00-06:00` for Central)
- Without the offset, times are interpreted as UTC

**Error: "CalendarListResponse not defined"**
- Install `pydantic[email]`: `pip install pydantic[email]`
- This dependency causes import failures if missing

## Security Notes

- Keep your `.env` file secure and private (it contains OAuth credentials)
- Never commit `.env` or `.gcp-saved-tokens.json` to Git
- Both are already in the calendar-mcp project's `.gitignore`

## Key Details

- **MCP Server**: `C:\Users\jbris\git\calendar-mcp\run_server.py`
- **Family Calendar ID**: `family08670323936699062610@group.calendar.google.com`
- **Timezone**: Central (CST = UTC-6, CDT = UTC-5)
- **OAuth Tokens**: `C:\Users\jbris\git\calendar-mcp\.gcp-saved-tokens.json`
