The file given at the challenge hints at Shamir's secret sharing with the parameters:
```
s1 = (1, 16574496934189166437989760024109621307864803687799947299613475428753) s2 = (2, 35023148766534734627803737865041674640657715166261601865167078885631)
s3 = (3, 64121950871223733865784458628075948492933127043287128307688163525575)
P = 8775995374187029296342525105279788494554392607902164611027353155027
```

The code below will allow us to reconstruct the secret.

```python
from sympy import mod_inverse

shares = [(1, 16574496934189166437989760024109621307864803687799947299613475428753), (2, 35023148766534734627803737865041674640657715166261601865167078885631), (3, 64121950871223733865784458628075948492933127043287128307688163525575)]
P = 8775995374187029296342525105279788494554392607902164611027353155027

def reconstruct_secret(shares, prime):
    x_coords, y_coords = zip(*shares)
    x_coords = list(x_coords)
    y_coords = list(y_coords)

    secret = 0
    for i in range(len(x_coords)):
        numerator = 1
        denominator = 1
        for j in range(len(x_coords)):
            if i != j:
                numerator *= -x_coords[j]
                denominator *= x_coords[i] - x_coords[j]
        term = y_coords[i] * numerator * mod_inverse(denominator, prime)
        secret = (secret + term) % prime
    return secret

secret = reconstruct_secret(shares, P)
print(secret.to_bytes((secret.bit_length() + 7) // 8, 'big').decode())
```

Flag: `SUCTF{5h4m1r5_d1r7y_53cr375}`