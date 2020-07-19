## Sarus: The Javascript library for WebSockets

## Introduction 

One of the major issues of web sockets on the client-side is trying to maintain a connection when there is a break or disconnect from the server. The Websocket API does not automatically reconnect when there is a break in connection with the server. Also, the client will no longer receive messages from the server even though the connection on the server was re-established. This poses a challenge to developers who have to manually handle socket reconnections as well as ensure that there is no data loss during the process.

How about having a tool that handles automatic socket reconnections and also ensures consistent delivery of messages?

This is where Sarus comes in. Sarus was built with the knowledge of all these issues in mind to handle all the stress a developer will have to go through by manually handling automatic socket reconnections, attaching event listener functions to WebSockets, and ensuring message delivery. 

There are other tools that can be used to address these issues, one of which is  [Reconnecting Web Socket](https://github.com/joewalnes/reconnecting-websocket). The advantage Sarus has over other tools is the myriad of options it gives to create a dynamic and robust WebSocket application on the client-side. 

Some of these features include

1. Attaching and removing event listener functions
2. Guaranteed message Delivery with persistent storage option on the browser
3. Delaying reconnection attempts
4. Delaying message resending attempts

We will be discussing each feature in the later sections

## Traditional Websocket Connection

Let's look at a sample socket client and server using the traditional approach

Socket Client

    const url = 'ws://localhost:8080' 
    const ws = new WebSocket(url)
    ws.onopen = () => {
      ws.send('hey') 
    }
    ws.onerror = (error) => {
      console.log(`WebSocket error: ${error}`)
    }
    ws.onmessage = (e) => {
      console.log(e.data)
    }
    ws.onclose = () => {
      console.log("connection closed");
    };

In the example above we create an instance of Websocket and attach several event listeners on it. The client connects to the socket server on port 8080

The code above works fine, but once the connection to the server is closed or stops working abruptly the client loses connection and doesn’t receive any new update from the socket even if the server is back up after some minutes. 

![](https://paper-attachments.dropbox.com/s_816548E3A1AB8A841C3DB73C110BD2243A8AC3356FC0CF6D2E4D2BA6964FBF56_1592865119181_image.png)


From the console above, the client received a message 'ho’ from the server and the connection was closed after 5 secs. After 10 seconds again, the server pushes back a message to the client but because the client connection has been closed, it couldn't get the new information from the server.

Socket Server

    const WebSocket = require('ws')
    const wss = new WebSocket.Server({ port: 8080 })
    wss.on('connection', (ws) => {
      ws.on('message', (message) => {
        console.log(`Received message => ${message}`)
      })
      console.log("running on port 8080")
      ws.send('ho!')
      setTimeout(() => {
         ws.close()
         console.log("Connection Closed")
      }, 5000);
      setTimeout(() => {
        ws.send('heyo, am back')
        console.log("Hello there, connection is back")
     }, 10000);
    })

In the example above, once the client is able to establish a connection to the server it sends a message saying hey, and the server responds with ho!

![](https://paper-attachments.dropbox.com/s_816548E3A1AB8A841C3DB73C110BD2243A8AC3356FC0CF6D2E4D2BA6964FBF56_1592865481618_image.png)


Although we can manually initiate a reconnection attempt when the onclose event is fired on the client-side, things start getting clumsy when we have a lot of clients that connect to the WebSocket server. 

Sarus solves this problem efficiently as well as provide other features like persisting the data to be sent to the server during retries in a queue using web storage(LocalStorage and Session Storage)


## Sarus to the rescue

Installation 

To install Sarus, run 

    npm i @anephenix/sarus

Initialize Sarus

They are a couple of options you can set while initializing Sarus. To see a complete list of the options. You can check out the  [doc](https://sarus.anephenix.com/)  for more info. 

You can set the type of data to be sent, Storage type, StorageKey, etc. To do a simple initialization you can use the snippet below


    // Connect to a WebSocket server
    const sarus = new Sarus({
      url: 'ws://localhost:8080',
    });

NB: Depending on your environment, don't forget to change to wss if 
your server is running over https and ws if your server is over HTTP or when you working locally

How Sarus handles automatic reconnections

    import Sarus from '@anephenix/sarus';
    
    const sarus = new Sarus({
      url: 'ws://localhost:8080',
    });
    
    sarus.on('open', () => {
      sarus.send('hello I am alive forever, from sarus');
      console.log("Hey there from sarus")
    });
    
    sarus.on('close', () => {
      console.log("Connection Closed")
    });

When a connection is closed, the close event listener will be fired and Sarus will perform an automatic reconnection immediately to the server and continues operation when the reconnection is successful. Messages sent from the server after reconnection on the client-side will still be delivered 


![](https://paper-attachments.dropbox.com/s_816548E3A1AB8A841C3DB73C110BD2243A8AC3356FC0CF6D2E4D2BA6964FBF56_1594135731012_image.png)


Even though the connection was closed on the server, Sarus was still able to maintain the connection as soon as the connection came back. The traditional client socket would not be able to receive updates once the connection closes on the server


## Sarus Event Listeners

Sarus also provides several events that listen for changes and events emitted from the server

    sarus.on('open', () => {
      sarus.send('hello I am alive forever, from sarus');
      console.log("Hey there from sarus")
    });

The open event listener is called when Sarus is able to successfully establish a connection to the socket server. An open connection denotes the ability to be able to send and receive messages from the server in real-time. When the connection to the server is successfully established, you can send a message to the server using the send method on the sarus instance. 


    sarus.send('hello I am alive forever, from sarus');

The error event listener is fired when an error is encountered in the web socket connection. 

    sarus.on('error', (error) => {
      console.log("Error from sarus", error)
    })

The onclose event listener is triggered when the connection to the server is closed. This is where one of the core strengths of Sarus comes to light. Using the traditional approach of a WebSocket connection, the developer will have to manually handle reconnections in the onclose event listener and probably wait for a few seconds before establishing a new connection. Sarus on the other hand handles this seamlessly and under the hood. 

    sarus.on('close', () => {
      console.log("Connection Closed")
    });

The message event listener is triggered when the server emits a message to the client.

    sarus.on('message', event => {
      console.log("I got a message from the server - sarus")
      console.log(event.data);
    })

It is also possible to define a function separately and assign it to the message event listener
Methods


    // Save data receieved into a database
    const saveToDb = (event) => {
      console.log(event.data);
    };
    // Attach the event listener
    sarus.on('message', saveToDb);


## Removing Event listener functions. 

Sarus provides the option of removing event listener functions from the sarus instance. This can be very handy when we need to perform an operation once during the entire lifecycle of the connection. To remove event listeners, call the off method on the sarus instance and pass the function variable to it.

    
    /*
      Remove a function from the Sarus 
      event listener by function variable
      The saveToDb is just an example of the function to be removed from the sarus instance after execution. It can be any custom function that has been defined
    */
    sarus.off('message', saveToDb);


## Closing Client Connection

Sarus provides the option to close a socket connection on the client-side. This can be handy while using an application. It can be used on logout on the application or when inactivity is discovered during the lifecycle of the application. You can close the connection by calling the disconnect method on the sarus instance.


    sarus.disconnect();

The  disconnect method on the sarus instance performs two actions


1. Set the reconnect automatically flag to false.
    This means that Sarus will not try to perform automatic reconnection as the flag will be set to false
    
2. Close the WebSocket connection.
    This closes the WebSocket connection and fires the close event listener on the sarus instance.


## Guaranteed message delivery

Sarus uses a message queue to handle the messages that need to be sent to the server. If a storage option is not provided when initializing sarus, once a user refreshes their browser the information to be sent in the queue will be lost because they are not persistent even when the socket connection is now open. To prevent this, sarus provides the ability to specify a storage option. Specifying a storage option means the messages to be sent to the server will be persistent even though the user refreshes their browser or leaves the application. Once the connection is successfully re-established, the messages will be sent to the server 

NB: The message queue is persisted with HTML5 browser storage, and there are two options - session storage, and local storage


    const sarus = new Sarus({
      url: 'ws://localhost:8080',
      storageType: 'session',
    });

You can specify the storage type which can either be session or local. Specifying session means it will use SessionStorage while specifying local means it will use Local Storage


    const sarus = new Sarus({
      url: 'ws://localhost:8080',
      storageType: 'local',
    });

By default, the key used to store the messages in the queue to web storage is ‘sarus’ but it can be changed to something else with the help of the storagekey option


    const sarus = new Sarus({
      url: 'ws://localhost:8080',
      storageType: 'local',
      storageKey: 'data',
    });


![](https://paper-attachments.dropbox.com/s_816548E3A1AB8A841C3DB73C110BD2243A8AC3356FC0CF6D2E4D2BA6964FBF56_1592889698848_image.png)


## Reconnection attempts

One of the ways of manually handling socket closures is trying to reconnect to the server by adding a timeout before creating a new connection. 

An example is given below


    function start(websocketServerLocation){
        ws = new WebSocket(websocketServerLocation);
        ws.onmessage = function(evt) { alert('message received'); };
        ws.onclose = function(){
            // Try to reconnect in 5 seconds
            setTimeout(function(){start(websocketServerLocation)}, 5000);
        };
    }

In the example above, when the client notices a disconnection from the server which fires the onclose event on the client, it tries to reconnect again after 5sec. Sarus provides this option by setting a retryConnectionDelay option to the initialization which adds a delay to the reconnection attempt to the server once a connection is lost. This is usually a good practice to avoid excessive attempts to reconnect when the server is not available, i.e. broken network or shutdown of local debug server. 


    const sarus = new Sarus({
      url: 'ws://localhost:8080',
      retryConnectionDelay: true,
    });

When the retryConnectionDelay is enabled, sarus will delay the reconnection attempt till 1s(1000ms). You can override this default time by specifying the delay time in milliseconds instead of using the boolean flag 


    const sarus = new Sarus({
      url: 'ws://localhost:8080',
      retryConnectionDelay: 500, // 500ms or 1/2 second
    });


## Messages sending re-attempts

It is also possible for a retried message to fail when the connection to the server is established once again.  Sarus provides an option to set retryProcessTimePeriod which applies a 50ms delay before attempting to resend a message. You can override the default time by adding time in milliseconds


    const sarus = new Sarus({
      url: 'ws://localhost:8080',
      retryProcessTimePeriod: 25, // 25ms
    });


## Conclusion

While the need for building applications that responds to users request and demands in real-time is on the rise, it is also very important to use technologies that capture different edge cases that can occur using real-time applications as well as provide solutions to these edge cases. 

Networks are unreliable and because Websockets use the TCP protocol which can experience different types of failures resulting in loss of connectivity, it's very crucial to use a very robust technology while building real-time applications to handle some of these issues. Sarus was built to handle the majority of the issues encountered while building WebSocket applications on the client-side. Also while Sarus is not a one size fit all solution, it's also important to be aware of the issues that can occur with WebSocket applications in production and find ways to mitigate them. 

You can check out more information about how to start using Sarus in your application in the official  [documentation](https://sarus.anephenix.com/) 
