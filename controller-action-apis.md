<div id="ACTs-list"></div>

# Controller Actions (ACT)
[<- Back to Holarchy](./README.md)


* **[Holarchy Core](#Holarchy-Core)**
    * [clear boolean flag at path](#clear-boolean-flag-at-path)
    * [set boolean flag at path](#set-boolean-flag-at-path)
    * [read name space indrectly](#read-name-space-indrectly)
    * [write sub action response](#write-sub-action-response)

* **[Cell Process Manager (CPM)](#Cell-Process-Manager-CPM)**
    * [create a new owned process](#create-a-new-owned-process)
    * [delete a owned process](#delete-a-owned-process)
    * [delegate an action request](#delegate-an-action-request)
    * [query a cell get subsystem cells](#query-a-cell)

* **[Cell Process Proxy (CPP)](#Cell-Process-Proxy-CPP)**
    * [connect a proxy to a shared process](#connect-a-proxy-to-a-shared-process)
    * [disconnect a proxy to a shared process](#disconnect-a-proxy-to-a-shared-process)

* [Observable Value Family (OV)](#Observable-Value-Family)
    * [read value from an OV](#ov-read-value)
    * [write value to an OV](#ov-write-value)
    * [reset an OV](#ov-reset-value)

* **Value Value Helper**
    `todo add Controller Actions after release`

## Holarchy Core

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

## Cell Process Manager (CPM)
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
        // string of cell process path, proxy path or cell process ID
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

## Cell Process Proxy (CPP)
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
<hr>
<br>

## Observable Value Family
<div id="ov-read-value">
    <strong>read a value from OV at given path</strong>
    (<a href="#ACTs-list">back to ACTs List</a>)
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

<div id="ov-write-value">
    <strong>write a value from OV at given path</strong>
    (<a href="#ACTs-list">back to ACTs List</a>)
</div>

```javascript
const actionRequest = {holarchy: {common: {actions: {ObservableValue: {writeValue: {
    path: "#.", // relative path of OV inside the provider Cell Model
    value, // opaque, match the valueTypeSpec

}}}}}}
```
<br>

<div id="ov-rest-value">
    <strong>reset OV at given path</strong>
    (<a href="#ACTs-list">back to ACTs List</a>)
</div>

```javascript
const actionRequest = {holarchy: {common: {actions: {ObservableValue: {resetValue: {
    path: "#.", // relative path of OV inside the provider Cell Model
}}}}}}
```
<br>

<hr>
<br>