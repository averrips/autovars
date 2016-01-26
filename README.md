# AutoVars
Reactive state management for Meteor + React

## Why should I use this?
Meteor and React are both great frameworks but integrated you may run into
the following issues when you build a complex, data heavy, highly interactive
app.

### 1. Performance issues due to rerendering of entire subtrees
React is a great addition to Meteor for several reasons, one of them is it's
components which allow you to encapsulate both view and logic. This is always
a bit of a pain when you use Blaze.

Unfortunately if you have an app with many nested components, each with their
own logic and derived state, rerendering those components may be too costly.

React was built with a nice shadow DOM to be able to quickly execute view
changes. But that doesn't help when child components have their own set of
calculations to run before they can render themselves. Those still have to run
before every render.

One approach is to separate logic and view and keep the React components as dumb
as possible. This is the approach Flux takes.

AutoVars goes in the other direction by making the components smarter so
renders become more fine grained and less frequent. It relies heavily on core
Meteor reactivity.

### 2. Bugs due to multiple state mutation flows: data, props and state
React has props to pass data between components and state to manage internal
component state. The integration with Meteor adds `.data` and `getMeteorData`.
Both `render` and `getMeteorData` become reactive and are executed when a
reactive dependency they use changes.

This means both React and Meteor's Tracker can execute your code. They do so
when they see fit. And they don't sync their timing.

This does not have to be a problem when you only use `getMeteorData` to pull in
data from a Meteor Mongo collection and from there on program React style. This
can be achieved with a strict code convention in your team.

Unfortunately when the team grows and you have more Meteor experts on the team,
`ReactiveVar`'s and `autoruns` may enter your project, possibly from other
packages. The only way to connect those to the React world is through
`getMeteorData` and then you will come to a point where it is very hard to
understand what triggers executing your code.

AutoVars solves this problem by replacing `.data` and `.state` as well as most
of the React component lifecycle with automatically executing functions, the
results of which can be accessed through `.autovars`.

## Show me an example!
See the examples directory for some examples.
