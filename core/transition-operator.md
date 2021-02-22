# Transition Operator
[<- back to Holarchy](../README.md)

<!-- reference -->
<!-- external references -->
[arccore filter]: https://encapsule.io/docs/ARCcore/filter
[arccore identifier]: https://encapsule.io/docs/ARCcore/identifier
<!-- core references -->
[ocd]: ../core/observable-controller-data.md
[opc]: ../core/observable-process-controller.md
[apm]: ../core/abstract-process-model.md
[top]: ../core/transition-operator.md
[act]: ../core/controller-action.md
[cp]: ../core/cell-procssor.md
[cm]: ../core/cell-model.md
[cmas]: ../core/cell-model-artifact-space.md
[cmt]: ../core/cell-model-template.md
<!-- holarchy cm: build-in-cell-model reference -->
[ovh]: ../build-in-cell-model/observable-value-helper
[ov]: ../build-in-cell-model/observable-value-family.md
<!-- root reference -->
[top list]: ../transition-operator-apis.md
[act list]: ../controller-action-apis.md

* [Introduction](#Introduction)
* [TOP ES6 Class APIs](#TOP-ES6-Class-APIs)

# Introduction

Transition Operator (TOP) is an ES6 class instantiated with operator new that implements a plug-in Boolean operator function used to evaluate cell process transition rules at runtime.

Transition Operator is used in the [Abstract Process Model (APM)][apm] to decide transition condition(s) between the *steps* of [APM][apm]. Holarchy has a built-in TOPs for most cases which can be found at [TOPs List][top list] of this document. Holarchy also allows developers to build their own TOPs that can be applied to [APM][apm] after it is registered in the [Cell Model (CM)][cm].
* [Build a TOP](#build-a-top)
* [Register a TOP](#register-a-top)
* [Use a TOP](#use-a-top)

## Build a TOP
```javascript
const holarchy = require("@encapsule/holarchy");

const constructRequest = {
    id: "a_unique_irut",
    name: "top_name",
    description: "the functionality of this top",
    operatorRequestSpec: { // a unique spec
        // unique spec of your choice
        
        // example
        ____types: "jsObject",
        topDoc: {
            ____types: "jsObject",
            alwaysTransit: { ____accept: "jsObject" }
        }
        // end of exmple
    },
    bodyFunction: function({context, operatorRequest}) {
        return {result: true}
    }
};

const topInstance = new holarchy.TransitionOperator(constructRequest);
if (!topInstance.isValid()) throw new Error(topInstance.toJSON());
module.exports = topInstance;
```

## Register a TOP

## Use a TOP