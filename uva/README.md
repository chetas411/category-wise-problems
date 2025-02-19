## DP _(2-states, 0-1 knapsack)_

1. [11450 - Wedding shopping](https://onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=2445)
    - Problem require us to find the maximum amount of dresses(have multiple brands) in given amount of money.
    - Kind of 0-1 Knapsack
    - States are, `money and index_of_brands`
      <details>
      <summary>Code sample </summary>
      
      ```cpp
       /*
        * price being the 2d-array storing data
        * memo being 2d-array used for memoization.
        */
  
       int ourFunction(CurrentMoney, index) {
        if (CurrentMoney < 0) 
          return -INF; /* INF, being very large values. */
                             
        if (index == TotalBrands) 
          return TotalMoney - CurrentMoney; 

        int &ans = memo[money][g];
                      
        if (ans != -1) 
          return ans;
        
        /* Loop isnt the part of the template, loop is just to access data from the given 2d data, 
         * if data were 1d then we would have gone without loop 
         */
        for (int brand = 1; brand <= price[index][0]; brand++)  /* at index,0 we have stored the size of that specific brand. */
          ans = max(ans, ourFunction(CurrentMoney - price[index][CurrentMoney], index + 1)); 
  
        return ans;
       }
  
      ```

      </details>




2. [562 - Dividing coins](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=503)
    - States are, `index and currentSum`
    - Variation of the above problem.
      <details>
      <summary>Code sample </summary>
      
      ```cpp
        const int UNKNOWN = -1;
        const int HIGHEST_POS_FOR_COIN_VAL = 501;

        int totalCoins;
        int memo[105][105 * 501];
        int coins[105];

        int dp(int index, int sum) {
          if (index == totalCoins)
            return sum;

          int &ans = memo[index][sum];

          if (ans == -1)
            ans = min(dp(index + 1, sum + coins[index]),
                      dp(index + 1, abs(sum - coins[index])));

          return ans;
        }

        int main() {
          int T;
          cin >> T;

          while (T--) {
            cin >> totalCoins;
            int highest = HIGHEST_POS_FOR_COIN_VAL * totalCoins;

            for (int i = 0; i < totalCoins; ++i) {
              cin >> coins[i];
              for (int j = 0; j < highest; ++j) {
                memo[i][j] = UNKNOWN;
              }
            }

            cout << dp(0, 0) << '\n';
          }
        }
      ```

      </details>
    
    
2. [1213 - Sum of Different Primes](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=3654)
    - first 3 state dp problem of mine.
    - States are, `index, n, k` _(not hard to realize)_.
    - Variation of the above problem, base on 01 knapsack.
      <details>
      <summary>Code sample </summary>
      
      ```cpp
        /*
         * arePrimes store prime no.s till 1120 because that was the limit given in the que. 
         * memo used to memoization purpose. 
         */
          
        vector<vector<vector<int>>> memo;
        int dp(int n, int k, int i) {
          if (n == 0 and k == 0)
            return 1;
          if (n == 0 or k == 0)
            return 0;
          if (n < 0 or k < 0)
            return 0;

          if (arePrimes[i] > n)
            return 0;

          int &ans = memo[n][k][i];
          if (ans != -1)
            return ans;
          ans = dp(n - arePrimes[i], k - 1, i + 1) + dp(n, k, i + 1);
          return ans;
        }

        void solve() {
          int n, k;
          while (cin >> n >> k) {
            if (n == 0 and k == 0)
              return;
            memo.resize(1121);
            for (auto &i : memo) {
              i = vvi(15, vi(200, -1));
            }

            cout << dp(n, k, 0) << '\n';
          }
        }                                      
      ```

      </details>









## DP _(1-state, Coin change)_

1. [147-Dollars](https://onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=83)
   - whatif we wish to calculate no. of solutions
   - Solution mentioned below didn't work on mentioned problem, **WHY?** (answered in second mentioned) 
   - given that each combination is counted differently
   - States are only in this case, `remaining money`
   - Pay attention to the for loop in the sample code's recursive function.
   - What this doing is running the loop from 0, hence forcing it to count every possible combinations.
   - If we don't wish to do so, we will have to pass the index from where it should continue counting. 
     <details>
     <summary>Code sample memoization version</summary>

     ```cpp
      vector<int> memo;
      vector<int> coins{1, 2, 3};
      
      int dp(int N) {
         print();
         if (N < 0)
         return 0;
         
         int &ans = memo[N];
         
         if (ans != 0)
            return ans;
         
         for (const auto &i : coins) {
            ans += dp(N - i);
         }
         return ans;
      }
      
      void solve() {
         double n;
         while (cin >> n) {
            int N = ((n + 0.001) * 100);
            if (N == 0)
               return;
      
             memo = vector<int> (40000, 0);
             memo[0] = 1;
     
             // consider N to be 4 
             cout << dp(N) << '\n';
         }
      }


     ```

     </details>



## DP _(2-states, Coin change)_

1. [147-Dollars](https://onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=83)
   - True coin change problem via memoization.
   - States as discussed in above one are two, `remainging money, current index`
   - advised to use `memo 2d array` such as **LESS ROWS, MORE COLS**, *i may be wrong*
   - surprisingly if `memo 2d array` declared as mentioned, code will be faster.
     <details>
     <summary>Code sample memoization version</summary>

     ```cpp
      ll dp(int N, int index) {
      if (N == 0) return 1;
      if (index >= coins.size() or N < 0) return 0;
      
      ll &ans = memo[index][N];
      
      if (ans != 0)
      return ans;
      
      ans = dp(N - coins[index], index) + dp(N, index + 1);
        return ans;
      }
      
      void solve() {
        double n;
        memo = vvll(11, vll(30001, 0));
        memo[0][0] = 1;
        int ans = dp(300, 0);
        
        while (cin >> n) {
            int N = ((n + 0.001) * 100);
            if (N == 0) return;
            
            int ans = memo[10][N];
            cout << std::fixed;
            cout << setprecision(2) << setw(3) << n;
            cout << setw(17) << dp(N, 0) << '\n';
        }
      }
     ```

     </details>



