# Encapsule Holarchy Package (0.0.47 Alexandrite)
* [Observable Controller Data (OCD)](./ocd.md)
* [Cell Model (CM)](./cell-model.md)
* [Cell Processor (CP)](./cell-processor.md)
* [Observable Process Controller (OPC)](./opc.md)

## Built-in Libraries
* [Controller Actions (ACT)](#Controller-Actions-(ACT))
* [Transition Operators (TOP)](#Transition-Operators-(TOP))

### Controller Actions (ACT)
* [Holarchy Core](#Holarchy-Core)
* [Cell Process Manager (CPM)](#Cell-Process-Manager-(CPM))
* [Cell Process Proxy](#Cell-Process-Proxy)

#### Holarchy Core

<strong>
**test test**
</strong>

```javascript
const actionRequest = {holarchy: {cm: {actions: {ocd: {clearBooleanFlag: {path: "~. or #."}}}}}}
```

<table>
    <tr>
        <th width=300>action</th>
        <th>action request</th>
    </tr>
    <tr>
        <td>clear a boolean falg at path</td>
        <td>

```javascript
const actionRequest = {holarchy: {cm: {actions: {ocd: {clearBooleanFlag: {path: "~. or #."}}}}}}
```
</td>
    </tr>
    <tr>
        <td>set a boolean falg at path</td>
        <td>

```javascript
const actionRequest = {holarchy: {cm: {actions: {ocd: {setBooleanFlag: {path: "~. or #."}}}}}}
```
</td>
    </tr>
    <tr>
        <td>read name space indrectly</td>
        <td>

```javascript
const actionRequest = {holarchy: {cm: {actions: {ocd: {readNamespaceIndirect: {path2: "~. or #."}}}}}}
```
</td>
    </tr>
    <tr>
        <td>write sub action response</td>
        <td>

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
</td>
    </tr>
</table>


#### Cell Process Manager (CPM)
<table>
    <tr>
        <th width= 300>action</th>
        <th>action request</th>
    </tr>
    <tr>
        <td>create a new owned process</td>
            <td>

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
</td>
    </tr>
    <tr>
        <td>delete a owned process</br> 
            (and the process tree under it)
        </td>
        <td>

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

</td>
    </tr>
</table>

#### Cell Process Proxy
<table>
    <tr>
        <th width= 300>action</th>
        <th>action request</th>
    </tr>
    <tr>
        <td>connect a proxy to a shared process</td>
        <td>

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
</td>
    </tr>
    <tr>
        <td>disconnect a proxy to a shared process.</br> 
            If the shared process is not connect by other proxy, it will be deleted
        </td>
        <td>

```javascript
const actionRequest = {CellProcessor: {proxy: {
    proxyCoordinates: "#...",
    connect: {
        disconnect: {}
}}};

const actionResult = {} //TBD
```
</td>
    </tr>
</table>

### Transition Operators (TOP)
* [Holarchy Core](#Holarchy-Core-1)
* [Cell Process Manager (CPM)](#Cell-Process-Manager-(CPM)-1)
* [Cell Process Proxy](#Cell-Process-Proxy-1)

#### Holarchy Core
<table>
    <tr>
        <th width= 200>Transitions</th>
        <th>Transition Operator</th>
    </tr>
    <tr>
        <td>array is empty</td>
        <td>

```javascript
const actionRequest = {holarchy: {cm: {operators: {ocd: {arrayIsEmpty: {
    path: "~. or #."
}}}}}}
```
</td>
    </tr>
    <tr>
        <td>array length equal to</td>
        <td>

```javascript
const actionRequest = {holarchy: {cm: {operators: {ocd: {array: {
    path: "~. or #."
    legnth: {equalToValue: ""} //ocd path or a number
}}}}}}
```
</td>
    </tr>
    <tr>
        <td>compare values</td>
        <td>

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
</td>
    </tr>
    <tr>
        <td></td>
        <td>There are greater than, identical to and less than transitor operators in the holarchy core as well.
        </td>
    </tr>
    <tr>
        <td>is namespace truthy</td>
        <td>

```javascript
const actionRequest = {holarchy: {cm: {operators: {ocd: {isNamespaceTruthy: {
    path: "#. or ~."
}}}}}}
```
</td>
    </tr>
    <tr>
        <td>namespace is map keyless</td>
        <td>

```javascript
const actionRequest = {holarchy: {cm: {operators: {ocd: {mapIsKeyless: {
    path: "#. or ~."
}}}}}}
```
</td>
    </tr>
</table>


#### Cell Process Manager (CPM)

#### Cell Process Proxy