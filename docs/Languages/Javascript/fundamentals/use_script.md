---
sidebar_position: 2
---

# Use Strict (Modern Mode)

JavaScript evolved without compatibility issues. New features were added to the language while old functionality didn’t change.

That had the benefit of never breaking existing code. But the downside was that any mistake or an imperfect decision made by JavaScript’s creators got stuck in the language forever.

This was the case until 2009 when ECMAScript 5 (ES5) appeared. It added new features to the language and modified some of the existing ones. To keep the old code working, most such modifications are off by default. You need to explicitly enable them with a special directive: `"use strict"`.

The directive looks like a string: "use strict" or 'use strict'. When it is located at the top of a script, the whole script works the “modern” way.

For example:

```js
"use strict";

// this code works the modern way
...
```

:::danger[Ensure that “use strict” is at the top]
Please make sure that `"use strict"` is at the top of your scripts, otherwise strict mode may not be enabled.

Strict mode isn’t enabled here:
```js
alert("some code");
// "use strict" below is ignored--it must be at the top

"use strict";

// strict mode is not activated
```

Only comments may appear above `"use strict"`.
:::

:::danger[There’s no way to cancel use strict]
There is no directive like `"no use strict"` that reverts the engine to old behavior.

Once we enter strict mode, there’s no going back.
:::

Modern JavaScript supports “classes” and “modules” – advanced language structures (we’ll surely get to them), that enable use strict automatically. So we don’t need to add the `"use strict"` directive, if we use them.