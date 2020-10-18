# Automated Measurements

---

**Measurements:**
- [Comparison Overview](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/automated-measurement/automated-measurement__push__template%401.0.0-beta.0.json?dl=0,https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/automated-measurement/automated-measurement__push__template%401.0.0-beta.0.json?dl=0)
- [Use push pipe to render values](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/automated-measurement/automated-measurement__push__template%401.0.0-beta.0.json?dl=0)
- [Use push rxLet to render values](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/automated-measurement/automated-measurement__push__template%401.0.0-beta.0.json?dl=0)

---

This performance measurements aim to test the automation of the performance tests through cypress e2e tests.

## Description

> The description is a copy of https://github.com/rx-angular/rx-angular/pull/379#issue-504201736

### What

This PR (the source branch itself, to be precise) serves as a shareable POC for integrating Chrome Devtools Protocol with our Cypress tests' configuration.

### Why

This change would potentially enable us to generate reports (flame charts) programmatically - using automated e2e tests. Those charts are normally accessible from the Chrome Devtools via "Performance" and "JavaScript Profiler" tabs.

### How

##### What is CDP?

I configured Cypress to work with the [`chrome-remote-interface`](https://github.com/cyrus-and/chrome-remote-interface) library by enabling it via Cypress [tasks](https://docs.cypress.io/api/commands/task.html). See `apps/demos-e2e/src/plugins/index.js` for a reference.

[Chrome Devtools Protocol docs](https://chromedevtools.github.io/devtools-protocol/) show the whole API from this protocol. `chrome-remote-interface` is a library that provides JS counterparts for the available methods of the CDP. I used `Profiler` methods for generating "JavaScript Profiler" reports and `Tracing` for "Performance" reports.

##### What tests are there?

For now, there are 2 example tests and 2 flame chart reports generated per test = 4 reports generated after the whole test suite. Tests are performed on the "RxLet vs Push pipe" demo page and the single test consists of clicking the "Toggle" button 6 times resulting in generating a list of 10000 elements 3 times. Each action has a 1 second of artificial delay so that the results on the flame chart would be more readable.

##### How to run tests?

To run tests run:
```
yarn nx run demos-e2e:e2e --watch
```

The `--watch` flag is optional. Report files will be generated in the directory of your codebase. Those files can be loaded to the "JavaScript Profiler" or "Performance" tabs in Devtools. Or you can use [Devtools Timeline Viewer](https://chromedevtools.github.io/timeline-viewer/) to also see it.

![BootstrappingPassing Values Nested](https://github.com/rx-angular/rx-angular-perf-measures/blob/main/automated-measurement/automated-measurement.png)

## Implementations


## Statically

```typescript

```

## Async Pipe

```typescript

```

## Push Pipe

```typescript

```

## LetDirective

```typescript

```

