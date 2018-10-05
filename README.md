# ng-set-state

Returns a strongly typed function that safely mutates this.state for the given context.

1. Set up State interface and initial state:

```
const initialState = {
  counter: 0
};
type State = Readonly<typeof initialState>;
```

2. Initialize state and setState inside component

```
export class StepperComponent {
  readonly state: State = initialState;
  readonly setState = createSetState<State>(this);
}
```

Returned function (setState) accepts an updater function with the signature:

```
(previousState) => nextState
```

State is a reference to the component state at the time the change is being applied. It should not be directly
mutated. Instead, changes should be represented by building a new object based on the input from state. For
instance, suppose we wanted to increment a value in state by 1:

```
handleStep(): void {
  this.setState((previousState) => ({
    counter: previousState.counter + 1
  }));
}
```

State received by the updater function is the previous state. The next state will be the output of the updater.

This paradigm allows us to extract state update functions to pure functions outside the class.

```
const incrementCounter = (previousState: State) => ({ counter: previousState.counter + 1 });

export class StepperComponent {
  readonly state: State = initialState;
  readonly setState = createSetState<State>(this);

  handleStep(): void {
    this.setState(incrementCounter);
  }
}
```
