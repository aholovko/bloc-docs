# CLI
The Orb Command Line Interface (Orb CLI) is a unified tool that provides a consistent interface for interacting with all parts of Orb. 

## Create DID
This command used for creating DID.

### Usage
```
did create [flags]
```

### Flags
* `domain` _[string]_ - URL to the Orb domain.
* `sidetree-url` _[array|string]_ - Array of one or more Sidetree URLs.
* `sidetree-write-token` _[string]_ - The Sidetree write token.
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `publickey-file` _[string]_ - The file contains the DID public keys.
* `service-file` _[string]_ - The file contains the DID services.
* `recoverykey` _[string]_ - The public key PEM used for recovery of the document.
* `recoverykey-file` _[string]_ - The file that contains the public key PEM used for recovery of the document.
* `updatekey` _[string]_ - The public key PEM used for validating the signature of the next update of the document.
* `updatekey-file` _[string]_ - The file that contains the public key PEM used for validating the signature of the next update of the document.

### Example

#### create cmd
```
did create --domain https://orb1.com --publickey-file ./publickeys.json --service-file ./services.json 
--recoverykey-file ./keys/recover/public.pem --updatekey-file ./keys/update/public.pem
```

#### publickeys.json
```
[
 {
  "id": "key1",
  "type": "Ed25519VerificationKey2018",
  "purposes": ["authentication"],
  "jwkPath": "./key1_jwk.json"
 },
 {
  "id": "key2",
  "type": "JwsVerificationKey2020",
  "purposes": ["capabilityInvocation"],
  "jwkPath": "./key2_jwk.json"
 }
]
```

#### key1_jwk.json
```
{
  "kty":"OKP",
  "crv":"Ed25519",
  "x":"o1bG1U7G3CNbtALMafUiFOq8ODraTyVTmPtRDO1QUWg",
  "y":""
}
```

#### key2_jwk.json
```
{
  "kty":"EC",
  "crv":"P-256",
  "x":"bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
  "y":"PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
}
```

#### services.json
```
[
  {
    "id": "svc1",
    "type": "type1",
    "priority": 1,
    "routingKeys": ["key1"],
    "recipientKeys": ["key1"],
    "serviceEndpoint": "http://www.example.com"
  },
  {
    "id": "svc2",
    "type": "type2",
    "priority": 2,
    "routingKeys": ["key2"],
    "recipientKeys": ["key2"],
    "serviceEndpoint": "http://www.example.com"
  }
]
```

## Resolve
This command used for resolving DID.

### Usage
```
did resolve [flags]
```

### Flags
* `domain` _[string]_ - URL to the Orb domain.
* `sidetree-url-resolution` _[array|string]_ - Array of one or more Sidetree URLs resolution.
* `auth-token` _[string]_ - The Auth token.
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `did-uri` _[string]_ - DID URI.
* `verify-resolution-result-type` _[string]_ - Verify resolution result type. Values [all, none, unpublished].

### Difference between domain and sidetree-url-resolution
Domain will be used to discover Orb resolution endpoint if you have Orb DID without discovery information. sidetree-url-resolution 
will not discover any endpoint and hit the resolution directly.

### Example

#### resolve cmd
```
did resolve --domain https://orb1.com --did-uri did:orb:3XvwJ:EiDnJwbKHkHdaco4khFeBzvSL1hZ4eBGQq3q1Yjrpi5d4g  
--verify-resolution-result-type all
```

#### resolve cmd DID with discovery information
```
did resolve --did-uri did:orb:https:orb1.com:uAAA:EiAFqEsuKDpwbfFxHfqP-TLdFPjqSrFWwgj8_dU64GMcEQ  
--verify-resolution-result-type all
```

## Update
This command used for updating DID.

### Usage
```
did update [flags]
```

### Flags
* `domain` _[string]_ - URL to the Orb domain.
* `sidetree-url-operation` _[array|string]_ - Array of one or more Sidetree URLs operation.
* `sidetree-url-resolution` _[array|string]_ - Array of one or more Sidetree URLs resolution.
* `sidetree-write-token` _[string]_ - The Sidetree write token.
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `did-uri` _[string]_ - DID URI.
* `add-publickey-file` _[string]_ - The file contains the DID public keys to be added or updated.
* `add-service-file` _[string]_ - The file contains the DID services to be added or updated.
* `nextupdatekey` _[string]_ - The public key PEM used for validating the signature of the next update of the document.
* `nextupdatekey-file` _[string]_ - The file that contains the public key PEM used for validating the signature of the next update of the document.
* `signingkey` _[string]_ - The private key PEM used for signing the update of the document.
* `signingkey-file` _[string]_ -  The file that contains the private key PEM used for signing the update of the document.
* `signingkey-password` _[string]_ -  The Signing key PEM password.

### Example

#### update cmd
```
did update --domain https://orb1.com --did-uri did:orb:3XvwJ:EiDnJwbKHkHdaco4khFeBzvSL1hZ4eBGQq3q1Yjrpi5d4g  
--add-publickey-file ./publickeys.json --add-service-file ./services.json --signingkey-file ./keys/update/key_encrypted.pem --signingkey-password 123
--nextupdatekey-file ./keys/update2/public.pem
```

#### publickeys.json
```
[
 {
 "id": "key2",
 "type": "JwsVerificationKey2020",
 "purposes": ["capabilityInvocation"],
 "jwkPath": "./key2_jwk.json"
 },
 {
  "id": "key3",
  "type": "Ed25519VerificationKey2018",
  "purposes": ["authentication"],
  "jwkPath": "./key3_jwk.json"
 }
]
```

#### key2_jwk.json
```
{
  "kty":"EC",
  "crv":"P-256",
  "x":"bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
  "y":"PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
}
```

#### key3_jwk.json
```
{
  "kty":"OKP",
  "crv":"Ed25519",
  "x":"o1bG1U7G3CNbtALMafUiFOq8ODraTyVTmPtRDO1QUWg",
  "y":""
}
```

#### services.json
```
[
  {
    "id": "svc3",
    "type": "type3",
    "priority": 3,
    "routingKeys": ["key3"],
    "recipientKeys": ["key3"],
    "serviceEndpoint": "http://www.example.com"
  }
]
```

## Recover
This command used for recovering DID.

### Usage
```
did recover [flags]
```

### Flags
* `domain` _[string]_ - URL to the Orb domain.
* `sidetree-url-operation` _[array|string]_ - Array of one or more Sidetree URLs operation.
* `sidetree-url-resolution` _[array|string]_ - Array of one or more Sidetree URLs resolution.
* `sidetree-write-token` _[string]_ - The Sidetree write token.
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `did-uri` _[string]_ - DID URI.
* `publickey-file` _[string]_ - The file contains the DID public keys to be recovered.
* `service-file` _[string]_ - The file contains the DID services to be recovered.
* `nextupdatekey` _[string]_ - The public key PEM used for validating the signature of the next update of the document.
* `nextupdatekey-file` _[string]_ - The file that contains the public key PEM used for validating the signature of the next update of the document.
* `nextrecoverkey` _[string]_ - The public key PEM used for validating the signature of the next recovery of the document.
* `nextrecoverkey-file` _[string]_ - The file that contains the public key PEM used for validating the signature of the next recovery of the document.
* `signingkey` _[string]_ - The private key PEM used for signing the recovery of the document.
* `signingkey-file` _[string]_ -  The file that contains the private key PEM used for signing the recovery of the document.
* `signingkey-password` _[string]_ -  The Signing key PEM password.

### Example

#### recover cmd
```
did recover --domain https://orb1.com --did-uri did:orb:3XvwJ:EiDZTmh3BNBzhwSlOdh3FwAdjzu4BkWly2MoTVNHoNdJpw 
--publickey-file ./publickeys.json  --service-file ./services.json 
--nextrecoverkey-file ./keys/recover2/public.pem --nextupdatekey-file ./keys/update3/public.pem 
--signingkey-file ./keys/recover/key_encrypted.pem --signingkey-password 123
```

#### publickeys.json
```
[
 {
  "id": "key-recover-id",
  "type": "Ed25519VerificationKey2018",
  "purposes": ["authentication"],
  "jwkPath": "./fixtures/did-keys/recover/key1_jwk.json"
 }
]
```

#### key1_jwk.json
```
{
  "kty":"OKP",
  "crv":"Ed25519",
  "x":"o1bG1U7G3CNbtALMafUiFOq8ODraTyVTmPtRDO1QUWg",
  "y":""
}

```

#### services.json
```
[
  {
    "id": "svc-recover-id",
    "type": "type1",
    "priority": 1,
    "routingKeys": ["key1"],
    "recipientKeys": ["key1"],
    "serviceEndpoint": "http://www.example.com"
  }
]
```

## Deactivate
This command used for Deactivating DID.

### Usage
```
did deactivate [flags]
```

### Flags
* `domain` _[string]_ - URL to the Orb domain.
* `sidetree-url-operation` _[array|string]_ - Array of one or more Sidetree URLs operation.
* `sidetree-url-resolution` _[array|string]_ - Array of one or more Sidetree URLs resolution.
* `sidetree-write-token` _[string]_ - The Sidetree write token.
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `did-uri` _[string]_ - DID URI.
* `signingkey` _[string]_ - The private key PEM used for signing deactivate of the document.
* `signingkey-file` _[string]_ -  The file that contains the private key PEM used for signing deactivate of the document.
* `signingkey-password` _[string]_ -  The Signing key PEM password.

### Example

#### deactivate cmd
```
deactivate-did --domain https://orb1.com --did-uri did:orb:3XvwJ:EiDnJwbKHkHdaco4khFeBzvSL1hZ4eBGQq3q1Yjrpi5d4g  
--signingkey-file ./keys/recover2/key_encrypted.pem --signingkey-password 123
```

## Follow
This command used for manage followers.

### Usage
```
follower [flags]
```

### Flags
* `outbox-url` _[string]_ - Outbox url.
* `actor` _[string]_ - Actor IRI.
* `to` _[string]_ - To IRI.
* `action` _[string]_ - Follower action (Follow, Undo).
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `auth-token` _[string]_ - Auth token.
* `follow-id` _[string]_ - Follow id required for undo action.



### Example

#### follow cmd (orb-2 server follows orb-1 server)
```
follower --outbox-url=https://orb-1.com/services/orb/outbox --actor=https://orb-2/services/orb
--to=https://orb-1/services/orb --action=Follow --auth-token=token123
```

#### undo cmd (orb-2 server unfollows orb-1 server)
```
follower --outbox-url=https://orb-1.com/services/orb/outbox --actor=https://orb-2/services/orb
--to=https://orb-1/services/orb --action=Undo --auth-token=token123 --follow-id=r232DD##
```

## Witness
This command used for manage witness.

### Usage
```
follower [flags]
```

### Flags
* `outbox-url` _[string]_ - Outbox url.
* `actor` _[string]_ - Actor IRI.
* `to` _[string]_ - To IRI.
* `action` _[string]_ - Follower action (Follow, Undo).
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `auth-token` _[string]_ - Auth token.
* `follow-id` _[string]_ - Follow id required for undo action.


### Example

#### witness cmd (orb-1 invites orb-2 to be a witness)
```
witness --outbox-url=https://orb-1.com/services/orb/outbox --actor=https://orb-1.com/services/orb
--to=https://orb-2.com/services/orb --action=InviteWitness --auth-token=token123
```
#### undo cmd (orb-1 uninvite orb-2 to be a witness)
```
witness --outbox-url=https://orb-1.com/services/orb/outbox --actor=https://orb-1.com/services/orb
--to=https://orb-2.com/services/orb --action=InviteWitness --auth-token=token123 --invite-witness-id=r2WW##
```
