# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

CATLINK Analytics Dashboard - A collection of standalone HTML dashboards for visualizing marketing performance data across multiple platforms (Instagram, TikTok, YouTube) and advertising channels.

## Technology Stack

- **Frontend**: Pure HTML5, CSS3, Vanilla JavaScript
- **Charting Libraries**: 
  - ECharts (primary) - loaded via CDN
  - Chart.js (ads dashboard) - loaded via CDN
- **Styling**: 
  - Inline CSS styles
  - Tailwind CSS (ads dashboard only)
- **No Build System**: Static HTML files, no compilation needed

## Project Structure

All dashboard files are located at the root level:
- `dashboard.html` - Main comprehensive dashboard
- `ad-dashboard.html` - Advertising performance with Chart.js
- Platform-specific dashboards: `instagram-dashboard.html`, `tiktok-dashboard.html`, `youtube-dashboard.html`
- Specialized views: `content-performance.html`, `quarterly-dashboard.html`, etc.

## Development Commands

Since this is a static HTML project with no build system:

```bash
# Open any dashboard in browser (macOS)
open dashboard.html

# Start a simple HTTP server for development (if Python installed)
python3 -m http.server 8000

# Then navigate to http://localhost:8000/dashboard.html
```

## Key Code Patterns

### Chart Initialization Pattern
```javascript
const chart = echarts.init(document.getElementById('chart-id'));
const option = {
    tooltip: { trigger: 'axis' },
    legend: { data: [...] },
    series: [...]
};
chart.setOption(option);

// Always include resize handler
window.addEventListener('resize', () => chart.resize());
```

### Common Chart Types Used
- Bar charts for performance comparisons
- Line charts for trends over time
- Pie/Donut charts for distribution
- Funnel charts for conversion analysis
- Radar charts for multi-dimensional metrics

### CSS Design System
- Primary gradient: `linear-gradient(135deg, #667eea 0%, #764ba2 100%)`
- Success: `#10b981`, Warning: `#f59e0b`, Error: `#ef4444`
- Card-based layouts with hover effects
- Consistent shadow: `0 10px 30px rgba(0, 0, 0, 0.1)`

### Business Metrics Calculations
```javascript
// ROAS (Return on Ad Spend)
const roas = sales / spend;

// Conversion Rate
const conversionRate = (orders / clicks) * 100;

// CTR (Click-Through Rate)
const ctr = (clicks / impressions) * 100;
```

## Coding Conventions

- **HTML IDs**: kebab-case (`platform-performance`)
- **CSS Classes**: kebab-case (`metric-card`)
- **JavaScript Variables**: camelCase (`adData`, `roasChart`)
- **Chart Options**: camelCase with "Option" suffix (`platformPerformanceOption`)
- **Language**: Mixed Chinese/English content

## Common Tasks

### Adding a New Dashboard
1. Copy an existing dashboard as template
2. Update the title and navigation
3. Replace chart IDs (must be unique)
4. Update data and chart configurations
5. Maintain consistent styling patterns

### Adding a New Chart
1. Create container: `<div id="unique-chart-id" style="height: 400px;"></div>`
2. Initialize after DOM loaded
3. Include resize handler
4. Follow existing option structure patterns

### Updating Data
Data is currently hardcoded in JavaScript. Look for arrays like:
```javascript
const adData = [
    { date: '2024-01-01', platform: '...', spend: ..., ... }
];
```

## Important Notes

- Each HTML file is self-contained - no shared JavaScript modules
- All external dependencies loaded via CDN
- No environment variables or configuration files
- Charts must be initialized after DOM is ready
- Always include resize handlers for responsive charts
- Maintain consistent visual design across all dashboards