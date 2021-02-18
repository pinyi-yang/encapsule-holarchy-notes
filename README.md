# Encapsule Holarchy Package (0.0.50 Crystallite)
*last updated 02/11/21*
<!-- references -->
[encapsule]: https://encapsule.io "Encapsule Project Homepage..."
[github]: https://github.com/Encapsule "Encapsule Project GitHub..."

The @encapsule/holarchy package is a runtime library (RTL) distributed in the [@encapsule/holistic package][encapsule] ([github][github]]). It is MIT-licensed libs & tools for building full-stack Node.js/HTML5 apps & services w/React based on System in Cloud (SiC) architecture.

It contains **low-level ES6 classes**, and **Built-in Cell Models and APIs** for holistic app development (`todo: add link holistic app development`):

**NOTICE:** If you are looking for document to develop app with encapsule project please go to holistic app development (`todo: add link holistic app development`). This document is for low-level code in encapslue/holarchy which is the base of holistic. If you are intereted in the framework itself, this is the right document.

* [ES6 Classes](#ES6-Classes)
    * [Observable Controller Data (OCD)](./core/observable-controller-data.md)
    * [Abstract Process Model (APM)](./core/abstract-process-model.md)
    * [Controller Action (ACT)](./core/controller-action.md)
    * [Transition Operator (TOP)](./core/transition-operator.md)
    * [Observable Process Controller (OPC)](./core/observable-process-controller.md)
    * [Cell Model (CM)](./core/cell-model.md)
    * [Cell Model Artifact Space (CMAS)](./core/cell-model-artifact-space.md) (0.0.51)
    * [Cell Model Template (CMT)](./core/cell-model-template.md) (0.0.51)
    * [Cell Processor (CP)](./core/cell-processor.md)
    * [Cell Process Plane](./core/cell-process-plane.md)
* [Built-in Cell Models and APIs](#Built-in-Cell-Models-and-APIs)
    * [Built-in Cell Models](#Built-in-Cell-Models-List)
    * [Built-in Controller Action List](#Built-in-Controller-Action-List)
    * [Built-in Transition Operator List](#Built-in-Transition-Operator-List)
* [Abbreviations](#Abbreviations)
    * [Primitives](#Primitives)
    * [Higher-order Abstractions](#Higher-order-Abstractions)

## ES6 Classes

|ES6 Class| Description |
|---|---|
| [Observable Controller Data (OCD)](./core/observable-controller-data.md) | manages celluar process(es) runtime data on behalf of a Cell Processor |
| [Abstract Process Model (APM)](./core/abstract-process-model.md) | defines memory requirements and/or Finite-State-Machine modeled stateful process behaviors abstractly as a declarative JSON document. |
| [Controller Action (ACT)](./core/controller-action.md) | is used to read and write data to the OCD in an OPC or CP. |
| [Transition Operator (TOP)](./core/transition-operator.md) | is used to read data from the OCD, perform some Boolean operator, and return true/false for APM memory judgements. |
| [Observable Process Controller (OPC)](./core/observable-process-controller.md) | provides memory management, and generic evaluation of cellular process(es) (cellular automata). |
| [Cell Model (CM)](./core/cell-model.md) |  defines an association between a group of APM, TOP, ACT, and subCM's. |
| [Cell Model Artifact Space (CMAS)](./core/cell-model-artifact-space.md) | represents a unique mapping between a CellModel "artifact label" value (a human-readable string) and a set of unique CellModel artifact IRUT ID's within that artifact space.  |
| [Cell Model Template (CMT)](./core/cell-model-template.md) |   |
| [Cell Processor (CP)](./core/cell-processor.md) | accepts a single CM that aggregates all the holarchy artifacts required to deduce OPC configuration, instantiate it, and launch the cellular process. |
| [Cell Process Plane](./core/cell-process-plane.md) | is used to derive CellModel and AbstractProcessModel ID IRUT's from dot-delimited, developer-defined string constants. |

</br>

## Built-in Cell Models and APIs
### Built-in Cell Models List
**Built-in Cell Models/Cell Models Factory
|Models| Description |
|---|---|
| [Cell Process Manager (CPM)](./build-in-cell-model/cell-process-manager.md) | A runtime synthesized top-level [Cell Model](./core/cell-model.md) during [Cell Processor](./core/cell-processor.md) construction that provides all basic TOPs and ACTs for cell process management  |
| [Cell Process Proxy (CPP)](./build-in-cell-model/cell-process-proxy.md) | A resuable helper cell model that allow developers to link namespace(s) defined in the own AbstractProcess Model(s) memory specifications to other cell processes |
| [Observable Value Template](./build-in-cell-model/observable-value-template.md) | **New (0.0.51)**. (`todo add more description`)|
| [Observable Value Proxy Worker Template](./build-in-cell-model/observable-value-proxy-worker-template.md) | **New (0.0.51)**.  (`todo add more description`)|
| [Observable Value Proxy](./build-in-cell-model/observable-value-proxy.md) | **New (0.0.51)**. (`todo add more description`) |

### Built-in Controller Action List
* **[Holarchy Core](./controller-action-apis.md#Holarchy-Core)**
    * [clear boolean flag at path](./controller-action-apis.md#clear-boolean-flag-at-path)
    * [set boolean flag at path](./controller-action-apis.md#set-boolean-flag-at-path)
    * [read name space indrectly](./controller-action-apis.md#read-name-space-indrectly)
    * [write sub action response](./controller-action-apis.md#write-sub-action-response)

* **[Cell Process Manager (CPM)](./controller-action-apis.md#Cell-Process-Manager-CPM)**
    * [create a new owned process](./controller-action-apis.md#create-a-new-owned-process)
    * [delete a owned process](./controller-action-apis.md#delete-a-owned-process)
    * [delegate an action request](./controller-action-apis.md#delegate-an-action-request)
    * [query a cell get subsystem cells](./controller-action-apis.md#query-a-cell)

* **[Cell Process Proxy (CPP)](./controller-action-apis.md#Cell-Process-Proxy-CPP)**
    * [connect a proxy to a shared process](./controller-action-apis.md#connect-a-proxy-to-a-shared-process)
    * [disconnect a proxy to a shared process](./controller-action-apis.md#disconnect-a-proxy-to-a-shared-process)

* **Observable Value**
    `todo add Controller Actions after release`

* **Value Observer**
    `todo add Controller Actions after release`

* **Value Observer Worker**
    `todo add Controller Actions after release`


### Built-in Transition Operator List
* **[Holarchy Core](./transition-operator-apis.md#Holarchy-Core)**
    * [array is empty](./transition-operator-apis.md#array-is-empty)
    * [array length equal to](./transition-operator-apis.md#array-length-equal-to)
    * [compare values](./transition-operator-apis.md#compare-values)
    * [is namespace truthy](./transition-operator-apis.md#is-namespace-truthy)
    * [namespace is map keyless](./transition-operator-apis.md#namespace-is-map-keyless)

* **[Cell Process Manager (CPM)](./transition-operator-apis.md#Cell-Process-Manager-CPM)**
    * [Cell is at step](./transition-operator-apis.md#cell-is-at-step)
    * [Operator delegation](./transition-operator-apis.md#operator-delegation)
    * [Ancestor (parent, child, descendant) Processes Active](./transition-operator-apis.md#ancestor-processes-active)
    * [Ancestor (parent, child, descendant) Processes all in step](./transition-operator-apis.md#ancestor-processes-all-in-step)
    * [Ancestor (child, descendant) Processes any in step](./transition-operator-apis.md#ancestor-processes-any-in-step)

* **[Cell Process Proxy (CPP)](./transition-operator-apis.md#Cell-Process-Proxy-CPP)**
    * [proxy status is](./transition-operator-apis.md#proxy-status-is)

## Abbreviations

### Primitives

OPC - ObservableProcessController

APM - AbstractProcessModel defines memory requirements and/or FSM-modeled stateful process behaviors abstractly as a declarative JSON document.

TOP - TransitionOperator plug-ins are used to read data from the OCD, perform some Boolean operator, and return true/false. They are always called by OPC during the process of celluar process evalution using process step transition operator declarations embedded in a cell's OPM.

ACT - ControllerAction plug-ins are used to read and write data to the OCD. Registered w/OPC, dispatched by OPC.act to process external actor requests. And, OPC requests made during cellular process evaluation using process step exit and enter action declarations embedded in a cell's OPM.

OCD - ObservableControllerData is a data storage container used by OPC for storing and managing cellular process runtime state. Ultimately, the contents of the OCD at runtime is the current runtime state (in total) of a cellular process "evaluating" inside of an OPC instance. The schema of this information derives from the CM passed to CP. Or, more specifically, from the CM's specific OPM declared OCD memory map which defines data and bindings between data and specific APM's.

### Higher-order Abstractions

CM - CellModel - defines an association between a group of APM, TOP, ACT, and subCM's.

CP - Processor - accepts a single CM that aggregates all the holarchy artifacts required to deduce OPC configuration, instantiate it, and launch the cellular process.

