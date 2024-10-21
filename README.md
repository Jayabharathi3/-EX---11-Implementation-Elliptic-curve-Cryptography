# EX - 11 : Implementation of Elliptic curve Cryptography

## AIM:
  Implementation of Elliptic Curve Cryptography using a Standard Library.

## ALGORITHM:
1.Start the program and import the required libraries.
2.Define the elliptic curve parameters (a, b) and prime modulus (p) that define the curve 
(y^2=x^3+ax+b)
3.Implement the point addition and point doubling operations for elliptic curves.
4.Implement scalar multiplication to perform cryptographic operations.
5.Use the scalar multiplication function to generate a public key from a private key.
6.Use ECC to perform key exchange or encryption.
7.End the program.

## PROGRAM:

```C
#include <stdio.h>

// Define the elliptic curve parameters: y^2 = x^3 + ax + b
#define CURVE_A 2
#define CURVE_B 3
#define PRIME_P 17 

// Define a structure for points on the elliptic curve
typedef struct {
    int x;
    int y;
} Point;

// Base point G on the curve
Point G = {5, 1};

// Function to perform modular inverse
int modInverse(int k, int prime) {
    for (int x = 1; x < prime; x++) {
        if ((k * x) % prime == 1)
            return x;
    }
    return -1;
}

// Function to perform point addition on the elliptic curve
Point pointAdd(Point P, Point Q) {
    Point R;
    int slope = ((Q.y - P.y) * modInverse(Q.x - P.x, PRIME_P)) % PRIME_P;

    R.x = (slope * slope - P.x - Q.x) % PRIME_P;
    R.y = (slope * (P.x - R.x) - P.y) % PRIME_P;

    if (R.x < 0) R.x += PRIME_P;
    if (R.y < 0) R.y += PRIME_P;

    return R;
}

// Function for scalar multiplication: n*P (repeated addition)
Point scalarMult(Point P, int n) {
    Point result = P;
    for (int i = 1; i < n; i++) {
        result = pointAdd(result, P);
    }
    return result;
}

int main() {
    prin
    int privateKey = 7;  // A simple private key
    Point publicKey = scalarMult(G, privateKey);  // Public key = privateKey * G

    // Display the private and public keys
    printf("Private Key: %d\n", privateKey);
    printf("Public Key: (%d, %d)\n", publicKey.x, publicKey.y);

    // Define a message point on the curve
    Point message = {6, 3};
    printf("Original Message: (%d, %d)\n", message.x, message.y);

    // Encrypt: C1 = k*G, C2 = M + k*Q
    int k = 5;  // Random value
    Point C1 = scalarMult(G, k);
    Point C2 = pointAdd(message, scalarMult(publicKey, k));
    printf("Ciphertext C1: (%d, %d)\n", C1.x, C1.y);
    printf("Ciphertext C2: (%d, %d)\n", C2.x, C2.y);

    // Decrypt: M = C2 - d*C1
    Point temp = scalarMult(C1, privateKey);
    temp.y = -temp.y % PRIME_P;  
    if (temp.y < 0) temp.y += PRIME_P;

    Point decryptedMessage = pointAdd(C2, temp);
    printf("Decrypted Message: (%d, %d)\n", decryptedMessage.x, decryptedMessage.y);

    return 0;
}
    
```

## OUTPUT:
![image](https://github.com/user-attachments/assets/87a9cebc-cef6-461f-bc63-317e0f9a0273)


## RESULT:
The implementation of Elliptic Curve Cryptography using a standard library is successful.
