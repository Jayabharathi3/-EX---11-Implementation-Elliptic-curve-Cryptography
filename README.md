# EX - 11 : Implementation of Elliptic curve Cryptography

## AIM:
  Implementation of Elliptic Curve Cryptography using a Standard Library.

## ALGORITHM:
### STEP-1:
Start the program and import the required libraries.
### STEP-2:
Define the elliptic curve parameters (a, b) and prime modulus (p) that define the curve 
(y^2=x^3+ax+b)
### STEP-3:
Implement the point addition and point doubling operations for elliptic curves.
### STEP-4:
Implement scalar multiplication to perform cryptographic operations.
### STEP-5:
Use the scalar multiplication function to generate a public key from a private key.
### STEP-6:
Use ECC to perform key exchange or encryption and end the program.

## PROGRAM:

```C
 #include <stdio.h>
struct Point 
{
    long long x;
    long long y;
};

long long mod_inverse(long long a, long long p) 
{
    long long m0 = p, t, q;
    long long x0 = 0, x1 = 1;
    if (p == 1) return 0;
    while (a > 1) 
    {
        q = a / p;
        t = p;
        p = a % p;
        a = t;
        t = x0;
        x0 = x1 - q * x0;
        x1 = t;
    }
    if (x1 < 0) x1 += m0;
    return x1;
}

struct Point point_addition(struct Point P, struct Point Q, long long a, long long p)
{
    struct Point R;
    if (P.x == Q.x && P.y == Q.y) 
    {
        long long s = (3 * P.x * P.x + a) * mod_inverse(2 * P.y, p) % p;
        R.x = (s * s - 2 * P.x) % p;
        R.y = (s * (P.x - R.x) - P.y) % p;
    } 
    else 
    {
        long long s = (Q.y - P.y) * mod_inverse(Q.x - P.x, p) % p;
        R.x = (s * s - P.x - Q.x) % p;
        R.y = (s * (P.x - R.x) - P.y) % p;
    }
    if (R.x < 0) R.x += p;
    if (R.y < 0) R.y += p;
    return R;
}

struct Point scalar_multiplication(struct Point P, long long k, long long a, long long p) 
{
    struct Point R = P;
    k = k - 1; 
    while (k > 0)
    {
        if (k % 2 == 1) 
        {
            R = point_addition(R, P, a, p);
        }
        P = point_addition(P, P, a, p);
        k /= 2;
    }
    return R;
}

int main() 
{
    long long a, b, p;  
    struct Point G;     
    long long private_key;
    struct Point public_key;
    printf("Enter the value of 'a' for the elliptic curve equation y^2 = x^3 + ax + b (mod p): ");
    scanf("%lld", &a);
    printf("Enter the value of 'b' for the elliptic curve equation y^2 = x^3 + ax + b (mod p): ");
    scanf("%lld", &b);
    printf("Enter the prime number 'p' for the finite field (modulus): ");
    scanf("%lld", &p);
    printf("Enter the x-coordinate of the base point G: ");
    scanf("%lld", &G.x);
    printf("Enter the y-coordinate of the base point G: ");
    scanf("%lld", &G.y);
    printf("Enter the private key: ");
    scanf("%lld", &private_key);
    public_key = scalar_multiplication(G, private_key, a, p);
    printf("\nElliptic Curve: y^2 = x^3 + %lldx + %lld (mod %lld)\n", a, b, p);
    printf("Base Point G: (%lld, %lld)\n", G.x, G.y);
    printf("Private Key: %lld\n", private_key);
    printf("Public Key: (%lld, %lld)\n", public_key.x, public_key.y);
    return 0;
}
    
```

## OUTPUT:
![image](https://github.com/user-attachments/assets/8595b593-b6a4-4ba9-bfdb-71837d6cd126)


## RESULT:
The implementation of Elliptic Curve Cryptography using a standard library is successful.
