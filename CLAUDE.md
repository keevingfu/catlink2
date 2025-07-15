# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

CATLINK Analytics Dashboard - A static multi-page application (MPA) for visualizing marketing performance data across social media platforms. The architecture uses iframe-based navigation with self-contained dashboard pages, each featuring platform-specific branding and real-time metric calculations.

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
- `index.html` - Main entry point with hierarchical navigation menu
- `catlink-ads_June*.html` - Advertising performance dashboards with Chart.js
- Platform-specific dashboards: `catlink-selfkoc_instagram-performance.html`, `catlink-selfkoc_tiktok-performance.html`, `catlink-selfkoc_youtube-performance.html`
- Weekly performance views: `catlink-selfkoc_week_performance.html`, `catlink-selfkoc_week_detail_performance.html`, `catlink-selfkoc_week_content_performance.html`
- Quarterly overviews: `catlink-quarterly-performance-dashboard.html`, `catlink-selfkoc_q2_overview.html`

## Development Commands

Since this is a static HTML project with no build system:

```bash
# Open main entry point in browser (macOS)
open index.html

# Start a simple HTTP server for development (if Python installed)
python3 -m http.server 8000
# Then navigate to http://localhost:8000/

# Alternative with Node.js (if available)
npx http-server
# Or
npx serve
```

## Architecture Patterns

### Navigation Architecture
The project uses an iframe-based Single Page Application pattern within index.html:
- Hierarchical collapsible menu system with smooth animations
- JavaScript-driven navigation (no URL routing)
- Active state tracking for current dashboard
- Loading indicators during iframe transitions

### Data Architecture
All dashboards follow a consistent data flow:
```
Hardcoded Data Arrays → Client-side Calculations → Chart Libraries → DOM
```
- No external APIs or databases
- Time-specific data embedded in each HTML file
- Real-time metric calculations performed on page load

## Key Code Patterns

### Chart Initialization Pattern
```javascript
// Standard pattern used across all dashboards
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

### Platform-Specific Branding
Each platform dashboard uses unique gradients:
```css
/* Instagram */
background: linear-gradient(135deg, #E1306C 0%, #F77737 50%, #FCAF45 100%);

/* TikTok */
background: #ff0050;

/* YouTube */
background: #ff0000;
```

### Business Metrics Calculations
```javascript
// ROAS (Return on Ad Spend)
const roas = sales / spend;

// Conversion Rate
const conversionRate = (orders / clicks) * 100;

// CTR (Click-Through Rate)
const ctr = (clicks / impressions) * 100;

// Engagement Rate (Social Media)
const engagementRate = ((likes + comments) / views * 100).toFixed(2);
```

### Data Structure Patterns
Common data formats across dashboards:
```javascript
// Social media account data
const accountData = [
    { account: 'catlink_official', views: 12500, likes: 890, comments: 156 }
];

// Time-series performance data
const dailyData = [
    { date: '2025-06-01', platform: 'Instagram', spend: 1000, sales: 3500 }
];
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
- The project uses a hierarchical collapsible menu structure in index.html for navigation
- Data is time-specific (e.g., June 2025) and hardcoded for each dashboard
- ECharts version: 5.4.3 (loaded from cdnjs.cloudflare.com)

## Cross-Dashboard Patterns

### Responsive Grid Layouts
All dashboards use consistent responsive patterns:
```css
.metrics-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 25px;
}
```

### Animation Patterns
- Card hover effects: `transform: translateY(-5px)`
- Smooth transitions: `transition: all 0.3s ease`
- Loading states during iframe navigation

### Performance Considerations
- Dashboards are loaded on-demand via iframes (lazy loading)
- Chart resize handlers prevent layout issues
- Minimal DOM manipulation for better performance