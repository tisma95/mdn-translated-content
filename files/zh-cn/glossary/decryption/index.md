---
title: 解密
slug: Glossary/Decryption
---

{{GlossarySidebar}}

在密码学（{{glossary("cryptography")}}）领域，**解密**是指把密文（{{glossary("ciphertext")}}）转换成明文（{{glossary("Plaintext")}}）的过程。

解密是一个密码学原语：它通过一种称作{{glossary("cipher")}}的编码技术，把加密信息转换成纯文本。正如加密一样，解密在当代密码学领域也是通过特有的算法，结合称作{{glossary("key")}}的密钥来工作。由于算法通常是公开的，若要保证加密安全，就必须确保密钥高度保密。

![The decryption primitive.](decryption.png)

解密是{{glossary("encryption","加密")}}的逆过程，如果密钥高度保密，在没有特定密钥的前提下从数学计算的角度来看，解密是很难做到的。这就取决于算法有多坚固，以及{{glossary("cryptanalysis","密码分析学")}}发展到了什么程度。

## 参见

- [加密与解密](/zh-CN/docs/Encryption_and_Decryption)
