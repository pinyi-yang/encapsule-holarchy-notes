# Encapsule Holarchy Package (0.0.50 Crystallite)
*last updated 02/04/21*
<!-- references -->
[encapsule]: https://encapsule.io "Encapsule Project Homepage..."
[github]: https://github.com/Encapsule "Encapsule Project GitHub..."

The @encapsule/holarchy package is a runtime library (RTL) distributed in the [@encapsule/holistic package][encapsule] ([github][github]]). It is MIT-licensed libs & tools for building full-stack Node.js/HTML5 apps & services w/React based on System in Cloud (SiC) architecture.

It contains **low-level ES6 classes**, and **Built-in Cell Models and APIs** for holistic app development (`todo: add link holistic app development`):

* [ES6 Classes](#ES6-Classes)
    * Observable Controller Data (OCD)
    * Observable Process Controller (OPC)
    * Abstract Process Model (APM)
    * Controller Action (ACT)
    * Cell Model (CM)
    * Cell Processor (CP)
    * Cell Process Plane
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
| [Observable Controller Data (OCD)](./core/observable-controller-data.md)| a data storage container used by OPC for storing and managing cellular process runtime state. |
| [Observable Process Controller (OPC)](./core/observable-process-controller.mds) |  |
| [Abstract Process Model (APM)](./core/abstract-process-model.md) | defines memory requirements and/or FSM-modeled stateful process behaviors abstractly as a declarative JSON document. |
| [Controller Action (ACT)](./core/controller-action.md) | ControllerAction plug-ins are used to read and write data to the OCD. |
|[Transition Operator (TOP)](./core/transition-operator.md)| is used to read data from the OCD, perform some Boolean operator, and return true/false. |
| [Cell Model (CM)](./core/cell-model.md) |  defines an association between a group of APM, TOP, ACT, and subCM's. |
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
| [Observable Value Factory](./build-in-cell-model/observable-value-factory.md) | **New (0.0.50)**.  A filter that manufactures a specialized ObservableValue Cell Model that (`todo add more description`)|
| [Value Observer](./build-in-cell-model/value-observer.md) | **New (0.0.50)**. a Cell Model provides a generic way to evaluate transtion operators (TOPs) and perform actions on an ObservableValue Cell |
| [Value Observer Worker Factory](./build-in-cell-model/value-observer-worker-factory.md) | **New (0.0.50)**. A filter that manufactures a specialized ValueObserverWorker Cell Model that (`todo add more description`) |

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
* **[Holarchy Core](./transition-operator-apis#Holarchy-Core)**
    * [array is empty](./transition-operator-apis#array-is-empty)
    * [array length equal to](./transition-operator-apis#array-length-equal-to)
    * [compare values](./transition-operator-apis#compare-values)
    * [is namespace truthy](./transition-operator-apis#is-namespace-truthy)
    * [namespace is map keyless](./transition-operator-apis#namespace-is-map-keyless)

* **[Cell Process Manager (CPM)](./transition-operator-apis#Cell-Process-Manager-CPM)**
    * [Cell is at step](./transition-operator-apis#cell-is-at-step)
    * [Operator delegation](./transition-operator-apis#operator-delegation)
    * [Ancestor (parent, child, descendant) Processes Active](./transition-operator-apis#ancestor-processes-active)
    * [Ancestor (parent, child, descendant) Processes all in step](./transition-operator-apis#ancestor-processes-all-in-step)
    * [Ancestor (child, descendant) Processes any in step](./transition-operator-apis#ancestor-processes-any-in-step)

* **[Cell Process Proxy (CPP)](./transition-operator-apis#Cell-Process-Proxy-CPP)**
    * [proxy status is](./transition-operator-apis#proxy-status-is)

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

