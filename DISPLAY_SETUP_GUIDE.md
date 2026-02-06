# Boathouse Battle Display - Setup Guide

## What Changed

### Major Improvements

1. **Dynamic Team Support**
   - Works with any number of teams (2, 3, 4, or more)
   - Automatically detects teams from Google Sheets
   - No hardcoded team names or colors

2. **Cleaner Code Structure**
   - Configuration section at top
   - Separated functions by responsibility
   - Better variable names
   - Removed duplicate/dead code

3. **Better Responsive Design**
   - Grid layout adapts to screen size
   - Mobile-friendly countdown timer
   - Flexible team display

4. **Improved Race Track**
   - Dynamic lane creation based on number of teams
   - Automatic boat positioning
   - Cleaner buoy rendering

5. **Automatic Data Parsing**
   - Reads team names from Scores sheet headers
   - No need to hardcode column positions
   - Handles variable number of teams automatically

## Configuration

### 1. Update Your Spreadsheet URL

Find this section at the top of the `<script>` tag:

```javascript
const CONFIG = {
  spreadsheetUrl: 'https://docs.google.com/spreadsheets/d/e/YOUR_SHEET_ID/pub?gid=YOUR_GID&single=true&output=csv',
  // ...
};
```

**How to get your URL:**
1. Open your Google Sheet
2. File → Share → Publish to web
3. Choose "Scores" sheet
4. Format: CSV
5. Copy the URL

### 2. Configure Your Teams

Update the `teams` array with your team info:

```javascript
teams: [
  { name: 'Team Red', color: '#b70000', boatImage: 'media/rowing_shell_red.png' },
  { name: 'Team Green', color: '#167401', boatImage: 'media/rowing_shell_green.png' },
  { name: 'Team Blue', color: '#0025ccff', boatImage: 'media/rowing_shell_blue.png' },
  { name: 'Team Purple', color: '#b406ffc8', boatImage: 'media/rowing_shell_purple.png' }
]
```

**Important:** Team names must match exactly (case-insensitive) with your Scores sheet column headers.

### 3. Update Competition End Date

```javascript
endDate: new Date('September 7, 2025 20:00:00 GMT-0700'),
```

### 4. Configure Week Settings

```javascript
weekSettings: {
  1: { minCourse: 2000, buoySpacing: 250 },
  2: { minCourse: 2000, buoySpacing: 250 },
  3: { minCourse: 2000, buoySpacing: 250 },
  4: { minCourse: 10000, buoySpacing: 1000 } // FINAL week
}
```

## Google Sheets Format Expected

Your published Scores sheet should look like this:

```
             A          B      C      D      E      F    G        H             I      J      K      L      M
Row 1:  Team: PORT    Week1  Week2  Week3  Week4  Total [BLACK] Team: STARBOARD Week1  Week2  Week3  Week4  Total ...
Row 2:  Total         1250   2340   1890   3120   8600         Total           1180   2210   2050   3340   8780  ...
Row 3:  (blank)
Row 4:  Alice Smith   450    680    520    890    2540         Bob Johnson     380    590    640    920    2530  ...
Row 5:  David Lee     320    540    410    720    1990         Emma Wilson     290    480    530    810    2110  ...
```

**Critical Requirements:**
- Row 1 must contain "Team: TEAMNAME" headers
- Row 2 must contain team totals
- Row 4+ contain individual player scores
- Each team section has columns: TeamName, Week1, Week2, Week3, Week4, Total

## Features

### Auto-Detection

The display automatically:
- **Detects number of teams** from Scores sheet headers
- **Finds team names** from "Team: NAME" pattern
- **Determines number of weeks** by counting Week columns
- **Calculates winners** by comparing team totals per week
- **Creates appropriate number of lanes** in race track

### Race Track

- **Dynamic lanes**: One lane per team
- **Boats positioned** based on team scores
- **Buoys spaced** according to week settings
- **Course length** adjusts to highest score

### Week Buttons

- **Winner indicators**: Shows 👑 and team name for winning weeks
- **Active highlighting**: Current week is highlighted
- **Hover effects**: Smooth transitions
- **Auto-generated**: Creates buttons for all weeks found in data

### Leaderboards

- **Top 3 players** per team
- **Medal icons**: 🥇 🥈 🥉
- **Sorted by score**: Highest to lowest
- **Auto-updating**: Refreshes every 15 seconds

### Countdown Timer

- **Digital clock style** display
- **Auto-updates** every second
- **Stops at zero** when competition ends
- **Responsive sizing** on mobile

## File Structure

```
your-project/
├── boathouse_battle_display.html (main file)
└── media/
    ├── boathouse_battle_icon.png
    ├── digital-7.ttf
    ├── rowing_shell_red.png
    ├── rowing_shell_green.png
    ├── rowing_shell_blue.png
    └── rowing_shell_orange.png
```

## Adding More Teams

To support 5 or 6 teams:

1. **Add team to CONFIG.teams:**
```javascript
{ name: 'Midship', color: '#9900cc', boatImage: 'media/rowing_shell_purple.png' }
```

2. **Add boat image** to media folder

3. **Ensure Scores sheet** has corresponding team section

That's it! The display will automatically:
- Create 5 lanes in the race track
- Display 5 team sections
- Show all 5 teams in results

## Customization Options

### Change Refresh Rate

```javascript
refreshInterval: 15000, // milliseconds (15 seconds)
```

### Change Default Week

```javascript
defaultWeek: 1, // Week to show on page load
```

### Adjust Track Width

In CSS:
```css
.race-track {
  width: 90%; /* Percentage of screen width */
  max-width: 1400px; /* Maximum width */
}
```

### Change Team Grid Layout

In CSS:
```css
.teams-container {
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  /* Change 300px to adjust minimum team card width */
}
```

## Troubleshooting

### Teams Not Showing Up

**Check:**
- Team names in CONFIG match Scores sheet headers exactly
- Scores sheet is published as CSV
- Headers follow "Team: TEAMNAME" format

### Boats Not Moving

**Check:**
- Scores sheet Row 2 has numeric totals
- Week number is valid (1-4 by default)
- JavaScript console for errors

### Winners Not Highlighting

**Check:**
- Row 2 totals are numbers, not text
- All teams have data for that week
- Multiple teams don't have identical scores (creates tie = no highlight)

### Page Looks Broken

**Check:**
- All image files exist in media/ folder
- Font file (digital-7.ttf) is present
- Browser console for 404 errors

### Data Not Updating

**Check:**
- Spreadsheet is published (not just shared)
- Published as CSV, not HTML
- Browser cache (try hard refresh: Ctrl+Shift+R)

## Browser Compatibility

Works in all modern browsers:
- ✅ Chrome/Edge (recommended)
- ✅ Firefox
- ✅ Safari
- ✅ Mobile browsers (iOS/Android)

## Performance Notes

- Fetches data every 15 seconds by default
- Lightweight: ~10KB HTML + CSS + JS
- No external dependencies
- Fast rendering even with many teams

## Advanced: Adding Custom Animations

You can add animation effects by modifying the CSS transitions:

```css
.boat {
  transition: left 1s ease-in-out; /* Slower boat movement */
}

.team-score {
  transition: color 0.3s ease;
}

.team-score:hover {
  transform: scale(1.05); /* Slight zoom on hover */
}
```

## Support

Common customizations:
1. **Different week lengths**: Edit CONFIG.weekSettings
2. **Different colors**: Edit CONFIG.teams color values
3. **Different boat images**: Replace files in media/ folder
4. **Different layout**: Modify CSS grid in .teams-container
