# Editorial for LOJ-1306: Solutions to an Equation

This is the classic question of finding the solutions for a linear Diophantine Equation.

```C++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
 
ll gcd(ll a, ll b, ll& x, ll& y) {
    if (b == 0) {
        x = 1;
        y = 0;
        return a;
    }
    ll x1, y1;
    ll d = gcd(b, a % b, x1, y1);
    x = y1;
    y = x1 - y1 * (a / b);
    return d;
}
void shift_solution(ll & x, ll & y, ll a, ll b, ll cnt) {
    x += cnt * b;
    y -= cnt * a;
}
bool find_any_solution(ll a, ll b, ll c, ll &x0, ll &y0, ll &g){
 
    g=gcd(abs(a), abs(b), x0, y0);
    if(c%g)     //if it's not 0 then ax+by=c has no integral solution
        return false;
    x0 *=c/g;
    y0 *=c/g;
    if(a<0) x0=-x0;
    if(b<0) y0=-y0;
    return true;
}
ll find_all_solutions(ll a, ll b, ll c, ll minx, ll maxx, ll miny, ll maxy) {
    ll x, y, g;
    if (!find_any_solution(a, b, c, x, y, g))
        return 0;
    a /= g;
    b /= g;
 
    ll sign_a = a > 0 ? +1 : -1;
    ll sign_b = b > 0 ? +1 : -1;
 
    shift_solution(x, y, a, b, (minx - x) / b);
    if (x < minx)
        shift_solution(x, y, a, b, sign_b);
    if (x > maxx)
        return 0;
    ll lx1 = x;
 
    shift_solution(x, y, a, b, (maxx - x) / b);
    if (x > maxx)
        shift_solution(x, y, a, b, -sign_b);
    ll rx1 = x;
 
    shift_solution(x, y, a, b, -(miny - y) / a);
    if (y < miny)
        shift_solution(x, y, a, b, -sign_a);
    if (y > maxy)
        return 0;
    ll lx2 = x;
 
    shift_solution(x, y, a, b, -(maxy - y) / a);
    if (y > maxy)
        shift_solution(x, y, a, b, sign_a);
    ll rx2 = x;
 
    if (lx2 > rx2)
        swap(lx2, rx2);
    ll lx = max(lx1, lx2);
    ll rx = min(rx1, rx2);
    if (lx > rx)
        return 0;
 
    return (rx - lx) / abs(b) + 1;
}
 
int main(){
    
    #ifndef ONLINE_JUDGE 
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    #endif
    
    cin.tie(0);
    ios_base::sync_with_stdio(false);
    
    ll a, b, c, x1, x2, y1, y2;
    cin>>a>>b>>c>>x1>>x2>>y1>>y2;
    
    //we are solving in Ax+By=C format but the i/p is in ax+by+c=0 format hence we reverse c's sign.
    c=-c;
    if(a==0 && b==0 && c==0) cout<<(x2-x1+1)*(y2-y1+1)<<'\n';
    else if(a==0 && b==0) cout<<0<<'\n';
    else if(a==0){
        if(c%b!=0 || y1>c/b || y2<c/b) cout<<0;
        else cout<<(x2-x1+1);
        cout<<'\n';
    }
    else if(b==0){
        if(c%a!=0 || x1>c/a || x2<c/a) cout<<0;
        else cout<<(y2-y1+1);
        cout<<'\n';
    }
    else cout<<find_all_solutions(a, b, c, x1,x2,y1,y2)<<'\n';

        
        
    
    return 0;
}
```
