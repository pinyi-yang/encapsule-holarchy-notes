# Controller Action
[<- back to Holarchy](../README.md)

<!-- reference -->
[arccore filter]: https://encapsule.io/docs/ARCcore/filter
[arccore identifier]: https://encapsule.io/docs/ARCcore/identifier
[ocd]: ./observable-controller-data.md
[opc]: ./observable-process-controller.md
[apm]: ./abstract-process-model.md
[top]: ./transition-operator.md
[top list]: ../transition-operator-apis.md
[act]: ./controller-action.md
[act list]: ../controller-action-apis.md
[cp]: ./cell-procssor.md
[cm]: ./cell-model.md

Controller Action (ACT) is an ES6 class instantiated with operator new that implements a plug-in data transformation function used to evaluate cell process step exit and step enter actions.

A valid registered ACT in any [Cell Process][cp] or [Observable Process Controller][opc] can invoke through thier .act(request_) method.

To be a valid ACT:
* construct with [arccore filter][arccore filter] syntax.
* unique in the host [CP][cp] or [OPC][opc] (unique *id* and *actionRequestSpec*)

* [Construction](#Construction)
* [ACT ES6 Class APIs](#ACT-ES6-Class-APIs)

## Construction
To construct a ACT instance:
```javascript
const holarchy = require("@encapsule/holarchy");

const actInstance = new holarchy.ControllerAction({
    id: "unique_irut",
    name: "your_act_name",
    description: "function_description",

    actionRequestSpec: {...}, // developer-defined filter spec
    actionResultSpec: {...}, // developer-defined filter spec, default to undefined

    bodyFunction: function({context, actionRequest}){} // developer-defined function
});
```
* *id*: a unique irut. You can use [arccore identifier][arccore identifier] to generate one.
* *name* and *description*: descriptive content for this ACT
* *actionRequestSpec*: The request message signature of ACT defined by developer following [arccore filter][arccore filter] syntax. If a request passed into the .act(request) of this ACT's host [CP][cp] or [OPC][opc] matches this signature, this ACT will be dispatched by its host.
* *actionResultSpec*: If ACT return a response.result, it need be declared in the *actionResultSpec*
* *bodyFunction*: a function implements the developer-defined controller action's runtime semantics. It owns an object as its arguement, which contains
    * *actionRequest*: the runtime request matches the *actionRequestSpec* signature may contain information for the action.
    * *context*:  [OPC][opc] context descriptor (hostOPCDescriptor) which dispatches the ACT
```javascript
//bodyFunction argument.context
const context = { //hostOPCDescriptor
    apmBindingPath: "~.dot.delimited.path"
    ocdi, // ocd instance in the host OPC
    act, // function, host OPC .act(request_) method
}

```

### Example
Create an ACT simply console.log the input content and return success after done.
```javascript
const holarchy = require("@encapsule/holarchy");

const actInstance = new holarchy.ControllerAction({
    id: "unique_irut",
    name: "Holarchy Doc Console Log",
    description: "demo ACT perform console.log on input content",

    actionRequestSpec: {
        ____types: "jsObject",
        holarchyDoc: {
            ____types: "jsObject",
            consoleLog: {
                ____types: "jsObject",
                content: {
                    ____accept: "jsString
                }
            }
        }
    },
    actionResultSpec: {
        ____accept: "jsString",
        ____inValueSet: "success"
    },

    bodyFunction: function({context, actionRequest}){
        const { content } = actionRequest.holarchyDoc.consoleLog;

        console.log(content);

        // if you need to perform another registered ACT
        // const actResponse = context.act({
        //     actorName: "holarchy doc - console log ACT",
        //     actorDescription: "....",
        //     actionRequest: {...}, // action request for the ACT 
        //     apmBindingPath: context.apmBindingPath
        // });
        // if (actResponse.error) return actResponse;

        return { result: "success" }
    }
});
```

If the above ACT is successfully registered in an [OPC][opc] or [CP][cp] (*actHostInstance*), it can be dispatched by:
```javascript
const actResponse = actHostInstance.act({
    actorName: "name of actor",
    actorTaskDescription: "console.log some content",
    actionRequest: {holarchyDoc: {consoleLog: {content: "Hello World"}}},
    //apmBindingPath, optional
});
if (actResponse.error) throw new Error(actResponse.error);

console.log(actResponse.result) // this will console log success
```


## ACT ES6 Class APIs
| Method | Description |
|-|-|
| .isValid() | check whether the current ACT instance is valid or not. Return true or false |
| .toJSON() | Convert the ACT isntance into an JSON object | 
| .getID() | get ACT ID for this ACT |
| .getVDID() |  |
| .getFilter() | get action filter (an [arrcore filter][arccore filter]) of this ACT |
| .getName() | get ACT *name* for this ACT |
| .getDescription() | get ACT *description* for this ACT |