---
layout: post
title:  "Swift World: What’s new in iOS 11 — Natural Language Processing"
date:   2017-06-19 20:00:00
categories: tutorial
---

I have introduced Core ML for general usage and Vision framework for image analysis with following articles.

* [Swift World: What’s new in iOS 11 — Core ML – SwiftWorld – Medium](https://medium.com/@NilStack/whats-new-in-ios-11-core-ml-44555927d750)
* [Swift World: What’s new in iOS 11 — Vision – CompileSwift – Medium](https://medium.com/@NilStack/swift-world-whats-new-in-ios-11-vision-456ba4156bad)

In this one, I will show you natural language processing in iOS 11. The main API is NSLinguisticTagger which is far more than a tagger. It’s another specific area to apply machine learning. In iOS 11, NSLinguisticTagger becomes more powerful. That’s why we involve it in this series although it’s not new.

In codes with NSLinguisticTagger, we use different tag schemes and options to analyze text in different ways. Tag scheme describes the result we want by analyzing the text. Tag options are items we want to omit like punctuation and whitespace. I will introduce tag scheme, options and their combination in the following examples.

To use NSLinguisticTagger, we first initialize an instance with tag schemes and further define the behavior with options for each scheme.

<script src="https://gist.github.com/NilStack/6ce3c3c0d35eb7c98ded504d557b9d89.js"></script>

Feed the tagger with text

<script src="https://gist.github.com/NilStack/2e88283a3666ba5d7dd66781be509086.js"></script>

Enumerate each tag and handle  the result

<script src="https://gist.github.com/NilStack/d63971b197ec1ffe741c1060e8a0c408.js"></script>

Let’s see what NSLinguisticTagger can do with examples. Please copy the code snippets to playground to get the results.

### Language Identification

The scheme here is NSLinguisticTagScheme.language. NSLinguisticTagger analyzes the text to get the dominant language.

<script src="https://gist.github.com/NilStack/62792bf293429d641d109dea1c563cd8.js"></script>

### Tokenization

With the tag scheme NSLinguisticTagScheme.tokenType, we will get the type of every token. The punctuations and whitespaces are omitted with omitPunctuation and omitWhitespace. Please try different options to see different results.

<script src="https://gist.github.com/NilStack/248a716b10b81ec5c429652df3ec63cf.js"></script>

### Lemmatization

With tag scheme NSLinguisticTagScheme.lemma, NSLinguisticTagger gives us stem form of each word token.

<script src="https://gist.github.com/NilStack/ac0a85940e65094bc6d403d32e6df36b.js"></script>

### NameType

NSLinguisticTagger with the scheme nameType helps us identify whether the token is named entity like personal name and place name.  

<script src="https://gist.github.com/NilStack/6c0cea83065e6b735c91f8b7cac89e19.js"></script>

### LexicalClass

To get each token’s lexical class, we use  NSLinguisticTagScheme. lexicalClass.

<script src="https://gist.github.com/NilStack/69ccc0ce6cfc7672cbc267e1037ccfaa.js"></script>

That’s all for NSLinguisticTagger. Thanks for your time.
