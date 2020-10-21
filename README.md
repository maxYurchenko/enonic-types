# TypeScript types for Enonic XP

[![npm version](https://badge.fury.io/js/enonic-types.svg)](https://badge.fury.io/js/enonic-types)

This library contains TypeScript types for Enonic XP.

## Code generation

We recommend using this library together with the [xp-codegen-plugin](https://github.com/ItemConsulting/xp-codegen-plugin) Gradle plugin. *xp-codegen-plugin* will create TypeScript `interfaces` for your content-types. Those interfaces will be very useful together with this library.

## Example

We have an Enonic service that returns an article by id.

```typescript
import { Request, Response } from "enonic-types/controller";
import { Article } from "../../site/content-types/article/article"; // 1

const contentLib = __non_webpack_require__("/lib/xp/content"); // 2

export function get(req: Request): Response { // 3
  const content = contentLib.get<Article>({ key: req.params.id! });

  if (content !== null) { // 4
    const article: Article = content.data;

    return { status: 200, body: article }
  } else {
    return { status: 404 };
  }
}
```

 1. We import an `interface Article { ... }` generated by [xp-codegen-plugin](https://github.com/ItemConsulting/xp-codegen-plugin).
 2. When we import something with `__non_webpack_require__`, it will automatically look up the correct interfaces for 
 XP-libraries if `__non_webpack_require__` is defined correctly (see below).
 3. We use the imported `Request` and `Response` to control the shape of our controller.
 4. `content` is of the type `Content<Article> | null`, so we have to do a null check before proceiding.
 
## Declare `__non_webpack_require__`

Your project should have a *types.ts* file where you can specify declarations. 

If you add (or replace the existing)
`__non_webpack_require__()` function with the following code, it will automatically look up the correct interfaces for 
Enonic XP-libraries. 

```typescript
type EnonicLibraryMap = import("enonic-types").EnonicLibraryMap;

declare const __non_webpack_require__: <K extends keyof EnonicLibraryMap | string = string>(path: K) => K extends keyof EnonicLibraryMap
  ? EnonicLibraryMap[K]
  : any;
```
 
## Supported libraries

 * [AuthLibrary](./src/auth.ts)
 * [CommonLibrary](./src/common.ts)
 * [ContentLibrary](./src/content.ts)
 * [ContextLibrary](./src/context.ts)
 * [ControllerLibrary](./src/controller.ts)
 * [EncodingLibrary](./src/encoding.ts)
 * [EventLibrary](./src/event.ts)
 * [HttpLibrary](./src/http.ts)
 * [I18nLibrary](./src/i18n.ts)
 * [IOLibrary](./src/io.ts)
 * [MailLibrary](./src/mail.ts)
 * [MenuLibrary](./src/menu.ts)
 * [NodeLibrary](./src/node.ts)
 * [PortalLibrary](./src/portal.ts)
 * [ProjectLibrary](./src/project.ts)
 * [RecaptchaLibrary](./src/recaptcha.ts)
 * [RepoLibrary](./src/repo.ts) 
 * [RouterLibrary](./src/router.ts) 
 * [SessionLibrary](./src/session.ts) 
 * [ThymeleafLibrary](./src/thymeleaf.ts)
 * [ValueLibrary](./src/value.ts)
 * [WebsocketLibrary](./src/websocket.ts)
