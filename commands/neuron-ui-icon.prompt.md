---
agent: agent
---

# AI-Assisted Neuron Icon Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Icon components in a React application. This guide provides comprehensive instructions for implementing Icon components, which serve as the foundation for visual communication across all Neuron applications.

## Sync Metadata

- **Component Version:** v3.0.2
- **Component Source:** `packages/neuron/ui/src/lib/icon/Icon.tsx`
- **Guideline Command:** `/neuron-ui-icon`
- **Related Skill:** `neuron-ui-icons`

## Introduction

The Icon system is a core part of the Neuron UI framework, designed to create consistent, scalable, and accessible iconography across all Neuron applications.

### What is the Icon System?

The Icon component provides standardized icon rendering for your application with support for:

- **baseIcons collection** - Curated icons divided into three variants: Light, Regular, and Solid
- **Custom theme-specific icons** - Available only within specific application themes
- **Multiple sizes** with consistent scaling across all devices
- **Color customization** through CSS variables and design tokens
- **FontAwesome integration** (internal use only - never use directly)
- **Accessibility features** with proper ARIA support and semantic meaning

### Key Features

- **Three Icon Variants**: Light (subtle), Regular (standard), Solid (emphasis)
- **baseIcons Library**: Curated collection ensuring design system compliance
- **Theme-Specific Custom Icons**: Koop, CPP, KNZ, and SUS icons available only in their respective themes
- **Scalable Sizing**: Predefined sizes with custom size support
- **Color Integration**: CSS variable-based color system with theme support
- **Performance Optimized**: Pre-selected icon set reduces bundle size
- **Accessibility**: Built-in accessibility features and semantic meaning
- **TypeScript Support**: Full type safety with comprehensive baseIcons typing

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of all available icons.

## Step 1: Basic Icon Implementation

### 1.1 Import the Icon Component

```tsx
import { Icon, baseIcons } from "@neuron/ui";
```

### 1.2 Basic Icon Usage

Here's a simple implementation using baseIcons:

```tsx
import { Icon, baseIcons, IconSize } from "@neuron/ui";

const MyComponent = () => {
  return (
    <div className="icon-examples">
      {/* Basic icon */}
      <Icon iconDef={baseIcons.circleInfoSolid} />

      {/* Icon with custom size */}
      <Icon iconDef={baseIcons.userRegular} size={IconSize.large} />

      {/* Icon with custom color */}
      <Icon iconDef={baseIcons.bellNotificationSolid} color="var(--colorDanger500)" />
    </div>
  );
};
```

### 1.3 Icon Variants

baseIcons are divided into three distinct variants based on visual weight and emphasis:

```tsx
import { Icon, baseIcons } from "@neuron/ui";

const IconVariants = () => {
  return (
    <div className="icon-variants">
      {/* Light variant - subtle emphasis, delicate appearance */}
      <Icon iconDef={baseIcons.userLight} />

      {/* Regular variant - standard emphasis, balanced appearance */}
      <Icon iconDef={baseIcons.userRegular} />

      {/* Solid variant - strong emphasis, bold appearance */}
      <Icon iconDef={baseIcons.personSolid} />
    </div>
  );
};
```

**When to use each variant:**

- **Light**: Secondary actions, subtle indicators, background elements
- **Regular**: Primary content, standard UI elements, navigation
- **Solid**: Call-to-action buttons, important alerts, emphasized states

## Step 2: Icon Sizes and Scaling

### 2.1 Predefined Icon Sizes

Use predefined sizes for consistent scaling:

```tsx
import { Icon, baseIcons, IconSize } from "@neuron/ui";

const IconSizes = () => {
  return (
    <div className="icon-sizes">
      <Icon iconDef={baseIcons.downloadSolid} size={IconSize.small} />
      <Icon iconDef={baseIcons.downloadSolid} size={IconSize.base} />
      <Icon iconDef={baseIcons.downloadSolid} size={IconSize.large} />
      <Icon iconDef={baseIcons.downloadSolid} size={IconSize.ultra} />
    </div>
  );
};
```

### 2.2 Custom Icon Sizes

For specific design needs, use custom size values:

```tsx
import { Icon, baseIcons } from "@neuron/ui";

const CustomSizedIcons = () => {
  return (
    <div className="custom-sized-icons">
      {/* Custom size in rem units */}
      <Icon iconDef={baseIcons.gearSolid} size="2rem" />

      {/* Custom size in pixels */}
      <Icon iconDef={baseIcons.calendarDaysSolid} size="24px" />

      {/* Custom size using CSS variables */}
      <Icon iconDef={baseIcons.houseSolid} size="var(--fontSizeHeading2)" />
    </div>
  );
};
```

## Step 3: Icon Colors and Theming

### 3.1 Color Customization

Control icon colors using CSS variables:

```tsx
import { Icon, baseIcons } from "@neuron/ui";

const ColoredIcons = () => {
  return (
    <div className="colored-icons">
      {/* Using theme colors */}
      <Icon iconDef={baseIcons.circleCheckSolid} color="var(--colorSuccess500)" />

      {/* Using danger color */}
      <Icon iconDef={baseIcons.circleExclamationSolid} color="var(--colorDanger500)" />

      {/* Using primary color */}
      <Icon iconDef={baseIcons.circleInfoSolid} color="var(--colorPrimary600)" />

      {/* Current color (inherits from parent) */}
      <Icon iconDef={baseIcons.linkSolid} color="currentColor" />
    </div>
  );
};
```

### 3.2 Context-Based Coloring

Let icons inherit color from their context:

```tsx
import { Icon, baseIcons } from "@neuron/ui";

const ContextColoredIcons = () => {
  return (
    <div className="context-colored">
      {/* Icon inherits color from parent element */}
      <span style={{ color: "var(--colorSuccess500)" }}>
        <Icon iconDef={baseIcons.checkSolid} />
        Success message
      </span>

      <span style={{ color: "var(--colorDanger500)" }}>
        <Icon iconDef={baseIcons.circleXmarkRegular} />
        Error message
      </span>
    </div>
  );
};
```

## Step 4: Complete BaseIcons Reference

### 4.0 All Available BaseIcons

The Neuron framework includes a curated collection of baseIcons organized by variant. Below is the complete list of all available icons:

#### Light Variant Icons (24 icons)

```tsx
import { Icon, baseIcons } from "@neuron/ui";

// Light icons - Subtle emphasis, delicate appearance
baseIcons.cookieLight;
baseIcons.houseWaterLight;
baseIcons.moneyLight;
baseIcons.penRulerLight;
baseIcons.taskListLight;
baseIcons.userLight;
baseIcons.contractLight;
baseIcons.calculatorRegular; // Note: Actually from Light variant
baseIcons.bellLight;
baseIcons.arrowRightLight;
baseIcons.arrowLeftLight;
baseIcons.reorderLight;
baseIcons.xmarkSmallLight;
baseIcons.penLight;
baseIcons.briefcaseLight;
baseIcons.compassDraftingLight;
baseIcons.rectangleListLight;
baseIcons.chartColumnLight;
baseIcons.awardLight;
baseIcons.starLight;
baseIcons.boxArchiveLight;
```

#### Regular Variant Icons (95 icons)

```tsx
// Regular icons - Standard emphasis, balanced appearance
baseIcons.userRegular;
baseIcons.userTieRegular;
baseIcons.buildingRegular;
baseIcons.floppyDiskRegular;
baseIcons.bookOpenCoverRegular;
baseIcons.rectangleList;
baseIcons.phoneRegular;
baseIcons.xmarkLargeRegular;
baseIcons.folderOpenRegular;
baseIcons.arrowUpRegular;
baseIcons.arrowDownRegular;
baseIcons.envelopeRegular;
baseIcons.bellRegular;
baseIcons.logoutRegular;
baseIcons.moonRegular;
baseIcons.settingsRegular;
baseIcons.plusRegular;
baseIcons.clockRotateLeftRegular;
baseIcons.minusRegular;
baseIcons.checkRegular;
baseIcons.rewindRegular;
baseIcons.filesRegular;
baseIcons.copyRegular;
baseIcons.arrowUpRightFromSquareRegular;
baseIcons.circleInfoRegular;
baseIcons.handStopRegular;
baseIcons.circleCheckRegular;
baseIcons.circleExclamationRegular;
baseIcons.circleXmarkRegular;
baseIcons.trashRegular;
baseIcons.arrowUpWideShortRegular;
baseIcons.arrowDownWideShortRegular;
baseIcons.arrowUpArrowDownRegular;
baseIcons.filterRegular;
baseIcons.moneyTransferRegular;
baseIcons.thumbTackRegular;
baseIcons.thumbTackSlashRegular;
baseIcons.penRegular;
baseIcons.clipboardRegular;
baseIcons.boltRegular;
baseIcons.eyeRegular;
baseIcons.eyeLowVisionRegular;
baseIcons.sidebarRegular;
baseIcons.lockRegular;
baseIcons.tagRegular;
baseIcons.fileImageRegular;
baseIcons.fileJpgRegular;
baseIcons.fileLinesRegular;
baseIcons.filePdfRegular;
baseIcons.filePngRegular;
baseIcons.fileXlsRegular;
baseIcons.fileXmlRegular;
baseIcons.fileZipRegular;
baseIcons.fileCsvRegular;
baseIcons.fileEpsRegular;
baseIcons.fileGifRegular;
baseIcons.fileDocRegular;
baseIcons.boldRegular;
baseIcons.italicRegular;
baseIcons.underlineRegular;
baseIcons.strikethroughRegular;
baseIcons.subscriptRegular;
baseIcons.superscriptRegular;
baseIcons.bracketCurlyRegular;
baseIcons.removeFormatRegular;
baseIcons.houseChimneyRegular;
baseIcons.handshakeRegular;
baseIcons.rectangleListRegular;
baseIcons.chartColumnRegular;
baseIcons.awardRegular;
baseIcons.starRegular;
baseIcons.boxArchiveRegular;
baseIcons.expandWideRegular;
baseIcons.listOlRegular;
baseIcons.listUlRegular;
baseIcons.mergeRegular;
baseIcons.arrowUpLeftRegular;
baseIcons.arrowLeftRegular;
baseIcons.arrowRightRegular;
baseIcons.downloadRegular;
baseIcons.gridRegular;
baseIcons.memoCircleInfoRegular;
baseIcons.circleHalfStrokeRegular;
baseIcons.penSwirlRegular;
baseIcons.scaleBalancedRegular;
baseIcons.arrowRightRotateRegular;
baseIcons.arrowLeftRotateRegular;
baseIcons.magnifyingGlassPlusRegular;
baseIcons.magnifyingGlassMinusSolid; // Note: Actually from Regular variant
baseIcons.circleRegular;
baseIcons.rectangleRegular;
baseIcons.highlighterRegular;
baseIcons.arrowsLeftRightRegular;
baseIcons.arrowsUpDownLeftRightRegular;
```

#### Solid Variant Icons (108 icons)

```tsx
// Solid icons - Strong emphasis, bold appearance
baseIcons.pipeSolid;
baseIcons.floppyDiskSolid;
baseIcons.messagesSolid;
baseIcons.checkSolid;
baseIcons.arrowsRotateSolid;
baseIcons.candleHolderSolid;
baseIcons.bookOpenCoverSolid;
baseIcons.squareArrowDownSolid;
baseIcons.chevronDownSolid;
baseIcons.chevronRightSolid;
baseIcons.chevronLeftSolid;
baseIcons.chevronUpSolid;
baseIcons.magnifyingGlassSolid;
baseIcons.barsSolid;
baseIcons.linkSolid;
baseIcons.elipsisVerticalSolid;
baseIcons.circleInfoSolid;
baseIcons.circleExclamationSolid;
baseIcons.personSolid;
baseIcons.sunSolid;
baseIcons.downloadSolid;
baseIcons.uploadSolid;
baseIcons.penSolid;
baseIcons.houseSolid;
baseIcons.pinSolid;
baseIcons.lockSolid;
baseIcons.moonSolid;
baseIcons.houseChimneySolid;
baseIcons.settingSolid;
baseIcons.calendarDaysSolid;
baseIcons.phoneSolid;
baseIcons.squareCheckSolid;
baseIcons.squareXSolid;
baseIcons.messageQuoteSolid;
baseIcons.messageSmsSolid;
baseIcons.squarePhoneSolid;
baseIcons.atSolid;
baseIcons.envelopeSolid;
baseIcons.boxArchiveSolid;
baseIcons.fileSolid;
baseIcons.memoSolid;
baseIcons.globeSolid;
baseIcons.scaleBalancedSolid;
baseIcons.taskListSolid;
baseIcons.bellNotificationSolid;
baseIcons.calculatorSolid;
baseIcons.handshakeSolid;
baseIcons.houseFireSolid;
baseIcons.trashCanSolid;
baseIcons.paperPlaneSolid;
baseIcons.doubleChevronLeftSolid;
baseIcons.doubleChevronRightSolid;
baseIcons.number0Solid;
baseIcons.number1Solid;
baseIcons.number2Solid;
baseIcons.number3Solid;
baseIcons.number4Solid;
baseIcons.number5Solid;
baseIcons.number6Solid;
baseIcons.number7Solid;
baseIcons.number8Solid;
baseIcons.number9Solid;
baseIcons.placeholder;
baseIcons.fileCertificateSolid;
baseIcons.fileUserSolid;
baseIcons.passportSolid;
baseIcons.arrowUpRightFromSquareSolid;
baseIcons.idCardSolid;
baseIcons.messageExclamationSolid;
baseIcons.moneyTransferSolid;
baseIcons.listTimelineSolid;
baseIcons.arrowProgressSolid;
baseIcons.circlePlaySolid;
baseIcons.carSolid;
baseIcons.lockOpenSolid;
baseIcons.fingerprintSolid;
baseIcons.buildingSolid;
baseIcons.userTieSolid;
baseIcons.handHoldingCircleDollarSolid;
baseIcons.suitcaseSolid;
baseIcons.briefcaseSolid;
baseIcons.circleQuestionSolid;
baseIcons.circlePlusSolid;
baseIcons.circleCheckSolid;
baseIcons.circleRightSolid;
baseIcons.circleDownSolid;
baseIcons.circleUpSolid;
baseIcons.paperclipSolid;
baseIcons.xmarkSolid;
baseIcons.printSolid;
baseIcons.arrowTurnLeftSolid;
baseIcons.clockThreeSolid;
baseIcons.eyeSolid;
baseIcons.copySolid;
baseIcons.rightFromBracketSolid;
baseIcons.faceLaughSolid;
baseIcons.awardSolid;
baseIcons.starSolid;
baseIcons.chartColumnSolid;
baseIcons.squareQuestionSolid;
baseIcons.fileCirclePlusSolid;
baseIcons.plusSolid;
baseIcons.minusSolid;
baseIcons.boltSolid;
baseIcons.listCheckSolid;
baseIcons.imageSolid;
baseIcons.calculatorSimpleSolid;
baseIcons.bedPulse;
baseIcons.stethoscope;
baseIcons.staffSnake;
baseIcons.mortarPestle;
baseIcons.briefcaseMedical;
baseIcons.users;
baseIcons.calendarDay;
```

**Total: 234 baseIcons** (24 Light + 95 Regular + 115 Solid)

### 4.1 Action Icons

Use action icons for interactive elements:

```tsx
import { Icon, baseIcons } from "@neuron/ui";

const ActionIcons = () => {
  return (
    <div className="action-icons">
      {/* File operations */}
      <Icon iconDef={baseIcons.floppyDiskSolid} />
      <Icon iconDef={baseIcons.downloadSolid} />
      <Icon iconDef={baseIcons.uploadSolid} />
      <Icon iconDef={baseIcons.copyRegular} />

      {/* Navigation */}
      <Icon iconDef={baseIcons.chevronLeftSolid} />
      <Icon iconDef={baseIcons.chevronRightSolid} />
      <Icon iconDef={baseIcons.arrowUpRegular} />
      <Icon iconDef={baseIcons.arrowDownRegular} />

      {/* CRUD operations */}
      <Icon iconDef={baseIcons.circlePlusSolid} />
      <Icon iconDef={baseIcons.penSolid} />
      <Icon iconDef={baseIcons.trashCanSolid} />
    </div>
  );
};
```

### 4.2 Status and Feedback Icons

Use status icons for user feedback:

```tsx
import { Icon, baseIcons } from "@neuron/ui";

const StatusIcons = () => {
  return (
    <div className="status-icons">
      {/* Success states */}
      <Icon iconDef={baseIcons.circleCheckSolid} color="var(--colorSuccess500)" />
      <Icon iconDef={baseIcons.checkSolid} color="var(--colorSuccess500)" />

      {/* Warning states */}
      <Icon iconDef={baseIcons.circleExclamationSolid} color="var(--colorWarning500)" />

      {/* Error states */}
      <Icon iconDef={baseIcons.circleXmarkRegular} color="var(--colorDanger500)" />

      {/* Info states */}
      <Icon iconDef={baseIcons.circleInfoSolid} color="var(--colorInfo500)" />
    </div>
  );
};
```

### 4.3 Communication Icons

Use communication icons for messaging features:

```tsx
import { Icon, baseIcons } from "@neuron/ui";

const CommunicationIcons = () => {
  return (
    <div className="communication-icons">
      {/* Messaging */}
      <Icon iconDef={baseIcons.messagesSolid} />
      <Icon iconDef={baseIcons.messageQuoteSolid} />
      <Icon iconDef={baseIcons.messageSmsSolid} />

      {/* Notifications */}
      <Icon iconDef={baseIcons.bellNotificationSolid} />
      <Icon iconDef={baseIcons.bellRegular} />

      {/* Contact */}
      <Icon iconDef={baseIcons.phoneSolid} />
      <Icon iconDef={baseIcons.envelopeSolid} />
    </div>
  );
};
```

## Step 5: Custom Icons Integration

### 5.1 Understanding Custom Icons

Custom icons in the Neuron system are **strictly theme-specific** and are only available within their designated application themes. These icons are not part of baseIcons and have specific usage restrictions.

#### Available Custom Icon Themes

- **Koop Icons**: Only available in Koop theme applications
- **CPP Icons**: Only available in CPP theme applications
- **KNZ Icons**: Only available in KNZ theme applications
- **SUS Icons**: Only available in SUS theme applications
- **Global Custom Icons**: Available across all themes (limited set)

### 5.2 Using Custom Icons

Custom icons require the `IconType.Custom` type and a specific icon name:

```tsx
import { Icon, IconType } from "@neuron/ui";

const CustomIconsExample = () => {
  return (
    <div className="custom-icons">
      {/* Global custom icon - available in all themes */}
      <Icon type={IconType.Custom} name="vehicles" size="24px" color="var(--colorPrimary600)" />

      {/* Theme-specific icon - only works in Koop theme */}
      <Icon type={IconType.Custom} name="koop-specific-icon" size="32px" />
    </div>
  );
};
```

### 5.3 Theme-Specific Icon Restrictions

**IMPORTANT**: Theme-specific custom icons are tied to application color themes and will NOT render in other themes.

```tsx
{
  /* ❌ WRONG: This will not work in CPP or KNZ themes */
}
const KoopIconInWrongTheme = () => (
  <Icon
    type={IconType.Custom}
    name="koop-branding-icon" // Only available in Koop theme
  />
);

{
  /* ✅ CORRECT: Use theme-appropriate icons */
}
const ThemeAppropriateIcon = () => {
  const { currentTheme } = useTheme();

  return (
    <>
      {currentTheme === "koop" && <Icon type={IconType.Custom} name="koop-specific-icon" />}
      {currentTheme === "cpp" && <Icon type={IconType.Custom} name="cpp-specific-icon" />}
      {/* Fallback to baseIcons for universal compatibility */}
      <Icon iconDef={baseIcons.buildingSolid} />
    </>
  );
};
```

### 5.4 Available Custom Icon Names

#### CPP Theme Icons

```tsx
import { Icon, IconType } from "@neuron/ui";

// ✅ CORRECT: CPP theme custom icons (only in CPP theme)
<Icon type={IconType.Custom} name="business" />
<Icon type={IconType.Custom} name="hafan" />
<Icon type={IconType.Custom} name="life" />
<Icon type={IconType.Custom} name="property" />
<Icon type={IconType.Custom} name="simplex" />
<Icon type={IconType.Custom} name="travel" />
<Icon type={IconType.Custom} name="vehicles" />
<Icon type={IconType.Custom} name="zamex" />
```

**Available CPP Icons:**

- `business`
- `hafan`
- `life`
- `property`
- `simplex`
- `travel`
- `vehicles`
- `zamex`

#### KOOP Theme Icons

```tsx
import { Icon, IconType } from "@neuron/ui";

// ✅ CORRECT: KOOP theme custom icons (only in KOOP theme)
<Icon type={IconType.Custom} name="business" />
<Icon type={IconType.Custom} name="business-outline" />
<Icon type={IconType.Custom} name="flexi" />
<Icon type={IconType.Custom} name="life" />
<Icon type={IconType.Custom} name="life-outline" />
<Icon type={IconType.Custom} name="property" />
<Icon type={IconType.Custom} name="property-outline" />
<Icon type={IconType.Custom} name="travel" />
<Icon type={IconType.Custom} name="travel-outline" />
<Icon type={IconType.Custom} name="vehicles" />
<Icon type={IconType.Custom} name="vehicles-outline" />
```

**Available KOOP Icons:**

- `business` / `business-outline`
- `flexi`
- `life` / `life-outline`
- `property` / `property-outline`
- `travel` / `travel-outline`
- `vehicles` / `vehicles-outline`

**Note:** Icons are theme-dependent and automatically adapt based on the active theme.

### 5.5 When to Use Custom Icons

Use custom icons for:

- **Theme-specific branding**: Only when working within the specific theme (Koop, CPP, KNZ, SUS)
- **Specialized business domain icons**: When baseIcons don't cover the specific use case
- **Global custom icons**: Universal icons available across all themes
- **Custom illustrations or logos**: Theme-appropriate visual elements

**Never use custom icons from other themes** - they will not render correctly and break the visual consistency.

## Step 6: Icon Props Reference

### 6.1 Core Icon Props

| Prop      | Type                                      | Default                | Description                                                    |
| --------- | ----------------------------------------- | ---------------------- | -------------------------------------------------------------- |
| iconDef   | `TBaseIcons`                              | required\*             | Icon from baseIcons collection (required for FontAwesome type) |
| type      | `IconType.FontAwesome \| IconType.Custom` | `IconType.FontAwesome` | Icon rendering type                                            |
| name      | `string`                                  | -                      | Custom icon name (required when type is Custom)                |
| size      | `IconSize \| string`                      | `IconSize.base`        | Icon size (predefined or custom)                               |
| color     | `string`                                  | `"currentColor"`       | Icon color (CSS value or variable)                             |
| className | `string`                                  | `"icon"`               | Additional CSS classes                                         |

\*iconDef is required only when using FontAwesome type (baseIcons)

### 6.2 baseIcons Type Reference

baseIcons are organized into three categories:

| Variant | Suffix    | Description                          | Use Case                               |
| ------- | --------- | ------------------------------------ | -------------------------------------- |
| Light   | `Light`   | Thin strokes, subtle appearance      | Secondary actions, background elements |
| Regular | `Regular` | Standard weight, balanced appearance | Primary content, navigation            |
| Solid   | `Solid`   | Bold strokes, strong emphasis        | CTAs, alerts, important actions        |

### 6.3 Custom Icon Props

| Prop  | Type                 | Required | Description                            |
| ----- | -------------------- | -------- | -------------------------------------- |
| type  | `IconType.Custom`    | ✅       | Must be set to Custom for custom icons |
| name  | `string`             | ✅       | The specific custom icon name          |
| size  | `IconSize \| string` | ❌       | Icon size (defaults to base)           |
| color | `string`             | ❌       | Icon color (defaults to currentColor)  |

### 6.4 Icon Size Values

The IconSize enum provides predefined sizes that map to CSS variables:

```tsx
export enum IconSize {
  ultra = "var(--sizingSizeIconUltra)",
  large = "var(--sizingSizeIconLarge)",
  base = "var(--sizingSizeIconBase)",
  small = "var(--sizingSizeIconSmall)",
  tiny = "var(--sizingSizeIconTiny)",
}
```

**Size Reference Table:**

| Size             | CSS Variable            | Rem Value | Pixel Equivalent\* | Use Case                         |
| ---------------- | ----------------------- | --------- | ------------------ | -------------------------------- |
| `IconSize.tiny`  | `--sizingSizeIconTiny`  | `0.5rem`  | `8px`              | Very small indicators, badges    |
| `IconSize.small` | `--sizingSizeIconSmall` | `0.75rem` | `12px`             | Small UI elements, inline text   |
| `IconSize.base`  | `--sizingSizeIconBase`  | `1rem`    | `16px`             | Standard UI elements (default)   |
| `IconSize.large` | `--sizingSizeIconLarge` | `1.5rem`  | `24px`             | Prominent UI elements            |
| `IconSize.ultra` | `--sizingSizeIconUltra` | `2.5rem`  | `40px`             | Headers, hero elements, emphasis |

\*Pixel values calculated at 16px base font size (1rem = 16px)

## Step 7: Best Practices

### 7.0 Critical Requirements

**MANDATORY: Always use baseIcons and IconSize enum for all icon implementations:**

```tsx
import { Icon, baseIcons, IconSize } from "@neuron/ui";

{
  /* ✅ CORRECT: baseIcons with IconSize enum */
}
<Icon iconDef={baseIcons.personSolid} size={IconSize.base} />;

{
  /* ❌ WRONG: Direct FontAwesome or static sizes */
}
<Icon iconDef={faUser} size="16px" />;
```

### 7.1 Icon Selection Guidelines

**Choose appropriate icon variants based on visual hierarchy:**

**Choose appropriate icon variants based on visual hierarchy:**

- **Light**: Subtle emphasis, secondary actions, background elements
- **Regular**: Standard emphasis, primary content, navigation
- **Solid**: Strong emphasis, important actions, call-to-action buttons

```tsx
import { Icon, baseIcons, IconSize } from "@neuron/ui";

{/* ✅ CORRECT: baseIcons with IconSize enum */}
<Icon iconDef={baseIcons.userLight} size={IconSize.small} />      {/* Subtle user indicator */}
<Icon iconDef={baseIcons.userRegular} size={IconSize.base} />     {/* Standard user icon */}
<Icon iconDef={baseIcons.personSolid} size={IconSize.large} />    {/* Prominent user action */}

{/* ❌ WRONG: Static size values instead of enum */}
<Icon iconDef={baseIcons.userLight} size="12px" />
<Icon iconDef={baseIcons.userRegular} size="1rem" />
```

### 7.2 Theme-Specific Icon Guidelines

**Always verify theme compatibility before using custom icons:**

```tsx
{
  /* ❌ WRONG: Using Koop icon in CPP theme */
}
<Icon type={IconType.Custom} name="koop-branding" />;

{
  /* ✅ CORRECT: Theme-aware icon usage */
}
{
  isKoopTheme ? (
    <Icon type={IconType.Custom} name="koop-branding" />
  ) : (
    <Icon iconDef={baseIcons.buildingSolid} /> // Fallback to baseIcons
  );
}
```

### 7.3 Color Usage Guidelines

- Use semantic colors for status icons
- Use `currentColor` for contextual coloring
- Maintain sufficient contrast for accessibility

```tsx
{/* Good: Semantic color usage */}
<Icon iconDef={baseIcons.circleCheckSolid} color="var(--colorSuccess500)" />
<Icon iconDef={baseIcons.circleExclamationSolid} color="var(--colorWarning500)" />

{/* Good: Contextual coloring */}
<span className="error-text">
  <Icon iconDef={baseIcons.circleXmarkRegular} />
  Error message
</span>
```

### 7.4 Size Guidelines

**ALWAYS use IconSize enum for consistency and design system compliance:**

- Use predefined IconSize enum values for all standard use cases
- Only use custom sizes for exceptional design requirements
- Ensure icons scale appropriately across devices

```tsx
import { Icon, baseIcons, IconSize } from "@neuron/ui";

{/* ✅ CORRECT: Always use IconSize enum */}
<Icon iconDef={baseIcons.gearSolid} size={IconSize.base} />
<Icon iconDef={baseIcons.bellNotificationSolid} size={IconSize.large} />
<Icon iconDef={baseIcons.circleInfoSolid} size={IconSize.small} />

{/* ❌ WRONG: Static size values */}
<Icon iconDef={baseIcons.gearSolid} size="16px" />
<Icon iconDef={baseIcons.gearSolid} size="1rem" />

{/* ⚠️ EXCEPTIONAL: Custom size only when absolutely necessary */}
<Icon iconDef={baseIcons.logoIcon} size="48px" /> // Only for special cases like logos
```

**Size Selection Guide:**

- **IconSize.tiny** (8px): Badges, very small indicators
- **IconSize.small** (12px): Inline text icons, compact UI
- **IconSize.base** (16px): Standard UI elements (default)
- **IconSize.large** (24px): Prominent actions, headers
- **IconSize.ultra** (40px): Hero elements, major emphasis

### 7.5 Accessibility Considerations

- Ensure sufficient color contrast
- Provide alternative text when icons convey meaning
- Use semantic HTML when icons are interactive

```tsx
{
  /* Good: Accessible icon usage */
}
<button aria-label="Delete item">
  <Icon iconDef={baseIcons.trashCanSolid} />
</button>;

{
  /* Good: Decorative icon with text */
}
<span>
  <Icon iconDef={baseIcons.circleInfoSolid} />
  Information
</span>;
```

## Step 8: Common Use Cases and Patterns

### 8.1 List Item Icons

Icons for list items and navigation:

```tsx
import { Icon, baseIcons } from "@neuron/ui";

const NavigationList = () => {
  return (
    <ul className="navigation-list">
      <li>
        <Icon iconDef={baseIcons.houseChimneySolid} />
        <span>Home</span>
      </li>
      <li>
        <Icon iconDef={baseIcons.personSolid} />
        <span>Profile</span>
      </li>
      <li>
        <Icon iconDef={baseIcons.settingSolid} />
        <span>Settings</span>
      </li>
    </ul>
  );
};
```

### 8.2 Form Field Icons

Icons for form elements and validation:

```tsx
import { Icon, baseIcons } from "@neuron/ui";

const FormFieldWithIcon = ({ hasError, children }) => {
  return (
    <div className="form-field">
      {children}
      {hasError ? (
        <Icon iconDef={baseIcons.circleExclamationSolid} color="var(--colorDanger500)" />
      ) : (
        <Icon iconDef={baseIcons.circleCheckSolid} color="var(--colorSuccess500)" />
      )}
    </div>
  );
};
```

### 8.3 Loading and Progress Icons

Icons for loading states and progress:

```tsx
import { Icon, baseIcons } from "@neuron/ui";

const LoadingIcon = () => {
  return <Icon iconDef={baseIcons.arrowsRotateSolid} className="icon icon--spinning" size={IconSize.large} />;
};
```

## Step 9: Common Mistakes to Avoid

### 9.1 Don't Use FontAwesome Components Directly

**Never import or use FontAwesome components directly:**

```tsx
{
  /* ❌ WRONG: Direct FontAwesome usage */
}
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faUser } from "@fortawesome/free-solid-svg-icons";
<FontAwesomeIcon icon={faUser} />;

{
  /* ✅ CORRECT: Use Icon component with baseIcons */
}
import { Icon, baseIcons } from "@neuron/ui";
<Icon iconDef={baseIcons.personSolid} />;
```

### 9.2 Don't Import FontAwesome Icons Directly

**Always use baseIcons instead of direct FontAwesome imports:**

```tsx
{
  /* ❌ WRONG: Direct icon imports */
}
import { faPlus } from "@fortawesome/free-solid-svg-icons";
<Icon iconDef={faPlus} />;

{
  /* ✅ CORRECT: Use baseIcons */
}
import { Icon, baseIcons } from "@neuron/ui";
<Icon iconDef={baseIcons.circlePlusSolid} />;
```

### 9.3 Don't Mix Icon Systems

**Maintain consistency by using only baseIcons and appropriate custom icons:**

```tsx
{
  /* ❌ WRONG: Inconsistent icon usage */
}
<div>
  <FontAwesomeIcon icon={faUser} />
  <Icon iconDef={baseIcons.personSolid} />
</div>;

{
  /* ✅ CORRECT: Consistent baseIcons usage */
}
<div>
  <Icon iconDef={baseIcons.personSolid} />
  <Icon iconDef={baseIcons.userRegular} />
</div>;
```

### 9.4 Don't Use Cross-Theme Custom Icons

**Never use custom icons from other themes:**

```tsx
{
  /* ❌ WRONG: Using Koop icons in CPP theme */
}
<Icon type={IconType.Custom} name="koop-specific-branding" />;

{
  /* ❌ WRONG: Using CPP icons in KNZ theme */
}
<Icon type={IconType.Custom} name="cpp-specialized-tool" />;

{
  /* ✅ CORRECT: Use theme-appropriate icons or baseIcons */
}
<Icon iconDef={baseIcons.buildingSolid} />; // Universal baseIcon
```

### 9.5 Don't Ignore Semantic Coloring

**Use appropriate semantic colors for status and feedback icons:**

```tsx
{/* ❌ WRONG: Non-semantic colors */}
<Icon iconDef={baseIcons.circleCheckSolid} color="red" />
<Icon iconDef={baseIcons.circleExclamationSolid} color="green" />

{/* ✅ CORRECT: Semantic colors */}
<Icon iconDef={baseIcons.circleCheckSolid} color="var(--colorSuccess500)" />
<Icon iconDef={baseIcons.circleExclamationSolid} color="var(--colorWarning500)" />
```

## Key Takeaways

The Neuron Icon component system provides a comprehensive, consistent foundation for iconography. Key points to remember:

1. **Always use baseIcons** from `@neuron/ui` for universal compatibility
2. **Never use FontAwesome components directly** - use the Icon component exclusively
3. **Understand the three variants**: Light (subtle), Regular (standard), Solid (emphasis)
4. **Respect theme restrictions**: Custom icons are strictly theme-specific (Koop, CPP, KNZ, SUS)
5. **Use semantic colors** for status and feedback icons with CSS variables
6. **Use predefined sizes** for consistency across the application
7. **Consider accessibility** when icons convey important meaning
8. **Verify theme compatibility** before using custom icons
9. **Fallback to baseIcons** when custom icons are not available in the current theme
10. **Maintain visual consistency** by using appropriate icon variants and weights

By following these guidelines, you'll create consistent, accessible, and theme-appropriate iconography across your Neuron applications.
