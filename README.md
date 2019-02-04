This package will fix unresolved Ember.js imports like
`import from '@ember/..'`
for JetBrains IDEs (Idea, WebStorm, ...)


# Installation

In your project folder invoke:

* `npm i intellij-emberjs-import-support --no-save`<br>
Note: double check that `intellij-emberjs-import-support` was not added to your `package.json` - this may lead to Ember.js' error:<br>
The `intellij-emberjs-import-support` addon could not be found at `../node_modules/intellij-emberjs-import-support`.


* in the project's root create `webpack.config.js` file with the following content:
```javascript
/* eslint-disable */
const path = require('path');

function aliasEmberDirs(appAlias) {
    const emberDirs = [
        '/application', '/array', '/component', '/controller', '/object', '/polyfills',
        '/service', '/routing', '/runloop', '/utils', ''
    ];
    const emberBaseDir = 'node_modules/intellij-emberjs-import-support/@ember';
    return emberDirs.reduce(function(result, dir) {
        result[`@ember${dir}`] = emberBaseDir + dir;
        return result;
    }, appAlias);
}

module.exports = {
    resolve: {
        modules: [
            path.join(__dirname, 'node_modules/intellij-emberjs-import-support/@ember-packages'),
            'node_modules',
        ],
        root: path.resolve(__dirname),
        alias: aliasEmberDirs({
            'src': 'app'
            // your application alias folders
            // for example: 'src': 'app', will allow you to `import { foo } from 'src/bar';`
            // where 'src/bar' is located in `app/bar` folder.
        })
    }
};
```
* Turn On Webpack support in JetBrains IDE: 
Preferences | Languages & Frameworks | JavaScript | Webpack  
and set the config file to `webpack.config.js` which was just created.

That's it.
`import { .. } from '@ember/..'` should be resolved by IDE now.

# Improvements

This package is not ready yet, so any help is appreciated. 

Feel free to create a Pull Requests with improvements / fixes.


# Licenses

This package is licensed under MIT. 
Copyright (c) 2019 Dmitriy Labuzov.

It also contains /@ember-packages subfolder from Ember.js (https://github.com/emberjs/ember.js/)
Which is also licensed under MIT.
Copyright (c) 2019 Yehuda Katz, Tom Dale and Ember.js contributors
