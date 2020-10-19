# Bootstrapping flat components

---

**Measurements:**
- [Comparison Overview](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstrapping-flat-components/bootstrapping-flat-components__static__template%401.0.0-bata.0.json?dl=0,https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstrapping-flat-components/bootstrapping-flat-components__rx-for-sync__template%401.0.0-bata.0.json?dl=0,https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstrapping-flat-components/bootstrapping-flat-components__rx-for-animation-frame__template%401.0.0-bata.0.json?dl=0)
- [Use static values](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstrapping-flat-components/bootstrapping-flat-components__static__template%401.0.0-bata.0.json?dl=0)
- [Use observables directly](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstrapping-flat-components/bootstrapping-flat-components__rx-for-sync__template%401.0.0-bata.0.json?dl=0)
- [Use async pipe and observables](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/bootstrapping-flat-components/bootstrapping-flat-components__rx-for-animation-frame__template%401.0.0-bata.0.json?dl=0)

---

This performance measurement aims to compare performance of flat component structures bootstrapping them with different scheduling methods.

## Description

We have a flat component structure counting 42 elements. On button click we display/hide the elements.

In the examples the way how the rendering is scheduled differs.

![Bootstrapping Flat Components](https://github.com/rx-angular/rx-angular-perf-measures/blob/main/bootstrapping-flat-components/bootstrapping-flat-components.png)

## Implementations

### Statically

```typescript

```

### rxFor sync

```typescript

```

### rxFor animation frame

```typescript

```

