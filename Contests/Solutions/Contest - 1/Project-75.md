## Explanation : 


The program calculates how many numbers with exactly 75 divisors exist as divisors of N!.

1. Prime Factorization Using Sieve:
	- We precompute the smallest prime factor for numbers up to MAX_N using the Sieve of Eratosthenes.
    
 2. Factorizing N! (Counting Prime Exponents):
    - We iterate over numbers from 1 to N and decompose them into their prime factors.
    - We maintain a count of how many times each prime appears in the factorization of N!.
    
 3. Counting Valid Numbers:
    - Using combinatorial logic, we count numbers whose divisors satisfy exactly 75.
    - We consider different cases, such as (74+1), (24+1, 2+1), (14+1, 4+1), and (4+1, 4+1, 2+1).
    
 4. Final Computation:
    - Using a function to count primes appearing with an exponent of at least X, we compute valid combinations.
    - The final count is printed as output.


## Solution in c++:


```cpp	
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int MAX_N = 101; // Maximum value of N (given constraint)
vector<int> smallestPrimeFactor(MAX_N); // Stores the smallest prime factor for each number

// Sieve of Eratosthenes to precompute smallest prime factors
void computeSmallestPrimeFactors() {
    for (int i = 2; i < MAX_N; i++) {
        if (smallestPrimeFactor[i] == 0) { // If i is prime
            smallestPrimeFactor[i] = i;
            for (int j = 2 * i; j < MAX_N; j += i) {
                if (smallestPrimeFactor[j] == 0) 
                    smallestPrimeFactor[j] = i;
            }
        }
    }
}

void countOMEGAs() {
    int N;
    cin >> N;
    map<int, int> primeExponentCount; // Stores the count of prime factors in N!

    // Factorize all numbers from 1 to N
    for (int i = 1; i <= N; i++) {
        int num = i;
        while (num > 1) {
            primeExponentCount[smallestPrimeFactor[num]]++;
            num /= smallestPrimeFactor[num];
        }
    }

    // Function to count how many primes have exponent >= X
    auto countPrimesWithExponentAtLeast = [&](int exponent) {
        int count = 0;
        for (auto [prime, exponentValue] : primeExponentCount) {
            if (exponentValue >= exponent) count++;
        }
        return count;
    };

    // Calculate the number of valid 75-divisor numbers
    ll totalWays = 0;
    totalWays += countPrimesWithExponentAtLeast(74);
    totalWays += countPrimesWithExponentAtLeast(24) * (countPrimesWithExponentAtLeast(2) - 1);
    totalWays += countPrimesWithExponentAtLeast(14) * (countPrimesWithExponentAtLeast(4) - 1);
    totalWays += (countPrimesWithExponentAtLeast(4) * (countPrimesWithExponentAtLeast(4) - 1) / 2) * (countPrimesWithExponentAtLeast(2) - 2);
    
    cout << totalWays << '\n';
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    
    computeSmallestPrimeFactors(); 
    countOMEGAs();
    
    return 0;
}
```





## Soluton in python : 
```python
import sys
from collections import defaultdict

MAX_N = 101  # Maximum value of N (given constraint)
smallest_prime_factor = [0] * MAX_N  # Stores the smallest prime factor for each number

def compute_smallest_prime_factors():
    for i in range(2, MAX_N):
        if smallest_prime_factor[i] == 0:  # If i is prime
            smallest_prime_factor[i] = i
            for j in range(2 * i, MAX_N, i):
                if smallest_prime_factor[j] == 0:
                    smallest_prime_factor[j] = i

def count_OMEGAs():
    N = int(input().strip())
    prime_exponent_count = defaultdict(int)  # Stores the count of prime factors in N!

    # Factorize all numbers from 1 to N
    for i in range(1, N + 1):
        num = i
        while num > 1:
            prime_exponent_count[smallest_prime_factor[num]] += 1
            num //= smallest_prime_factor[num]

    # Function to count how many primes have exponent >= X
    def count_primes_with_exponent_at_least(exponent):
        return sum(1 for exp in prime_exponent_count.values() if exp >= exponent)

    # Calculate the number of valid 75-divisor numbers
    total_ways = 0
    total_ways += count_primes_with_exponent_at_least(74)
    total_ways += count_primes_with_exponent_at_least(24) * (count_primes_with_exponent_at_least(2) - 1)
    total_ways += count_primes_with_exponent_at_least(14) * (count_primes_with_exponent_at_least(4) - 1)
    total_ways += (count_primes_with_exponent_at_least(4) * (count_primes_with_exponent_at_least(4) - 1) // 2) * (count_primes_with_exponent_at_least(2) - 2)
    
    print(total_ways)

if __name__ == "__main__":
    compute_smallest_prime_factors()
    count_OMEGAs()
```