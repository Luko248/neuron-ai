---
agent: agent
---

# Chart Component Guidelines

## Overview

The Chart component is a comprehensive wrapper around PrimeReact's Chart component, built on Chart.js. It provides a simplified interface for creating various types of charts including bar, line, pie, combo (mixed), and stacked charts with automatic theme support and consistent styling.

## Component Structure

```typescript
import { Chart, ChartColors } from '@neuron/ui';

// Basic usage
<Chart
  type="bar"
  data={chartData}
  options={chartOptions}
  width="900px"
  height="300px"
/>
```

## Data Structure

The Chart component accepts a simplified data structure through the `data` prop:

```typescript
type SimplifiedChartData = {
  labels: string[]; // X-axis labels or categories
  datasets: ChartDataSet[]; // Array of datasets to plot
};

type ChartDataSet = {
  label?: string; // Dataset label (shown in legend)
  type?: ChartType; // Specific chart type for combo charts
  data: (number | null)[]; // Data points
  backgroundColor?: string | string[]; // Fill colors
  borderColor?: string | string[]; // Border colors
  borderWidth?: number; // Border thickness
  fill?: boolean; // Fill area under line (line charts)
  tension?: number; // Curve tension (0 = straight lines)
  // Additional Chart.js dataset properties
};
```

## Chart Types

### Bar Chart

```typescript
const barData: SimplifiedChartData = {
  labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May'],
  datasets: [
    {
      label: 'Sales',
      backgroundColor: ChartColors.CelestialBlueOpacity,
      borderColor: ChartColors.CelestialBlue,
      data: [65, 59, 80, 81, 56],
      borderWidth: 2,
      hoverBackgroundColor: ChartColors.CelestialBlue,
    }
  ]
};

<Chart type="bar" data={barData} height="300px" width="900px" />
```

### Line Chart

```typescript
const lineData: SimplifiedChartData = {
  labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May'],
  datasets: [
    {
      label: 'Revenue',
      data: [65, 59, 80, 81, 56],
      fill: false,
      borderColor: ChartColors.CelestialBlue,
      tension: 0.4,
      pointBackgroundColor: ChartColors.CelestialBlue,
    }
  ]
};

<Chart type="line" data={lineData} height="300px" width="900px" />
```

### Pie Chart

```typescript
const pieData: SimplifiedChartData = {
  labels: ['Product A', 'Product B', 'Product C'],
  datasets: [
    {
      data: [300, 50, 100],
      backgroundColor: [
        ChartColors.MajorelleBlue,
        ChartColors.AppleGreen,
        ChartColors.SandyBrown,
      ]
    }
  ]
};

<Chart type="pie" data={pieData} height="300px" width="300px" />
```

### Combo Chart (Mixed Line and Bar)

```typescript
const comboData: SimplifiedChartData = {
  labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May'],
  datasets: [
    {
      type: 'line',
      label: 'Trend Line',
      borderColor: ChartColors.CelestialBlue,
      data: [50, 25, 12, 48, 56],
      fill: false,
      tension: 0.4,
    },
    {
      type: 'bar',
      label: 'Sales Volume',
      backgroundColor: ChartColors.EmeraldGreenOpacity,
      borderColor: ChartColors.EmeraldGreen,
      data: [21, 84, 24, 75, 37],
      borderWidth: 2,
    }
  ]
};

<Chart type="line" data={comboData} height="300px" width="900px" />
```

### Stacked Bar Chart

```typescript
const stackedData: SimplifiedChartData = {
  labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May'],
  datasets: [
    {
      label: 'Product A',
      backgroundColor: ChartColors.CelestialBlueOpacity,
      borderColor: ChartColors.CelestialBlue,
      data: [50, 25, 12, 48, 90],
      borderWidth: 2,
    },
    {
      label: 'Product B',
      backgroundColor: ChartColors.EmeraldGreenOpacity,
      borderColor: ChartColors.EmeraldGreen,
      data: [21, 84, 24, 75, 37],
      borderWidth: 2,
    }
  ]
};

const stackedOptions = {
  scales: {
    x: { stacked: true },
    y: { stacked: true }
  }
};

<Chart type="bar" data={stackedData} options={stackedOptions} />
```

## Color System

### ChartColors Enum

**MANDATORY**: Always use the `ChartColors` enum for consistent color styling:

```typescript
import { ChartColors } from "@neuron/ui";

// Available color pairs (solid and opacity versions):
ChartColors.AppleGreen / ChartColors.AppleGreenOpacity;
ChartColors.Blue / ChartColors.BlueOpacity;
ChartColors.Business / ChartColors.BusinessOpacity;
ChartColors.CelestialBlue / ChartColors.CelestialBlueOpacity;
ChartColors.Citrine / ChartColors.CitrineOpacity;
ChartColors.ElectricPurple / ChartColors.ElectricPurpleOpacity;
ChartColors.EmeraldGreen / ChartColors.EmeraldGreenOpacity;
ChartColors.Green / ChartColors.GreenOpacity;
ChartColors.IndianRed / ChartColors.IndianRedOpacity;
ChartColors.Life / ChartColors.LifeOpacity;
ChartColors.MajorelleBlue / ChartColors.MajorelleBlueOpacity;
ChartColors.Orange / ChartColors.OrangeOpacity;
ChartColors.Property / ChartColors.PropertyOpacity;
ChartColors.Red / ChartColors.RedOpacity;
ChartColors.SandyBrown / ChartColors.SandyBrownOpacity;
ChartColors.Travel / ChartColors.TravelOpacity;
ChartColors.Vehicles / ChartColors.VehiclesOpacity;
```

### Color Usage Patterns

- **Opacity colors** (e.g., `BlueOpacity`): Use for `backgroundColor`, area fills, and low-emphasis elements
- **Solid colors** (e.g., `Blue`): Use for `borderColor`, `hoverBackgroundColor`, and emphasis states
- **Product colors**: Use `Business`, `Life`, `Property`, `Travel`, `Vehicles` for insurance domain data

## Chart Options

### Basic Options Structure

```typescript
const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: {
      position: "top" as const,
      labels: {
        usePointStyle: true, // For pie charts
      },
    },
    tooltip: {
      mode: "index" as const,
      intersect: false,
    },
  },
  scales: {
    x: {
      beginAtZero: true,
      grid: {
        display: false, // Hide grid lines
      },
    },
    y: {
      beginAtZero: true,
      grid: {
        display: true,
      },
    },
  },
};
```

### Responsive Behavior

The Chart component automatically handles theme changes (light/dark mode) and provides responsive options:

```typescript
import { useResponsiveMediaQuery } from '@neuron/core';

const { isTablet, isMobile } = useResponsiveMediaQuery();

const responsiveHeight = isMobile ? '250px' : isTablet ? '300px' : '400px';
const responsiveWidth = isMobile ? '100%' : '900px';

<Chart
  type="bar"
  data={chartData}
  height={responsiveHeight}
  width={responsiveWidth}
/>
```

## Advanced Configuration

### Custom Scales Configuration

```typescript
const advancedOptions = {
  scales: {
    x: {
      type: "category",
      position: "bottom",
      grid: {
        display: false,
      },
      ticks: {
        maxRotation: 45,
        minRotation: 0,
      },
    },
    y: {
      beginAtZero: true,
      grid: {
        color: "rgba(0,0,0,0.1)",
      },
      ticks: {
        callback: function (value: number) {
          return `$${value}k`;
        },
      },
    },
  },
};
```

### Animation Configuration

```typescript
const animationOptions = {
  animation: {
    duration: 1000,
    easing: "easeInOutQuart",
    animateRotate: true,
    animateScale: true,
  },
};
```

### Custom Tooltips

```typescript
const tooltipOptions = {
  plugins: {
    tooltip: {
      callbacks: {
        label: function (context: any) {
          return `${context.dataset.label}: $${context.parsed.y}k`;
        },
        title: function (context: any) {
          return `Period: ${context[0].label}`;
        },
      },
    },
  },
};
```

## Data Integration Patterns

### API Data Transformation

```typescript
// Transform API response to chart data format
const transformApiData = (apiResponse: ApiChartData): SimplifiedChartData => {
  return {
    labels: apiResponse.periods.map((p) => p.label),
    datasets: apiResponse.series.map((series, index) => ({
      label: series.name,
      data: series.values,
      backgroundColor: getChartColor(index, true), // opacity version
      borderColor: getChartColor(index, false), // solid version
      borderWidth: 2,
    })),
  };
};

const getChartColor = (index: number, withOpacity: boolean): string => {
  const colors = [
    { solid: ChartColors.CelestialBlue, opacity: ChartColors.CelestialBlueOpacity },
    { solid: ChartColors.EmeraldGreen, opacity: ChartColors.EmeraldGreenOpacity },
    { solid: ChartColors.SandyBrown, opacity: ChartColors.SandyBrownOpacity },
    { solid: ChartColors.MajorelleBlue, opacity: ChartColors.MajorelleBlueOpacity },
    { solid: ChartColors.IndianRed, opacity: ChartColors.IndianRedOpacity },
  ];

  const colorPair = colors[index % colors.length];
  return withOpacity ? colorPair.opacity : colorPair.solid;
};
```

### Dynamic Data Updates

```typescript
const [chartData, setChartData] = useState<SimplifiedChartData>(initialData);

// Update chart data based on filters
const updateChartData = (newFilters: ChartFilters) => {
  fetchChartData(newFilters).then((apiData) => {
    const transformedData = transformApiData(apiData);
    setChartData(transformedData);
  });
};

useEffect(() => {
  updateChartData(currentFilters);
}, [currentFilters]);
```

## Props Reference

| Prop        | Type                  | Required | Default | Description                               |
| ----------- | --------------------- | -------- | ------- | ----------------------------------------- |
| `type`      | `ChartType`           | Yes      | -       | Type of chart: 'bar', 'line', 'pie', etc. |
| `data`      | `SimplifiedChartData` | Yes      | -       | Chart data with labels and datasets       |
| `options`   | `ChartOptions`        | No       | `{}`    | Chart.js configuration options            |
| `width`     | `string`              | No       | -       | Chart width (e.g., '900px', '100%')       |
| `height`    | `string`              | No       | -       | Chart height (e.g., '300px')              |
| `className` | `string`              | No       | -       | Additional CSS class                      |
| `style`     | `React.CSSProperties` | No       | -       | Inline styles                             |

## Accessibility Features

### ARIA Support

```typescript
const accessibleOptions = {
  plugins: {
    legend: {
      labels: {
        generateLabels: (chart: any) => {
          // Generate accessible legend labels
          return chart.data.datasets.map((dataset: any, i: number) => ({
            text: dataset.label,
            fillStyle: dataset.backgroundColor,
            hidden: !chart.isDatasetVisible(i),
            datasetIndex: i,
          }));
        },
      },
    },
  },
};
```

### Keyboard Navigation

- Charts are focusable and navigable using keyboard
- Legend items can be toggled with keyboard
- Tooltip information is accessible via screen readers

## Best Practices

### Data Preparation

1. **Validate data structure** before passing to Chart component
2. **Handle null/undefined values** in datasets appropriately
3. **Use consistent labeling** across similar charts
4. **Limit dataset count** to maintain readability (max 5-7 datasets)

### Performance Optimization

1. **Memoize chart data** when possible to prevent unnecessary re-renders
2. **Use appropriate chart types** for data visualization needs
3. **Implement responsive sizing** based on container and screen size
4. **Optimize animation settings** for better performance on lower-end devices

### Visual Design

1. **Always use ChartColors enum** for consistent theming
2. **Maintain color accessibility** standards (sufficient contrast)
3. **Use opacity versions** for backgrounds and solid versions for emphasis
4. **Choose appropriate chart types** based on data relationships
5. **Provide meaningful labels** and legends

### Error Handling

```typescript
const SafeChart: React.FC<ChartProps> = (props) => {
  if (!props.data?.labels?.length || !props.data?.datasets?.length) {
    return <div>No data available for chart</div>;
  }

  return <Chart {...props} />;
};
```

## Common Mistakes

❌ **Don't do this:**

```typescript
// Using custom colors instead of ChartColors enum
backgroundColor: '#FF5733'

// Missing data validation
<Chart type="bar" data={undefined} />

// Inconsistent sizing
<Chart width="100%" height="auto" />

// Too many datasets without clear distinction
datasets: [dataset1, dataset2, ..., dataset15]
```

✅ **Do this:**

```typescript
// Use ChartColors enum
backgroundColor: ChartColors.CelestialBlueOpacity

// Validate data before rendering
{data?.labels?.length && <Chart type="bar" data={data} />}

// Consistent sizing with explicit dimensions
<Chart width="900px" height="300px" />

// Reasonable number of datasets with clear colors
datasets: [dataset1, dataset2, dataset3] // Max 5-7 for readability
```

## Integration Examples

### Dashboard Widget

```typescript
const DashboardChart: React.FC<{ metrics: MetricsData }> = ({ metrics }) => {
  const chartData = useMemo(() => transformMetricsToChartData(metrics), [metrics]);
  const { isMobile } = useResponsiveMediaQuery();

  const options = {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
      legend: {
        position: isMobile ? 'bottom' : 'top' as const,
      }
    }
  };

  return (
    <div className="dashboard-chart">
      <Chart
        type="line"
        data={chartData}
        options={options}
        height={isMobile ? "250px" : "300px"}
        width="100%"
      />
    </div>
  );
};
```

### Report Chart with Export

```typescript
const ReportChart: React.FC<{ reportData: ReportData }> = ({ reportData }) => {
  const chartRef = useRef<any>(null);

  const exportChart = () => {
    if (chartRef.current) {
      const base64Image = chartRef.current.toBase64Image();
      // Handle export logic
    }
  };

  return (
    <div>
      <Chart
        ref={chartRef}
        type="bar"
        data={reportData}
        height="400px"
        width="100%"
      />
      <button onClick={exportChart}>Export Chart</button>
    </div>
  );
};
```

## For the AI Assistant

When implementing Chart components, you must:

### 🚨 MANDATORY Rules

1. **ALWAYS use ChartColors enum** - Never use custom hex colors or inline color values
2. **VALIDATE data structure** - Ensure data.labels and data.datasets exist before rendering
3. **USE opacity colors for backgrounds** - Use `*Opacity` variants for `backgroundColor`
4. **USE solid colors for borders** - Use solid color variants for `borderColor` and hover states
5. **IMPLEMENT responsive sizing** - Use explicit width/height or integrate with useResponsiveMediaQuery
6. **LIMIT dataset count** - Maximum 5-7 datasets for readability
7. **PROVIDE meaningful labels** - Always include dataset labels for legends and accessibility

### Chart Type Selection

- **Bar charts**: For categorical comparisons, discrete data points
- **Line charts**: For trends over time, continuous data
- **Pie charts**: For parts of a whole, percentage breakdowns (max 5-6 segments)
- **Combo charts**: For different data types in same visualization
- **Stacked charts**: For showing composition within categories

### Color Selection Strategy

When multiple datasets are needed:

1. Start with `CelestialBlue` / `CelestialBlueOpacity`
2. Add `EmeraldGreen` / `EmeraldGreenOpacity`
3. Continue with `SandyBrown`, `MajorelleBlue`, `IndianRed`
4. For insurance products, use `Business`, `Life`, `Property`, `Travel`, `Vehicles`

### Data Transformation Pattern

Always follow this pattern for API integration:

```typescript
const transformApiData = (apiData: ApiResponse): SimplifiedChartData => {
  return {
    labels: apiData.categories || apiData.periods || apiData.labels,
    datasets: apiData.series.map((series, index) => ({
      label: series.name,
      data: series.values,
      backgroundColor: getChartColor(index, true),
      borderColor: getChartColor(index, false),
      borderWidth: 2,
    })),
  };
};
```

### Responsive Implementation

Always implement responsive behavior:

```typescript
const { isMobile, isTablet } = useResponsiveMediaQuery();
const height = isMobile ? "250px" : isTablet ? "300px" : "400px";
```

### Required Options Structure

Always include these minimum options for consistency:

```typescript
const options = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: {
      position: "top" as const,
    },
  },
  scales:
    chartType !== "pie"
      ? {
          y: {
            beginAtZero: true,
          },
        }
      : undefined,
};
```

If you encounter chart data or need to implement charts, **NEVER create custom chart components** - always use the Neuron Chart component with the ChartColors enum and follow these guidelines exactly.

## Sync Metadata

- **Component Version:** v1.0.1
- **Component Source:** `packages/neuron/ui/src/lib/data/chart/Chart.tsx`
- **Guideline Command:** `/neuron-ui-chart`
- **Related Skill:** `neuron-ui-data`
