# PubSubXojo  
  
Publish-Subscribe module for Xojo. Ported my my LiveCode version of the same: https://github.com/stam66/skPubSub (if anything highlights the elegance of LiveCodeScript compared to the tortuous verbosity often needed in Xojo). A simple way to avoid raising convoluted events and event handlers especially where unrelated objects need to react to the same message.

The module includes 4 methods:
- **Public Sub Subscribe(eventName As String, callbackMethod As String, target As Object)** to register an object to receive notification

- **Public Sub Broadcast(eventName As String, data As Variant = Nil)** to broadcast a message with optional parameters

- **Public Sub Unsubscribe(eventName As String, callbackMethod As String = "", target As Object)**

- **Public Sub RemoveAllSubscriptions()**  


The module has a private property to store subscriptions: **Private mSubscriptions As Dictionary**  
_Dictionary structure: Dictionary[EventName:String] -> Dictionary[Target:WeakRef] -> String() of callback method names_  

---

### Example usage
```
// Add a method to your WebPage
Public Sub HandleDataChanged(data As Variant)
  MessageBox("Event received: " + data.StringValue)
End Sub

// In Opening event or a button:
PubSub.Subscribe("TestEvent", "HandleDataChanged", Self)
PubSub.Broadcast("TestEvent", "Hello World!")

// Test unsubscribe
PubSub.Unsubscribe("TestEvent", "HandleDataChanged", Self)
PubSub.Broadcast("TestEvent", "Should not appear") // Won't trigger

// Or test removing all
PubSub.RemoveAllSubscriptions()
```
