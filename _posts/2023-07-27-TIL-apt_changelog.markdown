---
layout: post
title:  "apt changelog"
date:   2023-07-27 00:00:00 -0500
categories: [TIL, Linux, sysadmin, security, CVE]
---

TIL about `apt changelog <package-name>` it was pretty useful to confirm that I was patched for [CVE-2023-2650](https://nvd.nist.gov/vuln/detail/CVE-2023-2650)

```
root@blueberrypi:~# apt changelog openssl
openssl (3.0.2-0ubuntu1.10) jammy-security; urgency=medium

  * SECURITY UPDATE: DoS in AES-XTS cipher decryption
    - debian/patches/CVE-2023-1255.patch: avoid buffer overrread in
      crypto/aes/asm/aesv8-armx.pl.
    - CVE-2023-1255
  * SECURITY UPDATE: Possible DoS translating ASN.1 object identifiers
    - debian/patches/CVE-2023-2650.patch: restrict the size of OBJECT
      IDENTIFIERs that OBJ_obj2txt will translate in
      crypto/objects/obj_dat.c.
    - CVE-2023-2650
  * Replace CVE-2022-4304 fix with improved version
    - debian/patches/CVE-2022-4304.patch: use alternative fix in
      crypto/bn/bn_asm.c, crypto/bn/bn_blind.c, crypto/bn/bn_lib.c,
      crypto/bn/bn_local.h, crypto/rsa/rsa_ossl.c.
```

Much easier than trying to dig through the online sources there is also a `yum` version `yum-changelog`

Stay safe out there and make sure to Lock Your Laptop
![Lock Your Laptop](/images/lock.jpeg)