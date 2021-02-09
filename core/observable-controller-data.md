# Observable Controller Data
[<- back to Holarchy](../README.md)

<!-- reference -->
[opc]: ./observable-process-controller.md
[cp]: ./cell-procssor.md
[arccore filter]: https://encapsule.io/docs/ARCcore/filter
[cm]: ./cell-model.md

* [Construction](#Construction)
* [OCD ES6 Class APIs](#OCD-ES6-Class-APIs)
* [Internal Notes](#Internal-Notes)

Observable Controller Data (OCD) is an ES6 class instantiated withoperator new that encapsulates a shared in-memory store data.

OCD provides absolute and relative *dot-delimited path* addressing, type introspection, and fully abstracted I/O operations via its readNamespace and writeNamespace class API's that enforce name/type/value constraints, and provide data normalization (cascading default values) for all read and write operations via [@encapsule/arccore.filter][arccore filter]. This ensures that data written into and read from an OCD instance is correct at runtime, always. [Observable Process Controller (OPC)][opc] uses OCD internally to to manage celluar process(es) runtime data on behalf of a [CellProcessor (CP)][cp] instance. But, you can use it wherever you require strong runtime data fidelity, and addressable read/write facility. e.g. if you're preparing a network request you can use OCD to build the request and then call toJSON.



## Construction
To construct a OCD instance:
```javascript
const holarchy = require("@encapsule/holarchy");

const ocdInstance = new holarchy.ObservableControllerData({
    spec: your_data_spec,
    data: your_data
});
```

**spec:** Data type and other limitations (value sets, default value...) of the stored the data defined by the [arccore filter][arccore filter] syntax.

**data:** The initialization data matches the spec. It will cause error if data and spec don't match.

## OCD ES6 Class APIs

| Method | Description |
|-|-|
| .isValid() | check whether the current OCD instance is valid or not. Return true or false |
| .toJSON() | Convert the OCD isntance into an JSON object | 
| .readNamespace(path_) | read the data from the given path_. Return {result: data} |
| .writeNamespace(path_, data_) | write the given data to given path_. data_ spec must match the spec at the path_. return readNamespace result at given path_ after writing |
| .getNamespaceSpec(path_) | return the spec declaration for the given path_, {result: spec } |
| .dataPathResolve({apmBindingPath, dataPath}) | return the OCD data path from a relative path (dataPath begin with #) |

# Internal Notes

## Construction
During the OCD construction, the spec is used to create an [arccore filter][arccore filter] to validate the data. Then, below is stored in the OCD ES6 class _private:

```javascript
ocdInstance._private = {
    storeData: data,
    storeDataSpec: spec,
    accessFilters: {
        read: {}, //TBD
        write: {}   //TBD
    }
}
```

