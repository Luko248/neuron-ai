---
agent: agent
---

# Neuron Footer Component Guidelines

## For the AI Assistant

This document provides comprehensive guidelines for implementing and configuring the Neuron Footer component within applications. It explains proper integration with Layout, theme handling, and customization options.

## Sync Metadata

- **Component Version:** v4.2.1
- **Component Source:** `packages/neuron/ui/src/lib/patterns/footer/Footer.tsx`
- **Guideline Command:** `/neuron-ui-footer`
- **Related Skill:** `neuron-ui-layout`

## Introduction

The Footer component provides a consistent application footer with links, copyright information, logo display, and theme switching functionality. It's designed to be used within the Layout component as part of a complete application structure.

## Component Structure

The Footer component consists of:

```
Footer
├── Logo Section (SVG logo based on current theme)
├── Copyright Text (or custom children)
├── Footer Actions
│   ├── Link List (configurable via props)
│   └── ThemeSwitch (theme toggle button)
```

## Integration with Layout

### Direct Integration (Recommended)

The Footer component **should be integrated within the Layout component via the footerContent prop** because:

1. The Layout component provides the proper structural container for the Footer
2. The Footer's theme switching button depends on the LayoutProvider context
3. The Footer needs access to the theme context to display the correct logo and copyright text
4. The Footer is typically shown on all pages of the application

### Layout Portal (Not Recommended for Footer)

While Layout Portal is available as an alternative way to include components in the layout, it is **not recommended for the Footer** because:

1. Layout Portal is intended for components that should only appear on specific pages
2. The Footer is a global component that should appear consistently across all pages
3. Using Layout Portal for Footer would require manual mounting on each page, risking inconsistency

Example of correct integration:

```tsx
// In app/elements/common/layout/components/AppLayout.tsx
import { useAuth } from "@neuron/auth";
import { Helmet, PRProvider, LayoutProvider, Layout, Footer } from "@neuron/ui";
import { useConfigContext } from "app/common/providers/configContext.created";
import { AppMenu } from "app/elements/common/menu/components/AppMenu";

/**
 * Main application layout that integrates footer and core providers
 */
export const AppLayout = () => {
  const { config } = useConfigContext();
  const { user } = useAuth();

  // Only render menu for authenticated users
  const menu = user && <AppMenu />;

  return (
    <Helmet>
      <PRProvider>
        <LayoutProvider>
          <Layout
            leftSide={menu}
            footerContent={<Footer />} {/* Footer goes in the footerContent prop */}
          >
            {/* Page content */}
          </Layout>
        </LayoutProvider>
      </PRProvider>
    </Helmet>
  );
};
```

## Theme Integration

The Footer component includes a theme toggle button and automatically adjusts its logo and copyright text based on the current theme:

1. The theme toggle button requires the LayoutProvider to function correctly
2. Logo selection is handled by the `useFooterLogo` hook which uses the theme context
3. Copyright text is handled by the `useFooterCopyRight` hook which also uses the theme context

Different logos are displayed based on the current theme:

- `cpp` or `sus` theme: CPP logo
- `koop` or `knz` theme: Koop logo
- Other themes: Default Neuron logo

## Footer Configuration

### Basic Usage

For basic usage with default settings:

```tsx
<Footer />
```

### Adding Links

You can add links to the footer:

```tsx
import { baseIcons, Footer } from "@neuron/ui";

<Footer
  links={[
    {
      children: "Help",
      to: "/help",
      iconLeft: baseIcons.bookOpenCoverSolid,
    },
    {
      children: "Contact Us",
      to: "/contact",
      iconLeft: baseIcons.messagesSolid,
    },
  ]}
/>;
```

### Custom Logo

You can provide a custom logo:

```tsx
import CustomLogo from "path/to/custom-logo.svg";

<Footer logoUrl={CustomLogo} />;
```

### Custom Content

You can replace the default copyright text with custom content:

```tsx
<Footer>
  <div className="d-flex flex-column">
    <p>Custom footer content</p>
    <p>© Your Company 2025</p>
  </div>
</Footer>
```

## Footer Props

The Footer component accepts the following props:

```typescript
export type FooterProps = {
  /**
   * Additional CSS class name for the footer.
   */
  className?: string;

  /**
   * An array of link props for rendering links in the footer.
   */
  links?: LinkProps[];

  /**
   * The children of the footer. If provided, replaces the default copyright text.
   */
  children?: ReactNode;

  /**
   * URL for the custom logo.
   */
  logoUrl?: string;
};
```

## Footer Link Configuration

Links in the footer use the standard Link component and support:

```typescript
type LinkProps = {
  children: ReactNode; // Link text or content
  to?: string; // Internal route for React Router
  href?: string; // External URL
  onClick?: () => void; // Click handler function
  iconLeft?: IconDefinition; // Font Awesome icon to show on the left
  target?: string; // Target for external links (_blank, etc.)
};
```

## Complete Example

Here's a complete example of a customized Footer:

```tsx
// In app/elements/common/footer/components/AppFooter.tsx
import { Footer, LinkProps } from "@neuron/ui";
import { baseIcons } from "@neuron/ui";
import { useTranslation } from "react-i18next";
import CustomLogo from "app/assets/logos/custom-logo.svg";

/**
 * Application footer component with custom links and logo
 */
export const AppFooter: React.FC = () => {
  const { t } = useTranslation("translation");

  const footerLinks: LinkProps[] = [
    {
      children: t("footer.help"),
      to: "/help",
      iconLeft: baseIcons.bookOpenCoverSolid,
    },
    {
      children: t("footer.contact"),
      to: "/contact",
      iconLeft: baseIcons.messagesSolid,
    },
    {
      children: t("footer.privacyPolicy"),
      href: "https://example.com/privacy",
      iconLeft: baseIcons.shieldSolid,
      target: "_blank",
    },
  ];

  return <Footer links={footerLinks} logoUrl={CustomLogo} />;
};
```

Then integrate it in your Layout:

```tsx
// In AppLayout.tsx
<Layout leftSide={menu} footerContent={<AppFooter />}>
  {children}
</Layout>
```

## Accessibility and Performance

1. **Proper ARIA attributes**: The Footer component includes appropriate ARIA attributes. If adding custom content, make sure to include appropriate aria-labels and roles.

2. **Keyboard navigation**: The ThemeSwitch in the Footer supports keyboard navigation. Ensure that any added links are keyboard accessible.

3. **Performance optimization**:

   - Use memoization with useMemo for footer links if they're dynamically generated
   - Consider useCallback for any footer-related event handlers
   - Use proper dependency arrays in all hooks

4. **TypeScript type safety**:
   - Use proper TypeScript types for all footer-related functions and components
   - Add JSDoc comments to footer helper functions and components

## Best Practices

1. **Always use with Layout**: Always integrate Footer within the Layout component via the footerContent prop.

2. **Use translation keys**: All footer text should use i18n translation keys.

3. **Keep links minimal**: Don't overwhelm the footer with too many links.

4. **Respect theme context**: Remember that the Footer uses theme context for its logo and copyright text.

5. **Use proper link types**: Use `to` for internal routes and `href` for external URLs.

6. **Responsive design**: The Footer is designed to be responsive; verify it looks good on all screen sizes.

7. **Use proper icons**: Add appropriate icons to footer links for better UX.

8. **Maintain consistency**: Keep footer styling and content consistent across your application.

9. **Test theme switching**: Ensure the theme toggle button works correctly and logo changes appropriately.

10. **Proper translation keys**: Use proper namespacing for footer-related translations.

Remember that the Footer component plays an important role in branding and navigation, and its theme switching functionality is tightly integrated with the LayoutProvider. Always ensure it's properly included within the Layout component's footerContent prop.
