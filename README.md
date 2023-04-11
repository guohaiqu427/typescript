# typescript

# general cooncept and setup 

- def: typed syntactic superset of JavaScript
- benefits:
    - move some errors from runtime to compile time 
    - It serves as the foundation for a great code authoring experience
- package.jsoon 
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
        "target": "ES3" // which level of JS support to target
    },
    "include": ["src"] // which files to compile
    }

    ```
- file type
    .ts files contain both type information and code that runs
    .js files contain only code that runs
    **.d.ts** files contain only type information
- node.js setup 
    tsconfig.json 
    ```
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
// TS is able to make a more specific assumption here
//
```

# objects 
```ts 
    {
        make: string
        model: string
        year: number
        chargeVoltage?: number  // optional   // (property) chargeVoltage?: number | undefined
        chargeVoltage: number | undefined   // must have chargeVoltage key
    }
```
- Excess property 
    - Remove extra property from the object
    - Add a new type annotation
    - Create a variable to hold this value, and assign type 

- index signatures
    ```ts
    {
        [k: string]: {
            country: string
            area: string
            number: string
        }
    }
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
!todo

# function 
```ts
    interface TwoNumberCalculation {
        (x: number, y: number): number
    }
    type TwoNumberCalc = (x: number, y: number) => number
```

- void
!todo 
```ts
    function invokeInFourSeconds(callback: () => undefined) {
    setTimeout(callback, 4000)
    }
    function invokeInFiveSeconds(callback: () => void) {
    setTimeout(callback, 5000)
    }
```
