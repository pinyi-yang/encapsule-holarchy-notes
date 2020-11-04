# Encapsule Holarchy Package (0.0.47 Alexandrite)
* [Observable Controller Data (OCD)](./ocd.md)
* [Cell Model (CM)](./cell-model.md)
* [Cell Processor (CP)](./cell-processor.md)
* [Observable Process Controller (OPC)](./opc.md)

## Built-in Libraries
* [Controller Actions (ACT)](#Controller-Actions-ACT)
* [Transition Operators (TOP)](#Transition-Operators-TOP)

### Controller Actions (ACT)
* [Holarchy Core](#Holarchy-Core)
* [Cell Process Manager (CPM)](#Cell-Process-Manager-CPM)
* [Cell Process Proxy (CPP)](#Cell-Process-Proxy-CPP)

#### Holarchy Core

<div><strong>clear boolean flag at path</strong></div>

```javascript
const actionRequest = {holarchy: {cm: {actions: {ocd: {clearBooleanFlag: {path: "~. or #."}}}}}}
```
<div><strong>set boolean flag att path</strong></div>

```javascript
const actionRequest = {holarchy: {cm: {actions: {ocd: {setBooleanFlag: {path: "~. or #."}}}}}}
```

<div><strong>read name space indrectly</strong></div>

```javascript
const actionRequest = {holarchy: {cm: {actions: {ocd: {readNamespaceIndirect: {path2: "~. or #."}}}}}}
```

<div><strong>write sub action response</strong></div>

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
<div><strong>create a new owned process</strong></div>

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

<div><strong>delete a owned process</strong> (and the process tree under it)</div>

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

<div><strong>delegate an action request</strong></div>

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

<div><strong>query a cell</strong> get subsystem cells</div>

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
<div><strong>connect a proxy to a shared process</strong></div>

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

<div><strong>disconnect a proxy to a shared process.</strong> (If the shared process is not connect by other proxy, it will be deleted)</div>

```javascript
const actionRequest = {CellProcessor: {proxy: {
    proxyCoordinates: "#...",
    disconnect: {}
}}};

const actionResult = {} //TBD
```
<br>

### Transition Operators (TOP)
* [Holarchy Core](#Holarchy-Core-1)
* [Cell Process Manager (CPM)](#Cell-Process-Manager-CPM-1)
* [Cell Process Proxy (CPP)](#Cell-Process-Proxy-CPP-1)
<br>

#### Holarchy Core

<div><strong>array is empty</strong></div>

```javascript
const actionRequest = {holarchy: {cm: {operators: {ocd: {arrayIsEmpty: {path: "~. or #."}}}}}}
```

<div><strong>array length equal to</strong></div>

```javascript
const actionRequest = {holarchy: {cm: {operators: {ocd: {array: {
    path: "~. or #."
    legnth: {equalToValue: ""} //ocd path or a number
}}}}}}
```

<div>
    <strong>compare values</strong><br>
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


<div><strong>is namespace truthy</strong></div>

```javascript
const actionRequest = {holarchy: {cm: {operators: {ocd: {isNamespaceTruthy: {path: "#. or ~."}}}}}}
```

<div><strong>namespace is map keyless</strong></div>

```javascript
const actionRequest = {holarchy: {cm: {operators: {ocd: {mapIsKeyless: {path: "#. or ~."}}}}}}
```
<hr>



#### Cell Process Manager (CPM)
<div><strong>Cell is at step</strong></div>

```javascript
const actionRequest = {CellProcessor: {cell: {
    cellCoordinates: {apmID: "", instanceName: ""}, // apmBindingPath, cellProcessID or relative path
    query: {inStep: {
        apmStep: ["step name"] // or a string of step
    }} 
}}}
```

<div><strong>Operator delegation</strong></div>

```javascript
const actionRequest = {CellProcessor: {cell: {
    cellCoordinates: {apmID: "", instanceName: ""}, // apmBindingPath, cellProcessID or relative path
    query: {inStep: {
        apmStep: ["step name"] // or a string of step
    }} 
}}}
```

<div><strong>Ancestor Processes Active</strong></div>

```javascript
const actionRequest = {CellProcessor: {cell: {
    cellCoordinates: {apmID: "", instanceName: ""}, // apmBindingPath, cellProcessID or relative path
    query: {
        filterBy: ["apm ID"], // or a single apm ID string
        ancestorProcessesActive: {}
    } 
}}}
```

<div><strong>Ancestor Processes all in step</strong></div>

```javascript
const actionRequest = {CellProcessor: {cell: {
    cellCoordinates: {apmID: "", instanceName: ""}, // apmBindingPath, cellProcessID or relative path
    query: {
        filterBy: ["apm ID"], // or a single apm ID string
        ancestorProcessesAllInStep: {
            apmStep: ["step name"], // or a single step name string
            omitCellProcessor: true // default true, exclude the CPM process step
        }
    } 
}}}
```

<div><strong>Ancestor Processes any in step</strong></div>

```javascript
const actionRequest = {CellProcessor: {cell: {
    cellCoordinates: {apmID: "", instanceName: ""}, // apmBindingPath, cellProcessID or relative path
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
<div><strong>proxy status is</strong></div>

```javascript
const actionRequest = {CellProcessor: {proxy: {
    proxyCoordinates: "#.",
    connect: {
        statusIs: "connected" //or disconnected or broken
    }
}}}
```

<hr>