![Logo](https://raw.githubusercontent.com/davidlari/Haring/master/Resources/HaringLogo.png)

Haring
=========

[![Version](https://img.shields.io/cocoapods/v/Haring.svg?style=flat)](http://cocoapods.org/pods/Haring)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![License](https://img.shields.io/cocoapods/l/Haring.svg?style=flat)](http://cocoapods.org/pods/Haring)
[![Platform](https://img.shields.io/cocoapods/p/Haring.svg?style=flat)](http://cocoapods.org/pods/Haring)

Haring is a customizable and extensible Markdown parser for iOS. It supports many of the standard Markdown elements through the use of Regular Expressions. It also allows customization of font and color attributes for all the Markdown elements.

Haring was forked from Ivan Bruel's MarkdownKit which appears to be no longer maintained.

Haring is named after PopArt artist [Keith Haring](https://en.wikipedia.org/wiki/Keith_Haring).

## Screenshot

![Example](https://raw.githubusercontent.com/ivanbruel/MarkdownKit/master/Resources/MarkdownKitExample.png)

## Installation

### Installation via CocoaPods

Haring is available through [CocoaPods](http://cocoapods.org). CocoaPods is a dependency manager that automates and simplifies the process of using 3rd-party libraries like Haring in your projects. You can install CocoaPods with the following command:

```ruby
gem install cocoapods
```

To integrate Haring into your Xcode project using CocoaPods, simply add the following line to your Podfile:

```ruby
pod "Haring"
```

Afterwards, run the following command:

```ruby
pod install
```

### Installation via Carthage

Haring is available through [Carthage](https://github.com/Carthage/Carthage). Carthage is a decentralized dependency manager that builds your dependencies and provides you with binary frameworks.

You can install Carthage via [Homebrew](http://brew.sh) with the following command:

```ruby
brew update
brew install carthage
```

To integrate Haring into your Xcode project using Carthage, simply add the following line to your Cartfile:

```ruby
github "davidlari/Haring"
```

Afterwards, run the following command:

```ruby
carthage update
```

## Supported Elements

```
*italic* or _italics_
**bold** or __bold__

# Header 1
## Header 2
### Header 3
#### Header 4
##### Header 5
###### Header 6

> Quote

* List
- List
+ List

`code` or ```code```
[Links](http://github.com/davidlari/Haring/)
```

## Usage

In order to use Haring to transform Markdown into NSAttributedString, all you have to do is create an instance of `MarkdownParser` and call the `parse(_)` function.

```swift
let markdownParser = MarkdownParser()
let markdown = "I support a *lot* of custom Markdown **Elements**, even `code`!"
label.attributedText = markdownParser.parse(markdown)
```

## Customization

```swift
let markdownParser = MarkdownParser(font: UIFont.systemFontOfSize(18))
markdownParser.automaticLinkDetectionEnabled = false
markdownParser.bold.color = UIColor.redColor()
markdownParser.italic.font = UIFont.italicSystemFontOfSize(300)
markdownParser.header.fontIncrease = 4
```

## Extensibility

To add new Markdown elements all you have to do is implement the `MarkdownElement` protocol (or descendants) and add it to the `MarkdownParser`.

```swift
import Haring

class MarkdownSubreddit: MarkdownLink {

  private static let regex = "(^|\\s|\\W)(/?r/(\\w+)/?)"

  override var regex: String {
    return MarkdownSubreddit.regex
  }

  override func match(match: NSTextCheckingResult,
                             attributedString: NSMutableAttributedString) {
    let subredditName = attributedString.attributedSubstringFromRange(match.rangeAtIndex(3)).string
    let linkURLString = "http://reddit.com/r/\(subredditName)"
    formatText(attributedString, range: match.range, link: linkURLString)
    addAttributes(attributedString, range: match.range, link: linkURLString)
  }

}
```

```swift
let markdownParser = MarkdownParser(customElements: [MarkdownSubreddit()])
let markdown = "**/r/iosprogramming** can be *markdown* as well!"
label.attributedText = markdownParser.parse(markdown)
```

## Example

To run the example project, clone the repo, and run `pod install` from the Example directory first.

## Acknowledgements

Haring was derived mostly from MarkdownKit by Ivan Bruel. It was created due to the apparent abandonment of MarkdownKit.

MarkdownKit was heavily inspired by [TSMarkdownParser](https://github.com/laptobbe/TSMarkdownParser) and also [SwiftyMarkdown](https://github.com/SimonFairbairn/SwiftyMarkdown).

Special thanks to [Michael Brown](https://github.com/mluisbrown) for helping out with the [UTF-16 Escaping/Unescaping](https://github.com/ivanbruel/Haring/blob/master/Haring/Classes/Extensions/String%2BUTF16.swift).

## License

Haring is available under the MIT license. See the LICENSE file for more info.
