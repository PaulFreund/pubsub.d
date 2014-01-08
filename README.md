pubsub.d
========
Publisher/Subscribe implementation for the D programming language.

## Usage
To use pubsub.d just copy the code from the source directory or include it as a dependency in your dub package:

    "dependencies": { 
        "pubsub-d" : "~master" 
    }

### Subscribing
You need to mixin the DispatchMapper before subscribers can be called:

    import pubsub;
    class Listeners {
        this() { mixin DispatchMapper; }
        ...
    }    

After that you can subscribe to events with member functions:
    
        @subscribe("events.importantEvent") void listenOne(string message) {
            writeln("Important event: " ~ message);
        }
    
Insted of single events it is also possible to listen to a group of events:
    
        @subscribe("events.*") void listenAll(string message) {
            writeln("An event has occured : " ~ message);
        }


### Publishing
To publish an event just call the publish function with the event name as the first parameter:

    publish("events.importantEvent", "This is the important message");

### Custom Dispatchers
By default pubsub uses a static dispatcher for publish and all subscribers. To create a "private" network, the DispatchMapper mixin and the publish function can be supplied with a custom Dispatcher:

    class Listeners {
        this(Dispatcher _dispatcher) { mixin DispatchMapper!_dispatcher; }
        ...
    }    
    
    ...
    publish(_dispatcher, "eventName", "Data");

    



