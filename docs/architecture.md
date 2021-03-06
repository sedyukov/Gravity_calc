# Architecture

![Bloc Architecture](assets/bloc_architecture_full.png)

Using the bloc library allows us to separate our application into three layers:

- Presentation
- Business Logic
- Data
  - Repository
  - Data Provider

We're going to start at the lowest level layer (farthest from the user interface) and work our way up to the presentation layer.

## Data Layer

> The data layer's responsibility is to retrieve/manipulate data from one or more sources.

The data layer can be split into two parts:

- Repository
- Data Provider

This layer is the lowest level of the application and interacts with databases, network requests, and other asynchronous data sources.

### Data Provider

> The data provider's responsibility is to provide raw data. The data provider should be generic and versatile.

The data provider will usually expose simple APIs to perform [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations.
We might have a `createData`, `readData`, `updateData`, and `deleteData` method as part of our data layer.

[data_provider.dart](_snippets/architecture/data_provider.dart.md ':include')

### Repository

> The repository layer is a wrapper around one or more data providers with which the Bloc Layer communicates.

[repository.dart](_snippets/architecture/repository.dart.md ':include')

As you can see, our repository layer can interact with multiple data providers and perform transformations on the data before handing the result to the business logic Layer.

## Business Logic Layer

> The business logic layer's responsibility is to respond to input from the presentation layer with new states. This layer can depend on one or more repositories to retrieve data needed to build up the application state.

Think of the business logic layer as the bridge between the user interface (presentation layer) and the data layer. The business logic layer is notified of events/actions from the presentation layer and then communicates with repository in order to build a new state for the presentation layer to consume.

[business_logic_component.dart](_snippets/architecture/business_logic_component.dart.md ':include')

### Bloc-to-Bloc Communication

> ???Every bloc has a state stream which other blocs can subscribe to in order to react to changes within the bloc.

Blocs can have dependencies on other blocs in order to react to their state changes. In the following example, `MyBloc` has a dependency on `OtherBloc` and can `add` events in response to state changes in `OtherBloc`. The `StreamSubscription` is closed in the `close` override in `MyBloc` in order to avoid memory leaks.

[bloc_to_bloc_communication.dart](_snippets/architecture/bloc_to_bloc_communication.dart.md ':include')

## Presentation Layer

> The presentation layer's responsibility is to figure out how to render itself based on one or more bloc states. In addition, it should handle user input and application lifecycle events.

Most applications flows will start with a `AppStart` event which triggers the application to fetch some data to present to the user.

In this scenario, the presentation layer would add an `AppStart` event.

In addition, the presentation layer will have to figure out what to render on the screen based on the state from the bloc layer.

[presentation_component.dart](_snippets/architecture/presentation_component.dart.md ':include')

So far, even though we've had some code snippets, all of this has been fairly high level. In the tutorial section we're going to put all this together as we build several different example apps.
