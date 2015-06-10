# An Intro to Implementing Webhooks in Ruby

With Real Examples, Too!
------


# Who am I?

## Kyle Tolle

[kyletolle.com](http://kyletolle.com) |
[@kyletolle](https://twitter.com/kyletolle)

Software Engineer at [Spatial Networks](http://spatialhetworks.com)

Implemented webhooks in [Fulcrum](http://fulcrumapp.com), our mobile data
collection service

___
Ours have been in operation for about a year and a half. I wanted to share
what I've learned. Maybe you'll find it useful or interesting. And I would
love to get your feedback as well.
------


# First off

Ask questions at any time!
___
If I don't know the answer, I can always ignore you!
------


## Slides for this talk

### Presentation:

[https://kyletolle.github.io/ruby_webhooks_intro](https://kyletolle.github.io/ruby_webhooks_intro)

### Code:

[https://github.com/kyletolle/ruby_webhooks_intro](https://github.com/kyletolle/ruby_webhooks_intro)
------


# Overview

- How clients use APIs
- Why webhooks are a thing
- Barebones webhooks demo
- Webhooks integration demo
- Other things we can do
- Recapping and wrapping up
------


## Our fictional client

Government Agency for Bear Attacks
___
This fictional organization will help make things a little more concrete.
Explain how they'll use Fulcrum.
------

### Vicious

![bear boxing attack](bear_boxing_attack.gif)
------

### Relentless

![cute bear attack](cute_bear_attack.gif)
------


### Unexpected

![eagle attacks bear](eagle_attacks_bear.gif)
------


## So I'm here

to talk about webhooks
------


## But first

we need some background
------


## Let's start with

what we know
------


# APIs
------


## Why do we have APIs?
___
Can you shout out some reasons for why we have APIs?
Those are all great ideas.
And I want to focus on one in particular.
------


## APIs allow

## people to get their data
------


## How do APIs work?

### The client calls us
___
When our clients want their data, they make a request to our servers.
The people in the office will use our API to get our their data on bear
attacks.
------


## Why are they really trying to get their data?

### To do things with it!
___
Their data is important. Taking action on it is valuable. Acting quickly gives
them an advantage.
They can share it with the public. Analyze it. Try to prevent more bear
attacks.
------


## How do they use the API?

- Call our API
- Frequently
- In a script/application
___
They write a script to hit our API every few days, or hours, or seconds
and do something with the data they get back.
------


This is

## polling
------


## Polling is straight-forward
___
And there are some upsides to it, which we won't get into here
------


But there are some

## Downsides
------


## #1

### Your client writes the code
------


### How long until

someone murders your servers?
___
They're likely to push the limits. Maybe they'll write an infinite loop which
hits your API as fast as it can.

Or, they'll have some "mission critical" need for ONE MILLION calls per month.
That's not a made up number. We had someone whose script really did this.

It's bad enough if one person does this, but imagine a few more people doing
the same thing. These legitimate requests can end up swamping your servers.
------


## #2

### To react faster, they have to poll more
------


### More polling means

## more resources

- development time
- support time
- processors
- memory
- bandwidth
___
For you, and for them. These costs add up, especially when you have more
customers.
------


## #3

### It's mostly useless
___
This isn't an existential crisis, I promise.
So what do I mean?
------


### Most polls won't return new data
___
The rate of their polling is constant.
They do it once every few seconds.
But the rate of their data changing is variable.
Maybe it only changes every couple days, or only during business hours.

Meaning, a majority of their polls won't return anything to act on.

They're hitting up yuor server all the time, and usually it's for nothing.
Talk about disappointing.
------


## #4

### And it's still not Real-Time™
___
Additionally, to approach acting in real-time, they have to poll more and
more frequently. Except that, even when polling faster, they won't have truly
Real-Time™ data.
------


## Okay

### we see the disadvantages
------


## But so what?

### We already have an API

- It exists
- It works
------


## Let's come back to what's important
------


### Fast response time

### is what's valuable

## &amp;

### Polling helps them react quickly

But it's only one approach
------


## How else can we help them react quickly?

Because, if we can do this...
------


### It'll make our service more valuable

And that means happier customers

Maybe even some extra revenue
------


We know that polling is

## Client --> Us
___
Client sends a request to us.
------


But what about the other way around?

## Us --> Client
___
What about us making a request to the client?
We can invert the relationship.
Flip the equation around.
------


## Hmm, this is interesting
------

### Instead of

## them calling us

to see if data changes

## We let them know

_when_ data has changed
------


## We do this over the web

since it's common infrastructure
___
No need to reinvent the wheel here.
------


## Client gives us a URL

## &amp;

## We have events we watch for

When an event happens, we make an HTTP POST request to our client's URL
------


# Boom, Webhooks!

![boom.gif](boom.gif)
------


## The Benefits
___
What are the benefits to this?
They're pretty much the opposite of the disadvantages of polling.
------


## #1

### We write the code
___
Our clients have better things to do than write the code for polling.
So we'll be the ones to write the code to send them their data.

We have control over how the sending works. So we can tailor the loads to
something we define as feasible.
------


## #2

### Fewer resources required
___
Because we're not making as many requests as we would be with polling.
Since we only send them data when something's happened, since we know when
something has happened.
We never make a request that has no data.
------


## #3

### We send the data just as it changes

This is Real-Time™
------


### Exciting for integration!

It's like Push Notifications for the web
------


## We've got our background!

### How about implementing it?
------


## Let's see a small example

### Show us making a single request

Start small &amp; build momentum
------


## Random Number Generator

    require_relative '../lib/webhooks'

    class RandomNumberGenerator
      include Webhooks

      def initialize
        self.webhooks = %w{http://localhost:9000}
      end

      def generate
        random_number = SecureRandom.random_number(1000)

        send_webhooks(:random_number, :generate, random_number)

        return random_number
      end
    end

------


### The Idea

- Generate a random number
- Send a webhook to our client with that number

We'll have a client server running, [Polis.rb](https://github.com/kyletolle/polis.rb)

A small web server which logs POST data
------


### In IRB

    require ./examples/random_number_generator.rb'
    > puts RandomNumberGenerator.new.generate
    => 863
------


### From the client

We see the webhook request

    $ PORT=9000 bf
    ...
    Logging POST request:
    ...
    Body:
    {"event":"random_number:generate","data":863}
    ...
___
I've omitted some of the output, to maintain focus on what' important: the
random number it received.
------


## Webhooks module

    require 'net/http'
    require 'securerandom'
    require 'json'

    module Webhooks
      attr_accessor :webhooks

      def send_webhooks(resource, action, data)
        event_type = "#{resource}:#{action}"
        payload    = JSON.generate({
          event: event_type,
          data:  data })

        webhooks.each do |webhook|
          url = URI.parse(webhook)

          Net::HTTP.start(url.host, url.port) do |http|
            headers       = { 'Content-Type' => 'application/json' }
            response      = http.post('/', payload, headers)
          end
        end
      end
    end
------


## Client code

    ...
    class Polis < Sinatra::Base
      ...
      post '/' do
        # Note: Thanks to this SO page: http://stackoverflow.com/a/6318491/249218
        http_headers = env.select{|h| h =~ /HTTP_/}
        body_string  = request.body.read

        ...

        logger.info <<-LOG
          Logging POST request:
          Headers:
          #{http_headers}
          Body:
          #{body_string}

        LOG

        status 200
        return body_string
      end

      ...
    end
------


## We sent our first webhook!
------


## There's no official spec

It's a general practice to

### Send data to a URL

### when something happens
___
The data we send is a JSON encoded string.
Lots of flesibility.
But that also means, there are things you'll have to try out on your own. Or issues you might have to re-solve in a way.
------


### How about another demo?

- push a button on a site
- generate a random number
- send a text message
------


## Random Number Button

### Before we push the button

![Before Button Bush](1_before_button_push.png)
------


## After we push the button

![After sending random number](2_sent_random_number.png)
------

## And the text sent

![And the text message](3_random_number_text.png)
------


### Client code

    ...
    class RandomNumberTexter < Sinatra::Base
      ...

      post '/' do
        request.body.rewind
        post_body = request.body.read

        if request.content_type == 'application/json'
          payload = JSON.parse post_body

          is_random_number_generate = payload['event'] == 'random_number:generate'
          return unless is_random_number_generate

          has_payload_data = payload['data']
          return unless has_payload_data

          random_number = payload['data']

          send_text(random_number)
          status 201

        else
          status 202
        end

        post_body
      end

      ...
    end
------


## It still feels like magic

![magic.gif](magic.gif)
------


## What Can I Do With Them?

Workflows are suddenly much easier

- Post support emails, chat requests, and help forum posts in support
  chatroom
- Kick off continuous integration build after pushing commits to GitHub
- Post to engineering chatroom when CI build/tests fails
- Text employee when they've been assigned work
- Reorder products, when inventory drops below threshold
___
Once the webhooks scaffolding is in place, you can tie many things together.
------


## After you see examples

### the floodgates of possibility open

![stampede.gif](stampede.gif)
------


## Who Uses Them?

Loads of companies!

- [Github](https://developer.github.com/webhooks/)
- [Slack](https://api.slack.com/outgoing-webhooks)
- [Dropbox](https://www.dropbox.com/developers/webhooks/docs)
- [Twilio](https://www.twilio.com/platform/webhooks)
___
Notes:
Companies large and small use webhooks to do more with their service.
Even if you don't implement them in your software, it can be nice to
understand how other people use them.
------


## [Zapier](https://zapier.com/)

Connects many services together, using webhooks

Clients can integrate with many services
------


## Webhooks Recap

- Communicate events between your server and a client's using HTTP
- Webhooks accompany an API to enable powerful integrations
- Only real requirement is a web server
- You can do lots of cool stuff with them
------


## Other things to think about

- Use background jobs
- Send requests sequentially
- Persisting requests
- Handle failures
- Performance
___
But these are for another talk!
------


## Webhooks Docs for Fulcrum

- [http://fulcrumapp.com/guides/webhooks/webhooks-for-push-notifications/](http://fulcrumapp.com/guides/webhooks/webhooks-for-push-notifications/)
- [http://fulcrumapp.com/developers/overview/webhooks/](http://fulcrumapp.com/developers/overview/webhooks/)
- [http://fulcrumapp.com/guides/webhooks/webhooks-getting-started/](http://fulcrumapp.com/guides/webhooks/webhooks-getting-started/)
------


## What's Next?

- Create gem(s) to help others add webhooks to their code
  - Would you find this useful?
- Create/extend tools for using, testing, debugging
- Make webhooks easier to debug
- Add scoping to webhooks
___
I'd like to hear from you if you think a gem would be helpful in adding
webhooks to your project.
------


## Links to Code

- Webhooks module, random number generator, random number button site
  - [https://github.com/kyletolle/webhooks.rb](https://github.com/kyletolle/webhooks.rb)
- Simple webhooks endpoint
  - [https://github.com/kyletolle/polis.rb](https://github.com/kyletolle/polis.rb)
- Random number texter
  - [https://github.com/kyletolle/random_number_texter](https://github.com/kyletolle/random_number_texter)
- Script to create this presentation
  - [https://github.com/kyletolle/markdown_to_reveal](https://github.com/kyletolle/markdown_to_reveal)

------


# Thanks!

