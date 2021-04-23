---
layout: post
title: Stop Being So Negative 
subtitle: Ruby is Too Expressive to Warrant the use of Negatives 
readtime: true
cover-img: '/assets/img/ruby_logo.png'
categories: []
tags: []
---

# Stop Being So Negative 

My brain hurts and I cringe a bit whenever I stumble across an unnecessarily complicated negative condition. Also, to be perfectly clear and honest, I never have written anything that remotely resembles any of these examples. Why would I say I've written terrible code when I've never written terrible code?

Let me show you what I mean:

```ruby
# Is the collection NOT empty?
if !collection.empty?
```

or
```ruby
# Is the thing NOT nil?
if !thing.nil?
```

Worse still is when a negative condition is paired with an `unless`.

```ruby
# UNLESS the collection is NOT empty, do the next thing.
unless !collection.empty?
```

Or the universally agreed upon most heinous double negative in Ruby:

```ruby
# UNLESS the thing is NOT nil, do the next thing.
unless !thing.nil?
```

The point here is that for some reason negative logic always seems tougher to understand than its positively phrased counterpart.

-----

# Ruby is Too expressive to Warrant the use of Negatives. 

The human brain evolved hundreds of thousands of years ago to be good at a great many things, but tracking negative logic is not one of them. At least that's the case for my particular human brain.

Ruby is so expressive, we should never have to write tough to read nonsense code like `if !collection.nil?`.

Instead we should be writing our conditions with a positive voice.

A collection not being empty is the same as saying a collection with at least one element. A thing not being nothing is the same as saying a thing that exists.

**Bad**

```ruby
if !collection.empty?
```

**Good**

```ruby
# present is a rails-ism
if collection.present?
```

What if we are not using Rails? Well Ruby has some non-negative tools for us as well!

```ruby
irb(main):001:1* collection = []
=> []
# Is there at least one element in our collection that is not 
# `nil` or `false`? In other, clearer, words are any of the
# elements in our collection truthy?
irb(main):002:0> collection.any?
=> false
```

If our goal is to run some code when a collection is not empty, `any?` is a pretty good choice.

```ruby
irb(main):001:0> collection = ['hello, world!']
=> ["hello, world!"]
irb(main):002:0> collection.any?
=> true
```

There can be some weirdness, however.

```ruby
irb(main):006:0> collection = [nil]
=> [nil]
irb(main):007:0> collection.any?
=> false
```

I think `present?` in Rails behaves mostly how I expect it to, so that ends up being the tool I reach for most often especially because I am almost always writing Ruby in a Rails context.

```ruby
[1] pry(main)> collection = []
=> []
[2] pry(main)> collection.present?
=> false
[3] pry(main)> collection = ['hello, world!']
=> ["hello, world!"]
[4] pry(main)> collection.present?
=> true

# Careful now. But, this kind of makes sense. There is something
# in the collection after all. That something just happens to be nothing.
[5] pry(main)> collection = [nil]
=> [nil]
[6] pry(main)> collection.present?
=> true
# :mindblown:
```

Let's look at our `nil?` negative check. What are we really saying when we ask is the `thing` NOT `nil?`? We are really asking, does `thing` exist?

**Bad**

```ruby
if !thing.nil?
```

**Good**

```ruby
if thing
```

If thing can be `nil`, `true`, or `false` and we only want to proceed if `thing` is `true` or `false`, we should try:

```ruby
# Again, stick to the positives.
if [true, false].include? thing
```

In my experience, this could be filed in the edge case category as typically checking for falsey values is good enough.

-----

# The only Acceptable Use for Unless

There is only one acceptable use for unless and that is as a guard statement at the very start of a method. It may not use any compound, multi-clause logic. Nor may it be placed anywhere but the very first line of a method because we do not hate our colleagues.

**Bad**

```ruby
unless condition
  ...
end
```

**Also Bad**

```ruby
def foo
  # some code
  ...

  unless something_happened_in_the_above_code 

  end
end
```

**The Worst**

```ruby
unless !condition
  ...
end
```

**Good**

```ruby
def foo
  return unless condition

  # mandatory empty line as a sign of humility.
  ...
  # remaining implementation that should only run if the condition is truthy.
end
```

Let's make our code bases more positive places for all involved. Keep the negativity to a minimum.
