<div id="TOPs-list"></div>

# Built-in Transition Operators (TOP)
[<- Back to Holarchy](./README.md)


* **[Holarchy Core](#Holarchy-Core)**
    * [array is empty](#array-is-empty)
    * [array length equal to](#array-length-equal-to)
    * [compare values](#compare-values)
    * [is namespace truthy](#is-namespace-truthy)
    * [namespace is map keyless](#namespace-is-map-keyless)

* **[Cell Process Manager (CPM)](#Cell-Process-Manager-CPM)**
    * [Cell is at step](#cell-is-at-step)
    * [Operator delegation](#operator-delegation)
    * [Ancestor (parent, child, descendant) Processes Active](#ancestor-processes-active)
    * [Ancestor (parent, child, descendant) Processes all in step](#ancestor-processes-all-in-step)
    * [Ancestor (child, descendant) Processes any in step](#ancestor-processes-any-in-step)

* **[Cell Process Proxy (CPP)](#Cell-Process-Proxy-CPP)**
    * [proxy status is](#proxy-status-is)

* **[Observable Value Family (OV)](#Observable-Value-Family)**
    * [OV is active](#ov-value-is-active)
    * [OV is available to read](#ov-value-is-available)
    * [OV value has updated](#ov-value-has-updated)

<br>

## Holarchy Core

<div id="array-is-empty">
    <strong>array is empty</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)
</div>

```javascript
const actionRequest = {holarchy: {cm: {operators: {ocd: {arrayIsEmpty: {path: "~. or #."}}}}}}
```

<div id="array-length-equal-to">
    <strong>array length equal to</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)
</div>

```javascript
const actionRequest = {holarchy: {cm: {operators: {ocd: {array: {
    path: "~. or #."
    legnth: {equalToValue: ""} //ocd path or a number
}}}}}}
```

<div id="compare-values">
    <strong>compare values</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)<br>
    There are greater than, identical to and less than transitor operators in the holarchy core as well.
</div>

```javascript
const actionRequest = {holarchy: {cm: {operators: {ocd: {compare: {values: {
    a: {
        path: "#. or ~."
        //or
        value: "", //number, string, boolean, null
    },
    b: {...}, //same as a
    operator: "=== or == or < or <= or > or >="
}}}}}}}
```


<div id="is-namespace-truthy">
    <strong>is namespace truthy</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)
</div>

```javascript
const actionRequest = {holarchy: {cm: {operators: {ocd: {isNamespaceTruthy: {path: "#. or ~."}}}}}}
```

<div id="namespace-is-map-keyless">
    <strong>namespace is map keyless</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)
</div>

```javascript
const actionRequest = {holarchy: {cm: {operators: {ocd: {mapIsKeyless: {path: "#. or ~."}}}}}}
```
<hr>



## Cell Process Manager (CPM)

<div id="cell-is-at-step">
    <strong>Cell is at step</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)
</div>

```javascript
const actionRequest = {CellProcessor: {cell: {
    // apmBindingPath, cellProcessID or relative path
    // default: "#", current cell
    cellCoordinates: {apmID: "", instanceName: ""}, 
    query: {inStep: {
        apmStep: ["step name"] // or a string of step
    }} 
}}}
```

<div id="operator-delegation">
    <strong>Operator delegation</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)
</div>

```javascript
const actionRequest = {CellProcessor: {cell: {
    // apmBindingPath, cellProcessID or relative path
    // default: "#", current cell
    cellCoordinates: {apmID: "", instanceName: ""}, 
    delegate: {
        operatorRequest: {} //a TOP
    } 
}}}
```

<div id="ancestor-processes-active">
    <strong>Ancestor Processes Active</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)
</div>

```javascript
const actionRequest = {CellProcessor: {cell: {
    // apmBindingPath, cellProcessID or relative path
    // default: "#", current cell
    cellCoordinates: {apmID: "", instanceName: ""}, 
    query: {
        filterBy: ["apm ID"], // or a single apm ID string
        ancestorProcessesActive: {}
    } 
}}}
```

<div id="ancestor-processes-all-in-step">
    <strong>Ancestor Processes all in step</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)
</div>

```javascript
const actionRequest = {CellProcessor: {cell: {
    // apmBindingPath, cellProcessID or relative path
    // default: "#", current cell
    cellCoordinates: {apmID: "", instanceName: ""}, 
    query: {
        filterBy: ["apm ID"], // or a single apm ID string
        ancestorProcessesAllInStep: {
            apmStep: ["step name"], // or a single step name string
            omitCellProcessor: true // default true, exclude the CPM process step
        }
    } 
}}}
```

<div id="ancestor-processes-any-in-step">
    <strong>Ancestor Processes any in step</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)
</div>

```javascript
const actionRequest = {CellProcessor: {cell: {
    // apmBindingPath, cellProcessID or relative path
    // default: "#", current cell
    cellCoordinates: {apmID: "", instanceName: ""}, 
    query: {
        filterBy: ["apm ID"], // or a single apm ID string
        ancestorProcessesAnyInStep: {
            apmStep: ["step name"], // or a single step name string
            omitCellProcessor: true // default true, exclude the CPM process step
        }
    } 
}}}
```

**Same pattern can also be applied to other relationship (ancestor, parent, child and descendant)**
* Active:
    * ancestorProcessesActive
    * parentProcessActive
    * childProcessesActive
    * descendantProcessesActive
* All in step:
    * ancestorProcessesAllInStep
    * parentProcessInStep (single process, no CPM omit option)
    * childProcessesAllInStep
    * descendantProcessesAllInStep
* Any in Step: 
    * ancestorProcessesAnyInStep
    * childProcessesAnyInStep
    * descendantProcessesAnyInStep

<hr>



## Cell Process Proxy (CPP)

<div id="proxy-status-is">
    <strong>proxy status is</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)
</div>

```javascript
const actionRequest = {CellProcessor: {proxy: {
    proxyCoordinates: "#.",
    connect: {
        statusIs: "connected" //or disconnected or broken
    }
}}}
```
<hr>
<br>

## Observable Value Family
<div id="ov-value-is-active">
    <strong>OV is active (exist at path)</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)
</div>

```javascript
const actionRequest = {holarchy: {common: {operators: {ObservableValue: {valueIsActive: {
    path: "#.", // relative path of OV inside the provider Cell Model
}}}}}}
```
<br>

<div id="ov-value-is-available">
    <strong>OV is available (a value has been written)</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)
</div>

```javascript
const actionRequest = {holarchy: {common: {operators: {ObservableValue: {valueIsAvailable: {
    path: "#.", // relative path of OV inside the provider Cell Model
}}}}}}
```
<br>

<div id="ov-value-has-updated">
    <strong>OV has a newer value than provided revision</strong>
    (<a href="#TOPs-list">back to TOPs List</a>)
</div>

```javascript
const actionRequest = {holarchy: {common: {operators: {ObservableValue: {valueHasUpdated: {
    path: "#.", // relative path of OV inside the provider Cell Model
    lastReadRevision: -1 // version number to compare whether the TOP caller has a different version from the ov at path
}}}}}}
```
<br>
<hr>
<br>