C# utilizes broadcasting feature of delegate to manage events handlers.
Since delegate can trigger whole its register methods when it is invoked
so it is treated as the broadcaster for all callback method.

C# has its own pattern to define the event and event handlers to create
the consistency for developing code to handle an event. The
implementation of event handler is illustrated through the example
below.

class Price

{

int value;

int upperLimit;

public Price (int initialValue, int upperLimit)

{

this.value = initialValue;

this.upperLimit = upperLimit;

}

public void alterPriceValue (int variance)

{

this.currrentValue += variance;

}

}

Suppose we want to create an event when the price reaches its upper
limit. The first is to define the event as a subclass of built-in
EventArgs superclass.

public class ValueReachUpperLimitEventArgs : EventArgs

{

public int upperLimit;

public int currentValue;

public ValueReachUpperLimitEventArgs(int upperLimit, int currentValue)

{

this.upperLimit = upperLimit;

this.currentValue = currentValue;

}

}

The event class may have none or several class field as the additional
information along with event signal.

The second step is to add a delegate object as event handler. The
delegate object for handling event must follow the method template
below.

public delegate void EventHandler\<TEventArgs\> (object source,
TEventArgs e)

You can use event keyword as a shorthand for the using delegate directly

public static event EventHandler\<ValueReachUpperLimitEventArgs\>
ValueReachUpperLimit;

then we create method as event trigger to fire an event.

protected virtual void onValueReachLimit (ValueReachUpperLimitEventArgs
e)

{

if (ValueReachUpperLimit != null) ValueReachUpperLimit(this, e);

}

we

public void alterPriceValue(int variance)

{

this.currrentValue += variance;

if (this.currrentValue \> this.upperLimit) onValueReachLimit(new
ValueReachUpperLimitEventArgs(this.upperLimit,this.currrentValue));

}
