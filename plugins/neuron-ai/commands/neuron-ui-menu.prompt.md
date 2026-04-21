---
agent: agent
---

# Neuron Menu Component Guidelines

## For the AI Assistant

Your task is to help users properly implement and configure the Neuron Menu component within the application architecture. This guide provides comprehensive information about the Menu component structure, integration with Layout, menu item configuration, and authentication handling.

## Sync Metadata

- **Component Version:** v5.5.6
- **Component Source:** `packages/neuron/ui/src/lib/menus/menu/Menu.tsx`
- **Guideline Command:** `/neuron-ui-menu`
- **Related Skill:** `neuron-ui-menu`

## Introduction

The Neuron Menu component provides a consistent navigation interface across the application. It features a collapsible sidebar with configurable menu items, user profile section, theme toggler, and notifications. The menu is always situated in the left side of the Layout component.

## Component Structure

The Menu component consists of several sub-components:

```
Menu
├── MenuHeader
│   └── MenuLogo
├── MenuLinkList
│   ├── MenuLink
│   │   └── MenuSubmenu (recursive for nested items)
├── MenuNotifications
├── MenuUser (for authenticated users)
│   ├── MenuUserLink
│   └── MenuUserLogout
├── MenuUserUnauthorized (for unauthenticated users)
└── MenuToggler (collapse/expand)
```

## Integration with Layout

### Direct Integration (Recommended)

The Menu component **should always be used within the leftSide property of the Layout component** because:

1. The Layout component provides the proper structural container for the Menu
2. The LayoutProvider is required for theme switching functionality
3. The Menu's theme switching button depends on the LayoutProvider context
4. The Menu is typically shown on all pages of the application

### Layout Portal (Not Recommended for Menu)

While Layout Portal is available as an alternative way to include components in the layout, it is **not recommended for the Menu** because:

1. Layout Portal is intended for components that should only appear on specific pages
2. The Menu is a global component that should appear consistently across all pages
3. Using Layout Portal for Menu would require manual mounting on each page, risking inconsistency

Example of correct placement:

```tsx
// In app/elements/common/layout/components/AppLayout.tsx
import { useAuth } from "@neuron/auth";
import { Outlet } from "react-router-dom";
import { Helmet, PRProvider, LayoutProvider, Layout } from "@neuron/ui";
import { useAppContextUrl, ConfigService } from "@neuron/core";
import { useConfigContext } from "app/common/providers/configContext.created";
import { AppMenu } from "app/elements/common/menu/components/AppMenu";

/**
 * Main application layout that integrates the menu and core providers
 * Uses authentication state to conditionally render the menu
 */
export const AppLayout = () => {
  return (
    <Helmet>
      <PRProvider>
        <LayoutProvider>
          <Layout
            leftSide={<AppMenu />}  {/* Menu goes in the leftSide prop of Layout */}
          >
            {children}
          </Layout>
        </LayoutProvider>
      </PRProvider>
    </Helmet>
  );
};
```

## Menu Implementation

Create a dedicated menu component that renders the Menu component with your application's menu items. This component should integrate with both ConfigService and useAppContextUrl from @neuron/core:

```tsx
// In app/elements/common/menu/components/AppMenu.tsx
import { getRoutes } from "constants/keys";
import { useComposeMenuItems } from "helpers/common/menu";
import { Menu, MenuLinkProps } from "@neuron/ui";
import { useAppContextUrl, ConfigService } from "@neuron/core";
import { useConfigContext } from "app/common/providers/configContext.created";

export const AppMenu: React.FC = () => {
  // Get application configuration
  const { config } = useConfigContext();

  // Get menu items from your helper
  const menuItems = useComposeMenuItems();

  // useAppContextUrl is NECESSARY for menu routing to work correctly
  // It provides the complete URL with proper context for navigation
  const { completeUrl } = useAppContextUrl();
  const LINKS = getRoutes(completeUrl).LINKS;

  return <Menu items={menuItems} logoutPath={LINKS.POST_LOGOUT_PAGE} />;
};
```

## Menu Item Configuration

### Menu Item Structure

Menu items are defined using the `MenuLinkProps` interface:

```typescript
export type MenuLinkProps = {
  path?: string; // Navigation path
  text: string; // Display text
  disabled?: boolean; // Whether the item is disabled
  active?: boolean; // Whether the item is active
  icon?: IconDefinition; // FontAwesome icon
  isAnchorElement?: boolean; // Use regular anchor instead of Router Link
  target?: string; // Target for external links (_blank, etc.)
  items?: MenuLinkProps[]; // Submenu items for nesting
  className?: string; // Additional CSS class
  isSubmenuItem?: boolean; // Whether it's part of a submenu
};
```

### Creating Menu Items

Create a helper to compose menu items:

```typescript
// In helpers/common/menu.ts
import { getRoutes } from "constants/keys";
import { useTranslation } from "react-i18next";
import { faUsers, faArchive } from "@fortawesome/pro-regular-svg-icons";
import { baseIcons, MenuLinkProps } from "@neuron/ui";
import { useAppContextUrl } from "@neuron/core";

export const useComposeMenuItems = (): MenuLinkProps[] => {
  const { t } = useTranslation("translation");
  const { completeUrl } = useAppContextUrl();
  const routes = getRoutes(completeUrl).LINKS;

  return [
    {
      text: t("menu.userList"), // Use i18n translations for text
      path: routes.USER_LIST_PATH, // Use route constants for paths
      icon: faUsers, // Use FontAwesome icons
    },
    // Nested menu example with parent navigation
    {
      text: t("menu.archives"),
      path: routes.ARCHIVES_OVERVIEW_PATH, // Parent menu can have its own link
      icon: faArchive,
      items: [
        // Nested submenu items
        {
          text: t("menu.archiveExample1"),
          path: routes.ARCHIVE_EXAMPLE_1_PATH,
        },
        {
          text: t("menu.archiveExample2"),
          path: routes.ARCHIVE_EXAMPLE_2_PATH,
        },
      ],
    },
  ];
};
```

### Internal vs External Routes

The Menu component handles both internal and external routes:

1. **Internal Routes**:

   - Set the `path` property to your application route
   - Don't set `isAnchorElement` or set it to `false`
   - Example: `{ text: "Users", path: routes.USER_LIST_PATH }`

2. **External Routes**:
   - Set the `path` property to a complete URL
   - Set `target="_blank"` for opening in a new tab
   - Set `isAnchorElement={true}` if you want to force using an anchor element
   - Example: `{ text: "Docs", path: "https://example.com", target: "_blank" }`

### Nested Menus

To create nested menus:

1. Add an `items` array to a menu item
2. Parent items can have both their own navigation (`path`) and submenu items
3. Nesting can go multiple levels deep

**Behavior Notes:**

- On desktop: Parent with `path` displays as clickable link
- On mobile: Parent with `path` shows as button to open submenu
- Parent maintains active state when submenu is open on mobile

Example:

```typescript
{
  text: "Parent Item",
  path: routes.PARENT_OVERVIEW_PATH, // Parent can have its own navigation
  icon: faFolder,
  items: [
    {
      text: "Child Item 1",
      path: routes.CHILD_1_PATH,
    },
    {
      text: "Nested Parent",
      path: routes.NESTED_PARENT_PATH, // Nested parents can also have navigation
      items: [
        {
          text: "Grandchild Item",
          path: routes.GRANDCHILD_PATH,
        }
      ]
    }
  ]
}
```

## Authentication and Authorization

The Menu component handles both authenticated and unauthenticated states:

### Authenticated Users

For authenticated users, the Menu shows:

- User profile section
- User-specific links
- Logout button

Configure user-specific links:

```typescript
// User menu links
const userLinks = [
  {
    text: "Profile",
    path: routes.USER_PROFILE_PATH,
    icon: faUser,
  },
  {
    text: "Settings",
    path: routes.USER_SETTINGS_PATH,
    icon: faCog,
  }
];

// In your AppMenu component
<Menu
  items={menuItems}
  userLinks={userLinks}
  logoutPath={LINKS.POST_LOGOUT_PAGE} />
```

### Unauthenticated Users

For unauthenticated users, you can either:

1. Set the `allowAuthorization` prop to false when rendering the Menu:

```typescript
<Menu
  items={menuItems}
  logoutPath={LINKS.LOGIN_PAGE}
  allowAuthorization={false} />
```

2. Or conditionally render the Menu component only for authenticated users (recommended approach):

```typescript
// Only render menu for authenticated users
const menu = user && <AppMenu />;

// Then pass to Layout
<Layout leftSide={menu} {...otherProps}>
  {children}
</Layout>
```

## Theme Switching

The Menu includes a theme toggle button that works with the LayoutProvider's theme context:

1. The LayoutProvider must be present in the component tree
2. The theme toggle in Menu automatically integrates with LayoutProvider
3. No additional configuration is needed

## Props Reference

The Menu component accepts the following props:

```typescript
export type MenuProps = {
  collapsed?: boolean; // Collapsed state
  setCollapsed?: (c: boolean) => void; // Set collapsed state
  items?: MenuLinkProps[]; // Menu items
  logoutPath: string; // Path for logout
  homepagePath?: string; // Path for homepage
  userLinks?: MenuLinkProps[]; // User profile links
  notifications?: NotificationProps[]; // Notifications
  allowAuthorization?: boolean; // Whether to show User menu
  logoCollapsedUrl?: string; // Logo URL for collapsed state
  logoUrl?: string; // Logo URL for expanded state
};
```

## Config Integration

The Menu component automatically loads menu items from the application config via ConfigService from @neuron/core. This is an important feature that you need to be aware of:

```typescript
// Inside Menu.tsx (internal implementation)
const config = ConfigService.getConfig<IMenuConfigFw>();

const configMenu = useMemo(() => {
  if (!config.menu) {
    return [];
  }
  if (Array.isArray(config.menu)) {
    return config.menu;
  }
  return Object.values(config.menu) as MenuLinkProps[];
}, [config.menu]);

const itemsWithGlobals = useMemo(() => {
  return [...configMenu, ...(items || [])];
}, [items, configMenu]);
```

This means:

1. Menu items defined in your application config will automatically be loaded
2. Items passed directly to the Menu component will be merged with config items
3. You can provide menu configuration through ConfigService

## useAppContextUrl Integration

The useAppContextUrl hook from @neuron/core is essential for proper menu routing. It provides:

1. The correct URL context for your application
2. Handles URL prefixing based on application context
3. Ensures menu navigation works correctly in all environments

**Always** use useAppContextUrl when working with routes in the Menu component:

```typescript
const { completeUrl } = useAppContextUrl();
const routes = getRoutes(completeUrl).LINKS;

// Now use routes.PATH_NAME for menu items
```

## Complete Example

### 1. Menu Items Configuration

```typescript
// In helpers/common/menu.ts
import { getRoutes } from "constants/keys";
import { useTranslation } from "react-i18next";
import { faUsers } from "@fortawesome/pro-regular-svg-icons";
import { baseIcons, MenuLinkProps } from "@neuron/ui";
import { useAppContextUrl, ConfigService } from "@neuron/core";

/**
 * Composes the menu items for the application navigation
 * Using both configured routes and translations
 * @returns Array of menu items with proper structure and translations
 */
export const useComposeMenuItems = (): MenuLinkProps[] => {
  const { t } = useTranslation("translation");
  const { completeUrl } = useAppContextUrl();
  const routes = getRoutes(completeUrl).LINKS;

  return [
    {
      text: t("menu.dashboard"),
      path: routes.DASHBOARD_PATH,
      icon: baseIcons.dashboardRegular,
    },
    {
      text: t("menu.users"),
      path: routes.USER_LIST_PATH,
      icon: faUsers,
    },
    {
      text: t("menu.settings"),
      icon: baseIcons.settingsRegular,
      items: [
        {
          text: t("menu.profile"),
          path: routes.PROFILE_PATH,
        },
        {
          text: t("menu.preferences"),
          path: routes.PREFERENCES_PATH,
        },
      ],
    },
    // External link example
    {
      text: t("menu.documentation"),
      path: "https://docs.example.com",
      icon: baseIcons.documentRegular,
      target: "_blank",
    },
  ];
};
```

### 2. App Menu Component

```tsx
// In app/elements/common/menu/components/AppMenu.tsx
import { getRoutes } from "constants/keys";
import { useComposeMenuItems } from "helpers/common/menu";
import { Menu, MenuLinkProps, NotificationProps } from "@neuron/ui";
import { useAppContextUrl } from "@neuron/core";
import { faUser, faCog } from "@fortawesome/pro-regular-svg-icons";
import { useTranslation } from "react-i18next";

/**
 * Application menu component that provides navigation structure
 * and user-specific links
 */
export const AppMenu: React.FC = () => {
  const { t } = useTranslation("translation");
  const menuItems = useComposeMenuItems();

  const { completeUrl } = useAppContextUrl();
  const LINKS = getRoutes(completeUrl).LINKS;

  // User-specific links
  const userLinks: MenuLinkProps[] = [
    {
      text: t("menu.myProfile"),
      path: LINKS.USER_PROFILE_PATH,
      icon: faUser,
    },
    {
      text: t("menu.mySettings"),
      path: LINKS.USER_SETTINGS_PATH,
      icon: faCog,
    },
  ];

  // Notifications example
  const notifications: NotificationProps[] = [
    { message: t("notifications.newMessage") },
    { message: t("notifications.taskCompleted") },
  ];

  return (
    <Menu items={menuItems} userLinks={userLinks} notifications={notifications} logoutPath={LINKS.POST_LOGOUT_PAGE} />
  );
};
```

### 3. Integration with Layout and neuron/core

```tsx
// In app/elements/common/layout/components/AppLayout.tsx
import { useAuth } from "@neuron/auth";
import { Outlet } from "react-router-dom";
import { Helmet, PRProvider, LayoutProvider, Layout, Footer, BreadCrumbs } from "@neuron/ui";
import { AppMenu } from "app/elements/common/menu/components/AppMenu";
import { useConfigContext } from "app/common/providers/configContext.created";

export const AppLayout = () => {
  const { user } = useAuth();
  const { config } = useConfigContext();

  // Only render menu for authenticated users
  const menu = user && <AppMenu />;

  return (
    <Helmet>
      <PRProvider>
        <LayoutProvider>
          <Layout leftSide={menu}>
            <div className="g-col-12 d-grid">
              <Outlet />
            </div>
          </Layout>
        </LayoutProvider>
      </PRProvider>
    </Helmet>
  );
};
```

## Best Practices

1. **Always use with Layout**: Always integrate Menu within the Layout component.

2. **Use translation keys**: All menu text should use i18n translation keys.

3. **Use route constants**: Use centralized route constants for all internal paths.

4. **Icons for clarity**: Add appropriate icons to main menu items for better UX.

5. **Limit nesting depth**: Keep menu nesting to a maximum of 3 levels deep for usability.

6. **Separate menu logic**: Keep menu items logic in a separate helper file.

7. **Consider mobile views**: The menu collapses automatically on smaller screens.

8. **Test authentication states**: Test both authenticated and unauthenticated views.

9. **Keep menus concise**: Avoid overwhelming users with too many menu options.

10. **Group related items**: Use submenus to group related functionality.

Remember that proper menu organization is crucial for user experience and application navigation. Follow these guidelines consistently across your project to ensure a clean and intuitive navigation structure.
