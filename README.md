# Bandyer Web Communication Center

The Communication Center allows the creation of a new channel that will facilitate the adoption of our technology within existing systems. The Communication Center creates a configured channel to forward all calls notifications and alerts. The latter regard all calls activities made by users - using the integrated technology - such as calls invitations, alerts of busy users and so on.

API docs: https://docs.bandyer.com/Bandyer-Web-Communication-Center

### How Bandyer Communication Center works?

The Bandyer **Communication Center** makes possible to create a Call between two or more participants. The client who initiate the call is called
*initiator*. The call *initiator* creates the call specifying the participants.

``` javascript 
import CommunicationCenter from '@bandyer/web-communication-center';
var BandyerCommCenter = CommunicationCenter.initialize('user_fake_1', 'wApp_fake123456789', 'sandbox');
BandyerCommCenter.connect()
```

The initialize method returns a CommunicationCenter object. Note that calling the method does not connect the SDK to Bandyer Servers. You must call the method [connect](https://docs.bandyer.com/Bandyer-Web-Communication-Center/classes/communicationcenter.html#connect).

#### Start a call

``` javascript 
var BandyerCommCenter = CommunicationCenter.initialize('user_fake_1', 'wApp_fake123456789', 'sandbox');
await BandyerCommCenter.connect()
BandyerCommCenter.call(['user_fake_1', 'user_fake_2'], {})
```
Once the SDK is connected to the Bandyer server, you can initiate a call using the .call method.
You must specify an array of participants and valid options for the call.

#### Handle incoming call

Once the SDK is connected, it fires events regarding the calls. One of the most important event is the `incoming_call` event. The `incoming_call` event handles the incoming call request from the server. For example:

``` javascript 
var BandyerCommCenter = CommunicationCenter.initialize('user_fake_1', 'wApp_fake123456789', 'sandbox');
await BandyerCommCenter.connect()
BandyerCommCenter.on('incoming_call', function(call){
	// call contains a call Object with all the relative methods such as answer,reject and hangUp.
	// For example: call.answer() || call.decline(declineReason) || call.hangUp(stopReason)
})
```

#### Answer a call

To answer a call, use the answer method in the Call object. The Call object is available either as a data in the incoming call event or from the global object using the **.getCall(roomAlias)** method.
For example:

``` javascript 
var BandyerCommCenter = CommunicationCenter.initialize('user_fake_1', 'wApp_fake123456789', 'sandbox');

// 1st method
await BandyerCommCenter.connect()
BandyerCommCenter.on('incoming_call', function(call){
	call.answer();
}) 

// 2nd method
await BandyerCommCenter.connect()
BandyerCommCenter.getCall('rmn_fake_1234').answer();

```
Once you answered the call, the Call object will be decorated with the token to connect to the Room of @bandyer/web-core-av.

#### Decline a call

To decline a call, use the decline method in the Call object. The Call object is available either as a data in the incoming call event or from the global object using the **.getCall(roomAlias)** method.
For example:

``` javascript 
var BandyerCommCenter = CommunicationCenter.initialize('user_fake_1', 'wApp_fake123456789', 'sandbox');

// 1st method
await BandyerCommCenter.connect()
BandyerCommCenter.on('incoming_call', function(call) {
	call.decline();
}) 

// 2nd method
await BandyerCommCenter.connect()
BandyerCommCenter.getCall('rmn_fake_1234').decline();

```


#### Events

The communication center object fires the following events:

| Event | Description |
| -------------- | :--------------------: |
| incoming\_call | Fired when you are receiving a call |
| call\_dial\_answered | Fired when a user answers to the ongoing call |
| call\_dial\_declined | Fired when a user declines to the ongoing call |
| call\_dial\_stopped | Fired when a user stops to the ongoing call |
| user\_connected | Fired when a user connects to the Communication Center |
| user\_disconnected | Fired when a user disconnects from Communication Center |


##### incoming\_call

The `incoming_call` event is fired when the communication center receives an incoming call for the user connected. The `incoming_call` event contains the Call object decorated with all the data required to connect to the room.

``` javascript 
var BandyerCommCenter = CommunicationCenter.initialize('user_fake_1', 'wApp_fake123456789', 'sandbox');

await BandyerCommCenter.connect()
BandyerCommCenter.on('incoming_call', function(call) {
	// Call object 
}) 

```

#### call\_dial\_answered

The `call_dial_answered` event is fired when the communication center receives an answer response related to the ongoing call. For every user who answer the call, you will receive the `call_dial_answered`.

#### call\_dial\_declined

The `call_dial_declined` event is fired when the communication center receives a decline response related to the ongoing call. For every user who decline the call, you will receive the `call_dial_declined`.

#### call\_dial\_stopped

The `call_dial_stopped` event is fired when the caller hangs up the call during the dial action.

#### user\_connected

The `user_connected` event is fired when a user of your company connects to Communication Center.

#### user\_disconnected

The `user_disconnected` event is fired when a user of your company disconnects from Communication Center.