# State Machine Event

## Implementation

### Human:  

This is my state machine:

```typescript
import { BattleshipAI } from './BattleshipAI'
import { IStrategy } from './type/IStrategy'
import { State } from './type/State'
import { RandomStrategy } from './strategy/RandomStrategy'
import { ShipOrientationStrategy } from './strategy/ShipOrientationStrategy'
import { SinkStrategy } from './strategy/SinkStrategy'

export class StateMachine {
  private state: State
  private randomStrategy: IStrategy
  private shipOrientationStrategy: IStrategy
  private sinkStrategy: IStrategy

  constructor(ai: BattleshipAI) {
    this.state = State.Random
    this.randomStrategy = new RandomStrategy(ai)
    this.shipOrientationStrategy = new ShipOrientationStrategy(ai)
    this.sinkStrategy = new SinkStrategy(ai)
  }

  public setState(state: State): void {
    this.state = state
  }

  public getStrategy(): IStrategy {
    switch (this.state) {
      case State.ShipOrientation:
        return this.shipOrientationStrategy
      case State.Sink:
        return this.sinkStrategy
      case State.Random:
      default:
        return this.randomStrategy
    }
  }

  public transition(ai: BattleshipAI): void {
    if (ai.isShipToSink()) {
      this.setState(State.Sink)
    } else if (ai.isShipHit()) {
      this.setState(State.ShipOrientation)
    } else {
      this.setState(State.Random)
    }
  }
}
```

This is my event emitter:

```typescript
export type Listener<T> = (payload: T) => void

export class EventEmitter<Events extends Record<symbol, any>> {
  private listeners: Map<symbol, Set<Listener<any>>> = new Map()

  on<K extends keyof Events & symbol>(
    event: K,
    listener: Listener<Events[K]>
  ): void {
    if (!this.listeners.has(event)) {
      this.listeners.set(event, new Set())
    }
    this.listeners.get(event)!.add(listener)
  }

  off<K extends keyof Events & symbol>(
    event: K,
    listener: Listener<Events[K]>
  ): void {
    const eventListeners = this.listeners.get(event)
    if (eventListeners) {
      eventListeners.delete(listener)
      if (eventListeners.size === 0) {
        this.listeners.delete(event)
      }
    }
  }

  emit<K extends keyof Events & symbol>(event: K, payload: Events[K]): void {
    const eventListeners = this.listeners.get(event)
    if (eventListeners) {
      eventListeners.forEach(listener => listener(payload))
    }
  }

  clear(event?: keyof Events & symbol): void {
    if (event) {
      this.listeners.delete(event)
    } else {
      this.listeners.clear()
    }
  }

  once<K extends keyof Events & symbol>(
    event: K,
    listener: Listener<Events[K]>
  ): void {
    const onceListener: Listener<Events[K]> = payload => {
      listener(payload)
      this.off(event, onceListener)
    }
    this.on(event, onceListener)
  }
}
```

Implement feature: state machine emits events when state changes.  

### AI:

Below is one way to implement the feature. In this example, we define a state change event (using a symbol) and create a type for our event payload. Then we update the `StateMachine` so that whenever its state changes via `setState`, it emits an event with both the previous and the new state. Listeners can subscribe to the `STATE_CHANGE` event via the embedded `EventEmitter`.

```typescript
import { BattleshipAI } from './BattleshipAI'
import { IStrategy } from './type/IStrategy'
import { State } from './type/State'
import { RandomStrategy } from './strategy/RandomStrategy'
import { ShipOrientationStrategy } from './strategy/ShipOrientationStrategy'
import { SinkStrategy } from './strategy/SinkStrategy'
import { EventEmitter } from './EventEmitter'

// Define a symbol for the state change event
export const STATE_CHANGE = Symbol('stateChange')

// Define the events type for the StateMachine emitter
interface StateMachineEvents {
  [STATE_CHANGE]: { oldState: State; newState: State }
}

export class StateMachine {
  private state: State
  private randomStrategy: IStrategy
  private shipOrientationStrategy: IStrategy
  private sinkStrategy: IStrategy

  // Create an event emitter instance
  public events: EventEmitter<StateMachineEvents>

  constructor(ai: BattleshipAI) {
    this.state = State.Random
    this.randomStrategy = new RandomStrategy(ai)
    this.shipOrientationStrategy = new ShipOrientationStrategy(ai)
    this.sinkStrategy = new SinkStrategy(ai)
    this.events = new EventEmitter<StateMachineEvents>()
  }

  public setState(state: State): void {
    if (this.state !== state) {
      const oldState = this.state
      this.state = state
      // Emit an event whenever the state changes
      this.events.emit(STATE_CHANGE, { oldState, newState: state })
    }
  }

  public getStrategy(): IStrategy {
    switch (this.state) {
      case State.ShipOrientation:
        return this.shipOrientationStrategy
      case State.Sink:
        return this.sinkStrategy
      case State.Random:
      default:
        return this.randomStrategy
    }
  }

  public transition(ai: BattleshipAI): void {
    if (ai.isShipToSink()) {
      this.setState(State.Sink)
    } else if (ai.isShipHit()) {
      this.setState(State.ShipOrientation)
    } else {
      this.setState(State.Random)
    }
  }
}
```

#### Explanation

- **Event Emitter Integration:**  
  We add a public property `events` of type `EventEmitter<StateMachineEvents>` that lets external components subscribe to state changes.

- **State Change Symbol and Event Payload:**  
  The `STATE_CHANGE` symbol is used as the event identifier, and its payload contains the old state and the new state.

- **Emitting the Event:**  
  The `setState` method first checks if the new state is different from the current state. If so, it emits the `STATE_CHANGE` event with the old and new state values.

This design allows other parts of your application to react when the state changes by subscribing to the `STATE_CHANGE` event.
