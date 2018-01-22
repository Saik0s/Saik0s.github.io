---
title: iOS UI elements styling
layout: post
date: 2018-01-15 22:48
image: /assets/images/markdown.jpg
headerImage: false
tag:
- swift
category: blog
author: saik0s
description: Styling UI elements using swift extensions
# jemoji: '<img class="emoji" title=":ramen:" alt=":ramen:" src="https://assets.github.com/images/icons/emoji/unicode/1f35c.png" height="20" width="20" align="absmiddle">'
---

# Swift styling using extensions

One morning I was working on the new screen for the app and did notice that I have such a lot of elements on the screens that it is tough to manage them in the way I usually do it. I decided to separate styles from screens source files. Luckily I was using Swift language, and it has a compelling feature called **extensions**.

Let's separate our UI styles into several groups and create files for each of them. I am going to style AsyncDisplayKit(Texture) elements, but this approach can be easily applied to UIKit elements. Here is the result we are going to achieve:

```swift
button.stylize(as: .remove)
```

So, first of all, let's create Style.swift file to provide all UI elements with stylize method. My idea is to develop Stylizable protocol, extension ASDisplayNode with this protocol and extension this protocol with the public function called stylize. But this function needs to have an argument. Let's create a generic structure called Style which is going to store a closure with UI element modification code.

```swift
public struct Style<Node> {
	internal let closure: (Node) -> Node

	public init(_ closure: @escaping (Node) -> Node) {
		self.closure = closure
	}
}
```

Now we can pass this struct as an argument to our stylize method.

```swift
public protocol Stylizable: class {
}

extension ASDisplayNode: Stylizable {
}

extension Stylizable where Self: ASDisplayNode {
	@discardableResult
	public func stylize(as style: Style<Self>) -> Self {
		return style.closure(self)
	}
}
```

Everything is ready to create our first style for our button. Our button has an ASButtonNode type. Let's create a ButtonStyles.swift file and add our first style to it:

```swift
import AsyncDisplayKit

extension Style where Node == ASButtonNode {
	public static var remove: Style {
		return Style { node in
			node.setBackgroundImage(
					UIImage(named: "delete"),
					for: .normal
			)
			return node
		}
	}
}
```

Using such approach autocomplete system is going to show us only button styles for buttons. 

Have fun!
