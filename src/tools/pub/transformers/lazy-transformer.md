---
layout: default
title: "Writing a Lazy Transformer"
description: "How to write a Pub transformer that runs lazily to improve your app's startup time."
permalink: /tools/pub/transformers/lazy-transformer
---

If you have a transformer that runs slowly&mdash;perhaps because the algorithm
is complex, has many steps, or the data set is large&mdash;you can improve
your app's startup time during the development cycle by making the
transformer <em>lazy</em>.
Pub runs a lazy transformer only when the generated asset is requested,
either by a user or by another transformer.

You can make either a standard transformer or an aggregate transformer run
lazily. This document describes how to create a lazy transformer using a
normal Pub transformer,
but the steps are similar for creating a lazy aggregate transformer.

Most of the steps for writing a lazy transformer are the same
as for writing a normal transformer. This document
only describes the differences specific to lazy transformers.
If you aren't familiar with how to write a normal Pub tranformer, see
[Writing a Pub Transformer](/tools/pub/transformers).

This page uses the lazy_transformer example which you can find
through [Examples of Transformer Code](/tools/pub/transformer/examples).

Note that this transformer example does not run slowly, but is used
for purposes of illustration. If the transformer were passed an
extremely large data set&mdash;the entire contents of the Library of Congress,
for example&mdash;laziness would greatly improve the app's startup time.

## Implementing a lazy transformer

A lazy transformer implements [LazyTransformer][]
from the [barback][] package.

[LazyTransformer]: http://www.dartdocs.org/documentation/barback/0.15.0+1/index.html#barback/barback.LazyTransformer
[barback]: https://pub.dartlang.org/packages/barback

### Implement `LazyTransformer`

In the Dart file with your transformer subclass,
extend the `Transformer` class from the barback package,
and implement `LazyTransformer`:

{% prettify dart %}
class CodedMessageConverter extends Transformer
                            implements LazyTransformer {
{% endprettify %}

### Implement `declareOutputs`

You must implement the only method in the `LazyTransformer` class,
[declareOutputs][], to declare the outputs generated by the
transformer. This enables Pub to determine what assets exist without
needing to immediately run the transformer.

The `declareOutputs` method is passed a `DeclaringTransform` object,
which gives the transformer access to the primary input's id.
For example:

{% prettify dart %}
void declareOutputs(DeclaringTransform transform) {
  transform.declareOutput(transform.primaryId
                                   .changeExtension('.shhhhh'));
}
{% endprettify %}

[declareOutputs]: http://www.dartdocs.org/documentation/barback/0.15.0+1/index.html#barback/barback.LazyTransformer@id_declareOutputs

That's all that you need to do!

## More information

* [Writing a Pub Transformer](/tools/pub/transformers/)
: How to write a Pub transformer that accepts a single primary input.
* [Examples of Transformer Code](/tools/pub/transformer/examples)
: Examples to get you started.
* [barback library](https://pub.dartlang.org/packages/barback)
: API docs for the barback package.
* [Barback - Can We Build It? Yes, We Can!](https://docs.google.com/a/google.com/document/d/1juHkCRg-1YH6LvwhGPHgF2ihX-UQtR1fv-8aknO7t_4/edit?pli=1#)
: A description of the barback asset system, written by a
member of the Dart engineering team.
