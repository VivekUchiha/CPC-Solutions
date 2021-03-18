# Not A Palindrome (Typing Skill)
The string cannot be transformed into an Anti-Palindrome if and only if all the characters of the string are same. So in this case output will be -1. As we can swap any two adjacent character any number of times we can get the lexicograpically largest string by this operation. So we will sort the string and then reverse it to get the lexicograpically largest string.

```
string solve (string S) {
   sort(S.begin(),S.end(),greater<char>());
   if(S[0]==S.back()){
       return "-1";
   }
   else{
       return S;
   }
}
```
# Hulk in the Gym
All the problem asks you to do is to break down the sequence into segments which are either non increasing or non decreasing while minimising the number of segments. This can be done by just adding partitions at points where the sign of a[i] - a[i-1] changes. (Hills and Valleys)
This can be implemented without hassle if you remove adjacent elements which are duplicate.
```
int minDays (vector<int> calories) {
   long long a,b;
   int ans = 1;
   //Removing adjacent duplicates
   calories.resize(unique(calories.begin(), calories.end()) - calories.begin()); 
   for(int i=2;i<calories.size();i++){
      a = calories[i] - calories[i-1];
      b = calories[i-1] - calories[i-2];
      if(a*b<0){
         ans++;
         i++;
      }
   }
   return ans;
}
```
# Count the spiders (Graph Basics)
For every node, if we consider it as the center(node A), then the problem reduces to finding number of possible pairs of neighbours at a distance 2 from it(every pair will correspond to 1 spider).

Consider a center node. We can iterate to all the vertices connected to it and push the corresponding number of  neighbours at a distance equal to 2 reachable from the center in a vector. To find the total number of spiders at that center, we have to find the product of every pair of integers in the vector.
```
int solve (int N, vector<vector<int>> Edges) {
   vector<vector<int>> adj(N+1);
   for(int i=0;i<N-1;i++){
      adj[Edges[i][0]].push_back(Edges[i][1]);
      adj[Edges[i][1]].push_back(Edges[i][0]);
   }
   const long long mod = 1e9 + 7;
   long long ans = 0;
   for(int i=1;i<=N;i++){
      long long total = 0;
      for(auto it:adj[i]){
         long long val = adj[it].size()-1;
         ans += (total)*val;
         ans%=mod;
         total += val;
      }
   }
   return ans;
}

```

# Divisor Count (Hard, Priority Queues / Heaps) (Can be skipped)
Let N = p1^a1	\*p2^a2	*......*pn^an. where p1,p2,...pn are primes.  
  
Count of Divisors C = (a1+1)(a2+1)....(an+1). For C to be power of 2, a(i) should be 1, 3, 7, ....  
  
When ai = 1, (ai+1) = 2 = 2^1  
ai = 3, (ai + 1) = 4 = 2^2  
ai = 7, (ai + 7) = 8 = 2^3 and so on.  
  
Which means if prime p is a factor of smallest such N (a1 = 1) . Then next we need to multiply (p\*p) ( a1 = 3 ) . Then p\*p\*p\*p ( a1 = 7).  
  
So we can store all primes less than 2\*10^6, using Sieve Algorithm in a priority_queue.  
Iteratively select the smallest number in priority_queue say p and multiply it with N, and add corresponding next available power in priority_queue.
