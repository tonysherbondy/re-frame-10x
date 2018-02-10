# Inter Epoch Traces

> "Music is the space between the notes." - Claude Debussy

`re-frame-trace` is built around the idea of epochs. An epoch captures all of the traces emitted by re-frame during the handling an event. But what about the traces that are emitted when re-frame *isn't* handling an event? These are the inter-epoch traces.

Inter-epoch traces are emitted under (at least) three circumstances:

* When views have local ratoms, used, for example, to handle the appearance of popups or mouseover annimations. When the state of these ratoms change, views will likely rerender, and the trace to do with this rerendering will flow from reagent to `re-frame`trace`. But there'll be no "current" epoch to put it in.  Because there was no event.
* A Figwheel recompile and subsequent code reload will cause all existing subscriptions to be destroyed, andwill trigger a complete re-render of your application
* `re-frame-trace` itself resetting the value in `app-db`. Again there will be a flurry of subscription and view trace, but no epoch, because there was no event.

The first of these is essential to the functioning of your application, whidh makes their trace interesting and informative. The last two are incidental to the tooling and are probably best ignored. Probably.

`re-frame-trace` collects any inter-epoch subscription traces and shows them in the subs panel with the next epoch. They are broken out into a separate section marked: "Inter-Epoch Subscriptions".