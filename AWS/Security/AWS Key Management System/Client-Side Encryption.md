
KMS sử dụng cơ chế **Envelope encryption** để tao mã hóa 2 lớp

Các bước decrypt data:

1. Use the `GenerateDataKey` operation to get a pair of plaintext data key và encrypted data key (**ciphertext**)
   
2. Use the plaintext data key to encrypt data locally, then erase the plaintext data key from memory.

3. Store the encrypted data key or **ciphertext** alongside the locally encrypted data.  

To decrypt data locally:

1. Use the Decrypt operation to decrypt the encrypted data key. The operation returns a plaintext copy of the data key.

2. Use the plaintext data key to decrypt data locally, then erase the plaintext data key from memory.  

Using the Decrypt operation to decrypt the plaintext data key is incorrect because there is no need to decrypt a plaintext, or 'unencrypted', data key.
