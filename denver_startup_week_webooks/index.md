# The Power Behind Real-Time Push Notifications Using Webhooks

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
------


## Slides for this talk

### Presentation:

TODO: Update this link...
[https://kyletolle.github.io/ruby_webhooks_intro](https://kyletolle.github.io/ruby_webhooks_intro)

### Code:

TODO: Update this link
[https://github.com/kyletolle/ruby_webhooks_intro](https://github.com/kyletolle/ruby_webhooks_intro)
------
 

# Getting a Feel

- How many have some knowledge of webhooks?
- How many have used them before?
- How many work where webhooks are offered?
___
Trying to get a feel for the familiarity that people have with the topic.
------


# Overview

TODO: Add some more stuff to this overview... Particularly the discussion
about startups in general.

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

TODO: This is where we want to begin talking about startups.

------

# Startups

TODO: This is going to be a big section as I test things out.

Startups find a niche.
They don't build a whole ecosystem.
They solve one problem, do it well.
Don't want to dilute your core product with extra junk.
Staying focused gives you differentiation. You're not cluttered.
Your value proposition is more clear.

Want to give your customers access to their data.
This lets you fit into their workflow.
Incumbents might force people to adapt to their workflow.
But startups are like a link in a chain that helps people be nimble.
Avoid vendor lock-in too. Builds goodwill. Makes more sense for your user too.
They can combine your focused product with other focused products.
They can adapt your product to their workflow, to their needs.
This is real value here. When you enable them to get more stuff done.

Flexibility is your power. Small is nimble. Small is agile. Small is focused.
Flexibility isn't something that the incumbents really have.

Some kinds of integration make sense to build.
But a general way to integrate with others is powerful.
You don't have to write custom integrations for everything.
You don't have to predict everything your customers will want or need.
You give your customers more power to do things that you could never actually
think of, because you're not in their shoes trying to solve their problems.
You'll have more flexibility going forward to integrate with more things that
didn't even exist when you first started.
You'll be able to integrate with many 3rd parties for essentially free.
Make it so your users can understand it and use it, and you're off.
Or you can help your customers do stuff, but the flexible nature will mean
it's less work for you and for them. That's a huge win.
You can't hardcode everything. You don't want to. Spread your resources out.
Pain to maintain. You want to stay focused.
The flexible approach maintains your relevancy going forward. You're not
hardcoding integration with service du jour, which will fall out of favor in a
week. You're giving the flexibility to integrate with many more things.
You don't have to start over when some new service shows up on the block.
The startup has to fit into the puzzle that is the customer's workflow.

How do we do that? APIs are a great way. Very flexible.

Then discuss the whole thing about the APIs like below.

And webhooks are another great way to expand that flexible, open approach.

TODO: How to sell it to your managers. Need to process that section and work
it into the stuff above...



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

------

## NEW TALK FOR DSW

Webhooks talk @ Denver Startup Week

# Intro

- How many have heard of webhooks?
- How many have used webhooks?
- How many work for a company or service that implements webhooks?

# Importance of Open

Avoiding vendor lock-in is important for small businesses and startups. You
haven't invented an entire ecosystem that you want to lock people into. And
you'll want your customers to have access to their data. If they combine it
with their own processes or some other sister services, then it's easier for
you to find your niche, make things easier for them in one area, and provide
them more value.

In fact, you don't want to build the entire ecosystem and dilute your main
offering. Staying focused when you're small gives you differentiation.

Some kinds of integration with specific partners might make a lot of sense.
But a general sort of way to integrate with others is even more powerful. If
it's easy to use and a pattern others can follow or use, then we have the
potential to integrate with many 3rd parties, essentially for free. We don't
build a custom integration for each one, but they can all use a single
approach.

When you're going against a large incumbent, flexibility is going to be a huge
advantage.

If you're flexible with what you allow your customers to do, that's less
custom integration you have to build yourself. Building a single type of
integration gives you and your customers more flexibility than you could
hard-code. And this is particularly important for maintaining relevancy going
forward. You don't have to integrate with every new thing if the customers
have a way to do this on their own. Think of your product as a link in a chain
of their workflow and you understand how important it is ti give them the
tools to work with other services. Startups are like pieces of the puzzle:
small, focused, built for a particular use case and position. The startup has
to fit into the puzzle or the rest of the ecosystem. API and webhooks are a
good way to do that.

Your internal stuff or features could be built on top of webhooks too. Might
be one way to approach micro-services. Or new features that aren't in the
core could be built on top of webhooks, and exposed in a seamless way.

---

How to sell it to your managers. How to convey the business value.

Rather than building a specific solution that's forcing them into our workflow, this allows people to fit us into their workflow. That's going to increase the chance of adoption of our service. Keeps us lightweight. People can swap out their backends easily, which is amazing. Customers don't mind paying for the service so long as they can get the data out. Webhooks and the API are a big way to do that.

Webhooks allow you to design your own workflow. Instead of trying to shoehorn the workflow from the system you bought into. Although, the complete customization is that you have to build it. There's some effort people have to do. But stuff like Zapier helps makes that easier.

We can focus on the niche, which is super good for us and for the customer. They can slot us in, without rearchitecting the world to use our service. And people don't have to use them right off the bat, but they have that option later. Which adds power.

Webhooks allow 2 APIs to talk together a lot more easily. You intercept a message and then do something with it. Makes it easier to build integrations.

In addition to Fulcrum, talk about how we use webhooks. Slack. Incoming and outgoing. Tender feeds into slack too. Outgoing sends a notification when something happens in slack, sends the data somewhere else. Incoming webhook is geobooze. Something gets done in another app and then posts a message in slack. Look at the docs for this.

Primiarly in fulcrum, people use webhooks to sync with an external database or service. Mirroring a table that exists in MySQL. Syncing data to CartoDB. Sending email notifications when a new record is created, and the status is "needs immediate attention", an email can be sent to a manager.

Push bullet has a good API for push notifications. Send a notification with a dispatch, clicking on the link, opens the record on the mobile device. When a record is assigned, send a link to who it was assigned to, and the URL can open up the app to that record. You could do something like notify people within a certain geo area. You could also assign to someone who was last known to be nearest.

Using webhooks with data shares is also cool. Our JSON payload of the webhook is like the API format, which isn't so readable. You'd have to parse the data. But treat it as a notification, and then fetch through the data share and you can get some human-readable data to then chuck into a DB. The data is more easily consummable in the data share. And if you need something simple, where you just need a field or two, you can hardcode the values. It's not just pushing your data somewhere, you can do other things too.

Bryan wanted to add to GeoBooze. When a new record is created without an ABV, then hit a web service to look it up and then update the record. Webhook makes a call to some other API and then you can write some data back to the API. Allows you to automatically enter some data from a service without manually doing it. You can automatically fetch the weather data from a service based on the location.

We have an integration team, and they've found that this allows customers to do all sorts of custom things. We can help them get set up, which is worthwhile for us and for them. It's a force multiplier. We don't have a huge engineering team, so this allows us to have a lot more impact. Lowers the barrier to building these custom workflows. Allows us to more easily do things that we could have never predicted. We don't need a core developer to build some integration, someone else can use our webhooks to build on top of it. They can read the docs and build their own stuff.

Keeps us flexible. We can build something more one off without adding to the core of our system. Can repurpose scripts to do other things. And they're programming language agnostic. PHP or Ruby or Node or whatever. As long as it can read JSON, you're able to use this. We're not writing custom libraries to support all different environments too.

Google App Scripts too. This takes no server setup. Just create an app script like you would a google doc. No needing to use heroku or a PHP server. And you can share it too, like you would a google doc.

