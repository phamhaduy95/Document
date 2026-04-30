# Events and EventHandlers in C#

C# uses the broadcasting feature of delegates to manage events. An event allows a class or object to notify other classes or objects when something of interest occurs.

## The Standard Event Pattern
To maintain consistency, C# follows a specific pattern for defining events and their handlers.

### 1. Define EventArgs
If your event needs to pass data, create a class that inherits from `EventArgs`.

```csharp
public class PriceChangedEventArgs : EventArgs
{
    public int OldPrice { get; }
    public int NewPrice { get; }

    public PriceChangedEventArgs(int oldPrice, int newPrice)
    {
        OldPrice = oldPrice;
        NewPrice = newPrice;
    }
}
```

### 2. Define the Event
Use the `event` keyword with the `EventHandler<T>` delegate.

```csharp
public class Price
{
    private int _currentPrice;
    public event EventHandler<PriceChangedEventArgs> PriceChanged;

    public void UpdatePrice(int newPrice)
    {
        int oldPrice = _currentPrice;
        _currentPrice = newPrice;

        // 3. Raise the event
        OnPriceChanged(new PriceChangedEventArgs(oldPrice, newPrice));
    }

    protected virtual void OnPriceChanged(PriceChangedEventArgs e)
    {
        // Use the null-conditional operator to raise the event safely
        PriceChanged?.Invoke(this, e);
    }
}
```

## Subscribing to Events
Other classes can "subscribe" to the event using the `+=` operator.

```csharp
var price = new Price();
price.PriceChanged += (sender, e) => {
    Console.WriteLine($"Price changed from {e.OldPrice} to {e.NewPrice}");
};

price.UpdatePrice(100);
```

### Summary of the Pattern:
1.  **Broadcaster**: The class that sends the event.
2.  **Subscriber**: The class that receives the event.
3.  **EventArgs**: The object containing the event data.
4.  **EventHandler**: The delegate that defines the signature for the event handler method.
