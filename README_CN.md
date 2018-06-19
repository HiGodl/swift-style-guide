郑重声明：
如果看不懂，估计是我翻译的不好
233333

哪里不对或者不准确的，（我把原文贴上来了，点击左侧三角形就能展开用来对照 2015-12-13）
若能指出，感激不尽~~~

如果没能及时更新，可能比较忙，或者比较懒 →_→
翻译后可以 email 或 `pull request`

----

<h1><details>
<summary>Swift 编码规范</summary>
A guide to our Swift style and conventions.
</details></h1>

<details><summary>按大概的先后顺序，本文尝试阐述下几点：</summary>
This is an attempt to encourage patterns that accomplish the following goals (in
rough priority order):
</details>

1. <details><summary>增强严谨性，减少程序员犯错的可能</summary>Increased rigor, and decreased likelihood of programmer error</details>
1. <details><summary>明确意图</summary>Increased clarity of intent</details>
1. <details><summary>减少冗余</summary>Reduced verbosity</details>
1. <details><summary>减少关于代码美观性的争论</summary>Fewer debates about aesthetics</details>

<details>
<summary>如果你有什么建议，请参考我们的 <a href="./CONTRIBUTING.md">贡献导引</a>，然后开个 <code>pull request</code>. :zap:
</summary>

If you have suggestions, please see our [contribution guidelines](CONTRIBUTING.md),
then open a pull request. :zap:
</details>

----

<h3><details><summary>留空</summary>Whitespace</details></h3>

<ul>
<li><details><summary>代码缩进用Tab代替空格</summary>Tabs, not spaces.</details></li>
<li><details><summary>文件末尾留一空行</summary>End files with a newline.</details></li>
<li><details><summary>用空行把代码分割成合理的块</summary>Make liberal use of vertical whitespace to divide code into logical chunks.</details></li>
<li><details><summary>行尾不要留任何空白字符</summary>Don’t leave trailing whitespace.</details>
<li><details><summary>空行不要有缩进</summary>Not even leading indentation on blank lines.</details></li>
</li>
</ul>

<h3><details>
<summary>尽量用 <code>let</code> 代替 <code>var</code></summary>

Prefer `let`-bindings over `var`-bindings wherever possible
</details></h3>

<details>
<summary>
尽量用 <code>let foo = ...</code> 代替 <code>var foo = ...</code> （包括不确定的时候）。到万不得已的时候，再用 <code>var</code> （也就是说：你 <i>知道</i> 这个值会改变，比如：有 <code>weak</code> 修饰的存储变量）。
</summary>

Use `let foo = …` over `var foo = …` wherever possible (and when in doubt). Only use `var` if you absolutely have to (i.e. you *know* that the value might change, e.g. when using the `weak` storage modifier).
</details>

<details>
<summary>
<i>理由：</i> 这两个关键字无论意图还是意义都很明确，但是 <code>let</code> 可以使代码更安全更清晰。
</summary>

_Rationale:_ The intent and meaning of both keywords are clear, but *let-by-default* results in safer and clearer code.
</details>

<br />

<details>
<summary>
<code>let</code> 保证它的值的永远不会变，对程序猿来说也是个 <i>清晰的标记</i>。因此之后的代码可以根据它的使用做出明确的推断。</summary>

A `let`-binding guarantees and *clearly signals to the programmer* that its value will never change. Subsequent code can thus make stronger assumptions about its usage.
</details>

<details>
<summary>
代码理解上也更容易。一旦使用了<code>var</code>，在之后用到时需要推测这个值会不会变，并且需要手动进行检查。
</summary>

It becomes easier to reason about code. Had you used `var` while still making the assumption that the value never changed, you would have to manually check that.
</details>

<details>
<summary>
相应地，任何时候看到 <code>var</code>，就假设它会变，并问自己为什么它会变。
</summary>

Accordingly, whenever you see a `var` identifier being used, assume that it will change and ask yourself why.
</details>

<h3><details><summary>
尽早地 <code>return</code> 或者 <code>break</code></summary>

Return and break early</details></h3>

<details>
<summary>当你某些操作需要通过条件判断去执行时，应当尽早地退出判断条件：避免下面这种写法</summary>

When you have to meet certain criteria to continue execution, try to exit early. So, instead of this:
</details>

```swift
if n.isNumber {
	// Use n here
} else {
	return
}
```

<details><summary>用这种写法代替：</summary>use this:</details>

```swift
guard n.isNumber else {
	return
}
// Use n here
```

<details>
<summary>当然可以使用 <code>if</code> 声明，但是更推荐用 <code>guard</code></summary>

You can also do it with `if` statement, but using `guard` is prefered
</details>

<details>
<summary><i>理由：</i> 你一但声明 <code>guard</code> 编译器会强制要求和 <code>return</code>, <code>break</code> 或者 <code>continue</code> 一起搭配使用，否则会产生编译时的错误。</summary>

because `guard` statement without `return`, `break` or `continue` produces a compile-time error, so exit is guaranteed.
</details>

<h3><details><summary>避免对可选类型强解包</summary>Avoid Using Force-Unwrapping of Optionals</details></h3>

<details><summary>如果有一个 <code>FooType?</code> 类型或 <code>FooType!</code> 类型的 <code>foo</code>，尽量不要强行解包（<code>foo!</code>）以得到关联值。</summary>

If you have an identifier `foo` of type `FooType?` or `FooType!`, don't force-unwrap it to get to the underlying value (`foo!`) if possible.
</details>

<details><summary>推荐用下面的做法代替：</summary>Instead, prefer this:</details>

```swift
if let foo = foo {
	// Use unwrapped `foo` value in here
} else {
	// If appropriate, handle the case where the optional is nil
}
```

<details><summary>或者使用可选链式调用，比如：</summary>

Alternatively, you might want to use Swift's Optional Chaining in some of these cases, such as:
</details>

```swift
// Call the function if `foo` is not nil. If `foo` is nil, ignore we ever tried to make the call
foo?.callSomethingIfFooIsNotNil()
```

<details><summary><i>理由：</i> <code>if let</code> 绑定可选类型使得代码更安全，强行解包可能导致运行时崩溃。</summary>

_Rationale:_ Explicit `if let`-binding of optionals results in safer code. Force unwrapping is more prone to lead to runtime crashes.
</details>

<h3><details><summary>避免隐式解包的可选类型</summary>Avoid Using Implicitly Unwrapped Optionals</details></h3>

<details><summary>如果 foo 可能为 <code>nil</code> ，尽可能的用 <code>let foo: FooType?</code> 代替 <code>let foo: FooType!</code>（注意：一般情况下，<code>?</code> 可以代替 <code>!</code>）</summary>

Where possible, use `let foo: FooType?` instead of `let foo: FooType!` if `foo` may be nil (Note that in general, `?` can be used instead of `!`).
</details>

<details><summary><i>理由：</i> 明确的可选类型使得代码更安全。隐式解包的可选类型也可能会挂掉。</summary>

_Rationale:_ Explicit optionals result in safer code. Implicitly unwrapped optionals have the potential of crashing at runtime.
</details>

<h3><details><summary>对于只读属性和 <code>subscript</code>，选用隐式的 getters 方法</summary>

Prefer implicit getters on read-only properties and subscripts
</details></h3>

<details><summary>如果可以，省略只读属性和 <code>subscript</code> 的 <code>get</code> 关键字</summary>

When possible, omit the `get` keyword on read-only computed properties and
read-only subscripts.</details>

<details><summary>所以应该这样写：</summary>So, write these:</details>

```swift
var myGreatProperty: Int {
	return 4
}

subscript(index: Int) -> T {
	return objects[index]
}
```

<details><summary>……而不是：</summary>… not these:</details>

```swift
var myGreatProperty: Int {
	get {
		return 4
	}
}

subscript(index: Int) -> T {
	get {
		return objects[index]
	}
}
```

<details><summary><i>理由：</i> 第一个版本的代码意图已经很清楚了，并且用了更少的代码</summary>

_Rationale:_ The intent and meaning of the first version are clear, and results in less code.
</details>

<h3><details><summary>对于顶级定义，永远明确的列出权限控制</summary>Always specify access control explicitly for top-level definitions</details></h3>

<details><summary>顶级函数，类型和变量，需要有详尽的权限控制说明符</summary>

Top-level functions, types, and variables should always have explicit access control specifiers:
</details>

```swift
public var whoopsGlobalState: Int
internal struct TheFez {}
private func doTheThings(things: [Thing]) {}
```

<details><summary>然而在这些函数/类型的内部，可以在合适的地方使用隐式权限控制：</summary>However, definitions within those can leave access control implicit, where appropriate:
</details>

```swift
internal struct TheFez {
	var owner: Person = Joshaber()
}
```

<details><summary><i>理由：</i> 很少有合适的场景可以将顶级定义指定为 <code>internal</code> ，这样做的时候要确保经过了仔细的判断。在定义的内部重用同样的权限控制说明符就显得多余，而且默认的通常是合理的。</summary>

_Rationale:_ It's rarely appropriate for top-level definitions to be specifically `internal`, and being explicit ensures that careful thought goes into that decision. Within a definition, reusing the same access control specifier is just duplicative, and the default is usually reasonable.
</details>

<h3><details><summary>当指定一个类型时，把冒号和标识符连在一起</summary>

When specifying a type, always associate the colon with the identifier
</details></h3>

<details><summary>当指定标示符的类型时，冒号要紧跟着标示符，然后空一格再写类型</summary>

When specifying the type of an identifier, always put the colon immediately
after the identifier, followed by a space and then the type name.
</details>

```swift
class SmallBatchSustainableFairtrade: Coffee { ... }

let timeToCoffee: NSTimeInterval = 2

func makeCoffee(type: CoffeeType) -> Coffee { ... }
```

<details><summary><i>理由：</i> 类型区分号是用来描述标识符的信息的，所以要跟它连在一起。</summary>

_Rationale:_ The type specifier is saying something about the _identifier_ so
it should be positioned with it.
</details>

<br />

<details><summary>此外，指定字典类型时，键类型后紧跟着冒号，接着加一个空格，之后才是值类型。</summary>

Also, when specifying the type of a dictionary, always put the colon immediately after the key type, followed by a space and then the value type.
</details>

```swift
let capitals: [Country: City] = [sweden: stockholm]
```

<h3><details><summary>需要时才写上 <code>self</code></summary>

Only explicitly refer to `self` when required
</details></h3>

<details><summary>当调用 <code>self</code> 的属性或方法时，默认隐式引用<code>self</code>：</summary>

When accessing properties or methods on `self`, leave the reference to `self` implicit by default:
</details>

```swift
private class History {
	var events: [Event]

	func rewrite() {
		events = []
	}
}
```

<details><summary>必要的时候再加上 <code>self</code>, 比如在闭包里，或者参数名冲突的情况下：</summary>

Only include the explicit keyword when required by the language—for example, in a closure, or when parameter names conflict:
</details>

```swift
extension History {
	init(events: [Event]) {
		self.events = events
	}

	var whenVictorious: () -> () {
		return {
			self.rewrite()
		}
	}
}
```

<details><summary><i>原因:</i> 在闭包里用 <code>self</code> 更加凸显它捕获 <code>self</code> 的语义，别处避免了冗长</summary>

_Rationale:_ This makes the capturing semantics of `self` stand out more in closures, and avoids verbosity elsewhere.
</details>

<h3><details><summary>首选 <code>struct</code> 而非 <code>class</code></summary>

Prefer structs over classes
</details></h3>

<details><summary>除非需要 <code>class</code> 才能提供的功能（比如 identity 或 <code>deinit</code>ializers），否则尽量用 <code>struct</code></summary>

Unless you require functionality that can only be provided by a class (like identity or deinitializers), implement a struct instead.
</details>

<details><summary>要注意到继承通常 <strong>不</strong> 是用类的理由，因为多态可以通过协议实现，重用可以通过组合实现。</summary>

Note that inheritance is (by itself) usually _not_ a good reason to use classes, because polymorphism can be provided by protocols, and implementation reuse can be provided through composition.
</details>

<details><summary>比如，这个类的分级</summary>

For example, this class hierarchy:
</details>

```swift
class Vehicle {
	let numberOfWheels: Int

	init(numberOfWheels: Int) {
		self.numberOfWheels = numberOfWheels
	}

	func maximumTotalTirePressure(pressurePerWheel: Float) -> Float {
		return pressurePerWheel * Float(numberOfWheels)
	}
}

class Bicycle: Vehicle {
	init() {
		super.init(numberOfWheels: 2)
	}
}

class Car: Vehicle {
	init() {
		super.init(numberOfWheels: 4)
	}
}
```

<details><summary>可以重构成下面这样：</summary>

could be refactored into these definitions:
</details>

```swift
protocol Vehicle {
	var numberOfWheels: Int { get }
}

func maximumTotalTirePressure(vehicle: Vehicle, pressurePerWheel: Float) -> Float {
	return pressurePerWheel * Float(vehicle.numberOfWheels)
}

struct Bicycle: Vehicle {
	let numberOfWheels = 2
}

struct Car: Vehicle {
	let numberOfWheels = 4
}
```

<details><summary><i>理由：</i> 值类型更简单，容易分析，并且 <code>let</code> 关键字的行为符合预期。</summary>

_Rationale:_ Value types are simpler, easier to reason about, and behave as expected with the `let` keyword.
</details>

<h3><details><summary>默认 <code>class</code> 为 <code>final</code></summary>

Make classes `final` by default
</details></h3>

<details><summary><code>class</code> 应该用 <code>final</code> 修饰，并且只有在继承的有效需求已被确定时候才能去使用子类。即便在这种情况（前面提到的使用继承的情况）下，根据同样的规则（<code>class</code> 应该用 <code>final</code> 修饰的规则），类中的定义（属性和方法等）也要尽可能的用 <code>final</code> 来修饰
</summary>

Classes should start as `final`, and only be changed to allow subclassing if a valid need for inheritance has been identified. Even in that case, as many definitions as possible _within_ the class should be `final` as well, following the same rules.
</details>

<details><summary><i>理由：</i> 组合通常比继承更合适，选择使用继承则很可能意味着在做出决定时需要更多的思考。</summary>

_Rationale:_ Composition is usually preferable to inheritance, and opting _in_ to inheritance hopefully means that more thought will be put into the decision.
</details>

<h3><details><summary>能不写类型参数的尽量省略</summary>

Omit type parameters where possible
</details></h3>

<details><summary>当对接收者来说一样时，参数化类型的方法可以省略接收者的类型参数。比如：</summary>

Methods of parameterized types can omit type parameters on the receiving type when they’re identical to the receiver’s. For example:
</details>

```swift
struct Composite<T> {
	…
	func compose(other: Composite<T>) -> Composite<T> {
		return Composite<T>(self, other)
	}
}
```

<details><summary>可以改成这样：</summary>could be rendered as:</details>

```swift
struct Composite<T> {
	…
	func compose(other: Composite) -> Composite {
		return Composite(self, other)
	}
}
```

<details><summary><i>理由：</i> 省略多余的类型参数让意图更清晰，并且通过对比，让返回值为不同的类型参数的情况也清楚了很多。</summary>

_Rationale:_ Omitting redundant type parameters clarifies the intent, and makes it obvious by contrast when the returned type takes different type parameters.
</details>

<h3><details><summary>定义操作符 两边留空格</summary>

Use whitespace around operator definitions
</details></h3>

<details><summary>当定义操作符时，两边留空格。不要这样：</summary>

Use whitespace around operators when defining them. Instead of:
</details>

```swift
func <|(lhs: Int, rhs: Int) -> Int
func <|<<A>(lhs: A, rhs: A) -> A
```

<details><summary>应该这样写：</summary>write:</details>

```swift
func <| (lhs: Int, rhs: Int) -> Int
func <|< <A>(lhs: A, rhs: A) -> A
```

<details><summary><i>理由：</i> 操作符由标点字符组成，如果后面直接跟随类型或者参数值，会让代码非常难读。加上空格分开他们就清晰了</summary>

_Rationale:_ Operators consist of punctuation characters, which can make them difficult to read when immediately followed by the punctuation for a type or value parameter list. Adding whitespace separates the two more clearly.
</details>

### 其他语言

* [English Version](https://github.com/github/swift-style-guide)
* [日本語版](https://github.com/jarinosuke/swift-style-guide/blob/master/README_JP.md)
* [한국어판](https://github.com/minsOne/swift-style-guide/blob/master/README_KR.md)
* [Versión en Español](https://github.com/antoniosejas/swift-style-guide/blob/spanish/README-ES.md)
* [Versão em Português do Brasil](https://github.com/fernandocastor/swift-style-guide/blob/master/README-PTBR.md)
* [فارسی](https://github.com/mohpor/swift-style-guide/blob/Persian/README-FA.md)


