Since `p` is relatively small, we can brute-force one of the private keys, then obtain the shared key.

Since `g^a % p = A`, we will brute-force the `a` value, then get the shared secret with `B^a % p`

```python
def bruteforce_shared_secret(p, g, A, B):
    for a in range(1, p):
        if pow(g, a, p) == A:
            return pow(B, a, p)

p = 58240601
g = 5
A = 37139928
B = 29910539

shared_secret = calculate_shared_secret(p, g, A, B)
print(f"Shared secret: {shared_secret}")
```

Flag: `SUCTF{43984826}`