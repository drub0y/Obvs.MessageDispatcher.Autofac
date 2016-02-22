#Obvs.MessageDispatcher.Autofac

Provides integration support between the core [Obvs.MessageDispatcher framework](http://github.com/drub0y/Obvs.MessageDispatcher) and the [Autofac dependency injection framework](http://autofac.org/) in the form
of an `IMessageHandlerSelector` implementation which resolves instances of `IMessageHandler<TMessage>
    ` from a specified Autofac container.

##Basic configuration
Configuration of this integration would look like this:

```
// Configure and create an Obvs service bus instance
var myObvsServiceBusConfiguration = ConfigureServiceBus();
var serviceBus = myServiceBusConfiguration.CreateServiceBus();
IContainer myAutofacContainer = BuildAutofacContainer();

// Create a message dispatcher for the events that are coming off the service bus
var dispatcherConfiguration = serviceBus.CreateMessageDispatcherFor(sb => sb.Events);

// Configure the dispatcher to use the IMessageHandlerSelector factory which will
// resolve IMessageHandler<TMessage> instances from the supplied Autofac container
dispatcherConfiguration.WithAutofacMessageHandlerSelectorFactory(myAutofacContainer);

// Start the message dispatcher
dispatcherConfiguration.DispatchMessages();
```

##Autofac Integration Details

###Extensions

This library also contains an extension method for Autofac's `ContainerBuilder` to help register any instances of `IMessageHandler<TMessage>
` found in any specified assemblies:

```
var containerBuilder = new ContainerBuilder();

containerBuilder.RegisterMessageHandlers(typeof(MyProgram).Assembly);
```

###Lifetime Scopes
The `WithAutofacMessageHandlerSelector` extension method provides a factory method which begins a new Autofac lifetime scope by calling
`IContainer::BeginLifetimeScope` on the supplied `IContainer` instance.
