# Problem Statement
You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

 

Example 1:

    Input: prices = [7,1,5,3,6,4]
    Output: 5
    Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
    Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
Example 2:

    Input: prices = [7,6,4,3,1]
    Output: 0
    Explanation: In this case, no transactions are done and the max profit = 0.
 

Constraints:

    1 <= prices.length <= 10^5
    0 <= prices[i] <= 10^4
 
# Solution 1
*Two pass Solution*
## Intuition
For each day if one knows what maximum value the Stock will reach after that day one can calculate the maximum profit he can earn if he buys the stock at that day (Max Stock Value After This Day - Stock Value At This Day), we figure out the highest profit one can earn on each day and compare the max profit of all days to figure out the max profit one can ever earn with that stock.

## Approach
1. Create an arrangement called Profit Tracker that must store the Max Profit one can earn for buying the stock on each day.
2. Start from the last day of the sequence of days and advance towards the first day.
3. As you progress from the last day to the first day of the sequence, Keep track of the Maximum value of stock encountered till now.
4. For Each Day Subtract that day's stock value from the Max Stock Value encountered till now. And store in the profit tracker arrangement.
5. Go through the Profit Tracker arrangement and take the maximum value out of it and return that value.


## Code

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        vector<int> profitTracker(prices.size());

        int maxStockValueTillNow = 0;

        for(int thatDay = (prices.size() - 1); thatDay >= 0; thatDay--) {
            maxStockValueTillNow = max(maxStockValueTillNow, prices[thatDay]);

            profitTracker[thatDay] = maxStockValueTillNow - prices[thatDay];
        }

        int maxProfit = 0;

        for(int day = 0; day < profitTracker.size(); day++) {
            maxProfit = max(maxProfit, profitTracker[day]);
        }

        return maxProfit;
    }
};
```

## Complexity

**Time Complexity: O(N)**

**Space Complexity: O(N)**

*Where:* 
- **N** is the number of stock values in the prices array.

<br/>
<br/>
<br/>

# Solution 2
Let's try decrease the auxiliary space that the program needs to run. *(One pass Solution)*
## Intuition
Intuition is the same as earlier 

## Approach
1. Start from the last day of the sequence of days and advance towards the first day. Keep track of the highest profit that could be earned out of all the days you have looked at.
2. As you progress from the last day to the first day of the sequence, Keep track of the Maximum value of stock encountered till now.
3. For Each Day Subtract that day's stock value from the Max Stock Value encountered till now. And update the maximum profit.
4. Return the Maximum Recorded Profit.


## Code

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxStockValueTillNow = 0;
        int maxProfit = 0;

        for(int thatDay = (prices.size() - 1); thatDay >= 0; thatDay--) {
            maxStockValueTillNow = max(maxStockValueTillNow, prices[thatDay]);

            int profitOnThisDay = maxStockValueTillNow - prices[thatDay];

            maxProfit = max(maxProfit, profitOnThisDay);
        }

        return maxProfit;
    }
};
```

## Complexity

**Time Complexity: O(N)**

**Space Complexity: O(1)**

*Where:* 
- **N** is the number of stock values in the prices array.
