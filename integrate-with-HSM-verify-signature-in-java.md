## Introduction 

For Corda project, we will sign transaction in Carbon using HSM and verify the signature in Cordapp which uses java.security.Signature under the hood. This docs will explain how to sign transaction with HSM and how the signature is verified. 

## Sign transaction with HSM 

Use this script to generate key pairs and signature:  

// code reference

* Generate master key 
```
const masterKeyLabel = generateBIP32MasterKey(); 
```
* Use the master key label to generate child path and public key. 
```
const childKeyDetails = await getBIP32ChildKeyAndSign( masterKeyLabel );
```
* Pass the master key, message hash (in hex) and child path to generate signature 
```
const signedTrx = await getBIP32ChildKeyAndSign ( masterKeyLabel, msgHash, childPath ); 
```
* Sample output: 
```
compressed public key: 030F92CF7082006E46ADBB7C51AD12435110595C59C45518E35C794AD6578D4FEE 

uncompressed public key: 040F92CF7082006E46ADBB7C51AD12435110595C59C45518E35C794AD6578D4FEEFCF13C1FDF511296845B61BADBED4EEE9D50A11A7CEE427224897015374577C3  

public key: 3056301006072A8648CE3D020106052B8104000A034200040F92CF7082006E46ADBB7C51AD12435110595C59C45518E35C794AD6578D4FEEFCF13C1FDF511296845B61BADBED4EEE9D50A11A7CEE427224897015374577C3 

signature: 7531DDE88945CC2431F69C237B7A5A52D042E44CFBE9FF2AC8C821E6EC507EC0ABC6AD1909A7C571E1C504261AEE5B92B3061D9AD7F921738C79806584CFD358 

ASN.1 DER signature: 304502207531DDE88945CC2431F69C237B7A5A52D042E44CFBE9FF2AC8C821E6EC507EC00221008A6A95A41D8E41F074C21E4680EAE4C90931489444DA4F62A58756804934181C 

message hash: 68656c6c6f20776f726c64 (raw message string: hello world) 
```
### Note: 

The key pair is generated using secp256k1 curve, the public key is compressed and signature is standard ECDSA format 

Since Corda supports ECDSA_SECP256K1_SHA256, the message needs to be hashed using SHA256 before passing to bip32 
```
const hash = crypto.createHash('sha256'); 

hash.update(msgHash); 

msgHash = hash.digest('hex'); 
```
The real key generating and signing is done in BIP32KeyDerivation.java. If you need to debug, change the code there and do docker-compose build 

## Verify transaction in Java 

* We use this util method in Corda API to verify the signature 
```
val result = Crypto.doVerify("ECDSA_SECP256K1_SHA256", publicKey, signatureData, dataBytes) 
```
* Contruct PublicKey object  from compressed public key string 

* Convert compressed to uncompressed public key string 

// code reference 

* Reconstruct EC Point and public key 

// code reference 

* Signature generated in HSM is ECDSA standard but Java uses ASN.1 DER format, so we need to add some extra bytes to make it work. 

    * The ECDSA signature is 64 bytes, concatenation of r and s, each is 32 bytes 
```
r|s = b2b31575f8536b284410d01217f688be3a9faf4ba0ba3a9093f983e40d630ec722a7a25b01403cff0d00b3b853d230f8e96ff832b15d4ccc75203cb65896a2d5 

ASN.1 DER signature is the following format: 

0x30|b1|0x02|b2|vr|0x02|b3|vs 

b1 = Length of remaining data 

b2 = Length of vr 

b3 = Length of vs 

vr = the signed big-endian encoding of the value "𝑟", of minimal length 

vs = the signed big-endian encoding of the value "s", of minimal length 
```
* Be careful that the first bit of the first byte specifies the sign of the value (that's "signed") I.e. if the first byte of "𝑟" or "s" converted to 8-bit binary and the top bit is 1, it must be added byte 0x00 at the top of the byte array, hence the signature can be 70, 71 or 72 bytes 

// code reference 

* The data bytes are message in hex before SHA256 hashing converted to bytes 

## Reference 

* To understand more about ECDSA: Elliptic Curve Signatures 

https://cryptobook.nakov.com/digital-signatures/ecdsa-sign-verify-messages 

* How to sign/verify transaction in Java 

https://metamug.com/article/security/sign-verify-digital-signature-ecdsa-java.html 

* Public key, private key and signature bytes in Java with deeper details 

http://fog.misty.com/perry/ccs/EC/all-EC.html 

* Explain more about adding byte 0x00 

https://crypto.stackexchange.com/questions/1795/how-can-i-convert-a-der-ecdsa-signature-to-ASN.1 

https://crypto.stackexchange.com/questions/57731/ecdsa-signature-rs-to-asn1-der-encoding-question 