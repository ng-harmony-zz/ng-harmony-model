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

Also, storage is implemented via Adapters, extendibly

Not to be forgotten:
* In order to ease duplication pains we use _facebook flow_ for type validation
* In order to further standards compliance I try to use
    - _ES2017 async/await_ whereever feasable
    - _ES2016 Promises_ instead of angular $q
    - fetch polyfill instead of angular $http

```javascript
// @flow
import "whatwg-fetch";
import { Service } from "ng-harmony-core";
import "ng-harmony-log";
import { Logging, Implements, Mixin } from "ng-harmony-decorator";
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
        return json;
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
```
The Transformer Classes are the building blocks for deriveds/DynamicViews
```javascript
export class IOTransform {
    in () {}
    out () {}
}

@Implements(IOTransform)
export class PropertyTransformer {
    derived = {};
    constructor (data) {
        if (typeof data !== "undefined" && data !== null) {
            this.in(data);
        }
    }
    in (o) {
        Object.keys(o).forEach((k, i) => {
            this.derived[k] = o[k];
        });
    }
}

export class CRUD {
    create () {}
    read () {}
    update () {}
    delete () {}
}

@Implements(CRUD)
export class MirrorService extends Service {
    store: MirrorModel;
    _state = {
        initialized: false,
        idle: true
    };
    static TIMEOUT = {
        ERROR: 3000,
        REFRESH: null,
        REFRESH_INTERVAL: null
    };
    static RECEPTOR: Array<Model> = [];

    constructor (...args) {
        super(...args);

        if (typeof this.constructor.TIMEOUT.REFRESH === "number" &&
            this.constructor.TIMEOUT.REFRESH > 0 &&
            typeof this.constructor.TIMEOUT.REFRESH_INTERVAL === "undefined") {
            this.constructor.TIMEOUT.REFRESH_INTERVAL = this.setInterval(this.constructor.TIMEOUT.REFRESH, this.read);
        }
    }

    get state () {
        return this._state;
    }
    set state(o) {
        if (typeof o.initialized !== "undefined") {
            this._state.initialized = o.initialized;
        }
        if (typeof o.idle !== "undefined") {
            this._state.idle = o.idle;
        }
    }
    digest (status) {
        let json = this.store.serialize();
        this.constructor.RECEPTOR.forEach((ctrl, i, receptors) => {
            ctrl.$scope.model.deserialize(json);
        });
    }
}

export class FirebaseService extends MirrorService {
    create () {

    }
    read () {

    }
    update () {

    }
}

export class AjaxService extends MirrorService {

}

export class GithubService extends AjaxService {

}
```

## CHANGELOG
*v0.0.4* Migration of Base Service hereabouts
*v0.0.3* ClientModel Transformer
*v0.0.2* ConcreteServices, DynamicViewModel aka ClientModel
*v0.0.1* File Structure ... plus got carried away in hacking the foundation
