#Obvs.MessageDispatcher.Autofac

Provides integration support between the core [Obvs.MessageDispatcher framework](http://github.com/drub0y/Obvs.MessageDispatcher) and the [Autofac dependency injection framework](http://autofac.org/) in the form 
of an `IMessageHandlerSelector` implementation which resolves instances of `IMessageHandler<TMessage>` from a specified Autofac container. 

##Basic configuration
Configuration of this integration would look like this:

```
// Configure and create an Obvs service bus instance
var myObvsServiceBusConfiguration = ConfigureServiceBus();
var serviceBus = myServiceBusConfiguration.CreateServiceBus();
IContainer myAutofacContainer = BuildAutofacContainer();

// Create a message dispatcher for the events that are coming off the service bus
var dispatcherConfiguration = serviceBus.CreateMessageDispatcherFor(sb => sb.Events);

// Configure the dispatcher with a "simple" message handler factory and configure that 
// with all the IMessageHandler instances in my program's assembly.
dispatcherConfiguration.WithAutofacMessageHandlerSelectorFactory(myAutofacContainer);

// Start the message dispatcher
dispatcherConfiguration.DispatchMessages();
``` 

##Autofac Extensions

This library also contains an extension method for Autofac's `ContainerBuilder` to help register any instances of `IMessageHandler<TMessage>` found in any specified assemblies:

```
var containerBuilder = new ContainerBuilder();

containerBuilder.RegisterMessageHandlers(typeof(MyProgram).Assembly);
``` 

