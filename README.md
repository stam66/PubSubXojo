# PubSubXojo  
  
Publish-Subscribe module for Xojo. Ported my my LiveCode version of the same: https://github.com/stam66/skPubSub (if anything highlights the elegance of LiveCodeScript compared to the tortuous verbosity often needed in Xojo). A simple way to avoid raising convoluted events and event handlers especially where unrelated objects need to react to the same message. Now changed to delegate based module for type safety and speed.

The module implements a delegate:
Delegate: `EventCallback(data As Variant)`  
All callbacks' signatures now need to have a single variant parameter even if it’s nil.

The module has a private property to store subscriptions: `Private mSubscriptions As Dictionary`  
_Dictionary structure: Dictionary[EventName:String] → Dictionary[Target:Object] → EventCallback()_  

The module includes 6 methods:
- `Subscribe(eventName As String, callback As EventCallback, target As Object)` _Subscribe an object to a message that may be broadcast_
- `Broadcast(eventName As String, data As Variant = Nil)` _Broadcast a message to all subscribed objects_
- `Unsubscribe(eventName As String, callback As EventCallback, target As Object)` _Unsubscribe an object from a specific messaage that may be broadcast_
- `UnsubscribeAll(eventName As String, target As Object)` _Unsubscribe all callbacks for a target from an event_
- `UnsubscribeTarget(target As Object)` _Remove all subscriptions for a target across all events_
- `RemoveAllSubscriptions()`  

---

### Example usage
```
// Add a method to your WebPage
Public Sub HandleDataChanged(data As Variant)
  MessageBox("Event received: " + data.StringValue)
End Sub

// In Opening event or a button:
PubSub.Subscribe("TestEvent", AddressOf HandleDataChanged, Self)
PubSub.Broadcast("TestEvent", "Hello World!")

// Test unsubscribe
PubSub.Unsubscribe("TestEvent", AddressOf HandleDataChanged, Self)
PubSub.Broadcast("TestEvent", "Should not appear") // Won't trigger

// Or test removing all
PubSub.RemoveAllSubscriptions()
```
