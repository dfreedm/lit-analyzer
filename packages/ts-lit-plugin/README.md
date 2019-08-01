<h1 align="center">ts-lit-plugin</h1>
<p align="center">
  <b>Typescript plugin that adds type checking and code completion to lit-html</b></br>
  <sub><sub>
</p>

<br />

<p align="center">
		<a href="https://npmcharts.com/compare/ts-lit-plugin?minimal=true"><img alt="Downloads per month" src="https://img.shields.io/npm/dm/ts-lit-plugin.svg" height="20"/></a>
<a href="https://www.npmjs.com/package/ts-lit-plugin"><img alt="NPM Version" src="https://img.shields.io/npm/v/ts-lit-plugin.svg" height="20"/></a>
<a href="https://david-dm.org/runem/lit-analyzer"><img alt="Dependencies" src="https://img.shields.io/david/runem/lit-analyzer.svg" height="20"/></a>
<a href="https://github.com/runem/lit-analyzer/graphs/contributors"><img alt="Contributors" src="https://img.shields.io/github/contributors/runem/lit-analyzer.svg" height="20"/></a>
	</p>


<p align="center">
  <img src="https://user-images.githubusercontent.com/5372940/62078476-02c1ec00-b24d-11e9-8de5-1322012cbde2.gif" alt="Lit plugin GIF"/>
</p>


[![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)](#installation)

## ➤ Installation

First, install the plugin:

```bash
npm install ts-lit-plugin -D
```


Then add a `plugins` section to your [`tsconfig.json`](http://www.typescriptlang.org/docs/handbook/tsconfig-json.html):

```json
{
  "compilerOptions": {
    "plugins": [
      {
        "name": "ts-lit-plugin"
      }
    ]
  }
}
```

Finally, restart you Typescript Language Service, and you should start getting diagnostics from `ts-lit-plugin`.

**Note:**
* If you use Visual Studio Code you can also install the [lit-plugin](https://marketplace.visualstudio.com/items?itemName=runem.lit-plugin) extension. 
* If you would rather use a CLI, you can install the [lit-analyzer](https://github.com/runem/lit-analyzer/blob/master/packages/lit-analyzer).

[![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)](#configuration)

## ➤ Configuration

You can configure this plugin through your `tsconfig.json`. 

### Example

```json
{
  "compilerOptions": {
    "plugins": [
      {
        "name": "ts-lit-plugin",
        "strict": true,
        "rules": {
          "no-unknown-tag-name": "off",
          "no-unknown-event": "warn"
        }
      }
    ]
  }
}
```

### Available options

<!-- prettier-ignore -->
| Option | Description | Type | Default |
| :----- | ----------- | ---- | ------- |
| `strict` | Enabling strict mode will change which rules are applied as default (see list of [rules](https://github.com/runem/lit-analyzer/blob/master/docs/readme/rules.md)) | `boolean` | false |
| `rules` | Enable/disable individual rules or set their severity. Example: `{"no-unknown-tag-name": "off"}` | `{"rule-name": "off" \| "warn" \| "error"}` | The default rules enabled depend on the `strict` option |
| `disable` | Completely disable this plugin. | `boolean` | false |
| `dontShowSuggestions` | This option sets strict as  | `boolean` | false |
| `htmlTemplateTags` | List of template tags to enable html support in. | `string[]` | ["html", "raw"] | |
| `cssTemplateTags` | This option sets strict as | `string[]` | ["css"] |
| `globalTags` |  List of html tag names that you expect to be present at all times. | `string[]` | |
| `globalAttributes` | List of html attributes names that you expect to be present at all times. | `string[]` | |
| `globalEvents` | List of event names that you expect to be present at all times | `string[]` | |
| `customHtmlData` | This plugin supports the [custom vscode html data format](https://code.visualstudio.com/updates/v1_31#_html-and-css-custom-data-support) through this setting. | [Vscode Custom HTML Data Format](https://github.com/Microsoft/vscode-html-languageservice/blob/master/docs/customData.md). Supports arrays, objects and relative file paths | |


[![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)](#rules)

## ➤ Rules

The default severity of each rule depend on the `strict` [configuration option](#-configuration). Strict mode is disabled as default.

Each rule can have severity of `off`, `warning` or `error`. You can toggle rules as you like.

**Validating custom elements**

<!-- prettier-ignore -->
| Rule    | Description | Severity normal | Severity strict |
| :------ | ----------- | --------------- | --------------- |
| [no-unknown-tag-name](#-no-unknown-tag-name) | The existence of tag names are checked. Be aware that not all custom elements from libraries will be found out of the box. | off | warning |
| [no-missing-import](#-no-missing-import)    | When using custom elements in HTML it is checked if the element has been imported and is available in the current context. | off | warning |
| [no-unclosed-tag](#-no-unclosed-tag)         | Unclosed tags, and invalid self closing tags like custom elements tags, are checked. | warning | error |


**Validating binding names**

<!-- prettier-ignore -->
| Rule    | Description | Severity normal | Severity strict |
| :------ | ----------- | --------------- | --------------- |
| [no-unknown-attribute](#-no-unknown-attribute-no-unknown-property)<br> [no-unknown-property](#-no-unknown-attribute-no-unknown-property) | You will get a warning whenever you use an unknown attribute or property within your `lit-html` template. | off | warning |
| [no-unknown-event](#-no-unknown-event)       | When using event bindings it's checked that the event names are fired. | off | off |
| [no-unknown-slot](#-no-unknown-slot)         | Using the "@slot" jsdoc tag on your custom element class, you can tell which slots are accepted for a particular element. | off | warning |


**Validating binding types**

<!-- prettier-ignore -->
| Rule    | Description | Severity normal | Severity strict |
| :------ | ----------- | --------------- | --------------- |
| [no-invalid-boolean-binding](#-no-invalid-boolean-binding)       | Disallow boolean attribute bindings on non-boolean types. | error | error |
| [no-expressionless-property-binding](#-no-expressionless-property-binding) | Disallow property bindings without an expression. | error | error |
| [no-noncallable-event-binding](#-no-noncallable-event-binding)   | Disallow event listener bindings with a noncallable type. | error | error |
| [no-boolean-in-attribute-binding](#-no-boolean-in-attribute-binding) | Disallow attribute bindings with a boolean type. | error | error |
| [no-complex-attribute-binding](#-no-complex-attribute-binding)   | Disallow attribute bindings with a complex type. | error | error |
| [no-nullable-attribute-binding](#-no-nullable-attribute-binding) | Disallow attribute bindings with nullable types such as "null" or "undefined".  | error | error |
| [no-incompatible-type-binding](#-no-incompatible-type-binding)   | Disallow incompatible type in bindings.  | error | error |
| [no-invalid-directive-binding](#-no-invalid-directive-binding)   | Disallow using built-in directives in unsupported bindings. | error | error |

**Validating LitElement**

<!-- prettier-ignore -->
| Rule    | Description | Severity normal | Severity strict |
| :------ | ----------- | --------------- | --------------- |
| [no-incompatible-property-type](#-no-incompatible-property-type) | When using the @property decorator in Typescript, the property option `type` is checked against the declared property Typescript type | error | error |
| [no-unknown-property-converter](#-no-unknown-property-converter) | LitElement provides default converters. For example 'Function' is not a valid default converter type for a LitElement-managed property. | error | error |
| [no-invalid-attribute-name](#-no-invalid-attribute-name)         | When using the property option `attribute`, the value is checked to make sure it's a valid attribute name. | error | error |
| [no-invalid-tag-name](#-no-invalid-tag-name)                     | When defining a custom element the tag name is checked to make sure it's valid. | error | error |

**Validating CSS**

<!-- prettier-ignore -->
| Rule    | Description | Severity normal | Severity strict |
| :------ | ----------- | --------------- | --------------- |
| [💅 no-invalid-css](#-no-invalid-css) | CSS within the tagged template literal `css` will be validated. | warning | error |


### Validating custom elements

All web components in your code are analyzed using [web-component-analyzer](https://github.com/runem/web-component-analyzer) which supports native custom elements and web components built with LitElement.

#### 🤷‍ no-unknown-tag-name

Web components defined in libraries need to either extend the global `HTMLElementTagNameMap` (typescript definition file) or include the "@customElement tag-name" jsdoc on the custom element class.

Below you will see an example of what to add to your library typescript definition files if you want type checking support for a given html tag name.

```typescript
declare global {
  interface HTMLElementTagNameMap {
    "my-element": MyElement;
  }
}
```

#### 📣 no-missing-import

When using custom elements in HTML it is checked if the element has been imported and is available in the current context. It's considered imported if any imported module (or their imports) defines the custom element.

The following example is considered a warning:
```js
// No import of "my-element"
html`<my-element></my-element>`
```

The following example is not considered a warning:
```js
import "my-element.js";
html`<my-element></my-element>`
```


#### ☯ no-unclosed-tag

Unclosed tags, and invalid self closing tags like custom elements tags, are checked.

The following examples are considered warnings:
```js
html`<div>`
html`<video />`
html`<custom-element />`
```

The following examples are not considered warnings:
```js
html`<div></div>`
html`<custom-element></custom-element>`
html`<video></video>`
html`<input />`
```

### Validating binding names

Attributes, properties and events are picked up on custom elements using [web-component-analyzer](https://github.com/runem/web-component-analyzer) which supports native custom elements and web components built with LitElement.

#### ✅ no-unknown-attribute, no-unknown-property

You will get a warning whenever you use an unknown attribute or property. This check is made on both custom elements and built in elements. 

**The following example is considered a warning:**
```js
html`<input .valuuue="${value}" unknownattribute="button" />`
```

**The following example is not considered a warning:**
```js
html`<input .value="${value}" type="button" />`
```

#### ⚡️ no-unknown-event

You can opt in to check for unknown event names. Using the `@fires` jsdoc or the statement `this.dispatch(new CustomElement("my-event))` will make the event name available. All event names are accepted globally because events bubble. 

The following example is considered a warning:
```js
html`<input @iinput="${console.log}" />`
```

The following example is not considered a warning:
```js
html`<input @input="${console.log}" />`
```

#### 📬 no-unknown-slot

Using the "@slot" jsdoc tag on your custom element class, you can tell which slots are accepted for a particular element. Then you will get warnings for invalid slot names and if you forget to add the slot attribute on elements without an unnamed slot.

```js
/**
 * @slot - This is a comment for the unnamed slot
 * @slot right - Right content
 * @slot left
 */
class MyElement extends HTMLElement {
}
customElements.define("my-element", MyElement);
```

The following example is considered a warning:
```js
html`
<my-element>
  <div slot="not a slot name"></div>
</my-element>
`
```

The following example is not considered a warning:
```js
html`
<my-element>
  <div></div>
  <div slot="right"></div>
  <div slot="left"></div>
</my-element>
`
```


### Validating binding types

Be aware that many checks involving analyzing bindings will work better in Typescript files because we have more information about the values being bound.

#### ❓ no-invalid-boolean-binding

It never makes sense to use the boolean attribute binding on a non-boolean type.

The following example is considered a warning:
```js
html`<input ?type="${"button"}" />`
```

The following example is not considered a warning:
```js
html`<input ?disabled="${isDisabled}" />`
```

#### ⚫️ no-expressionless-property-binding

Because of how `lit-html` [parses bindings internally](https://github.com/Polymer/lit-html/issues/843) you cannot use the property binding without an expression.

The following example is considered a warning:
```js
html`<input .value="text" />`
```

The following example is not considered a warning:
```js
html`<input .value="${text}" />`
```

#### 🌀 no-noncallable-event-binding

It's a common mistake to incorrectly call the function when setting up an event handler binding instead of passing a reference to the function. This makes the function call whenever the code evaluates. 

The following examples are considered warnings:
```js
html`<button @click="${myEventHandler()}">Click</button>`
html`<button @click="${{hannndleEvent: console.log()}}">Click</button>`
```

The following examples are not considered warnings:
```js
html`<button @click="${myEventHandler}">Click</button>`
html`<button @click="${{handleEvent: console.log}}">Click</button>`
```

#### 😈 no-boolean-in-attribute-binding

You should not be binding to a boolean type using an attribute binding because it could result in binding the string "true" or "false". Instead you should be using a **boolean** attribute binding.

This error is particular tricky, because the string "false" is truthy when evaluated in a conditional.

The following example is considered a warning:
```js
html`<input disabled="${isDisabled}" />`
```

The following example is not considered a warning:
```js
html`<input ?disabled="${isDisabled}" />`
```

#### ☢️ no-complex-attribute-binding

Binding an object using an attribute binding would result in binding the string "[object Object]" to the attribute. In this cases it's probably better to use a property binding instead.

The following example is considered a warning:
```js
html`<my-list listitems="${listItems}"></my-list>`
```

The following example is not considered a warning:
```js
html`<my-list .listItems="${listItems}"></my-list>`
```


#### ⭕️ no-nullable-attribute-binding 

Binding `undefined` or `null` in an attribute binding will result in binding the string "undefined" or "null". Here you should probably wrap your expression in the "ifDefined" directive.

The following examples are considered warnings:
```js
html`<input value="${maybeUndefined}" />`
html`<input value="${maybeNull}" />`
```

The following examples are not considered warnings:
```js
html`<input value="${ifDefined(maybeUndefined)}" />`
html`<input value="${ifDefined(maybeNull === null ? undefined : maybeNull)}" />`
```

#### 💔 no-incompatible-type-binding

Assignments in your HTML are typed checked just like it would be in Typescript.

The following examples are considered warnings:
```js
html`<input type="wrongvalue" />`
html`<input placeholder />`
html`<input max="${"hello"}" />`
html`<my-list .listItems="${123}"></my-list>`
```

The following examples are not considered warnings:
```js
html`<input type="button" />`
html`<input placeholder="a placeholder" />`
html`<input max="${123}" />`
html`<my-list .listItems="${listItems}"></my-list>`
```

#### 💥 no-invalid-directive-binding

Directives are checked to make sure that the following rules are met: 
* `ifDefined` is only used in an attribute binding.
* `class` is only used in an attribute binding on the 'class' attribute.
* `style` is only used in an attribute binding on the 'style' attribute.
* `unsafeHTML`, `cache`, `repeat`, `asyncReplace` and `asyncAppend` are only used within a text binding.

The directives already make these checks on runtime, so this will help you catch errors before runtime.

The following examples are considered warnings:
```js
html`<button value="${unsafeHTML(html)}"></button>`
html`<input .value="${ifDefined(myValue)}" />`
html`<div role="${class(classMap)}"></div>`
```

The following examples are not considered warnings:
```js
html`<button>${unsafeHTML(html)}</button>`
html`<input .value="${myValue}" />`
html`<input value="${myValue}" />`
html`<div class="${class(classMap)}"></div>`
```



### Validating LitElement

#### 💞 no-incompatible-property-type

When using the @property decorator in Typescript, the property option `type` is checked against the declared property Typescript type.

The following examples are considered warnings:
```js
class MyElement extends LitElement {
  @property({type: Number}) text: string;
  @property({type: Boolean}) count: number;
  @property({type: String}) disabled: boolean;
  @property({type: Object}) list: ListItem[];
}
```

The following examples are not considered warnings:
```js
class MyElement extends LitElement {
  @property({type: String}) text: string;
  @property({type: Number}) count: number;
  @property({type: Boolean}) disabled: boolean;
  @property({type: Array}) list: ListItem[];
}
```

#### 👎 no-unknown-property-converter

The default converter in LitElement only accepts `String`, `Boolean`, `Number`, `Array` and `Object`, so all other values for `type` are considered warnings. This check doesn't run if a custom converter is used.

The following example is considered a warning:
```js
class MyElement extends LitElement {
  static get properties () {
    return {
      callback: {
        type: Function
      },
      text: {
        type: MyElement
      }
    }
  }
}
```

The following example is not considered a warning:
```js
class MyElement extends LitElement {
  static get properties () {
    return {
      callback: {
        type: Function,
        converter: myCustomConverter
      },
      text: {
        type: String
      }
    }
  }
}
```


#### ⁉️ no-invalid-attribute-name

When using the property option `attribute`, the value is checked to make sure it's a valid attribute name.

The following example is considered a warning:
```js
class MyElement extends LitElement {
  static get properties () {
    return {
      text: {
        attribute: "invald=name"
      }
    }
  }
}
```

#### ⁉️ no-invalid-tag-name

When defining a custom element, the tag name is checked to make sure it's a valid custom element name.

The following example is considered a warning:
```js
@customElement("wrongElementName")
class MyElement extends LitElement {
}

customElements.define("alsoWrongName", MyElement);
```

The following example is not considered a warning:
```js
@customElement("my-element")
class MyElement extends LitElement {
}

customElements.define("correct-element-name", MyElement);
```

### Validating CSS

`lit-analyzer` uses [vscode-css-languageservice](https://github.com/Microsoft/vscode-css-languageservice) to validate CSS.

#### 💅 no-invalid-css

CSS within the tagged template literal `css` will be validated. 

The following example is considered a warning:
```js
css`
  button
    background: red;
  }
`
```

The following example is not considered a warning:
```js
css`
  button {
    background: red;
  }
`
```

[![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)](#documenting-slots-events-attributes-and-properties)

## ➤ Documenting slots, events, attributes and properties

Code is analyzed using [web-component-analyzer](https://github.com/runem/web-component-analyzer) in order to find properties, attributes and events. Unfortunately, sometimes it's not possible to analyze these things by looking at the code, and you will have to document how your component looks using `jsdoc`like this:

```js
/**
 * This is my element
 * @attr size
 * @attr {red|blue} color - The color of my element
 * @prop {String} value
 * @prop {Boolean} myProp - This is my property
 * @fires change
 * @fires my-event - This is my own event
 * @slot - This is a comment for the unnamed slot
 * @slot right - Right content
 * @slot left
 */
class MyElement extends HTMLElement { 
}

customElements.define("my-element", MyElement);
```



[![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)](#contributors)

## ➤ Contributors
	

| [<img alt="Rune Mehlsen" src="https://avatars2.githubusercontent.com/u/5372940?s=460&v=4" width="100">](https://twitter.com/runemehlsen) | [<img alt="Andreas Mehlsen" src="https://avatars1.githubusercontent.com/u/6267397?s=460&v=4" width="100">](https://twitter.com/andreasmehlsen) | [<img alt="You?" src="https://joeschmoe.io/api/v1/random" width="100">](https://github.com/runem/lit-analyzer/blob/master/CONTRIBUTING.md) |
|:--------------------------------------------------:|:--------------------------------------------------:|:--------------------------------------------------:|
| [Rune Mehlsen](https://twitter.com/runemehlsen)  | [Andreas Mehlsen](https://twitter.com/andreasmehlsen) | [You?](https://github.com/runem/lit-analyzer/blob/master/CONTRIBUTING.md) |


[![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)](#license)

## ➤ License
	
Licensed under [MIT](https://opensource.org/licenses/MIT).