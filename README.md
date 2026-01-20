# Google Sheets Todo List

A simple web app that displays your to-do list from a Google Sheet.

## Quick Start

1. Open `index.html` in your browser
2. Enter your Google Sheets configuration
3. View your todos!

## Setup

### Step 1: Prepare Your Google Sheet

Create a Google Sheet with columns like:

| Task | Status | Priority | Due Date |
|------|--------|----------|----------|
| Buy groceries | pending | high | 2024-01-25 |
| Call mom | done | medium | |
| Finish report | pending | high | 2024-01-22 |

**Supported columns** (flexible naming):
- **Task**: task, todo, item, title, name, description
- **Status**: status, done, completed, complete, checked
- **Priority**: priority, importance, level
- **Due Date**: due, due date, deadline, date

**Completed status values**: done, complete, completed, yes, true, 1, x, checkmark

### Step 2: Make Your Sheet Public

1. Open your Google Sheet
2. Click **Share** (top right)
3. Under "General access", select **Anyone with the link**
4. Set permission to **Viewer**
5. Click **Done**

### Step 3: Get Your Spreadsheet ID

Your spreadsheet URL looks like:
```
https://docs.google.com/spreadsheets/d/1AbC123XyZ.../edit
```
The ID is the part between `/d/` and `/edit`: `1AbC123XyZ...`

### Step 4: Get a Google API Key

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project (or select existing)
3. Enable the **Google Sheets API**:
   - Go to **APIs & Services** > **Library**
   - Search for "Google Sheets API"
   - Click **Enable**
4. Create an API key:
   - Go to **APIs & Services** > **Credentials**
   - Click **Create Credentials** > **API Key**
   - Copy the key

**Optional**: Restrict the API key to only Google Sheets API for security.

### Step 5: Connect

1. Open `index.html` in your browser
2. Enter your Spreadsheet ID
3. Enter your API Key
4. Click **Connect**

Your credentials are saved in browser localStorage for convenience.

## Features

- Auto-detects column names
- Shows task completion status
- Displays priority badges (high/medium/low)
- Shows due dates
- Stats dashboard (total/completed/pending)
- Refresh button to reload data
- Responsive design
- Works offline after initial load

## Customizing

### Change the sheet/range

The default range is `Sheet1!A:D`. Change this to:
- Different sheet: `MyTasks!A:D`
- More columns: `Sheet1!A:F`
- Specific range: `Sheet1!A1:D100`

### Styling

All CSS is in the `<style>` tag in `index.html`. Customize colors, fonts, etc.

## Troubleshooting

**"API key not valid"**
- Make sure Google Sheets API is enabled in your Cloud project
- Check that the API key is correct

**"The caller does not have permission"**
- Make sure your sheet is shared as "Anyone with the link can view"

**No todos showing**
- Check that your sheet has data in the specified range
- Verify column headers exist in the first row

## License

MIT
