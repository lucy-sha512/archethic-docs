---
id: tpm
title: TPM Implementation
---

This section explains the HRT TPM Library implementation. 
:::success
Reference Files:
[uniris-tpm.c](https://github.com/UNIRIS/tpm-core/blob/main/uniris-tpm.c)
[uniris-tpm.h](https://github.com/UNIRIS/tpm-core/blob/main/uniris-tpm.h)
:::

## Global Variables
:::warning
The global variables are defined as static to maintain the static lifecycle of the global variable, to prevent data leak and external access of the variables.
:::

## void keyToASN():
:::info
This function converts raw elliptical public key generated by TPM   to ASN1 DER encoding. 
:::


TPM generated uncompressed public key do not include the curve information required for elliptic key cryptography. keyToASN() encodes the raw public key by appending curve information to it.

The ASN DER Public Key is an outer structure which contains 2 inner structures. First inner structure having key type and curve type and second inner structure containning the raw key. The structure containning public key is a header containing [0x00 0x04 x coordinate y coordinate].

The following structure  is the format of ASN DER: 
:::danger
ASN DER Public Key = [ [ [keytype] [curvetype] ]  [publickey] ]
:::

**Logic Flow:**
The function adds the headers squentially and then the raw x coordinate of public key and then the y coordinate finally the size is updated.

## void signToASN(BYTE *r, INT sizeR, BYTE *s, INT sizeS, INT *asnSignSize)
:::success
Converts uncompressed signature values to ASN DER format.
:::

TPM generates the raw signature in form of integer values : R & S. signToASN() converts these raw values  into ASN DER format. It first prepends the ASN sequence then checks the MSB of R .  If it is 1 then it prepends a byte (0) to R otherwise it move on to increase the index pointed to the asn by the size of R.
Similarly, it does the above for S.


## void generatePublicKey(INT keyIndex)

:::warning
Generates public key on the endorsement key hierarchy of TPM by taking one byte key index as input.
:::
 
Firstly, the template of the public key is defined in the inPublicECC which contains the endorsement key template such that certificate on the keys can be generated except modifying the endorsement key object attributes. 
The inPublicECC structure defines the following sub-structure:
1.  publicArea : defines the attributes of the public key to be generated. For endorsement key the signing operation is restricted due to privacy concern, defined under this structure.  In this case we are generating key in the endordement hierarchy by using the template of the endorsement key.
* The object attributes of generating key are as follows:


    * TPMA_OBJECT_USERWITHAUTH: Signifies the approval of "USER" actions with associated with the public key with a password.
    * TPMA_OBJECT_ADMINWITHPOLICY: Signifies the Approval of "ADMIN" role actions with this object may only be done with a policy session.
    * TPMA_OBJECT_SIGN_ENCRYPT: For a symmetric cipher object, the private portion of the key be used to encrypt. For other objects, the private portion of the key can be used to sign.
    * TPMA_OBJECT_DECRYPT:The private portion of the key can be used to decrypt
    * TPMA_OBJECT_FIXEDTPM: Indicates that the hierarchy of the key genrated cannot be changed.
    * TPMA_OBJECT_FIXEDPARENT:Indicates that the parent of the object cannot be changed.
    * TPMA_OBJECT_SENSITIVEDATAORIGIN: Indicates that the sensitive data is generated by the TPM on the key generation except the authvalue.


```
.objectAttributes = (TPMA_OBJECT_USERWITHAUTH |
                                 TPMA_OBJECT_ADMINWITHPOLICY |
                                 TPMA_OBJECT_SIGN_ENCRYPT |
                                 TPMA_OBJECT_DECRYPT |
                                 TPMA_OBJECT_FIXEDTPM |
                                 TPMA_OBJECT_FIXEDPARENT |
                                 TPMA_OBJECT_SENSITIVEDATAORIGIN),

```
* Object attributes for generating under Endorsement key hierarchy:
    * TPMA_OBJECT_RESTRICTED:  Key usage is restricted to manipulate structures of known format.

:::info
Endorsement key has same template except that there is no SIGN_ENCRYPT FLAG in the object attribute.
:::


```
.objectAttributes = (TPMA_OBJECT_RESTRICTED |
                                 TPMA_OBJECT_ADMINWITHPOLICY |
                                 TPMA_OBJECT_DECRYPT |
                                 TPMA_OBJECT_FIXEDTPM |
                                 TPMA_OBJECT_FIXEDPARENT |
                                 TPMA_OBJECT_SENSITIVEDATAORIGIN),
```


2.   authPolicy: this substructure contains a 32 byte buffer with values exactly same  as endorsement key attributes. This parameter associates the generated key template to the TPM hence during certificate generation the CA is able to return the certificate for the public key generated under Endorsement key hierarchy.

```
.authPolicy = {
                .size = 32,
                .buffer = {0x83, 0x71, 0x97, 0x67, 0x44, 0x84, 0xB3, 0xF8, 0x1A, 0x90, 0xCC,
                           0x8D, 0x46, 0xA5, 0xD7, 0x24, 0xFD, 0x52, 0xD7, 0x6E, 0x06, 0x52,
                           0x0B, 0x64, 0xF2, 0xA1, 0xDA, 0x1B, 0x33, 0x14, 0x69, 0xAA}},
```

4.    parameters: In the parameter structure we define the algorithm to be used for private key cryptography and public key cryptography operations.
```
 .parameters.eccDetail = {.symmetric = {
                                         .algorithm = TPM2_ALG_AES,
                                         .keyBits.aes = 128,
                                         .mode.sym = TPM2_ALG_CFB,
                                     },
                                     .scheme = {.scheme = TPM2_ALG_NULL, .details = {.ecdsa = {.hashAlg = TPM2_ALG_SHA256}}},
                                     .curveID = TPM2_ECC_NIST_P256,
                                     .kdf = {.scheme = TPM2_ALG_NULL, .details = {}}},
```

 After definning the template of the public key, a unique data is passed to each key in the unique structure of inPublicEC which is root key hash and key index. For the root key the root key hash is 0 and key index is 0. 
 
```
.unique.ecc = {.x = {.size = 32, .buffer = {0}}, .y = {.size = 32, .buffer = {0}}},
```
The primary key is created by using Esys_CreatePrimary() function with the following parameters:
* ESYS_TR_RH_ENDORSEMENT: To generate key in the endorsement hierarchy6.
* ESYS_TR_PASSWORD: indicates a password authorization
* inPublicECC: the public key template defined is passed.

Finally the created key is converted to ASN DER format.


## setRootKey()
:::danger
Initializes root key by calling generatePublicKey(0) since 0 is the index of root key.
:::


It also sets the root key hash. It is calculated by concatenating the raw x and y part of the root key and then hashing it.

The rootkey hash is stored statically and is important for every new primary key generation since it is passed as parameter to the unique structure of inPublicEC.X part. The key index is passed as parameter to the inpublicEc.y.

## updateHandlesIndexes()
:::success
Increments the current index value by 1 and also updates all the corresponding keys.
:::
Flushes the previous key handles index and points it to the nextKeyHandle  then increments NEXT index by 1 to store it in the nextkeyindex. Then generates a new public key by sending nextkeyIndex as the parameter. Finally it assigns the currentKeyhandle to the nextkey handle .

## initializeTPM(INT keyIndex):
:::warning
Initializes TPM  context by calling Esys_Initialize() function and sets the previous key handle and nextkey handle as null. Then it sets the root key and key index.

:::

## getKeyIndex():
:::info
Returns previous key index because that is the "current" key index used for performing signature.

:::

## setKeyIndex(INT keyIndex):
:::danger
Sets the previous key index (which is our current key) to the key index passed as parameter.
:::


Also sets keyIndex+1 as the nextKey index.

For the key generated at after first  initialization it flushes the previous key handle and generates the key with keyIndex then points then populates the previous key handle with the current key handle value.
Next it generates the public key with keyIndex+1 and stores it in the nextkey handle. 


## getPublicKey(INT keyIndex, INT *publicKeySize):
:::success
Returns the public key for the given index.
:::

Takes keyIndex and returns root key if the keyINDEX is 0, next key if the keyindex matches with the nextKey index,  previous key if the keyINdex matches with the previous Key index. 

If it matches with none of these indexes, then it flushes the root key from the tpm (due to the limit of max 3 transient handles), generates the key for the corresponding keyIndex and copies it into temp key then flushes it from the TPM. Finally, it regenerates the root key and then returns temp key.



## signECDSA(INT keyIndex, BYTE *hashToSign, INT *eccSignSize, bool increment):
:::warning
Signs the given hash using the key referred by the key index.
:::
Converts the byte  hash to TPM2B_hash object and then checks the key index . If it is root key or previous key then assigns it to the signing key handle otherwise; 

Sets the previous key index to the given keyindex  by calling setkeyindex() function and assigns it to the signing handle. It signs the hash using Esys_Sign() function. Finally the signature is converted to ASN DER format which is returned by signECDSA().


## getECDHPoint(INT keyIndex, BYTE *euphemeralKey):
:::info
Performs Elliptic Curve Diffe Hellmen Key Exchange using the private part of the key referred by the Key Index and public euphemeral key. Returns the derived shared secret uncompressed point.
:::


Takes the key index and checks whether it's previous key, next key or root key. If it's one of these then it assigns it to the  ECDH key handle  else it removes the root key and generates a new key for the given key index and use it in the ECDH handle. 

Next, it re-structures the euphemeral key with the format 04 x y and generates an ECDH point using the Esys_ECDH_ZGen() function and stores it 04 x y format in zPoint which is then returned by the function.


