# 1. Encoding

## Definition

Encoding converts data from one format to another so it can be transmitted or stored correctly.

It is **not meant for security**.

Anyone who knows the encoding scheme can decode it.

## Common Encoding Types

- Base64
- URL Encoding
- ASCII
- UTF-8
- Unicode
- HTML Encoding

# 2. Encryption

## Definition

Encryption converts readable data into unreadable data using a key.

Only someone with the correct key can recover the original data.

## Purpose

Protect sensitive information.

Examples

- Bank transactions
- HTTPS
- WhatsApp messages
- Credit cards
- Medical records

# 3. Hashing

## Definition

Hashing converts data into a fixed-size value.

It is **one-way**.

You cannot recover the original input from the hash.

### Why do websites hash passwords instead of encrypting them?

Because websites don't need to know your original password. They only need to verify that the password you entered matches the stored hash. Hashing limits the damage if the password database is compromised.