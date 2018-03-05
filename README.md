# Swift tips and tricks

Here's list of Swift tips & tricks with all additional sources (playgrounds, images) that I would like to share. Also you can find them on [Twitter @szubyak](https://twitter.com/szubyak) and [Facebook @szubyakdev](https://www.facebook.com/szubyakdev), where you can ask questions and respond with feedback. I will realy glad to have you there! 😀

## Table of contents

[#1 Safe way to return element at specified index](https://github.com/Luur/SwiftTips#1-safe-way-to-return-element-at-specified-index)<br />
[#2 Easy way to hide Status Bar](https://github.com/Luur/SwiftTips#2-easy-way-to-hide-status-bar)<br />
[#3 Enumerated iteration](https://github.com/Luur/SwiftTips#3-enumerated-iteration)

## [#1 Safe way to return element at specified index](https://twitter.com/szubyak/status/950345927054778368)

You can extend collections to return the element at the specified index if it is within bounds, otherwise nil.

```swift
extension Collection {
    subscript (safe index: Index) -> Element? {
        return indices.contains(index) ? self[index] : nil
    }
}

let cars = ["Lexus", "Ford", "Volvo", "Toyota", "Opel"]
let selectedCar1 = cars[safe: 3] // Toyota
let selectedCar2 = cars[safe: 6] // not crash, but nil
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#2 Easy way to hide Status Bar](https://twitter.com/szubyak/status/950687583222337537)

Ever faced the problem that u can't hide status bar because of `prefersStatusBarHidden` is `get-only`? The simplest solution is to `override` it 🧐👨‍💻

```swift
let vc = UIViewController()
vc.prefersStatusBarHidden = true // error
print("statusBarHidded \(vc.prefersStatusBarHidden)") // false

class TestViewController: UIViewController {
    override var prefersStatusBarHidden: Bool {
        return true
    }
}

let testVC = TestViewController()
print("statusBarHidded \(testVC.prefersStatusBarHidden)") // true
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#3 Enumerated iteration](https://twitter.com/szubyak/status/951039299759362048)

Use `enumerated` when you iterate over the collection to return a sequence of pairs `(n, c)`, where `n` - index for each element and `c` - its value 👨‍💻💻
```swift
for (n, c) in "Swift".enumerated() {
    print("\(n): \(c)")
}
```
Result:

```
0: S
1: w
2: i
3: f
4: t
```
Also be careful with this tricky thing, `enumerated` on collection will not provide actual indices, but monotonically increasing integer, which happens to be the same as the index for Array but not for anything else, especially slices.

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#4 Combinations of pure functions](https://twitter.com/szubyak/status/953993641391017984)

`flatMap` func is effectively the combination of using `map` and `joined` in a single call, in that order. It maps items in array A into array B using a func you provide, then joins the results using concatenation.

Functions `min` and `max` could be also combinations of  `sorted.first` and `sorted.last` in single call.

```swift
let colors = ["red", "blue", "black", "white"]

let min = colors.min() // black
let first = colors.sorted().first // black

let max = colors.max() // white
let last = colors.sorted().last // white
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#5 Profit to compiler](https://twitter.com/szubyak/status/954329152160780288)

Do you know that using `map` gives profit to the compiler: it's now clear we want to apply some code to every item in an array, then like in `for` loop we could have `break` on halfway through.

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#6 Tips for writing error messages](https://twitter.com/szubyak/status/955054366419013632)

1) Say what happened and why
2) Suggest a next step
3) Find the right tone (If it’s a stressful or serious issue, then a silly tone would be inappropriate)

[Common​ ​Types​ ​of​ ​Error​ ​Messages](../master/Sources/6/Common​​Error​​Messages.pdf)

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#7 Testing settings](https://twitter.com/szubyak/status/955468985121886208)

1) Even if you don't write UI Tests, they still take considerable amount of time to run. Just skip it.
2) Enable code coverage stats in Xcode, it helps to find which method was tested, not tested, partly tested. But don’t pay too much attention to the percentage 😊.

![](../master/Sources/7/img.jpeg)

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#8 `forEach` and `map` execution order difference](https://twitter.com/szubyak/status/956538549444186112)

Execution order is interesting difference between `forEach` and `map`: `forEach` is guaranteed to go through array elements in its sequence, while `map` is free to go in any order.

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#9 Change type of items in array](https://twitter.com/szubyak/status/956544356370059265)

Two ways of changing type of items in array and obvious difference between them 🧐👨‍💻

```swift
let numbers = ["1", "2", "3", "4", "notInt"]
let mapNumbers = numbers.map { Int($0) }  // [Optional(1), Optional(2), Optional(3), Optional(4), nil]
let flatNumbers = numbers.flatMap { Int($0) } // [1, 2, 3, 4]
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#10 Invoke `didSet` when property’s value is set inside `init` context](https://twitter.com/szubyak/status/961707655105531905)

Apple's docs specify that: "Property observers are only called when the property’s value is set outside of initialization context."

`defer` can change situation 😊

```swift
class AA {
    var propertyAA: String! {
        didSet {
            print("Function: \(#function)")
        }
    }

    init(propertyAA: String) {
        self.propertyAA = propertyAA
    }
}

class BB {
    var propertyBB: String! {
        didSet {
            print("Function: \(#function)")
        }
    }

    init(propertyBB: String) {
        defer {
            self.propertyBB = propertyBB
        }
    }
}

let aa = AA(propertyAA: "aa")
let bb = BB(propertyBB: "bb")
```
Result:

```
Function: propertyBB
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#11 Fake AppDelegate](https://twitter.com/szubyak/status/963013740265385984)

Unit testing shouldn’t have any side effects. While running tests, Xcode firstly launches app and thus having the side effect of executing any code we may have in our App Delegate and initial View Controller. Fake AppDelegate in your `main.swift` to prevent it.

You can find `main.swift` file [here](../master/Sources/11/main.swift)

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#12 Semicolons in Swift](https://twitter.com/szubyak/status/963144748004454400)

Do you need semicolons in Swift ? Short answer is NO, but you can use them and it will give you interesting opportunity. Semicolons enable you to join related components into a single line.

```swift
func sum(a: Int, b: Int) -> Int {
    let sum = a + b; return sum
}
```

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#13 Group objects by property](https://twitter.com/szubyak/status/966248375988490240)

One more useful extension 🔨💻 Gives you opportunity to group objects by property 👨‍💻🧐

```swift
extension Sequence {
    func group<GroupingType: Hashable>(by key: (Iterator.Element) -> GroupingType) -> [[Iterator.Element]] {
        var groups: [GroupingType: [Iterator.Element]] = [:]
        var groupsOrder: [GroupingType] = []
        forEach { element in
            let key = key(element)
            if case nil = groups[key]?.append(element) {
                groups[key] = [element]
                groupsOrder.append(key)
            }
        }
        return groupsOrder.map { groups[$0]! }
    }
}
```

Usage:

```swift
struct Person {
    var name: String
    var age: Int
}

let mike = Person(name: "Mike", age: 18)
let john = Person(name: "John", age: 18)
let bob = Person(name: "Bob", age: 56)
let jake = Person(name: "Jake", age: 56)
let roman = Person(name: "Roman", age: 25)

let persons = [mike, john, bob, jake, roman]

let groupedPersons = persons.group { $0.age }

for persons in groupedPersons {
    print(persons.map { $0.name })
}
```

Result:

```
["Mike", "John"]
["Bob", "Jake"]
["Roman"]
```

Also in-box [alternative](https://developer.apple.com/documentation/swift/dictionary/2919592-init)

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)
