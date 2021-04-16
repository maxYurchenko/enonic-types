# TypeScript types for Enonic XP

[![npm version](https://badge.fury.io/js/enonic-types.svg)](https://badge.fury.io/js/enonic-types)

This library contains TypeScript types for Enonic XP.

## Installing *enonic-types*

Install *enonic-types* from npm by running:

```bash
npm i --save-dev enonic-types
```

Add support to use typed `import` updating your *tsconfig.json* file (or *src/main/resources/tsconfig.server.json* if 
you are using the [webpack-starter](https://github.com/enonic/starter-webpack)) with the `"types"` field:

```json
{
  "compilerOptions": {
    "types": ["node", "enonic-types"]
  }
}

```

And you're ready to start coding!

## Code generation

We recommend using this library together with the [xp-codegen-plugin](https://github.com/ItemConsulting/xp-codegen-plugin) Gradle plugin. *xp-codegen-plugin* will create TypeScript `interfaces` for your content-types. Those interfaces will be very useful together with this library.

## Example

We have an Enonic service that returns an article by id.

```typescript
import { Request, Response } from "enonic-types/controller";
import { Article } from "../../site/content-types/article/article"; // 1
import contentLib from "/lib/xp/content"; // 2

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
 2. If you added *enonic-types* to the types in your *tsconfig.json* or *tsconfig.server.json*  (see above), TypeScript should now
    be able to add types to all the standard XP-libraries.
 3. We use the imported `Request` and `Response` to control the shape of our controller.
 4. `content` is of the type `Content<Article> | null`, so we have to do a null check before proceiding.
 
## Using `__non_webpack_require__`

If your project is using `__non_webpack_require__`, you should update your *types.ts* file to add type support to it.

If you add (or replace the existing)
`__non_webpack_require__()` function with the following code, it will automatically look up the correct interfaces for 
Enonic XP-libraries. 

```typescript
type LibMap = import("enonic-types/libs").EnonicLibraryMap;

declare const __non_webpack_require__: <K extends keyof LibMap | string = string>(path: K) => K extends keyof LibMap
  ? LibMap[K]
  : any;
```
 
## Supported libraries

 * [AdminLibrary](./src/admin.ts)
 * [AuthLibrary](./src/auth.ts)
 * [CacheLibrary](./src/cache.ts)
 * [ClusterLibrary](./src/cluster.ts)
 * [CommonLibrary](./src/common.ts)
 * [ContentLibrary](./src/content.ts)
 * [ContextLibrary](./src/context.ts)
 * [ControllerLibrary](./src/controller.ts)
 * [CronLibrary](./src/cron.ts)
 * [EncodingLibrary](./src/encoding.ts)
 * [EventLibrary](./src/event.ts)
 * [FreeMarkerLibrary](./src/freemarker.ts)
 * [HttpLibrary](./src/http.ts)
 * [I18nLibrary](./src/i18n.ts)
 * [IOLibrary](./src/io.ts)
 * [MailLibrary](./src/mail.ts)
 * [MenuLibrary](./src/menu.ts)
 * [NodeLibrary](./src/node.ts)
 * [NotificationsLibrary](./src/notifications.ts)
 * [PortalLibrary](./src/portal.ts)
 * [ProjectLibrary](./src/project.ts)
 * [RecaptchaLibrary](./src/recaptcha.ts)
 * [RepoLibrary](./src/repo.ts) 
 * [RouterLibrary](./src/router.ts) 
 * [SessionLibrary](./src/session.ts) 
 * [TaskLibrary](./src/task.ts) 
 * [TestingLibrary](./src/testing.ts) 
 * [ThymeleafLibrary](./src/thymeleaf.ts)
 * [TurboLibrary](./src/turbo.ts)
 * [ValueLibrary](./src/value.ts)
 * [WebsocketLibrary](./src/websocket.ts)
