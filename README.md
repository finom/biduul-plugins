# biduul-plugins

This is the [monorepo](https://en.wikipedia.org/wiki/Monorepo) for official [Biduul](https://github.com/Letiliel/biduul) plugins. 

![image](https://user-images.githubusercontent.com/1082083/126315449-5bacd995-0c72-4bb2-a687-fb5db77a7260.png)

Every plugin has full access to Biduul features (current symbol, trading functions, stats etc) and can create any number of widgets (either its none or multiple). Plugins are created with a global variable `window.biduulPlugin` that accepts a function that is going to be called by Biduul once 
Biduul API documentation itself is going to be made later.

## Third-party plugins

Some officially supported plugins may have "Third-party" label. 

![image](https://user-images.githubusercontent.com/1082083/126335242-c59523a5-3fc9-498f-90ea-46ef2058d3d9.png)

The label means that the plugin wasn't made by Biduul team, therefore we can't guarantee that the plugin is safe to use. Users are going to see a warning once such plugin is disabled via Plugins menu that says that even though the plugin was turned off it still can control application and personal data (such as API keys).

![image](https://user-images.githubusercontent.com/1082083/126336113-88e7123f-ddd0-485f-8075-38f79b363ec2.png)


## Custom plugins

Besides enabling official plugins, it is possible to add a plugin using its name on NPM,

![image](https://user-images.githubusercontent.com/1082083/126337885-d9bd9310-9e8f-43a4-ac1c-f6e840e45816.png)

or provide direct URL to a JavaScript file.

![image](https://user-images.githubusercontent.com/1082083/126338092-37b72e1c-38e0-4a4c-91c4-026637795267.png)

## TypeScript

Biduul plugins can be implemented with TypeScript. Types are published as [biduul-types](https://www.npmjs.com/package/biduul-types) package.

```ts
import * as t from 'biduul-types';

window.biduulPlugin((store: t.RootStore) => {
  const widget = store.customization.createWidget({
    ...
  });

  const createOrder = async (side: 'BUY' | 'SELL', quantity: number) => {
    await store.trading.marketOrder({
      quantity, side, symbol: store.persistent.symbol,
    });
  };

...

```

## React and Hot Module Replacement

All official Biduul plugins are implemented with [React](https://reactjs.org/) but it's [not a requirement](https://github.com/Letiliel/biduul-plugins/tree/main/packages/hello-world). An example of such plugin can be found at [hello-world-react](https://github.com/Letiliel/biduul-plugins/tree/main/packages/hello-world-react). The example also demonstrates hot module replacement: a feature of used code environment that allows to re-render components with no need to reload the application completely.

![](https://raw.githubusercontent.com/Letiliel/biduul-plugins/main/.assets/hmr.gif)

## Publishing

Biduul plugins are published at NPM via `npm publish`, trerefore every plugin has its own version. By the time being latest plugin version is used once installed but later we're going to need to restrict that to avoid incompatibility issues and improve Biduul security (in case if NPM package accesss was corrupted).

package.json file of any plugin needs to include `"main"` field that points to the plugin code **bundled as a single JavaScript file**.

```json
{
  "name": "biduul-awesome-plugin",
  "description": "My awesome plugin!",
  "main": "dist/bundle.js",
  ...
```

Once a plugin installed, it's going to be loaded via `<script>` tag using [UNPKG CDN](https://unpkg.com/).
 

