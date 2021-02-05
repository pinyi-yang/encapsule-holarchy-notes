# Observable Value Factory
[<- back to Holarchy](../README.md)
* [Filter (Create ObservableValue Cell Model)](#Filter-Create-ObservableValue-Cell-Model))
* [Observable Value Cell Model](#Observable-Value-Cell-Model)
* [Use ObservableValue Cel Model](#Use-ObservableValue-Cel-Model)

**Observable Value Factory** is a new filter published in Holistic 0.0.50. It creates a specialized ObservableValue Cell Model based on the inputs.

The ObservableValue Cell Model is designed to be used as a **helper** in a host Cell that allows the host to publish its specified property to other [Value Observers](./value-observer.md) for easy observing and access. It is a new cell communication method in Holistic 0.0.50.

`todo add relation between ObservableValue, ValueObserver and ValueObserverWorker`

## Filter (Create ObservableValue Cell Model)
Observablue Value Factory can simply create a type-specifed ObservableValue Cell Model for any host cell:

```javascript
const holarchyCM = require("@encapsule/holarchy-cm");

const factoryResponse = holarchyCM.factories.makeObservableValueCellModel({
    cellID: "irut for the specified ObservableValue cell model",
    apmID: "irut for the specified ObservableValue APM",
    valueTypeLabel: "label for the specified value",
    valueTypeDescription: "description how the published value",
    valueTypeSpec: {} // any type
    //it is the spec of the value
});
if (factoryResponse.error) throw new Error(factoryResponse);

const ObservableValueCellModel = factoryResponse.result;
```

<br>

For the cellID and apmID, the recommended name convention is to use arccore:

```javascript
const arccore = require("@encapsule/arccore");

const cellID = arccore.identifier.irut.fromReference("host_cell_name.ObservableValue.CellModel.value_spec_name");
const apmID = arccore.identifier.irut.fromReference("host_cell_name.ObservableValue.AbstractProcessModel.value_spec_name");
```

Then, the ObservableValueCellModel is ready to be used a helper in the host cell.

## Observable Value Cell Model
```javascript
// value from the fitler input above
const valueTypeSpec;

const ObservableValueCellOCD = {
    value: valueTypeSpec, // same as specified in filter
    reivsion: -1
}
```

**steps**
1. uniitialized: default start step
    * => observable-value-initialize
2. observable-value-initialize: 
    * #.revision > -1 => observable-value-ready
    * => observable-value-reset

### Controller Actions
**reset**
```javascript
const actionRequest = {holarchy: {cm: {actions: {ObservableValue: {reset: {}}}}}}
```
reset the ObservableValue CP into initial state
<br>

**write**
```javascript
const actionRequest = {holarchy: {cm: {actions: {ObservableValue: {write: {
    value: valueTypeSpec
}}}}}}
```
write input value into #.value, update revision and change step into observable-value-ready

**read (TBD)**
```javascript
const actionRequest = {holarchy: {cm: {actions: {ObservablueValue: {read: {}}}}}}
```
function TBD

## Use ObservableValue Cel Model
Once a valid value-specified ObservableValue Cell Model is obtained, it can be used as a helper like any other cell model. Here is an example.
```javascript
const hostCellOCDSpec = {
    ____types: "jsObject",
    ____defaultValue: {},
    observableValues: { // optional namespace
        ____types: "jsObject",
        ____defaultValue: {},
        nameForSpecifiedObserValue: {
            ____types: "jsObject",
            ____defaultValue: {},
            ____appdsl: {
                apm: "the irut apm id in the filter"
            }
        }
    }
}
```

**NOTICE**
A value-specified ObservableValue helper can **ONLY** make a value observable in the host cell. In order to let other cells observe the value, following Cell Models are also necessary:
* [Value Observer Worker](./value-observer-worker.md): `todo add introduction on function of Value Observer Worker`
* [Value Observer](./value-observer.md): `todo add introduction on the function of Value Observer`