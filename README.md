# simple-i18n

Minimalistic translation handling utility

## Features

- translation of keys
- parameter interpolation
- translation function available in any JavaScript context or function

## FAQ

### Why not store selected language and bundles in Redux or React state?

Normally users are not changing language so frequently. Normally it happens once during initial application setup.
So there is not much sense of optimizing the user experience around language change use case.
Designing the application for language change use case by putting the selected language in React state or Redux makes a lot of complexity for basically no gain.
It makes the translation handling tightly coupled to React / Redux which offten results in unnecessarly complex components.

### Why not store selected language just in a variable?

We expect that the utility to be used in both application and shared components. It possible that the application is using a different version of utility. In this case the bundler (Webpack) will bundle two instances of the utility. With that the selected language in application may differ from selected language in shared component.

### Why reload the page on language change?

As users change the language very rarely we can make the application simpler instead of optimizing for language change use case. With page reload we don't need to think what we have to refetch for newly selected language. We don't even need to listen for language change event. Also we can use the selected language and translate function in any javascript function without the need to inject React / Redux state.

### Why call a function "t"?

Normally the translation function is the most frequently used function in application. It has to be used any time we want to display any text on the screen. So choosing a very short name can reduce the noise in source code. So developers can focus more on message keys instead of function name. One of the most popular internationalization-framework called i18next is also using "t" function for translation function.

### Why not use some popular package for translation handling

This untility is only 618 bytes. And covers the needed use cases. In contrast i18next is 40.1KB, react-intl is 29.4KB.

## Usage

```typescript
import { setLanguage } from "./utils";

setLanguage("fr"); // -> sets language to 'fr' in local variable
```

```typescript
import { getLanguage } from "./utils";

getLanguage(); // -> 'fr'
```

```typescript
import { translate } from "./utils";

const bundles = { fr: { Greeting: "Bonjour" } };
translate(bundles, "Greeting"); // -> 'Bonjour'
```

```typescript
import en from "./messages_en.json"; // <- { "Greeting": "Hello {name}!" }
import fr from "./messages_fr.json"; // <- { "Greeting": "Bonjour {name}!" }
import { translate } from "./utils";

export const t = (id: string, params: Record<string, string> = {}) =>
  translate({ en, fr }, id, params);

t("Greeting", { name: "Alex" }); // -> 'Bonjour Alex!'
```
