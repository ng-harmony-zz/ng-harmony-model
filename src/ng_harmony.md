# Ng-Harmony
============

## Development

![Harmony = 6 + 7;](logo.png "Harmony - Fire in my eyes")

Please concoct a rather useful gist here ...

## Concept

This extra lib to ng-harmony will serve the purpose of *[...]*

Use it in conjunction with

* [literate-programming](http://npmjs.org/packages/literate-programming "click for npm-package-homepage") to write markdown-flavored literate JS, HTML and CSS
* [jspm](https://www.npmjs.com/package/jspm "click for npm-package-homepage") for a nice solution to handle npm-modules with ES6-Module-Format-Loading ...

## Files

This serves as literate-programming compiler-directive

[build/index.js](#Compilation "save:")

You can extend these literate-programming directives here ... the manual is (on jostylr@github/literate-programming)[https://github.com/jostylr/literate-programming]

## Compilation

The basic Model is just a validating Class.
All Models derive from that class and implement - via `decorators` - their
own validating mechanisms for each property.
Please use ES7-class-properties ...

```javascript
import { Service } from "ng-harmony-core";
import "ng-harmony-log";
import { Logging } from "ng-harmony-decorator";
```

The _Model_ Class is a Base for data-validation and dynamicViews/deriveds, and in itself, can work with json and output json.

```javascript
@Logging()
export class Model {
    constructor(data) {        
        data && this.deserialize(data, true);
    }
    serialize () {
        let json = {};
        Object.keys(this).forEach((p, i, list) => {
            json[p] = this[p] instanceof Model ? this[p].serialize() : this[p];
        });
        return JSON.stringify(json);
    }
    deserialize (data, soft: boolean = false) {
        Object.keys(this).forEach((p, i, list) => {
            try {
                this[p] = data[p] || null;
            } catch (e) {
                if (soft) {
                    this.log(e);
                } else {
                    throw e;
                }
            }
        });
    }
}

export class AjaxService extends Service {
    constructor(...args) {
        super(...args);

        this.constructor.MAP.forEach((tile) => {
            Object.defineProperty(this.constructor.prototype, tile.method, {
                configurable: true,
                enumerable: false,
                value: () => {
                    return this.$http(tile);
                }
            });
        })
    }
}
```

## CHANGELOG
*v0.0.6,7* Publish bump
*v0.0.5* Minimalism - getting rid of anything too fancy
*v0.0.4* Migration of Base Service hereabouts
*v0.0.3* ClientModel Transformer
*v0.0.2* ConcreteServices, DynamicViewModel aka ClientModel
*v0.0.1* File Structure ... plus got carried away in hacking the foundation
