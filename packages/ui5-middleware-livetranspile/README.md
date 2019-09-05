# UI5 middleware for live transpiling ES6+ sources

Middleware for [ui5-server](https://github.com/SAP/ui5-server), doing on-the-fly transpilation of ES6+ sources to ES5 (incl IE11 compatability :) )

## Install

```bash
npm install ui5-middleware-livetranspile --save-dev
```

## Configuration options (in `$yourapp/ui5.yaml`)

- debug: true|false  
verbose logging

## Usage

1. Define the dependency in `$yourapp/package.json`:

```json
"devDependencies": {
    // ...
    "ui5-middleware-livetranspile": "*"
    // ...
}
```

2. configure it in `$yourapp/ui5.yaml`:

```yaml
server:
  customMiddleware:
      - name: ui5-middleware-livetranspile
        afterMiddleware: compression
        configuration:
          debug: true
```

## How it works

The middleware intercepts every `.js`-file before it is sent to the client. The file is then transpiled on-the-fly via `babel`, including dynamic creation of a `sourcemap`.

The transpiled code and the `sourcemap` are subsequently delivered to the client instead of the original `.js`-file. Because of the `sourcemap`, setting breakpoints in the **original (ES6+) source** will cause the debugger to stop **when the corresponding transpiled source code is reached**. 

> `async/await` is transpiled at runtime, but the required `asyncGenerator` sources are not yet delivered on the fly. They need to be `sap.ui.require`d or `<script src="...">`d separately.  

## Misc/FAQ

`.js`-files requested by the server that are missing in the application (such as `Component-preload.js`) are logged as a `WARN` message, but will not cause the middleware to break/stop.

## License
Beerware License <https://fedoraproject.org/wiki/Licensing/Beerware>

When you like this stuff, buy @pmuessig or @vobu a beer when you see them.