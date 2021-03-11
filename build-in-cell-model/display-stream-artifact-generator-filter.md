# Display Stream Artifact Generator Filter (WIP)
[<- back to Holarchy](../README.md)
*last updated 03/10/21*

<!-- reference -->
<!-- external references -->
[arccore filter]: https://encapsule.io/docs/ARCcore/filter
[arccore identifier]: https://encapsule.io/docs/ARCcore/identifier
<!-- core references -->
[ocd]: ../core/observable-controller-data.md
[opc]: ../core/observable-process-controller.md
[apm]: ../core/abstract-process-model.md
[top]: ../core/transition-operator.md
[act]: ../core/controller-action.md
[cp]: ../core/cell-procssor.md
[cm]: ../core/cell-model.md
[cmas]: ../core/cell-model-artifact-space.md
[cmt]: ../core/cell-model-template.md
<!-- holarchy cm: build-in-cell-model reference -->
[ovh]: ../build-in-cell-model/observable-value-helper
[ov]: ../build-in-cell-model/observable-value-family.md
[dv]: ../build-in-cell-model/display-view-family.md
[dsag filter]: ../build-in-cell-model/display-stream-artifact-generator-filter.md
<!-- root reference -->
[top list]: ../transition-operator-apis.md
[act list]: ../controller-action-apis.md
[ipc]: ../inter-process-communication.md

***Unofficial Notes***

## Introduction

generate coupled [Synthesized Display View CM][dv] and d2r2 component based on the input *displayViewSynthesizeRequest* and *reactComponentClass*. 
It extends the input *reactComponentClass* into *ViewDisplayProcess* class defined within filter.

## Make Request
```javascript
const holarchy = require("@encapsule/holarchy");
const holarchyCM = require("@encapsule/holarchy-cm");

const generateResponse = holarchyCM.generateDisplayStreamModels.request({
    displayViewSynthesizeRequest: {
        //object matches the DisplayView_T.synthesizeCellModel arguments
        // check Display View Family for more details
    },
    reactComponentClass, // react component class
});
if (generateResponse.error) throw new Error(generateResponse.error)
const { CellModel, d2r2Component } = generateResponse.result;
// CellModel: Synthesized Display View CM
// d2r2Component: a d2r2Component corresponding to above Cell Model
```

### Request
* *displayViewSynthesizeRequest*: object that matches the [Display View Template][dv] *synthesizeCellModel* method spec. (check its [readme][dv]) for more details
* *reactComponentClass*: a react component class that based on the Syntheiszed Dispaly View CM with the *displayViewSynthesizeRequest*

### Result
* *CellModel*: Synthesized Display View CM with *displayViewSynthesizeRequest*
* *d2r2Component*: a d2r2 component. Its below property is defined by this generator:
    * *reactComponent*: a extended React.Component class based on the *reactComponentClass* with
        * construction: add above Synthesized Display View CM ACT-linkDisplayProcess: `display-process-activated`
        * componentWillUnmount: add above Synthesized Display View CM ACT-linkDisplayProcess: `display-process-deactivating`
    * *renderDataBindingSpec* of the d2r2 component: this generator creates an unique spec based on the apm ID,  *cellModelLabel* and *displayLayoutSpec* of the above *CellModel*.
At frontend development, to obtain d2r2 *renderDataBindingSpec* for a d2r2 component, the *cellModelLabel* is neccessary:
`todo: what will be the best practice to obtain it?`
<!-- 1. synthesize the Displa View CM again
2. get the apmID
2. obtain the spec -->

