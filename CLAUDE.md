# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

CATLINK Analytics Dashboard - A static multi-page application (MPA) for visualizing marketing performance data across social media platforms. The architecture uses iframe-based navigation with self-contained dashboard pages, each featuring platform-specific branding and real-time metric calculations.

## Technology Stack

- **Frontend**: Pure HTML5, CSS3, Vanilla JavaScript
- **Charting Libraries**: 
  - ECharts 5.4.3 (primary) - loaded via CDN
  - Chart.js 4.4.0 (ads dashboards) - loaded via CDN
- **Styling**: 
  - Inline CSS styles
  - Tailwind CSS (ads dashboards only) - loaded via CDN
- **No Build System**: Static HTML files, no compilation needed

## Project Structure

All dashboard files are located at the root level:
- `index.html` - Main entry point with hierarchical navigation menu
- `catlink-ads_*.html` - Advertising performance dashboards with Chart.js
- `catlink-selfkoc_*.html` - Self-KOC social media performance dashboards
- `catlink_ads_meta-*.html` - Meta (Facebook) ads specific dashboards
- Platform-specific: `*_instagram-performance.html`, `*_tiktok_*.html`, `*_youtube-*.html`
- Time period views: `*_weekly_*.html`, `*_quarterly-*.html`, `*_q2_overview.html`

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
- Menu structure supports nested categories with expand/collapse functionality

### Data Architecture
All dashboards follow a consistent data flow:
```
Hardcoded Data Arrays → Client-side Calculations → Chart Libraries → DOM
```
- No external APIs or databases
- Time-specific data embedded in each HTML file
- Real-time metric calculations performed on page load
- Data structures vary by dashboard type (ads vs. social media)

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
- Scatter plots for correlation analysis (CTR vs CPC)
- Mixed charts (bar + line) for comprehensive views

### CSS Design System

#### Color Palette
- Primary gradient: `linear-gradient(135deg, #667eea 0%, #764ba2 100%)`
- Success: `#10b981`, Warning: `#f59e0b`, Error: `#ef4444`, Info: `#3b82f6`
- Card-based layouts with hover effects
- Consistent shadow: `0 10px 30px rgba(0, 0, 0, 0.1)`

#### Platform-Specific Branding
```css
/* Instagram */
background: linear-gradient(135deg, #E1306C 0%, #F77737 50%, #FCAF45 100%);

/* TikTok */
background: #ff0050;

/* YouTube */
background: #ff0000;

/* Facebook/Meta */
background: linear-gradient(135deg, #1877f2 0%, #42b3f4 100%);
```

#### Dark Theme Support
Recent dashboards include dark theme styling:
```css
background: linear-gradient(135deg, #1a1a1a 0%, #0a0a0a 100%);
backdrop-filter: blur(10px);
```

### Business Metrics Calculations
```javascript
// ROAS (Return on Ad Spend)
const roas = sales / spend;

// Conversion Rate
const conversionRate = (orders / clicks) * 100;

// CTR (Click-Through Rate)
const ctr = (clicks / impressions) * 100;

// CPM (Cost Per Mille)
const cpm = (spend / impressions) * 1000;

// Engagement Rate (Social Media)
const engagementRate = ((likes + comments) / views * 100).toFixed(2);

// Cost Per Engagement
const costPerEngagement = adSpend / totalEngagement;
```

### Data Structure Patterns
```javascript
// Social media account data
const accountData = [
    { account: 'catlink_official', views: 12500, likes: 890, comments: 156 }
];

// Time-series performance data
const dailyData = [
    { date: '2025-06-01', platform: 'Instagram', spend: 1000, sales: 3500 }
];

// Campaign performance data
const campaignData = [
    { week: 1, spend: 356.11, ctr: 1.67, cpc: 0.80, cpm: 13.27, revenue: 3090.91, roas: 8.68 }
];

// Video content data
const topVideos = [
    { platform: 'tiktok', url: '...', likes: 371, comments: 3, views: 243000, date: '2025/7/14' }
];
```

## Coding Conventions

- **HTML IDs**: kebab-case (`platform-performance`)
- **CSS Classes**: kebab-case (`metric-card`, `kpi-section`)
- **JavaScript Variables**: camelCase (`adData`, `roasChart`)
- **Chart Options**: camelCase with "Option" suffix (`platformPerformanceOption`)
- **Language**: Mixed Chinese/English content in UI, English-only in code

## Common Tasks

### Adding a New Dashboard
1. Copy an existing dashboard as template (choose based on data type)
2. Update the title and page metadata
3. Replace all chart IDs (must be unique)
4. Update data arrays with new metrics
5. Maintain consistent styling patterns
6. Add to index.html navigation menu

### Adding a New Chart
1. Create container: `<div id="unique-chart-id" style="height: 400px;"></div>`
2. Initialize after DOM loaded or in DOMContentLoaded event
3. Include resize handler for responsiveness
4. Follow existing option structure patterns
5. Use consistent color schemes based on dashboard type

### Implementing Video Preview Feature
For dashboards with video content:
```javascript
// Lazy loading with Intersection Observer
const imageObserver = new IntersectionObserver((entries, observer) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            // Load video thumbnail
        }
    });
});

// Inline video preview
function playVideo(platform, videoId, button) {
    // Stop other videos
    // Load appropriate embed based on platform
}
```

### Updating Data
Data is currently hardcoded in JavaScript. Look for arrays typically named:
- `adData` - advertising metrics
- `platformData` - platform-specific metrics
- `weeklyMetrics` - time-based aggregations
- `topVideos` - content performance data

## Important Notes

- Each HTML file is self-contained - no shared JavaScript modules
- All external dependencies loaded via CDN (no local dependencies)
- No environment variables or configuration files
- Charts must be initialized after DOM is ready
- Always include resize handlers for responsive charts
- Maintain consistent visual design across all dashboards
- Data is time-specific and hardcoded for each dashboard
- Insight boxes with recommendations are standard for new dashboards
- Performance optimization through lazy loading for media content
- Git repository initialized but no CI/CD pipeline

## Recent Dashboard Patterns

### KPI Cards Structure
```html
<div class="kpi-card success">
    <div class="kpi-icon"><!-- SVG icon --></div>
    <div class="kpi-label">Metric Name</div>
    <div class="kpi-value">$19.6K</div>
    <div class="kpi-change positive"><!-- Change indicator --></div>
    <div class="kpi-period">Time Period</div>
</div>
```

### Insight Box Pattern
```html
<div class="insight-box">
    <div class="insight-title">
        <svg><!-- Icon --></svg>
        Key Insights
    </div>
    <div class="insight-content">
        <strong>Main insight</strong> with supporting details.
        <ul class="recommendation-list">
            <li>Actionable recommendation</li>
        </ul>
    </div>
</div>
```

### Performance Considerations
- Dashboards are loaded on-demand via iframes (lazy loading)
- Chart resize handlers prevent layout issues
- Minimal DOM manipulation for better performance
- Video thumbnails use lazy loading with Intersection Observer
- Skeleton screens for loading states