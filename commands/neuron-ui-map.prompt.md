---
agent: agent
---

# AI-Assisted Neuron Map Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Map components in a React application. This guide provides instructions for implementing the Map component, which renders interactive maps using the Leaflet library with customizable markers, zoom controls, and action buttons.

## Sync Metadata

- **Component Version:** v3.1.0
- **Component Source:** `packages/neuron/ui/src/lib/patterns/map/Map.tsx`
- **Guideline Command:** `/neuron-ui-map`
- **Related Skill:** `neuron-ui-composites`

## Introduction

The Map component is a composite pattern component that provides interactive map functionality using Leaflet. It supports customizable positioning, zoom controls, markers, and action buttons for map-related operations.

### Key Features

- **Interactive Maps**: Built on Leaflet library for smooth map interactions
- **Customizable Markers**: Show/hide markers with custom positioning
- **Zoom Controls**: Configurable zoom levels and zoom restrictions
- **Drag Controls**: Enable/disable map dragging
- **Action Buttons**: Add custom action buttons for map operations
- **Responsive Sizing**: Configurable width and height
- **API Integration**: Uses Mapy.cz tiles with API key support
- **Click Geocoding**: Optional `onMapClick` prop triggers reverse geocoding via Mapy.com API and returns structured address data

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Map component.

## Step 1: Basic Map Implementation

### 1.1 Import the Map Component

```tsx
import { Map } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the Map component:

```tsx
import { Map } from "@neuron/ui";

const BasicMapExample = () => {
  return <Map lat={49.195061} lng={16.606836} width="400px" height="300px" />;
};
```

### 1.3 Map with Custom Settings

```tsx
import { Map } from "@neuron/ui";

const CustomMapExample = () => {
  return (
    <Map
      lat={50.0755}
      lng={14.4378} // Prague coordinates
      width="600px"
      height="400px"
      zoom={12}
      allowZoom={true}
      allowDraging={true}
      showMarker={true}
    />
  );
};
```

## Step 2: Map with Action Buttons

### 2.1 Map with External Links

```tsx
import { Map, baseIcons } from "@neuron/ui";

const MapWithActions = () => {
  const handleKatastrClick = () => {
    window.open("https://nahlizenidokn.cuzk.cz/", "_blank");
  };

  const handleRuianClick = () => {
    window.open("https://vdp.cuzk.cz/", "_blank");
  };

  return (
    <Map
      lat={49.195061}
      lng={16.606836}
      width="500px"
      height="350px"
      actions={[
        {
          children: "Katastr nemovitostí",
          variant: "plain",
          iconRight: baseIcons.arrowUpRightFromSquareRegular,
          onClick: handleKatastrClick,
        },
        {
          children: "RUIAN",
          variant: "plain",
          iconRight: baseIcons.arrowUpRightFromSquareRegular,
          onClick: handleRuianClick,
        },
      ]}
    />
  );
};
```

### 2.2 Map with Custom Actions

```tsx
import { Map, baseIcons } from "@neuron/ui";

const MapWithCustomActions = () => {
  const handleDirections = () => {
    const url = `https://maps.google.com/maps?daddr=${49.195061},${16.606836}`;
    window.open(url, "_blank");
  };

  const handleShare = () => {
    const url = `https://maps.google.com/maps?q=${49.195061},${16.606836}`;
    navigator.clipboard.writeText(url);
    alert("Map location copied to clipboard!");
  };

  const handleFullscreen = () => {
    // Implement fullscreen logic
    console.info("Opening map in fullscreen");
  };

  return (
    <Map
      lat={49.195061}
      lng={16.606836}
      width="100%"
      height="400px"
      zoom={15}
      actions={[
        {
          children: "Get Directions",
          variant: "secondary",
          iconLeft: baseIcons.locationArrowRegular,
          onClick: handleDirections,
        },
        {
          children: "Share Location",
          variant: "plain",
          iconLeft: baseIcons.shareRegular,
          onClick: handleShare,
        },
        {
          children: "Fullscreen",
          variant: "plain",
          iconLeft: baseIcons.expandRegular,
          onClick: handleFullscreen,
        },
      ]}
    />
  );
};
```

## Step 3: Dynamic Map Content

### 3.1 Map with Dynamic Coordinates

```tsx
import { Map } from "@neuron/ui";
import { useState } from "react";

const DynamicMapExample = () => {
  const [coordinates, setCoordinates] = useState({
    lat: 49.195061,
    lng: 16.606836,
  });

  const locations = [
    { name: "Brno", lat: 49.195061, lng: 16.606836 },
    { name: "Prague", lat: 50.0755, lng: 14.4378 },
    { name: "Ostrava", lat: 49.8209, lng: 18.2625 },
  ];

  return (
    <div>
      <div style={{ marginBottom: "1rem" }}>
        {locations.map((location) => (
          <button
            key={location.name}
            onClick={() => setCoordinates({ lat: location.lat, lng: location.lng })}
            style={{ marginRight: "0.5rem" }}
          >
            {location.name}
          </button>
        ))}
      </div>

      <Map lat={coordinates.lat} lng={coordinates.lng} width="500px" height="300px" zoom={12} />
    </div>
  );
};
```

### 3.2 Map with Conditional Features

```tsx
import { Map, baseIcons } from "@neuron/ui";
import { useState } from "react";

const ConditionalMapExample = () => {
  const [showMarker, setShowMarker] = useState(true);
  const [allowInteraction, setAllowInteraction] = useState(true);

  return (
    <div>
      <div style={{ marginBottom: "1rem" }}>
        <label>
          <input type="checkbox" checked={showMarker} onChange={(e) => setShowMarker(e.target.checked)} />
          Show Marker
        </label>

        <label style={{ marginLeft: "1rem" }}>
          <input type="checkbox" checked={allowInteraction} onChange={(e) => setAllowInteraction(e.target.checked)} />
          Allow Interaction
        </label>
      </div>

      <Map
        lat={49.195061}
        lng={16.606836}
        width="500px"
        height="300px"
        showMarker={showMarker}
        allowZoom={allowInteraction}
        allowDraging={allowInteraction}
        actions={[
          {
            children: "Reset View",
            variant: "secondary",
            iconLeft: baseIcons.arrowsRotateRegular,
            onClick: () => {
              // Reset map view logic would go here
              console.info("Resetting map view");
            },
          },
        ]}
      />
    </div>
  );
};
```

## Step 4: Responsive Map Implementation

### 4.1 Responsive Map Container

```tsx
import { Map, baseIcons } from "@neuron/ui";
import { useState, useEffect } from "react";

const ResponsiveMapExample = () => {
  const [mapSize, setMapSize] = useState({ width: "100%", height: "300px" });

  useEffect(() => {
    const handleResize = () => {
      const width = window.innerWidth;
      if (width < 768) {
        setMapSize({ width: "100%", height: "250px" });
      } else if (width < 1024) {
        setMapSize({ width: "100%", height: "350px" });
      } else {
        setMapSize({ width: "100%", height: "400px" });
      }
    };

    handleResize();
    window.addEventListener("resize", handleResize);
    return () => window.removeEventListener("resize", handleResize);
  }, []);

  return (
    <div style={{ maxWidth: "800px", margin: "0 auto" }}>
      <Map
        lat={49.195061}
        lng={16.606836}
        width={mapSize.width}
        height={mapSize.height}
        zoom={13}
        actions={[
          {
            children: "View Details",
            variant: "primary",
            iconRight: baseIcons.eyeRegular,
            onClick: () => console.info("View details clicked"),
          },
        ]}
      />
    </div>
  );
};
```

## Step 5: Click Geocoding (`onMapClick`)

### 5.1 How It Works

Passing `onMapClick` enables click interaction on the map:

1. User clicks anywhere on the map
2. The marker immediately moves to the clicked position
3. A reverse geocoding request is sent to `GET https://api.mapy.com/v1/rgeocode`
4. The callback is called with a `MapClickPoint` object containing the structured address data

When `onMapClick` is **not** provided the map is fully static — no click listener is registered.

### 5.2 Basic Click Handler

```tsx
import { Map, MapClickPoint } from "@neuron/ui";

const MapWithClick = () => {
  const handleClick = (point: MapClickPoint) => {
    console.log(point.city, point.zip, point.country);
  };

  return <Map lat={49.195061} lng={16.606836} width="100%" height="400px" zoom={13} onMapClick={handleClick} />;
};
```

### 5.3 Click Handler with State

```tsx
import { Map, MapClickPoint } from "@neuron/ui";
import { useState } from "react";

const MapWithAddress = () => {
  const [point, setPoint] = useState<MapClickPoint | null>(null);

  return (
    <div>
      <Map lat={49.195061} lng={16.606836} width="100%" height="360px" zoom={13} onMapClick={setPoint} />

      {point && (
        <div>
          <p>
            {point.street} {point.houseNumber}, {point.city} {point.zip}
          </p>
          <p>
            {point.country} ({point.countryCode})
          </p>
        </div>
      )}
    </div>
  );
};
```

### 5.4 Click Handler Opening a Modal

```tsx
import { Map, MapClickPoint, Modal, Button, baseIcons, IconSize } from "@neuron/ui";
import { useState } from "react";

const MapWithModal = () => {
  const [point, setPoint] = useState<MapClickPoint | null>(null);
  const [showModal, setShowModal] = useState(false);

  const handleMapClick = (clicked: MapClickPoint) => {
    setPoint(clicked);
    setShowModal(true);
  };

  const handleHide = () => setShowModal(false);

  return (
    <>
      <Map lat={49.195061} lng={16.606836} width="100%" height="400px" zoom={13} onMapClick={handleMapClick} />

      <Modal
        closeOnBackdropClick
        description="Reverse geocoding result for the selected map point."
        icon={{ iconDef: baseIcons.pinSolid, size: IconSize.large }}
        leftActionsZone={
          <Button variant="secondary" onClick={handleHide}>
            Close
          </Button>
        }
        show={showModal}
        size="lg"
        title="Location detail"
        onHide={handleHide}
      >
        {point && (
          <p>
            {point.street} {point.houseNumber}, {point.city} {point.zip}, {point.country}
          </p>
        )}
      </Modal>
    </>
  );
};
```

### 5.5 `MapClickPoint` Type Reference

```typescript
import { MapClickPoint } from "@neuron/ui";
```

| Field               | Type                  | Description                           |
| ------------------- | --------------------- | ------------------------------------- |
| `lat`               | `number`              | Clicked latitude                      |
| `lng`               | `number`              | Clicked longitude                     |
| `address`           | `string \| undefined` | Full address string from the API      |
| `street`            | `string \| undefined` | Street name                           |
| `houseNumber`       | `string \| undefined` | House / descriptive number            |
| `orientationNumber` | `string \| undefined` | Orientation number (Czech addresses)  |
| `city`              | `string \| undefined` | Municipality name                     |
| `zip`               | `string \| undefined` | Postal code                           |
| `country`           | `string \| undefined` | Country name                          |
| `countryCode`       | `string \| undefined` | ISO country code (e.g. `CZ`)          |
| `region`            | `string \| undefined` | Administrative region                 |
| `location`          | `string \| undefined` | Short locality label from API         |
| `rawItems`          | `MapClickRawItem[]`   | Full raw response items from Mapy.com |

### 5.6 Supporting Types

```typescript
import { MapClickRawItem, MapClickRegionalItem } from "@neuron/ui";
```

`MapClickRawItem` — single item from the `/v1/rgeocode` response array.
`MapClickRegionalItem` — one node in the `regionalStructure` hierarchy (municipality, country, region, etc.).

---

## Step 6: Map Props Reference

### 6.1 Core Map Props

| Prop           | Type                             | Default   | Description                                                                                                        |
| -------------- | -------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------ |
| `lat`          | `number`                         | —         | **Required** — Latitude for map center and marker                                                                  |
| `lng`          | `number`                         | —         | **Required** — Longitude for map center and marker                                                                 |
| `width`        | `string`                         | `"380px"` | Width of the map container                                                                                         |
| `height`       | `string`                         | `"160px"` | Height of the map container                                                                                        |
| `allowZoom`    | `boolean`                        | `true`    | Whether zooming is allowed                                                                                         |
| `allowDraging` | `boolean`                        | `true`    | Whether dragging is allowed                                                                                        |
| `showMarker`   | `boolean`                        | `true`    | Whether to show marker at coordinates                                                                              |
| `zoom`         | `number`                         | `10`      | Initial zoom level (1–19)                                                                                          |
| `actions`      | `ButtonProps[]`                  | —         | Array of action buttons rendered below the map                                                                     |
| `onMapClick`   | `(point: MapClickPoint) => void` | —         | When provided, enables click interaction: moves marker to click position and calls back with geocoded address data |

### 6.2 Common Coordinate Examples

| Location   | Latitude  | Longitude |
| ---------- | --------- | --------- |
| Prague     | 50.0755   | 14.4378   |
| Brno       | 49.195061 | 16.606836 |
| Ostrava    | 49.8209   | 18.2625   |
| Bratislava | 48.1486   | 17.1077   |
| Vienna     | 48.2082   | 16.3738   |

### 6.3 Zoom Level Guidelines

| Zoom Level | View Type | Use Case             |
| ---------- | --------- | -------------------- |
| 1-3        | World     | Continental view     |
| 4-6        | Country   | National view        |
| 7-10       | Region    | Regional/city view   |
| 11-13      | City      | Urban area           |
| 14-16      | District  | Neighborhood         |
| 17-19      | Street    | Detailed street view |

## Step 7: Best Practices

### 7.1 When to Use Map Component

**Use Map for:**

- Location displays
- Address visualization
- Geographic data presentation
- Property/venue location
- Route planning interfaces

```tsx
{
  /* Good: Property location display */
}
<Map
  lat={propertyLat}
  lng={propertyLng}
  width="100%"
  height="300px"
  actions={[
    {
      children: "Get Directions",
      onClick: handleDirections,
    },
  ]}
/>;
```

**Don't use Map for:**

- Non-geographic data visualization
- Simple location text display
- Performance-critical contexts without need

### 7.2 Performance Considerations

- Use appropriate zoom levels for context
- Limit the number of action buttons
- Consider lazy loading for maps not immediately visible
- Use responsive sizing appropriately

```tsx
{
  /* Good: Appropriate sizing and zoom */
}
<Map
  lat={lat}
  lng={lng}
  width="100%"
  height="350px"
  zoom={12} // Appropriate for city view
/>;
```

### 7.3 Action Button Guidelines

- Use clear, descriptive button labels
- Include appropriate icons
- Limit to 2-4 actions for better UX
- Use external link icons for external navigation

```tsx
{
  /* Good: Clear actions with appropriate icons */
}
const actions = [
  {
    children: "Get Directions",
    iconLeft: baseIcons.locationArrowRegular,
    onClick: handleDirections,
  },
  {
    children: "View on Google Maps",
    iconRight: baseIcons.arrowUpRightFromSquareRegular,
    onClick: handleExternalMap,
  },
];
```

### 7.4 Accessibility Considerations

- Provide alternative text descriptions for map content
- Ensure action buttons are keyboard accessible
- Consider users who cannot interact with maps
- Provide coordinate information as text when relevant

```tsx
{
  /* Good: Accessible map with context */
}
<div>
  <p>
    Location: {address} (Coordinates: {lat}, {lng})
  </p>
  <Map lat={lat} lng={lng} width="100%" height="300px" actions={actions} />
</div>;
```

## Step 8: Common Mistakes to Avoid

### 8.1 Don't Use Invalid Coordinates

```tsx
{
  /* ❌ INCORRECT: Invalid coordinates */
}
<Map
  lat={999} // Invalid latitude
  lng={999} // Invalid longitude
/>;

{
  /* ✅ CORRECT: Valid coordinates */
}
<Map
  lat={49.195061} // Valid latitude (-90 to 90)
  lng={16.606836} // Valid longitude (-180 to 180)
/>;
```

### 8.2 Don't Forget Required Props

```tsx
{
  /* ❌ INCORRECT: Missing required props */
}
<Map width="400px" height="300px" />;

{
  /* ✅ CORRECT: Include required lat/lng */
}
<Map lat={49.195061} lng={16.606836} width="400px" height="300px" />;
```

### 8.3 Don't Use Inappropriate Zoom Levels

```tsx
{
  /* ❌ INCORRECT: Inappropriate zoom for context */
}
<Map
  lat={49.195061}
  lng={16.606836}
  zoom={1} // Too zoomed out for a specific location
/>;

{
  /* ✅ CORRECT: Appropriate zoom for location */
}
<Map
  lat={49.195061}
  lng={16.606836}
  zoom={13} // Good for city/district view
/>;
```

### 8.4 Don't Overload with Actions

```tsx
{
  /* ❌ INCORRECT: Too many actions */
}
<Map
  lat={lat}
  lng={lng}
  actions={[
    { children: "Action 1" },
    { children: "Action 2" },
    { children: "Action 3" },
    { children: "Action 4" },
    { children: "Action 5" },
    { children: "Action 6" },
  ]}
/>;

{
  /* ✅ CORRECT: Limited, focused actions */
}
<Map
  lat={lat}
  lng={lng}
  actions={[
    { children: "Get Directions", iconLeft: baseIcons.locationArrowRegular },
    { children: "Share Location", iconLeft: baseIcons.shareRegular },
  ]}
/>;
```

## Key Takeaways

The Neuron Map component provides interactive mapping functionality:

1. **Required Coordinates** - Always provide valid lat/lng coordinates
2. **Responsive Sizing** - Use appropriate width/height for context
3. **Zoom Levels** - Choose appropriate zoom for the use case
4. **Action Buttons** - Limit to essential, well-labeled actions
5. **Performance** - Consider lazy loading and appropriate sizing
6. **Accessibility** - Provide alternative text and context
7. **Interactive Controls** - Configure zoom/drag based on needs
8. **External Integration** - Use actions for external map services
9. **Dynamic Content** - Support coordinate updates and conditional features
10. **User Experience** - Balance functionality with simplicity
11. **Click Geocoding** - Pass `onMapClick` to enable click interaction; its presence alone enables the feature — no separate boolean flag needed
12. **MapClickPoint** - All geocoded fields are optional; always handle `undefined` gracefully since geocoding may return partial data

By following these guidelines, you'll create effective map displays that provide users with clear geographic context and useful interaction options.
