#Obvs.MessageDispatcher.Autofac

Provides integration support between the core `Obvs.MessageDispatcher` framework and the Autofac dependency injection framework in the form 
of an `IMessageHandlerSelector` which resolves instances of `IMessageHandler<TMessage>` from a specified Autofac `Container`. 