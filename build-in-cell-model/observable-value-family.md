# Observable Value Family (OV)
[<- back to Holarchy](../README.md)

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
<!-- root reference -->
[top list]: ../transition-operator-apis.md
[act list]: ../controller-action-apis.md
[ipc]: ../inter-process-communication.md

## Introduction
Observable Value Cell Model Family (OV) is a family of Cell Models sythesized by Observable Value Template - a [Cell Model Template][cmt] instance. Each member provides a counted read/write mailbox over [a specific data type](#artifacts-specialization). It sypically actiated as helper cell that define input and ouput mailboxes of a host cell.

Observalube Value Cell Family are one of members in the [Inter-process Communication][ipc] Observable Value Method. It serves as an container for data in a provider cell that need to be observed by [Observer Value Helper][ovh]. ONLY data (value) inside a sythesized Observable Value Cell can be observed by a [Observer Value Helper][ovh].

* [Sythesize Specialized Observable Value Cell Model](#Sythesize-Specialized-Observable-Value-Cell-Model)
* [Use SynthesizeSpecialized OV](#use-synthesized-ov)
* [Observable Value Cell Model Family APIs](#Observable-Value-Cell-Model-Family-APIs)
* [Example](#example)

## Sythesize Specialized Observable Value Cell Model
To sythesized a specialized Observerable Value [Cell Model][cm], simple use the *.synthesizeCellModel* method with the specialization data match the pre-defined specialization data spec of Observablue Value Template:
```javascript
const holarchyCM = require("@encapsule/holarchy-cm");

const synthesizeResponse = holarchyCM.cmtObservableValue.synthesizeCellModel({
    cellModelLabel: "name_for_the_sythesized_observalue_value_cell",
    specializationData: {
        // pre-defined namespace in the Observablue Value Template
        valueTypeDescription: "description for the observable value contained",
        valueTypeSpec: arccore_filter_spec_match_the_observable_data
    }
});
if (synthesizeResponse.error) throw new Error(synthesizeResponse.error);
const sythesizedObservablueValueCM = synthesizeResponse.result;
```
**cellModelLabel**: unique label in the Observablue Value Family that decides the id of the synthesized.
**valueTypeDescription**: description for the observablue value contained
**valueTypeSpec**: spec match the data will be stored. See [Example](#example) for more details.
<br><br>

The sythesized [Cell Model][cm] will share same [ACTs][act], [TOPs][top], steps in [APM][apm] and most [OCD][ocd] data spec but a specialized *mailbox* namespace in [OCD][ocd] data spec.
```javascript
// sythesizedObservablueValueCM:
{
    id: "irut_based_on_cellModelLabel",
    name: "name of the synthesized OV cell",
    description: "",
    apm: {
        id: "irut_based_on_cellModelLabel",,
        name: "apm name of the synthesized OV cell",
        description: "",
        ocdDataSpec: {
            ____types: "jsObject",
            ____defaultValue: {},
            mailbox: {
              ____types: ["jsUndefined", "jsObject"],
              value: valueTypeSpec_at_synthesis
            },
            revision: {
              ____types: "jsNumber",
              ____defaultValue: -1
            },
            dact: {
              ____accept: ["jsUndefined", "jsObject"]
            }
        },
        steps: {
            "uninitialized": {
                description: "Default starting process step.",
                transitions: [
                    {
                        transitionIf: {holarchy: {cm: {operators: {ocd: {compare: {values: {
                                    a: { path: "#.revision"},
                                    operator: ">",
                                    b: { value: -1}
                        }}}}}}},
                        nextStep: "observable-value-ready"
                    }, 
                    {
                        transitionIf: { always: true},
                        nextStep: "observable-value-reset"
                    }
                ]
            },
            "observable-value-reset": {
                description: "ObservableValue has not yet been written and is in reset process step."
            },
            "observable-value-ready": {
                description: "ObservableValue has been written and can be read and/or observed for subsequent update(s) by a value producing cell process."
            }
        }

    },
    subcells: [ 
        cmObservableValueBase, // Generic behaviors of ObservableValue_T, container for APIs
    ]
};
```
***mailbox.value***: this namespace in ocdDataSpec will be the ONLY specialization for each member in the Observable Value Family. It is defined by the *valueTypeSpec* during [Sythesize Specialized Observable Value Cell Model](#Sythesize-Specialized-Observable-Value-Cell-Model)
***dact***: dact stands for deferred action which can be defined by the [ACT-setDefferredAction](#set-defferred-action). It stored an registered Controller Action request that will be trigger everytime the [ACT-readValue](#read-value) of a syntesized Observable Value Cell is called. It turns the Observable Value into the lazy evaluation mode. 
***cmObservableValueBase***: this subcell is purely a container for the [shared APIs](#Observable-Value-Cell-Model-Family-APIs) in the Observable Value Family. Automatically included in the Observable Value Family.
<br><br>

## Use-SynthesizeSpecialized-OV
The purpose of using a sythesized OV is to make the *value* inside the *mailbox* observable by [Observable Value Helper][ovh]. It is usually used as a helper in the data provider Cell Model with a matching *valueTypeSpec*.
```javascript
const synthesizedOVAPMID = sythesizedObservablueValueCM.apm.id

const providerCM = {
    // a Cell Model Declaration
    apm: {
        // ...
        ocdDataSpec: {
            ____types: "jsObject",
            ____defaultValue: {},
            // namespace of your choice
            ov: {
                ____types: "jsObject",
                ____defaultValue: {},
                ____appdsl: { apm: synthesizedOVAPMID }
            }
        }
        // ...
    }
}
```
For more detail, please check the [Example](#example). If you are interested in the details of [Inter-process Communication][ipc] with Observable Value Method, please check the [Inter-process Communication][ipc] documents.


## Observable Value Cell Model Family APIs
The Observable Value Cell Model Family has 3 ACTs and 3 TOPs of its own.

**Controller Action**
* [ACT-readValue](#read-value): read the *value* from *mailbox*.
* [ACT-writeValue](#write-value): write input value into *mailbox.value*.
* [ACT-resetValue](#reset-value): clear *mailbox*. *mailbox* changes to undefined
* [ACT-setDefferredAction](#set-defferred-action): set an action request to the *dact* namespace in ocd which will be performed everytime Observable Value Cell ACT-readValue is performed.

**Transition Operator**
* [TOP-valueIsActive](#value-is-active): check if OV is exists in the provider Cell
* [TOP-valueIsAvailable](#value-is-available): check if OV is in step `observable-value-ready`
* [TOP-valueHasUpdated](#value-has-updated): check if OV has a newer value 

<hr>
<div id="read-value">
    <strong>ACT-readValue</strong>
    (<a href="#Observable-Value-Cell-Model-Family-APIs">back to List</a>)
</div>

```javascript
const actionRequest = {holarchy: {common: {actions: {ObservableValue: {readValue: {
    path: "#." // relative path of OV inside the provider Cell Model
}}}}}}

const actionResult: {
    error: "",
    result: {} //opaque
}
```
<br>

<div id="write-value">
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

<div id="rest-value">
    <strong>ACT-resetValue</strong>
    (<a href="#Observable-Value-Cell-Model-Family-APIs">back to List</a>)
</div>

```javascript
const actionRequest = {holarchy: {common: {actions: {ObservableValue: {resetValue: {
    path: "#.", // relative path of OV inside the provider Cell Model
}}}}}}
```
<br>

<div id="set-defferred-action">
    <strong>ACT-setDefferredAction</strong>
    (<a href="#Observable-Value-Cell-Model-Family-APIs">back to List</a>)
</div>

```javascript
const actionRequest = {holarchy: {common: {actions: {ObservableValue: {setDefferredAction: {
    path: "#.", // relative path of OV inside the provider Cell Model
    dact: {
        //a regestered action request 
    }
}}}}}}
```
<br>
<hr>


<div id="value-is-active">
    <strong>TOP-valueIsActive</strong>
    (<a href="#Observable-Value-Cell-Model-Family-APIs">back to List</a>)
</div>

```javascript
const actionRequest = {holarchy: {common: {operators: {ObservableValue: {valueIsActive: {
    path: "#.", // relative path of OV inside the provider Cell Model
}}}}}}
```
<br>

<div id="value-is-available">
    <strong>TOP-valueIsAvailable</strong>
    (<a href="#Observable-Value-Cell-Model-Family-APIs">back to List</a>)
</div>

```javascript
const actionRequest = {holarchy: {common: {operators: {ObservableValue: {valueIsAvailable: {
    path: "#.", // relative path of OV inside the provider Cell Model
}}}}}}
```
<br>

<div id="value-has-updated">
    <strong>TOP-valueHasUpdated</strong>
    (<a href="#Observable-Value-Cell-Model-Family-APIs">back to List</a>)
</div>

```javascript
const actionRequest = {holarchy: {common: {operators: {ObservableValue: {valueHasUpdated: {
    path: "#.", // relative path of OV inside the provider Cell Model
    lastReadRevision: -1 // version number to compare whether the TOP caller has a different version from the ov at path
}}}}}}
```
<br>

## Example