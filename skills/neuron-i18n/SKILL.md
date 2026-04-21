---
name: neuron-i18n
description: Implement internationalization using i18next in Neuron applications. Use when adding translations, localizing text, or managing translation files. CRITICAL: Never use hardcoded text - all user-visible strings must use t() function. Covers translation file structure, naming conventions, usage patterns, and common/feature-specific organization.
---

# Internationalization (i18n)

Implement internationalization using i18next in Neuron applications.

**CRITICAL: Never use hardcoded text - all user-visible strings must use t() function.**

❌ **Forbidden:** `<Literal label={{ text: "Name" }} />`
✅ **Required:** `<Literal label={{ text: t("form.name") }} />`

## Process

1. **Import useTranslation** - Get t() function
2. **Add translations** - Create translation keys in JSON files
3. **Use t() function** - Replace all hardcoded text
4. **Organize by feature** - Group related translations
5. **Follow naming conventions** - Use consistent key structure

## File Structure

```
apps/starter/src/i18n/
├── cs/                       # Czech translations
│   ├── common/               # Common/shared translations
│   │   ├── auth.i18n.json    # Authentication-related translations
│   │   ├── errors.i18n.json  # Error messages
│   │   ├── feedback.i18n.json # Feedback messages
│   │   ├── form.i18n.json    # Form-related labels and messages
│   │   └── menu.i18n.json    # Menu-related translations
│   ├── pages/                # Page-specific translations
│   │   ├── homePage/         # Each page has its own directory
│   │   │   └── homePage.i18n.json
│   │   ├── userProfilePage/
│   │   │   └── userProfilePage.i18n.json
│   │   └── ...               # Other page translations
│   ├── components/           # Component-specific translations
│   │   ├── actionBar/        # Each component has its own directory
│   │   │   └── actionBar.i18n.json
│   │   ├── table/
│   │   │   └── table.i18n.json
│   │   └── ...               # Other component translations
│   └── index.ts              # Consolidates all translations
├── i18n.helpers.ts           # Helper functions for i18n
├── i18next.d.ts              # TypeScript declarations for i18next
├── index.ts                  # Entry point for i18n
└── initI18n.ts              # i18n initialization
```

## Translation Files

**Common translations (actions.json):**

```json
{
  "save": "Uložit",
  "cancel": "Zrušit",
  "delete": "Smazat",
  "edit": "Upravit",
  "create": "Vytvořit",
  "search": "Hledat"
}
```

**Validation translations (validation.json):**

```json
{
  "required": "Toto pole je povinné",
  "invalidEmail": "Neplatná e-mailová adresa",
  "minLength": "Minimální délka je {{min}} znaků",
  "maxLength": "Maximální délka je {{max}} znaků"
}
```

**Feature translations (personDetail.json):**

```json
{
  "title": "Detail osoby",
  "firstName": "Jméno",
  "lastName": "Příjmení",
  "birthDate": "Datum narození",
  "email": "E-mail",
  "phone": "Telefon"
}
```

## Usage in Components

**Import and use:**

```tsx
import { useTranslation } from "react-i18next";

const PersonDetailForm = () => {
  const { t } = useTranslation();

  return (
    <form className="grid form-gap">
      <Input name="firstName" control={control} labelText={t("personDetail.firstName")} required />
      <Input name="lastName" control={control} labelText={t("personDetail.lastName")} required />
      <Button type="submit">{t("actions.save")}</Button>
    </form>
  );
};
```

## Naming Conventions

**Key structure:**

- Use camelCase for keys: `firstName`, `lastName`
- Use dot notation for nesting: `personDetail.firstName`
- Group by feature: `personDetail.*`, `dashboard.*`
- Use common namespace for shared: `actions.*`, `validation.*`

**Examples:**

- `actions.save` - Common action
- `validation.required` - Common validation
- `personDetail.title` - Feature-specific
- `errors.notFound` - Common error

## Interpolation

**With variables:**

```json
{
  "welcome": "Vítejte, {{name}}!",
  "itemCount": "Počet položek: {{count}}"
}
```

**Usage:**

```tsx
t("welcome", { name: user.name });
t("itemCount", { count: items.length });
```

## Examples

**Example 1: Form with translations**

```tsx
const { t } = useTranslation();

<form>
  <Input labelText={t("form.name")} />
  <Input labelText={t("form.email")} />
  <Button>{t("actions.submit")}</Button>
</form>;
```

**Example 2: Panel with title**

```tsx
const { t } = useTranslation();

<Panel title={t("personDetail.title")}>
  <p>{t("personDetail.description")}</p>
</Panel>;
```

**Example 3: Validation messages**

```tsx
const schema = z.object({
  email: z.string().email({ message: t("validation.invalidEmail") }),
  name: z.string().min(1, { message: t("validation.required") }),
});
```

## Best Practices

- Never use hardcoded text in components
- Always use t() function for all user-visible strings
- Organize translations by feature in separate files
- Use common translations for shared text (actions, validation, errors)
- Use camelCase for translation keys
- Use dot notation for nested keys
- Add interpolation for dynamic values
- Group related translations together
- Keep translation files focused and organized
- Import useTranslation at component level
