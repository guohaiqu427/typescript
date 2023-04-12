# typescript

# general cooncept and setup 

- def: typed syntactic superset of JavaScript
- benefits:
    - move some errors from runtime to compile time 
    - It serves as the foundation for a great code authoring experience
- package.jsoon 
    ```json
    "scripts": {
        "dev": "tsc --watch --preserveWatchOutput"
    }
    ``` 
    tsc: typescript compiler
    --watch: countiunous watching file changes 
    --preserveWatchOutput: keep console output
    ```json
    {
        "name": "hello-ts",
        "license": "NOLICENSE",
        "version": "0.0.1",
        "devDependencies": {
            "typescript": "^4.3.2"
        },
        "scripts": {
            "dev": "tsc --watch --preserveWatchOutput"
        }
    }
    ```
- tsconfig.json
    ```json
    {
    "compilerOptions": {
        "outDir": "dist", // where to put the TS files 
        // tsc --outDir dist
        "target": "ES3" // which level of JS support to target // default es3
    },
    "include": ["src"] // which files to compile
    }

    ```
- file type
    .ts files contain both type information and code that runs
    .js files contain only code that runs
    **.d.ts** files contain only type information
    ```ts
    /**
     * Add three numbers
    * @param a first number
    * @param b second
    */
    export async function addNumbers(a: number, b: number) {
    await timeout(500)
    return a + b
    }
    ```
- node.js setup 
    - node runs with common js 
    tsconfig.json 
    ```json
    "compilerOptions": {
        "outDir": "dist",
        "module": "CommonJS",
    }
    ```

# variable and value 
```ts
let age = 6         // type : number
const count = 1     // type: 1 // literal type
let endTime         // type: any 
let endTime: Date   // type: Date // type annotation
function add(a: number, b: number): number {} 
// In TypeScript, variables are “born” with their types.
// TS is able to make type assumption
// 
```

# objects 
```ts 
    (car: {
        make: string
        model: string
        year: number
        chargeVoltage?: number  // optional   // (property) chargeVoltage?: number | undefined
        chargeVoltage: number | undefined   // must have chargeVoltage key
    })
```
- Excess property 
    - Remove extra property from the object
    - Add a new type annotation
    - Create a variable to hold this value, and assign type 

- index signatures
    ```ts
    const phones : {
        [k: string]: {
            country: string
            area: string
            number: string
        } | undefined  
    } = {}
    const phones = {
        home: { country: "+1", area: "211", number: "652-4515" },
        work: { country: "+1", area: "670", number: "752-5856" },
        fax: { country: "+1", area: "322", number: "525-4357" },
    }
    ```

# Array
    ```ts
        const fileExtensions = ["js", "ts"] //  string[]
        let myCar = [2002, "Toyota", "Corolla"] // (string | number)[]
        const [year, make, model] = myCar //all vars are : string | number

        // Tuples
        // a fixed length with defeined type
        let myCar: [number, string, string] = [
            2002,
            "Toyota",
            "Corolla",
        ]
        // limitation: 
        const numPair: [number, number] = [4, 5, 6] // valid ts check, which fails 6
        numPair.push(6) // [4, 5, 6]                // able to push to arr
        numPair.pop() // [4, 5]                     // able to pop item 
    ```

# Union Types : OR
    ```ts
    function flipCoin(): "heads" | "tails" {
        if (Math.random() > 0.5) return "heads"
        return "tails"
    }
    const outcome = flipCoin() // type: "heads" | "tails"
    ```
    ```ts
    const outcome: ["error", Error] | ["success", {
        name: string;
        email: string;
    }]
    ```

# Type guards
```ts
    const outcome: ['error', Error] | ['success', {name:string}]= maybeGetUserInfo()  // returns a 
    const [first, second] = outcome

    if (second instanceof Error) {  // type guard
        second                      // type:  Error
    }else{
        second                      // type: {name:string}
    }
```

```ts
    const outcome: ['error', Error] | ['success', {name:string}]= maybeGetUserInfo()  // returns a 
    const [first, second] = outcome

    if (outcome[0] === "error") {   // Discriminated Unions
        second                      // type:  Error
    }else{
        second                      // type: {name:string}
    }
```


# Intersection : AND
!todo 

# Type aliases
- Duplicate type identifier are not allowed
- Inheritance in type aliases:
    - combine existing types with new behavior by using Intersection (&) types.
    ```ts
        // we can export and import the types
        export type UserContactInfo = {
        name: string
        email: string
        }
        // --- 
        type SpecialDate = Date & { getReason(): string }

        const newYearsEve: SpecialDate = {
        ...new Date(),
        getReason: () => "Last day of the year",
        }
    ```

# Interfaces
- An interface is a way of defining an object type. 
    ```ts
        interface UserInfo {
        name: string
        email: string
        }
    ```
- Inheritance in interfaces
    - a “sub-interface” extends from a base interface
    ```ts
    interface Animal {
        isAlive(): boolean
    }
    interface Mammal extends Animal {
        getFurOrHairColor(): string
    }
    interface Dog extends Mammal {
        getBreed(): string
    }
    ```
- Implement INTERFACE on CLASS
    ```TS
    interface AnimalLike {
        eat(food): void
    }
    class Dog implements AnimalLike {
        bark() {
            return "woof"
        }
        eat(food) {
            consumeFood(food)
        }
    }
    ```
- multiple inheritance
    ```ts
    class LivingOrganism {
        isAlive() {
            return true
        }
    }
    interface AnimalLike {
        eat(food): void
    }
    interface CanBark {
        bark(): string
    }
    
    class Dog extends LivingOrganism
              implements AnimalLike, CanBark{
        bark() {
            return "woof"
        }
        eat(food) {
            consumeFood(food)
        }
    }
    ```
- open interface 
    - can have multiple delcarations 
    ```ts
    interface AnimalLike {
        isAlive(): boolean
    }

    function feed(animal: AnimalLike) {}

    interface AnimalLike {
        eat(food): void
    }
    ```

# when to use Type and Interface 
!todo
- If you need to define something other than an object type 
(e.g., use of the | union type operator), 
you must use a type alias
- If you need to define a type to use with the implements heritage term, 
it’s best to use an interface
- If you need to **allow consumers of your types to augment?** them, 
you must use an interface.

# Recursion
    ```ts
    type NestedNumbers = number | NestedNumbers[]
    // const val: NestedNumbers = [3, 4, [5, 6, [7], 59], 221]
    ```

# JSON 
```ts
type JSONPrimitive = string | number | boolean | null
type JSONObject = { [k: string]: JSONValue }
type JSONArray = JSONValue[]
type JSONValue = JSONArray | JSONObject | JSONPrimitive
```

# function 
- type & interface
    ```ts
    interface TwoNumberCalculation {
        (x: number, y: number): number
    }
    type TwoNumberCalc = (x: number, y: number) => number
    ```

- void
    - the return value of a void function is ignored.
    - void should only be used as function return type
    ```ts
    function invokeInFourSeconds(callback: () => undefined) {
    setTimeout(callback, 4000)
    }
    function invokeInFiveSeconds(callback: () => void) {
    setTimeout(callback, 5000)
    }
    ```

- function overloads
```ts
type FormSubmitHandler = (data: FormData) => void
type MessageHandler = (evt: MessageEvent) => void
 
function handleMainEvent(   // case 1
  elem: HTMLFormElement,
  handler: FormSubmitHandler
)
function handleMainEvent(   // case 2
  elem: HTMLIFrameElement,
  handler: MessageHandler
)

function handleMainEvent(   // general 
  elem: HTMLFormElement | HTMLIFrameElement,
  handler: FormSubmitHandler | MessageHandler
) {}
 
const myFrame = document.getElementsByTagName("iframe")[0]
const myForm = document.getElementsByTagName("form")[0]


handleMainEvent(myFrame, (val) => {}
handleMainEvent(myForm, (val) => {}

```

- this 
```ts
// .bind
// .call
// .apply 

function myClickHandler(
  this: HTMLButtonElement,
  event: Event
) {
  this.disabled = true
}
const myButton = document.getElementsByTagName("button")[0]
const boundHandler = myClickHandler.bind(myButton)
boundHandler(new Event("click"))
// myClickHandler.call(myButton, new Event("click"))
```

# class
```ts
class Car {
  make: string
  model: string
  year: number
  constructor(make: string, model: string, year: number) {
    this.make = make
    this.model = model
    this.year = year
  }
}
let sedan = new Car("Honda", "Accord", 2017)
// sedan.activateTurnSignal("left") // not safe!
// new Car(2017, "Honda", "Accord") // not safe!
```
```ts
class Car {
  constructor(
    public make: string,
    public model: string,
    public year: number
  ) {}
}
 
const myCar = new Car("Honda", "Accord", 2017)
myCar.make
```

- limited exposure 
    - public : everyone (this is the default)
    - private : the instance itself, and subclasses **# privateAlias**
    - protected : only the instance itself
    - readonly: in combination with the other 3, can assign in constructor
    - disappear when compile

# Types & Values 
- top types: any possible value allowed by the system
    - any 
    - unknown : need to use type guard with it. 
    ```ts
    let myUnknown: unknown = 14 
    if (typeof myUnknown === 'number'){
        myUnknown
    }
    ```
- bottom types: 
    - never: no possible value allowed by the system
        - useful for **Exhaustive conditionals**

- **is** type guard: 
    ```ts
    // interface
    interface CarLike {
        make: string
        model: string
        year: number
    }
    // to be checked
    let maybeCar: unknown

    // guard
    function isCarLike(valueToTest: any):valueToTest is CarLike{
        retrun (
            // user defined checks that return true or false 
        )
    }
    // use guard
    if(isCarLike(maybeCar)){
        maybeCar //  maybeCar: CarLike
    }


    ```

- nullish values: 
    - null: there is a value, and that value is nothing. 
    - undefined: the value isn’t available (yet?)
    - void: function’s return value should be ignored
    ```ts
    type GroceryCart = {
        fruits?: { name: string; qty: number }[]
        vegetables?: { name: string; qty: number }[]
    }
    // cart.fruits.push({ name: "kumkuat", qty: 1 }) ts error: fruit could be undefined
    cart.fruits!.push({ name: "kumkuat", qty: 1 }) // success
    // !: tells ts to ignore undefined, it will have a value

    ```

# Generics
- This allows us to traffic that type information in one side of the function and out the other.

- General Type
    ```TS
    function identity<T>(arg: T): T{
    return arg;
    }

    let myIdentity: <T>(arg: T) => T = identity;
    ```

- Generic Type Extends
    ```ts
    interface HasId {
    id: string
    }
    interface Dict<T> {
    [k: string]: T
    }
    function listToDict<T extends HasId>(list: T[]): Dict<T> {

    }
    ```