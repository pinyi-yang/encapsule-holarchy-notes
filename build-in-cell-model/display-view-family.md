# Display View Family (WIP)
[<- back to Holarchy](../README.md)
*last updated 03/22/21*

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
Display View Family is [Cell Models][cm] synthesized with [Display View Template](#Display-View-Template) (a [Cell Model Template][cmt] instance) for `todo add purpose`. 

Each member of Display View Family has a specialized  [Observable Value][ov] at path  `#.outputs.displayView` which is synthesized through [Display Stream Message Template](#Display-Stream-Message-Template) depending on the *displayLayoutSpec*.

Display View Family has :
* same APM steps
* same OCD except `#.outputs.displayView`
* same [ACTs][act]
* specialized [OV][ov] at `#.outputs.displayView`

During development, it is recommended to create a Synthesized Display View CM with the [Display Stream Artifact Generator][dsag] so that the synthesized Display View CM and its bundle d2r2 component (`todo: add link to d2r2 component`) can be generated in-sync at the same time.

### Display View Template
Display View Template is a [Cell Model Template][cmt] with:
* *cmasScop*: `@encapsule/holarchy-cm` [CMAS][cmas]
* *templateLabel*: `DisplayView`
* *specializationDataSpec*: 
```javascript
const specializationDataSpec = {
    ____types: "jsObject",
    description: { ____accept: "jsString" }, // description of synthesized member function
    displayElement: {
        ____types: "jsObject",
        ____defaultValue: {},
        displayLayoutSpec: { ____accept: "jsObject" }
    }
}
```
It is used to synthesize a Display View Family member by the *.synthesizeCellModel* method in the [Cell Model Template][cmt] with the *specializationData* matches the above *specializationDataSpec*.


## Synthesize Display View Cell Model
As mentioned above, It is recommended to create a synthesized Display View CM through the [Display Stream Artifact Generator][dsag]. But if one want to create a synthesized Display View CM without using the built-in generator:

```javascript
const holarchyCM = require("@encapsule/holarchy-cm");

const synthesisResponse = holarchyCM.cmtDisplayView.synthesizeCellModel({
    cellModelLabel: "name_of_cell",
    specializationData: {
        description: "description on function of synthesized cell",
        displayElement: {
            displayLayoutSpec: {
                //... spec match a registered d2r2 view components
            }
        }
    }
});

if (synthesisResponse.error) throw new Error(synthesisResponse.error);
const synthesizedDisplayViewCM = synthesisResponse.result;
```
* *cellModelLabel*: a unique name in the `@encapsule/holarchy-cm∂DisplayView` [CMAS][cmas].
* *description*: developer-provided description of the function/purpose to the synthesized Display View Cell Model.
* *displayElement.displayLayoutSpec*: an [arccore filter][arccore filter] spec that can take empty argument as filter request.

In holistic app development (`todo: add link to holistic app development readme`), *displayElement.displayLayoutSpec* should always match the render data spec of a registered [@encapsule/d2r2][encapsule] component in the Components Router.

During synthesis, 
* [Display Stream Message Template](#Display-Stream-Message-Template), *cellModelLabel* and *displayLayoutSpec* are used to create a **Synthesized Display Stream Message CM**.
* The **Synthesized Display Stream Message CM** is used as a helper at path `#.outputs.displayView` with is the specialized namespace of Display View family.
```javascript
const synthesizedDVOCDSpec = {
    inputs: {
        ____types: "jsObject",
        ____defaultValue: {},
        displayViews: {
            ____types: "jsObject",
            ____asMap: true,
            ____defaultValue: {},
            subviewLabel, // Observable Value Helper
        },
        dynamicSubDisplayViews, // ObservableValueHelperMap Helper
        }
    },
    outputs: {
        ____types: "jsObject",
            ____defaultValue: {},
        displayView, // above Synthesized Display Stream Message CM as helper
    },
    core: {
        // Cell Model internal used namespaces
        ____types: "jsObject",
        ____defaultValue: {},
        viewDisplayProcess: {
            ____types: ["jsUndefined", "jsObject"],
            displayName: {
                ____accept: "jsString"
            },
            displayPath: {
                ____accept: "jsString"
            },
            thisRef: {
                ____accept: "jsObject"
            }
        }
    }
}
```

**Steps**
* uninitialized: default start step
    1. => display-view-initialize
* display-view-initialize: (ACT-stepWorker: "initialize")
    1. => display-view-wait-view-display
* display-view-wait-view-display:
    1. `#.core.viewDisplayProcess` truthy  => display-view-display-process-linked
* display-view-display-process-linked:
* display-view-quiescent:

### Display View APIs
WIP
Display View family have 1 public ACT ([ACT-linkDisplayProcess](#dv-link-display-process)) and 1 private ACT ([ACT-stepWorker](#dv-step-worker)) stored in its subcell DisplayViewBase and shared across all the members

[ACT-linkDisplayProcess](#dv-link-display-process) of a Synthesized Display View CM is mainly used in its corresponding d2r2 

<div id="dv-link-display-process">
    <strong>ACT-linkDisplayProcess</strong>
</div>
<!-- ? why does this ACT have a different spec  -->

```javascript
const actionRequest = {holarchy: {common: {actions: {service: {html5: {display: {view: {
    linkDisplayProcess: {
        notifyEvent: "vd-root-activated", // "vd-child-activated", "vd-deactivating"
        reactElement: {
            displayName: "some_name", // cellModelLabel
            displayPath: "displayInstanceX.displayInstanceY.displayInstanceZ",
            displayInstance: "unique_name_under_path",
            displayViewAPMID: "irut", // if React Element is mounted via ViewDisplayProcess::mountSubViewDisplay 
            // method,
            thisRef: {
                // The React.Element sets thisRef to `this` inside its onComponentDidMount and componentWillUnmount
                // methods so it's backing DisplayView_T cell can stream data to the component directly w/out 
                // re-render of the entire VDOM. 
            }
        }
    }
}}}}}}}}

const actionResult: {
    error: "",
    result: {} //opaque
}
```
<br>

<div id="dv-step-worker">
    <strong>ACT-writeValue</strong>
    (<a href="#Observable-Value-Cell-Model-Family-APIs">back to List</a>)
</div>

```javascript
const actionRequest = {holarchy: {common: {actions: {ObservableValue: {writeValue: {
    path: "#.", // relative path of OV inside the provider Cell Model
    value, // opaque, match the valueTypeSpec

}}}}}}
```
<br>


## Display Stream Message Template


## Examples