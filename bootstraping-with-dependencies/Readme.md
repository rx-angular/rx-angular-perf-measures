# Bootstrapping components with dependencies

---

**Measurements:**

- [Bootstraping all components](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstraping-with-dependencies/bootstrap-time-all-components.json)
- [Component without dependencies](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstraping-with-dependencies/bootstrap-time-pure.json)
- [Component injects a service](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstraping-with-dependencies/bootstrap-time-service-inject.json)
- [Component extends a service](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstraping-with-dependencies/bootstrap-time-service-extend.json)
- [Component injects RxState](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstraping-with-dependencies/bootstrap-time-state-inject.json)
- [Component extends RxState](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstraping-with-dependencies/bootstrap-time-state-extend.json)
- [Component with Async Pipe](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstraping-with-dependencies/bootstrap-time-async-pipe.json)
- [Component with Push Pipe](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstraping-with-dependencies/bootstrap-time-push-pipe.json)
- [Component with Let Directive](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstraping-with-dependencies/bootstrap-time-let-directive.json)

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
