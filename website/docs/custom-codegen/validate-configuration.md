---
id: validate-configuration
title: Validate Plugin Configuration
---

Each plugin can also provide a function to validate the configuration before executing it.

You can use this function to:

- test for other plugin's existence (for example, if your plugin requires another plugin to function correctly)
- validate the name and path of the output file
- validate the schema or documents

And much more!

To add your plugin validation method, export a function called `validate` from your plugin file:

```js
module.exports = {
  plugin(schema, documents, config, info) {
    const typesMap = schema.getTypeMap()

    return Object.keys(typesMap).join('\n')
  },
  validate: (schema, documents, config, outputFile, allPlugins) => {}
}
```

> `outputFile` is the name of the output file, and you can use it to enforce the specific filename of a specific file extension.

> `allPlugins` is a list of all plugins requested in this specific output file - use it to create dependencies between plugins.

You can now check the schema, documents, configuration, output file and sibling plugins, and in case something does not fits your requirements, throw an `Error`:

```js
module.exports = {
  plugin(schema, documents, config, info) {
    const typesMap = schema.getTypeMap()

    return Object.keys(typesMap).join('\n')
  },
  validate(schema, documents, config, outputFile, allPlugins) {
    if (!config.mustHave) {
      throw new Error(`You must specify "mustHave" in my plugin configuration!`)
    }
  }
}
```
