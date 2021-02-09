# Observable Process Controller
[<- back to Holarchy](../README.md)

<!-- reference -->
[arccore filter]: https://encapsule.io/docs/ARCcore/filter
[ocd]: ./observable-controller-data.md
[opc]: ./observable-process-controller.md
[apm]: ./abstract-process-model.md
[top]: ./transition-operator.md
[act]: ./controller-action.md
[cp]: ./cell-procssor.md
[cm]: ./cell-model.md

* [Construction](#Construction)
* [OPC ES6 Class APIs](#OPC-ES6-Class-APIs)

Observable Process Controller (OPC) is an ES6 class instantiated with operator new that provides memory management, and generic evaluation of cellular process(es) (cellular automata). Developers do **NOT** typically use the OPC class directly but instead rely on [CellProcessor (CP)][cp] to create and manage an OPC runtime instance on their behalf based on information derived from a [CellModel (CM)][cm].

# Construction
**OPC construction is generally done by [CP][cp]**

```javascript
const holarchy = require("@encapsule/holarchy");

const opcInstance = new holarchy.ObservableProcessController({
    id: "22_char_irut",
    name: "your_opc_system_name", // optional
    description: "your_opc_system_description", // optional

    // OCD runtime spec for the opc system
    ocdTemplateSpec: {...}, // default to { ___types: "jsObject"}

    // array of arrays of unique Abstract Process Model class instances
    abstractProcessModelSets: [
        [{...}, {...}, ...],
        [{...}, {...}, ...],
        ...
    ],

    // array of arrays of unique Controller Action class instances
    controllerActionSets: [
        [{...}, {...}, ...],
        [{...}, {...}, ...],
        ...
    ],

    // array of arrays of unique Transition Operator class instances
    transitionOperatorSets: [
        [{...}, {...}, ...],
        [{...}, {...}, ...],
        ...
    ],

    options: {
        evaluate: {
            // max number of frames allowed per system evaluation
            maxFrames: 64, // 0~256
            // when to do first evaluation
            firstEvaluation: "construction", // or action
        }
    }
});
```

[Cell Processor][cp] constructs an OPC instance with all the [Abstract Process Model (APM)][apm], [Transition Operator (TOP)][top] and [Controller Action (ACT)][act] from its input [Cell Model (CM)][cm].


# OPC ES6 Class APIs

| Method | Description |
|-|-|
| .isValid() | check whether the current OPC instance is valid or not. Return true or false |
| .toJSON() | Convert the OPC isntance into an JSON object | 
| .act(request_) | find [ACT][act] that matches request_ and perform the ACT |