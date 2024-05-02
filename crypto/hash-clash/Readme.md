The custom hash function iterates over the characters one by one and updates the hash value.

This means that if we find a collision of length 2, for instance, we can add 62 of the same characters to each string, and they will remain collided.

We find a collision of length 2 with the python script below:

```rb
import random
import string

def custom_hash(key):
    hash_value = 5381
    for char in key:
        hash_value = ((hash_value << 5) + hash_value) + ord(char)
    return hash_value

hashes = {}
collision_found = False
while not collision_found:
    # Generate a random alphanumeric string
    key = ''.join(random.choices(string.ascii_letters + string.digits, k=2))
    # Calculate hash
    hash_value = custom_hash(key)
    if hash_value in hashes:
        collision_found = True
        print(f"Collision found for hashes {hashes[hash_value]} and {key}")
    else:
        hashes[hash_value] = key

```

Output: `Collision found for hashes Bf and CE`

The following strings will also collide:
```
Bfaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
CEaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

Giving these to the server gives us the flag: `SUCTF{b3w4r3_0f_c0ll1510n_4774ck5}`