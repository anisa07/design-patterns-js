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
