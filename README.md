# Encapsule Holarchy Package (0.0.47 Alexandrite)
* [Observable Controller Data (OCD)](./ocd.md)
* [Cell Model (CM)](./cell-model.md)
* [Cell Processor (CP)](./cell-processor.md)
* [Observable Process Controller (OPC)](./opc.md)

## Built-in Libraries
* [Controller Actions (ACT)](#Controller-Actions-ACT)
* [Transition Operators (TOP)](#Transition-Operators-TOP)

<div id="ACTs-list"></div>

### Controller Actions (ACT)
[Back to Library List](#Built-in-Libraries)

* [Holarchy Core](#Holarchy-Core)
    * [clear boolean flag at path](#clear-boolean-flag-at-path)
    * [set boolean flag at path](#set-boolean-flag-at-path)
    * [read name space indrectly](#read-name-space-indrectly)
    * [write sub action response](#write-sub-action-response)

* [Cell Process Manager (CPM)](#Cell-Process-Manager-CPM)
    * [create a new owned process](#create-a-new-owned-process)
    * [delete a owned process](#delete-a-owned-process)
    * [delegate an action request](#delegate-an-action-request)
    * [query a cell get subsystem cells](#query-a-cell)

* [Cell Process Proxy (CPP)](#Cell-Process-Proxy-CPP)
    * [connect a proxy to a shared process](#connect-a-proxy-to-a-shared-process)
    * [disconnect a proxy to a shared process](#disconnect-a-proxy-to-a-shared-process)

#### Holarchy Core

<div id="clear-boolean-flag-at-path">
    <strong>clear boolean flag at path</strong>
    (<a href="#ACTs-list">back to ACTs List</a>)
</div>

```javascript
const actionRequest = {holarchy: {cm: {actions: {ocd: {clearBooleanFlag: {path: "~. or #."}}}}}}
```
<div id="set-boolean-flag-at-path">
    <strong>set boolean flag at path</strong>
    (<a href="#ACTs-list">back to ACTs List</a>)
</div> 

```javascript
const actionRequest = {holarchy: {cm: {actions: {ocd: {setBooleanFlag: {path: "~. or #."}}}}}}
```

<div id="read-name-space-indrectly">
    <strong>read name space indrectly</strong>
    (<a href="#ACTs-list">back to ACTs List</a>)
</div>

```javascript
const actionRequest = {holarchy: {cm: {actions: {ocd: {readNamespaceIndirect: {path2: "~. or #."}}}}}}
```

<div id="write-sub-action-response">
    <strong>write sub action response</strong>
    (<a href="#ACTs-list">back to ACTs List</a>)
</div>

```javascript
const actionRequest = {holarchy: {core: {writeSubactionResponse: {
    subactionRequest: {}, //action request
    writeResponsePath: "~. or #."
}}}}

const actionResult: {
    error: "",
    result: {} //opaque
}
```
<hr>

#### Cell Process Manager (CPM)
<div id="create-a-new-owned-process">
    <strong>create a new owned process</strong>
    (<a href="#ACTs-list">back to ACTs List</a>)
</div>

```javascript
const actionRequest = {CellProcessor: {process: {
    processCoordinates: {
        // string of cell process path or cell process ID
        //or 
        apmID: "irut",
        instanceName: "",
    },
    activate: {
        processData: {
            //init data for apm
        }
    }
}}};

const actionResult = {
    apmBindingPath: "~....",
    cellProcessID: "irut"
}
```

<div id="delete-a-owned-process">
    <strong>delete a owned process</strong>
    (<a href="#ACTs-list">back to ACTs List</a>)</br>
    (and the process tree under it)
</div>

```javascript
const actionRequest = {CellProcessor: {process: {
    processCoordinates: {
        // string of cell process path or cell process ID
        //or 
        apmID: "irut",
        instanceName: "",
    },
    deactivate: {}
}}};

const actionResult = {
    apmBindingPath: "~....",
    cellProcessID: "irut"
}
```

<div id="delegate-an-action-request">
    <strong>delegate an action request</strong>
    (<a href="#ACTs-list">back to ACTs List</a>)
</div>

```javascript
const actionRequest = {CellProcessor: {cell: {
    cellCoordinates: {
        // string of cell process path or cell process ID
        //or 
        apmID: "irut",
        instanceName: "",
    },
    delegate: {
        actionRequest: {} //sub aciton request
    }
}}};

const actionResult = {
    //opaque
}
```

<div id="query-a-cell">
    <strong>query a cell</strong> get subsystem cells
    (<a href="#ACTs-list">back to ACTs List</a>)
</div>

```javascript
const actionRequest = {CellProcessor: {cell: {
    cellCoordinates: {
        // string of cell process path or cell process ID
        //or 
        apmID: "irut",
        instanceName: "",
    },
    query: {
        filterBy: {apmIDs: [""]},
        // resultSets undefined give all sub namespace true.
        resultSets: {parent: false, ancestors: false, children: false, descendants: false}
    }
}}};

const actionResult = {
    query: {
        cellProcessID: "",
        apmBindingPath: "",
        apmID: "",
        resultSets: {...}
    },
    parent: {cellProcessID: "", apmBindingPath: "", apmID: ""},
    ancestors: [{cellProcessID: "", apmBindingPath: "", apmID: ""}],
    children: [{cellProcessID: "", apmBindingPath: "", apmID: ""}],
    descendants: [{cellProcessID: "", apmBindingPath: "", apmID: ""}],
}
```

<hr>

#### Cell Process Proxy (CPP)
<div id="connect-a-proxy-to-a-shared-process">
    <strong>connect a proxy to a shared process</strong>
    (<a href="#ACTs-list">back to ACTs List</a>)
</div>

```javascript
const actionRequest = {CellProcessor: {proxy: {
    proxyCoordinates: "#...",
    connect: {
        processCoordinates: {
            // string of cell process path or cell process ID
            //or 
            apmID: "irut",
            instanceName: "",
        }
    }
}}};

const actionResult = {} //TBD
```

<div id="disconnect-a-proxy-to-a-shared-process">
    <strong>disconnect a proxy to a shared process.</strong> 
    (<a href="#ACTs-list">back to ACTs List</a>)</br>
    (If the shared process is not connect by other proxy, it will be deleted)
</div>

```javascript
const actionRequest = {CellProcessor: {proxy: {
    proxyCoordinates: "#...",
    disconnect: {}
}}};

const actionResult = {} //TBD
```
<br>

<div id="TOPs-list"></div>

### Transition Operators (TOP)
[Back to Library List](#Built-in-Libraries)

* [Holarchy Core](#Holarchy-Core-1)
    * [array is empty](#array-is-empty)
    * [array length equal to](#array-length-equal-to)
    * [compare values](#compare-values)
    * [is namespace truthy](#is-namespace-truthy)
    * [namespace is map keyless](#namespace-is-map-keyless)

* [Cell Process Manager (CPM)](#Cell-Process-Manager-CPM-1)
    * [Cell is at step](#cell-is-at-step)
    * [Operator delegation](#operator-delegation)
    * [Ancestor (parent, child, descendant) Processes Active](#ancestor-processes-active)
    * [Ancestor (parent, child, descendant) Processes all in step](#ancestor-processes-all-in-step)
    * [Ancestor (child, descendant) Processes any in step](#ancestor-processes-any-in-step)

* [Cell Process Proxy (CPP)](#Cell-Process-Proxy-CPP-1)
    * [proxy status is](#proxy-status-is)

<br>

#### Holarchy Core

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



#### Cell Process Manager (CPM)

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



#### Cell Process Proxy (CPP)

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