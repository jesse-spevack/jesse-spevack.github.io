---
layout: post
title: How I use a Hash Instead of Case Statement in Ruby 
subtitle: Case Statements are Yesterday's News. Impress your friends with Hashes instead. 
readtime: true
cover-img: '/assets/img/ruby_logo.png'
categories: []
tags: []
---

# Case Statements are so 2020

#### Spice up 2021 with a Hash instead.

I wanted to share a technique I reach for in my Ruby toolkit when I find myself in a situation that calls for some sort of case statement.

Let's imagine our codebase has an item. Let's pretend that there are two types of items, each requiring a slightly different processing. We might be tempted to write:

```ruby
case item
when Fancy
  FancyProcessor.call(item)
when Simple
  SimpleProcessor.call(item)
end
```

There is nothing wrong with this code. I just happen to prefer a different approach. Instead I like to define a hash that allows me to turn my case statement into a lookup.

```ruby
PROCESSOR_MAP = {
  Fancy => FancyProcessor,
  Simple => SimpleProcessor
}

processor = PROCESSOR_MAP[item.class]
processor.call(item)
```

Now, perhaps we need some default behavior for items that are neither simple nor fancy. With a case statement, we just add a final else clause.

```ruby
case item
when Fancy
  FancyProcessor.call(item)
when Simple
  SimpleProcessor.call(item)
else
  DefaultProcessor.call(item)
end
```

We can do this with our Hash as well by adding a default value to our hash:

```ruby
# Normally new keys in a hash get a value of `nil`, but we can overwrite this
# by using Hash.new and passing in our preferred default value.
PROCESSOR_MAP = Hash.new(DefaultProcessor)
PROCESSOR_MAP.merge({
  Fancy => FancyProcessor,
  Simple => SimpleProcessor
})

# Now if item.class is neither `Fancy` nor `Simple`
# We'll get the default `DefaultProcessor` we assigned as our hash's default value.
processor = PROCESSOR_MAP[item.class]
processor.call(item)
```

I prefer the hash for two reasons. I think it expresses the idea a bit more succinctly. I also know that very soon we will invent a new type of item and processor. When that happens it feels like updating the code is more of a configuration issue than adding new business logic. Here's what I might do:

```ruby
CONFIG = { 
  Fancy => FancyProcessor,
  Simple => SimpleProcessor,
  NewShiny => NewShinyProcessor
}

PROCESSOR_MAP = Hash.new(DefaultProcessor).merge(CONFIG)

PROCESSOR_MAP.fetch(item.class).call(item)
```

This is a contrived example and the result does not look great. The big picture here is what is important: It is fun and exciting to swap a case statement for a hash in Ruby.