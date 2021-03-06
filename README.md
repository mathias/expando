<p align="center">
  <img title="Expando logo" src='https://cloud.githubusercontent.com/assets/2220/19525778/b1b3130e-95e7-11e6-9efd-09f195f720ff.png' width=300 />
</p>

<p align="center">
  <a title="Gem Version" href="https://rubygems.org/gems/expando">
    <img src="https://img.shields.io/gem/v/expando.svg" />
  </a>
   <a title="Linux Build Status" href="https://travis-ci.org/expando-lang/expando">
    <img src="https://img.shields.io/travis/expando-lang/expando/master.svg?label=Linux%20build" />
  </a>
  <a title="Code Climate" href="https://codeclimate.com/github/expando-lang/expando">
    <img src="https://img.shields.io/codeclimate/github/expando-lang/expando.svg" />
  </a>
  <a title="Test Coverage" href="https://codeclimate.com/github/expando-lang/expando/coverage">
    <img src="https://img.shields.io/codeclimate/coverage/github/expando-lang/expando.svg" />
  </a>
  <a title="Dependency Status" href="https://gemnasium.com/expando-lang/expando">
    <img src="https://img.shields.io/gemnasium/expando-lang/expando.svg" />
  </a>
  <a title="Security" href="https://hakiri.io/github/expando-lang/expando/master">
    <img src="https://hakiri.io/github/expando-lang/expando/master.svg" />
  </a>
</p>

Expando is a translation language for easily defining user utterance examples when building conversational interfaces for Natural Language Understanding services like [API.AI](https://api.ai), [LUIS](https://www.luis.ai/), or the [Alexa Skills Kit](https://developer.amazon.com/alexa). This is roughly analagous to the concept of building grammars for speech recognition systems. 

## What's all this, then?

The following line of Expando:

```text
(is it possible to|can I|how do I) return (something|an item)
```

...would be expanded by producing the Cartesian product of the phrases in parentheses that are separated by pipes:

```text
is it possible to return something
is it possible to return an item
can I return something
can I return an item
how do I return something
how do I return an item
```

This encoding makes it much easier to manage multiple user utterance examples when building conversational interfaces.

Using Expando, you can:

* House your intents and entities in version control, simplifying collaboration.
* Use the CLI to automatically update your intents and entities (only supports API.AI, at the moment).
* Make use of the expansion syntax to dramatically simplify the encoding of utterance examples.

## Installation

This reference implementation of the Expando language is built with Ruby, and packaged as a [gem](https://rubygems.org/). You can install it with:

```console
$ gem install expando
```

## Getting started

The Expando CLI features an `init` command for initializing new Expando projects:

```console
$ mkdir support-bot
$ cd support-bot
$ expando init
✓ intents directory created
✓ entities directory created
✓ .expando.rc.yaml file created
✓ circle.yaml file created
```

This will create `intents` and `entities` directories, for housing the utterance examples themselves, as well as some configuration files.

### Configure API.AI integration

If you'll be using Expando to update the intents and entities of an existing API.AI agent, you'll need to copy the client access token and developer access token for the agent to `.expando.rc.yaml`:

```yaml
# API.AI credentials - add the credentials for your agent below
:client_access_token: REPLACE_WITH_TOKEN
:developer_access_token: REPLACE_WITH_TOKEN
```

### Set up continuous integration

The `circle.yaml` file can be used to configure [CircleCI](https://circleci.com/) to enable automatically updating an API.AI agent when commits are pushed to your Expando project's repo.

### Create intent and entity files

Let's assume we have an agent on API.AI named `support-bot`. If we want to use the Expando syntax for one of this agent's intents named `openHours`, we'd create a file in the `intents` directory named `openHours.txt`.

It's also possible to create expandable entity examples in the same manner. A file named `entities/products.txt` would match to a `products` entity on API.AI.

## Syntax

Currently, Expando supports phrase combinations, and optional phrases.

### Phrase combination

Using the above example, we could include the following line of Expando in the file `intents/openHours.txt`:

```text
(when|what times) are you open
```

This would be expanded by creating a version of this utterance with each of the phrases enclosed by paretheses and separated by pipes:

```text
when are you open
what times are you open
```

If multiple sets of phrases are included on the same line, a Cartesian product of each of the phrases will be created. The following line of Expando:

```text
(when|what times) are (you|y'all|you guys) open
```

...would result in this full list of utterances:

```text
when are you open
when are y'all open
when are you guys open
what times are you open
what times are y'all open
what times are you guys open
```

### Optional phrases

By making the final phrase in a set blank, you can make it optional. The following Expando:

```text
what are your (open|) hours
```

...would result in:

```text
what are your open hours
what are your hours
```

It's also possible to make an entire set of phrases optional:

```text
what are your (open|business|) hours
```

...results in:

```text
what are your open hours
what are your business hours
what are your hours
```

Essentially, you're making the last phrase in the set an empty string.

## Updating API.AI

In order to update intents or entities on API.AI, use the following commands:

```console
$ expando update intents
$ expando update entities
```

It's also possible to target specific entities or intents for updating:

```console
$ expando update intents openHours
```

You can also access full help for the Expando CLI:

```console
$ expando --help
```

..and also help for specific Expando CLI commands:

```console
$ expando update --help
```

## Documentation

Documentation for the source code of the `expando` gem itself can be viewed [here](http://www.rubydoc.info/github/expando-lang/expando/master).

## Credits

<p align="center"><a href="https://voxable.io"><img title="Voxable logo" src="https://cloud.githubusercontent.com/assets/2220/14663745/8b5688dc-0689-11e6-95b9-7765fa59128e.png" /></a></p>

Initial work on Expando was graciously funded by the good folks at [vThreat](https://vthreat.com). Expando is brought to you by [Voxable](http://voxable.io), a conversational interface agency in Austin, Texas.