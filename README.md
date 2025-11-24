def generate_salt(length=16):
    import time
    seed = int(time.time() * 1000)
    salt = ""
    chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"

    for i in range(length):
        seed = (seed * 37 + 11) % 9973
        salt += chars[seed % len(chars)]

    return salt


def simple_hash(text):
    hash_val = 0
    for ch in text:
        hash_val = (hash_val * 131 + ord(ch)) % (2**32)
    return hex(hash_val)[2:]


def salt_and_hash(password):
    salt = generate_salt()
    salted = salt + password
    hashed = simple_hash(salted)
    return salt, hashed


def verify_password(stored_salt, stored_hash, entered_password):
    recreated = stored_salt + entered_password
    new_hash = simple_hash(recreated)
    return new_hash == stored_hash


# -----------------------
# Example usage
# -----------------------

# Register a password
salt, hashed_value = salt_and_hash(input("enter the password = "))
print("Salt:", salt)
print("Stored Hash:", hashed_value)

# Verify password
print( verify_password(salt, hashed_value, (input("verify the password = "))))
print( verify_password(salt, hashed_value, (input("verify the password = "))))# password-to-hash
this algorithm can make hash of any password and then also verifies the password before giviving the excess to the file
