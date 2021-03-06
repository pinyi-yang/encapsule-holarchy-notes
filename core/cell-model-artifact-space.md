# Cell Model Artifact Space (CMAS)
[<- back to Holarchy](../README.md)
*last updated 03/05/21*

<!-- reference -->
<!-- external references -->
[arccore filter]: https://encapsule.io/docs/ARCcore/filter
[arccore identifier]: https://encapsule.io/docs/ARCcore/identifier
<!-- core references -->
[ocd]: ./observable-controller-data.md
[opc]: ./observable-process-controller.md
[apm]: ./abstract-process-model.md 
[top]: ./transition-operator.md
[act]: ./controller-action.md
[cp]: ./cell-procssor.md
[cm]: ./cell-model.md
[cmas]: ./cell-model-artifact-space.md
[cmt]: ./cell-model-template.md
<!-- root reference -->
[top list]: ../transition-operator-apis.md
[act list]: ../controller-action-apis.md

Cell Model Artifact Space (CMAS) is an ES6 class instantiated with operator new that represents a unique mapping between a [Cell Model][cm] "artifact label" value (a human-readable string) and a set of unique [Cell Model][cm] artifact IRUT ID's within that artifact space. It is used primarily to ensure that [Cell Model][cm] instances synthesized via [Cell Model Template][ctm] class instances that may be later resolved by querying the appropriate CellModelArtifactSpace instance w/a so-called [Cell Model][cm] label string in order to obtain IRUT ID's.

It is a tool for 
* easy creation of traceable and conflict-free runtime irut for [Cell Models][cm], especially for runtime-created [Cell Models][cm]
* easy index of irut for access [Cell Models][cm] and its cell in [OPC][opc] or [CP][cp]

Table of Content
* [Construction](#Construction)
* [CMAS ES6 Class APIs](#CMAS-ES6-Class-APIs)
* [Example](#Example)

## Construction
To create a CMAS instance:
```javascript
const holarchy = require("@encapsule/holarchy");

const cmasInstance = new holarchy.CellModelArtifactSpace({
    spaceLabel: "your_space_label"
});

if (!cmasInstance.isValid()) throw new Error(cmasInstance.toJSON());

module.exports = cmasInstance;
```

*spaceLabel* is a artifact label (a human-readable string) for [Cell Model][cm] (a space). It will defined the irut for all your [ACTs][act], [TOPs][top] and sub [Cell Models][cm] (sub spaces) with the [CMAS ES6 Class APIs](#CMAS-ES6-Class-APIs) based on their own labels.

At the root level, one can use its app name as the *spaceLabel*. To check the spaceLabel of a CMAS instance:
```javascript
const spacelabeOfCMAS = cmasInstance.spaceLabel;
```

# CMAS ES6 Class APIs
| Method | Description |
|-|-|
| .isValid() | check whether the current CMAS instance is valid or not. Return true or false |
| .toJSON() | Convert the CMAS isntance into an JSON object |
| .mapLabels(request_) | get iruts for ACTs, TOPs, CMs and so on for current CMAS based on input |
| .makesubspaceInstance(request_) | create a sub CMAS of current CMAS based on the request_ |

* .mapLabels(request_) ([example](#example))
```javascript
const makeLabelRequest_ = {
    // one or more below
    CM: "cm_name",
    APM: "apm_name",
    ACT: "act_name", // CM label is required
    TOP: "top_name", // CM label is required
    OTHER: "other_name"
};

const makeLabelReturn = {
    result: {
        CM: "cm_name",
        CMID: "irut", // based on the name
        APM: "apm_name",
        APMID: "irut", // based on the name
        ACT: "act_name",
        ACTID: "irut", // based on the name
        TOP: "top_name",
        TOPID: "irut", // based on the name
        OTHER: "other_name",
        OTHERID: "irut", // based on the name
    }
}
```

* .makesubspaceInstance(request_) ([example](#example))
```javascript
const request_ = {
    spaceLabel: "sub_space_name"
}
```

## Example
1. Suppose we are creating an app name: `my_app`. We can start with a CMAS:
```javascript
const holarchy = require("@encapsule/holarchy");

const myAppCMAS = new holarchy.CellModelArtifactSpace({
    spaceLabel: "my_app"
});

if (!myAppCMAS.isValid()) throw new Error(myAppCMAS.toJSON());
```
<br>
<br>

2. In this app, we will create a [Cell Model][cm]: `some_cm` and we need irut ID for both the CM and APM:
```javascript
const makeLabelResponse = myAppCMAS.mapLabels({
    // notice it is OK to have the same name
    CM: "some_cm",
    APM: "some_cm"
});

console.log(makeLabelResponse)
// console: 
// {
//   error: null,
//   result: {
//     CM: 'some_cm',
//     CMID: 'M9_rG9wbUN4aRlMZcPBhfQ',
//     APM: 'some_cm',
//     APMID: 'o_8k2_8UJ_BxFZZPxMAXJQ',
//   }
// }
```
It gives us a tracable and conflict-free irut for both APM and CM of `some_cm` under the `my_app` space.
<br>
<br>

3. Then we want to create an ACT called `an_action` for a CM `some_cm`.
```javascript

console.log(myAppSomeCMSubCMAS.mapLabels({
    CM: "some_cm",
    ACT: "an_action"
}))
// console: 
// {
//   error: null,
//   result: {
//     CM: 'some_cm',
//     CMID: 'M9_rG9wbUN4aRlMZcPBhfQ',
//     ACT: 'an_action',
//     ACTID: 'LY9fH4DJ7i-V0q4VwYyNVw',
//   }
// }
```
