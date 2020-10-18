# Passing Values Nested

---

**Measurements:**
- [Comparison Overview](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/passing-values-nested/passing-values-nested__static__template%401.0.0-bata.0.json?dl=0,https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/passing-values-nested/passing-values-nested__observable__template%401.0.0-bata.0.json?dl=0,https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/passing-values-nested/passing-values-nested__async__template%401.0.0-bata.0.json?dl=0,https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/passing-values-nested/passing-values-nested__push__template%401.0.0-bata.0.json?dl=0,https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/passing-values-nested/passing-values-nested__rx-let-component__template%401.0.0-bata.0.json?dl=0,https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/passing-values-nested/passing-values-nested__rx-let-embedded-view__template%401.0.0-bata.0.json?dl=0)
- [Use static values](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/passing-values-nested/passing-values-nested__static__template%401.0.0-bata.0.json?dl=0)
- [Use observables directly](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/passing-values-nested/passing-values-nested__observable__template%401.0.0-bata.0.json?dl=0)
- [Use async pipe and observables](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/passing-values-nested/passing-values-nested__async__template%401.0.0-bata.0.json?dl=0)
- [Use push pipe and observables](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/passing-values-nested/passing-values-nested__push__template%401.0.0-bata.0.json?dl=0)
- [Use let directive and component based change detection](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/passing-values-nested/passing-values-nested__rx-let-component__template%401.0.0-bata.0.json?dl=0)
- [Use let directive and embedded view based change detection](https://chromedevtools.github.io/timeline-viewer/?loadTimelineFromURL=https://raw.githubusercontent.com/rx-angular/rx-angular-perf-measures/main/passing-values-nested/passing-values-nested__rx-let-embedded-view__template%401.0.0-bata.0.json?dl=0)

---

This performance measurements aim to compare performance of nested component structures with updating values passed over input bindings.

## Description

We have a 5 level deep nested component structure. On button click we increment the value.
The value is handed over through all nested components over input bindings.

In the examples the way how the value is handed over differs.

Yellow circles count the number of dirty checks, red circles display the passed value.

![Passing Values Nested](https://github.com/rx-angular/rx-angular-perf-measures/blob/main/passing-values-nested/passing-values-nested.png)

## Implementations

### Statically

```typescript

@Component({
  selector: 'rxa-recursive-static',
  template: `
    <ng-container *ngIf="level === 0; else: branch">
        <p>Level {{total-level}}</p>
        <ng-container *embeddedViewLet="value$; let value">{{value}}<ng-container>
    </ng-container>
    <ng-template #branch>
        <p>Level {{total-level}}</p>
        <rxa-recursive-static [total]="total" [level]="level-1" [value]="value"></rxa-recursive-static>  
    </ng-template>
  `,
  host: {
    class: 'd-flex w-100'
  },
  providers: [CdHelper],
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class RecursiveStaticComponent {

  @Input()
  set depth(d){
    this.total = d;
    this.level = this.total -1;
  }

  @Input()
  total = 0;

  @Input()
  level = 0;

  @Input()
  value;

}
```

### Pass Observable directly

```typescript
@Component({
  selector: 'rxa-recursive-observable',
  template: `
    <ng-container *ngIf="level === 0; else: branch">
        <p>Level {{total - level}}</p>
        <ng-container *embeddedViewLet="value$; let value">{{value}}<ng-container>
    </ng-container>
    <ng-template #branch>
        <p>Level {{total - level}}</p>
        <rxa-recursive-observable [total]="total" [level]="level-1" [value$]="value$"></rxa-recursive-observable>
    </ng-template>
  `,
  host: {
    class: 'd-flex w-100'
  },
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class RecursiveObservableComponent {

  @Input()
  set depth(d) {
    this.total = d;
    this.level = this.total - 1;
  }

  @Input()
  total = 0;

  @Input()
  level = 0;

  @Input() value$: Observable<any>;

}
```

### Async Pipe

```typescript
@Component({
  selector: 'rxa-recursive-async',
  template: `
    <ng-container *ngIf="level === 0; else: branch">
        <p>Level {{total-level}}</p>
        {{value$ | async}}
    </ng-container>
    <ng-template #branch>
        <p>Level {{total-level}}</p>
        <rxa-recursive-async [total]="total" [level]="level-1" [value]="value$ | async"></rxa-recursive-async>
    </ng-template>
  `,
  host: {
    class: 'd-flex w-100'
  },
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class RecursiveAsyncComponent {
  @Input()
  set depth(d){
    this.total = d;
    this.level = this.total -1;
  }

  @Input()
  total = 0;

  @Input()
  level = 0;

  value$ = new Subject();
  @Input()
  set value(v) {
    this.value$.next(v)
  };

}
```

### Push Pipe

```typescript
@Component({
  selector: 'rxa-recursive-push',
  template: `
    <ng-container *ngIf="level === 0; else: branch">
        <p>Level {{total-level}}</p>
        {{value$ | push}}
    </ng-container>
    <ng-template #branch>
        <p>Level {{total-level}}</p>
        <rxa-recursive-push [total]="total" [level]="level-1" [value]="value$ | push"></rxa-recursive-push>
    </ng-template>
  `,
  host: {
    class: 'd-flex w-100'
  },
  providers: [CdHelper],
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class RecursivePushComponent {
  @Input()
  set depth(d){
    this.total = d;
    this.level = this.total -1;
  }

  @Input()
  total = 0;

  @Input()
  level = 0;

  value$ = new Subject();
  @Input()
  set value(v) {
    this.value$.next(v)
  };

}
```

### LetDirective - Component based Change Detection

```typescript
@Component({
  selector: 'rxa-recursive-component-let',
  template: `
    <ng-container *ngIf="level === 0; else: branch">
        <p>Level {{total-level}}</p>
        <rxa-renders *componentLet="value$; let v" [value$]="v"></rxa-renders>
    </ng-container>
    <ng-template #branch>
        <p>Level {{total-level}}</p>
        <rxa-recursive-component-let [total]="total" *componentLet="value$; let v" [level]="level-1" [value]="v"></rxa-recursive-component-let>
    </ng-template>
  `,
  host: {
    class: 'd-flex w-100'
  },
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class RecursiveComponentLetComponent {
  @Input()
  set depth(d){
    this.total = d;
    this.level = this.total -1;
  }

  @Input()
  total = 0;

  @Input()
  level = 0;

  value$ = new ReplaySubject(1)
  @Input()
  set value(v) {
    this.value$.next(v)
  };

}
```

### LetDirective - EmbeddedView based Change Detection

```typescript
@Component({
  selector: 'rxa-recursive-embedded-view-let',
  template: `
    <ng-container *ngIf="level === 0; else: branch">
        <p>Level {{total-level}}</p>
        <ng-container *componentLet="value$; let value">{{value}}</ng-container>
    </ng-container>
    <ng-template #branch>
        <p>Level {{total-level}}</p>
        <rxa-recursive-embedded-view-let [total]="total" *componentLet="value$; let v" [level]="level-1" [value]="v"></rxa-recursive-embedded-view-let>
    </ng-template>
  `,
  host: {
    class: 'd-flex w-100'
  },
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class RecursiveEmbeddedViewLetComponent {
  @Input()
  set depth(d){
    this.total = d;
    this.level = this.total -1;
  }

  @Input()
  total = 0;

  @Input()
  level = 0;

  value$ = new ReplaySubject(1)
  @Input()
  set value(v) {
    this.value$.next(v)
  };

}
```

