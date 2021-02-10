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
* [Internal Notes](#internal-notes)

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

    ocdInitData: {...}, // default to {}

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

## .act(request_)
The act method allows you to perform any valid registered ACTs in the *controllerActionSets*. In the request_,
```javascript
const opcInstance.act({
    actorName: "name_of_the_actor",
    actorTaskDescription: "purpose of this action",
    actionRequest, // object matches one ACT in the controllerActionSets,
    apmBindingPath, // dot path of the actor, default to relative path #
})
```

# Internal Notes
The OPC construction will create the below object and store it in the _private:

```javascript
```

inherit from construction request: *id, name, description, options* and *ocdTemplateSpec*.

* *iid*: runtime random generate irut
* *apmMap*: apm map from *abstractProcessModelSets* ([Construction - Step 2](#2.-Register-APM-instances))
* *ocdRuntimeBaseSpec*: synthesized ocd spec from *ocdTemplateSpec*, built-in base spec and *apmMap* ([Construction - Step 3](#3.-process-cell-runtime-plane-memory-specification))
* *opmiSpecPaths*: descriptor for embedded APMs ([Construction - Step 3](#3.-process-cell-runtime-plane-memory-specification))
* *ocdi*: OCD instance contruct from *ocdRuntimeBaseSpec* and *ocdInitData* in construction request ([Construction - Step 4](#4.-configure-cell-runtime-plane)).
* *actionDispatcherFilterMap*: a map with all built-in ACTs and ACTs in *controllerActionSets* with operation ID as key ([Construction - Step 4](#4.-configure-cell-runtime-plane)).
* *actionDispatcher*: an arccore discriminator with all ACTs in *actionDispatcherFilterMap* ([Construction - Step 4](#4.-configure-cell-runtime-plane)).
* *transitionDispatcherFilterMap*: ([Construction - Step 4](#4.-configure-cell-runtime-plane)).
* *transitionDispatcher*: ([Construction - Step 4](#4.-configure-cell-runtime-plane)).

**ACT related namespaces**
* *opcActorStack*: array of actor descriptors ([APIs](#apis))


## Construction
During the construction:

### 1. Process definition
*id, iid, name, description* and *options* inherit from the construction request or generate at runtime.

### 2. Register APM instances
*apmMap*

OPC will check every APM instances in the *abstractProcessModelSets* in construction request, and then store it in the *apmMap* with apm ID as key and apm instance as value

### 3. Process cell runtime plane memory specification
*ocdTemplateSpec* and create *ocdRuntimeBaseSpec*
1. verify *ocdTemplateSpec* with arccore filter
2. sync built-in ocd runtime base spec (a basic spec to allow opc to run) with *ocdTemplateSpec* to create *ocdRuntimeBaseSpec*
3. sync all the *____appdsl.apm* binding with the apm in the *apmMap* and keep the embedded apm spec path in *opmiSpecPaths*

### 4. Configure cell runtime plane
*ocdInitData*, *ocdRuntimeBaseSpec* from step 3

1. Memory Controller 
    * create OCD instance with *ocdRuntimeBaseSpec* and *ocdInitData*. 
    * Store instance in *ocdi*
2. Action Request Bus
    * add built-in ACTs and ACTs in *controllerActionSets* into *actionDispatcherFilterMap* with *operationID* as key and *action filter* as value
    * create a arccore discriminator with all the *action filters* and store in *actionDispatcher*
3. Operator Request Bus
    * add built-in TOPs and TOPs in *transitionOperatorSets* into *transitionDispatcherFilterMap* with *operationID* as key and *filter* as value
    * create a arccore discriminator with all the *filters* and store in *transitionDispatcher*

## APIs
### .act(request_)
The .act(request_) is the method allow any actor to perform any registered ACT in the *actionDispatcherFilterMap* by using 
*actionDispatcher*. It performs ACTs recursively (you can perform ACTs in other ACT)

When .act(request_) is called:
1. *controllerActionRequest* will be created from request_:
```javascript
var controllerActionRequest = {
    context: {
    apmBindingPath: request_.apmBindingPath,
    ocdi: this._private.ocdi,
    act: this.act
    },
    actionRequest: request_.actionRequest
};
```

2. *opcActorDescriptor* will be pushed to *opcActorStack*
```javascript
const opcActorDescriptor = {
    actorName: request_.actorName,
    actorTaskDescription: request_.actorTaskDescription
}
```

3. pass *controllerActionRequest* into *actionDispatcher* to get ACT filter and perform ACT.

4. evaluate opc itself with private method ._evaluate()

5. return ACT result in 3 and evaluate result in 4
```javascript
return {result: {
    actionResult: actionResponse.result,
    lastEvaluation: evaluateResponse.result
}}
```

### ._evaluate()
It is a private method that evaluate all APM in the opc to see whether any transitions or further ACTs need to perform after an ACT.

1. push evaluate actor descriptor to *opcActorStack*
2. perform evaluation with evaluation filter
    * get ocd spec for opc
    * read ocd data
    * get all ocd namespace binding to apm
    * get transitions and check transition
    * if condition meets, transit
    * if ACT exist in transtion, repeat .act(request_)

3. update *evalCount* and *lastEvalResponse*