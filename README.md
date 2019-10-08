# design-patterns-js
understanding design patterns

## Singleton
The main idea of this pattern that in app only one instance of this class exists. The contructor of such classes should be private and the instance can be available with static method e.g. getInstance:

```javascript
class MySingleton {
  static #instance;
  
  static getInstance(name) {
    if (!MySingleton.#instance) {
      MySingleton.#instance = new MySingleton(name);
    }

    return MySingleton.#instance;
  }
  
  #name;
  
  constructor(name) {
    this.#name = name;
  }
}

const instance1 = MySingleton.getInstance('name1');
const instance2 = MySingleton.getInstance('name2');
```
instance1 === instance2 gives true, cause both of variables are references to one and the same object

**P.S.** in the example above constructor is not private, but it can be achieved with e.g. https://github.com/tc39/proposal-private-methods

## Multiton
This pattern allows to create definite number of instances of some class. Instances are saved in a map. Each instance has a key. When a user tries to create a new instance with a key, the map of instances is checked:
- if it has such key, then created instance will returned 
- if map doesn't have it and definite number of instances is not reached then a new instance will be created 
- if definite number of instances is reached then appropriate message will be sent

```javascript
class LimitedNumOfInstances {
    #name;

    constructor(name) {
      this.#name = name
    }
}

class MyMultiton {
  #numOfInstances;
  #classToLimit;
  #mapOfInstaces = new Map();
  
  constructor(numOfInstances, classToLimit) {
    this.#numOfInstances = numOfInstances;
    this.#classToLimit = classToLimit;
  }
  
  getInstance(key, name) {
    if (!this.#mapOfInstaces.has(key)) {
      if (this.#mapOfInstaces.size < this.#numOfInstances) {
        const newLimitedInstance = new this.#classToLimit(name);
        
        this.#mapOfInstaces.set(key, newLimitedInstance);
        
        return newLimitedInstance;
      } else {
        console.log(`Only ${this.#numOfInstances} number of instances of this class is allowed`)
      
        return null;
      }
    }
    
    return this.#mapOfInstaces.get(key)
  } 
}
 ```
 
## Pub/Sub (Publication/Subscription)

This pattern allows one class/module to notify another one about some changes or send data. According to this patterns these modules are independent and don't influence on each other. To achieve this EventBus is used, it has preserves all registred subscribers callbacks and two methods subscribe to register new subscriber, and publish to notify subscribers. So EventBus is kind of mediator between two classes.

Lets create EventBus, and two classes

```javascript
const EventBus = {
  subscribers: {},
  
  subscribe(subsciber, callback) {
    if (!subscribers[subsciber]) {
      this.subscribers[subsciber] = [];
    }
    
    this.subscribers[subsciber].push(callback);
  },
  
  publish(subsciber, data) {
    if (!this.subscribers[subsciber]) {
      return;
    }
    
    this.subscribers[subsciber].forEach(callback => callback(data));
  }
}

class Sender {
   #name;

  constructor(name) {
    this.#name = name;
  }

  send() {
    EventBus.publish('test-pub/sub', this.#name);
  }
}

class Listener {
  constructor() {
    EventBus.subscribe('test-pub/sub', this.listen)
  }
  
  listen(data) {
    console.log(`I just listened ${data}`)
  }
}

```
