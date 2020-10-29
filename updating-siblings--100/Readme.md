# Updating Siblings 100

---

**Measurements:**
- [Comparison Overview](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/updating-siblings--100/updating-siblings--100__local--animation-frame__template%401.0.0-beta.0.json?dl=0,https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/updating-siblings--100/updating-siblings--100__chunked__template%401.0.0-beta.0.json?dl=0,https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/updating-siblings--100/updating-siblings--100__postTask-user-visible__template%401.0.0-beta.0.json?dl=0,https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/updating-siblings--100/updating-siblings--100__postTask-user-blocking__template%401.0.0-beta.0.json?dl=0,https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/updating-siblings--100/updating-siblings--100__postTask-background__template%401.0.0-beta.0.json?dl=0)
- [local - animation frame](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/updating-siblings--100/updating-siblings--100__local--animation-frame__template%401.0.0-beta.0.json?dl=0)
- [chunk](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/updating-siblings--100/updating-siblings--100__chunked--animation-frame__template%401.0.0-beta.0.json?dl=0)
- [postTask - user-visible](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/updating-siblings--100/updating-siblings--100__postTask--user-visible__template%401.0.0-beta.0.json?dl=0)
- [postTask - user-blocking](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/updating-siblings--100/updating-siblings--100__postTask--user-blocking__template%401.0.0-beta.0.json?dl=0)
- [postTask - background](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/updating-siblings--100/updating-siblings--100__postTask--background__template%401.0.0-beta.0.json?dl=0)

---

This performance measurements aim to compare performance of a sibling component structures with rendering the children over `*ngFor` updating their content over `*rxLet` with different strategies.

## Description

We have 100 items rendered over `*ngFor` to create the content structure, the content is rendered over a `*rxLet`
 directive. On button click we toggle the value rendered in the directive.
 
In the examples the way how the `*rxLet` directives are configured is different in their use strategy.

Orange boxes contain work, and empty boxes just differ in their background color.

![Updating Siblings 100](https://github.com/rx-angular/rx-angular-perf-measures/blob/main/updating-siblings--100/updating-siblings--100.png)

## Implementations

```typescript
@Component({
  selector: 'rxa-sibling-strategies',
  template: `
        <ng-container *ngFor="let sibling of siblings; trackBy:trackBy">
          <div class="sibling" *rxLet="filled$; let f; strategy: strategy >&nbsp;</div>
        </ng-container>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class SiblingCustomComponent {

  strategy; // Different by example

  siblings = [];
  filled$ = new BehaviorSubject<boolean>(false);

  @Input()
  set count(num: number) {
    this.siblings = toBooleanArray(num);
    this.filled$.next(!this.filled$.getValue());
  };

  trackBy = i => i;
}
```

### Local Strategy

```typescript
const strategy = {
  work: (cdRef) => cdRef.detectChanges(),
  behavior: (work: any) => o$ => o$.pipe(
                coalesceWith(priorityTickMap[SchedulingPriority.animationFrame]),
                tap(() => work())
              );
};
```

### Chunked Strategy

```typescript
const strategy = {
  work: (cdRef) => cdRef.detectChanges(),
  behavior: (work: any, context: any) => o$ => o$.pipe(
                               scheduleOnGlobalTick({
                                 priority: 0,
                                 work: work,
                                 scope: context
                               })
                             );
};
```

### PostTask UserVisible Strategy

```typescript
const strategy = {
   work: (cdRef) => cdRef.detectChanges(),
   behavior: () => o$ => o$.pipe(switchMap(v => postTaskTick({ priority: 'user-visible' }, work).pipe(mapTo(v))))
};
```

### PostTask UserVisible Strategy

```typescript
const strategy = {
  work: (cdRef) => cdRef.detectChanges(),
  behavior: () => o$ => o$.pipe(switchMap(v => postTaskTick({ priority: 'user-blocking' }, work).pipe(mapTo(v))))
};
```

### PostTask Background Strategy

```typescript
const strategy = {
  work: (cdRef) => cdRef.detectChanges(),
  behavior: () => o$ => o$.pipe(switchMap(v => postTaskTick({ priority: 'background' }, work).pipe(mapTo(v))));
};
```
