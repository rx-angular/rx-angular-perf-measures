# Bootstrapping components with dependencies

---

**Measurements:**

- [Bootstraping all components]()
- [Component without dependencies]()
- [Component injects a service]()
- [Component extends a service]()
- [Component injects RxState]()
- [Component extends RxState]()
- [Component with Async Pipe]()
- [Component with Push Pipe]()
- [Component with Let Directive]()

---

This performance measurement aims to compare bootstrap time of the component with different dependencies.

## Description

We have a component that displays a text.

- Component without dependencies. `text` property defined inside component.
- Component injects a service. `text` property defined inside the service.
- Component extends a service. `text` property defined inside the service.
- Component injects RxState. `text` property is a part of state.
- Component extends RxState. `text` property is a part of state.
- Component with Async Pipe. `text$` property defined inside component and displayed with `async` pipe.
- Component with Push Pipe. `text$` property defined inside component and displayed with `push` pipe.
- Component with Let Directive. `text$` property defined inside component and displayed with `rxLet` directive.
