# Display Stream Artifact Generator Filter (WIP)
[<- back to Holarchy](../README.md)
*last updated 03/18/21*

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
    * *reactComponent*: a extended React.Component class ([View Display Process](#View-Display-Process)) based on the *reactComponentClass* 
    * *renderDataBindingSpec* of the d2r2 component: this generator creates an unique spec based on the apm ID,  *cellModelLabel* and *displayLayoutSpec* of the above *CellModel*.


#### View Display Process
View Display Process Class is an extend React.Component class defined by this generator filter with modified:
* **contruction**: add above Synthesized Display View CM ACT-linkDisplayProcess: `vd-root-activated` or `vd-child-activated` based one the *renderContext.d2r2BusState* passed in.
* **componentWillUnmount**: add above Synthesized Display View CM ACT-linkDisplayProcess: `display-process-deactivating`
* **mountSubViewDisplay**: method to render a sub view display by its:
    * *cmasScope*:
    * *displayViewCellModelLabel*:
    * *displayInstance*:
    * *displayLayout*:
**mountSubViewDisplay** lets developer convenient use d2r2 components without worrying their actual render spec as the actual render spec of d2r2 components is coded by this generator filter with complex patterns

```javascript
const d2r2ComponentReactComponent =  class ViewpathPage extends React.Component {
            constructor(props_) {
                super(props_);
                console.log("AppPage::constructor");
                this.displayName = "AppPage";
            }

            render() {
                console.log("AppPage::render");
                return (
                    <div>
                        <h1>{this.displayName}</h1>
                        // to render a sub view display
                        {this.mountSubViewDisplay({ cmasScope: app_scope, displayViewCellModelLabel: "any_display_view_label", displayInstance: "unique_name_1", displayLayout: {} })}
                        {this.mountSubViewDisplay({ cmasScope: app_scope, displayViewCellModelLabel: "any_display_view_label", displayInstance: "unique_name_2", displayLayout: {} })}
                    </div>
                );
            }
        }
```