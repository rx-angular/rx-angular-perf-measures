# Passing Values Nested

## Statically

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

## Observable

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

## Async Pipe

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

## Push Pipe

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

## LetDirective - Component based Change Detection

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

## LetDirective - EmbeddedView based Change Detection

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

