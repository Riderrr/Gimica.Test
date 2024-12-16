# Test task answers

**Problem 1:** 
The most obvious approach to moving an object with linear speed with less code:

```csharp
public class Mover : MonoBehaviour
{
    public Vector3 target;
    public float speed = 1.0f;
    
    private const float MoveStopThreshold = 0.1f;

    void Update()
    {
        if (Vector3.Distance(transform.position, target) <= MoveStopThreshold)
        {
            return;
        }

        transform.position = Vector3.MoveTowards(transform.position, target, speed * Time.deltaTime);
    }
}

```

The next version will be faster. Here, we can avoid calculating the square root to check the distance.

```csharp
public class Mover : MonoBehaviour
{
    Vector3 target;
    public float speed = 1.0f;

    private const float MoveStopThreshold = 0.1f;
    private float _moveStopThresholdSqr;

    private void Start()
    {
        _moveStopThresholdSqr = MoveStopThreshold * MoveStopThreshold;
    }

    void Update()
    {
        Vector3 direction = target - transform.position;
        float distanceSqr = direction.sqrMagnitude;

        if (distanceSqr <= _moveStopThresholdSqr)
        {
            return;
        }

        transform.position += direction.normalized * (speed * Time.deltaTime);
    }
}
```

**Problem 2:** 

Here’s the link to projects GitHub: [GitHub link](https://github.com/Riderrr/Unity.Gimica.Test)

And this is WebGl build for faster test: [Preview link](https://riderrr.github.io/Gimica.Test.Preview/)

**Problem3:** 

Here’s the solution:

```csharp
public class Solution
{
    private const int MaxPricesCount = 105;
    private const int MinPriceValue = 0;
    private const int MaxPriceValue = 104;

    public int MaxProfit(int[] prices)
    {
        if (prices.Length > MaxPricesCount)
        {
            Debug.LogWarning("Prices count is too big. Max allowed count is " + MaxPricesCount +
                             "We will use only first " + MaxPricesCount + " prices");
            Array.Resize(ref prices, MaxPricesCount);
        }

        if (prices.Length <= 1)
        {
            Debug.Log("There are no prices to calculate profit");
            return 0;
        }

        var minPrice = int.MaxValue;
        var maxProfit = 0;

        for (var i = 0; i < prices.Length; i++)
        {
            if(prices[i] < MinPriceValue || prices[i] > MaxPriceValue)
            {
                Debug.LogWarning("Price value is out of range. Allowed range is from " + MinPriceValue + " to " + MaxPriceValue);
                return 0;
            }
            
            if (prices[i] < minPrice)
            {
                minPrice = prices[i];
                maxProfit = 0;
            }
            else if (prices[i] - minPrice > maxProfit)
            {
                maxProfit = prices[i] - minPrice;
            }
        }

        return maxProfit;
    }
}
```