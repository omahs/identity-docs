# Issue a JSON (JWT) Verifiable Credential

This tutorial offers step-by-step guidance on how to issue a [JSON credential](https://www.w3.org/TR/vc-data-model/#json), encoded as a [JSON Web Token (JWT)](https://www.w3.org/TR/vc-data-model/#json-web-token), a [W3C compliant proof format.](https://www.w3.org/TR/vc-data-model/#proof-formats)

> ⚠️ **Before you begin...**
>
> Make sure you've correctly [configured the cheqd plugin's agent settings](../../../guides/software-development-kits-sdks/veramo-sdk-for-cheqd/setup-cli.md) for Veramo CLI

## Background

The subject (holder) of this credential will be an off-ledger [`did:key`](https://github.com/w3c-ccg/did-method-key) DID. The rationale for using off-ledger DIDs when issuing a credential is because the fact that they _aren't_ persisted on ledger means they cannot be correlated or tracked to specific users/individuals.

This allows a user to have _multiple_ `did:key` identities, and the identifier/handle they reveal when sharing a [Verifiable Credential](broken-reference) is context-dependent and better for privacy.

The _issuer_ of the credential, on the other hand, uses [an on-ledger `did:cheqd` DID](../../did-operations/) since this needs to be publicly-accessible and cryptographically verifiable.

## Instructions

### 1. Create an off-ledger `did:key` subject DID

Normally, the credential holder will _provide_ a `did:key` DID they have generated themselves (usually done in the background by apps they are using). So, this step _typically_ isn't done by a credential issuer.

For the purpose of this tutorial though, we recommend you [create a `did:key` subject DID](../../did-operations/create-subject-did.md) for the later steps.

### 2. Begin credential creation

Start credential generation using:

```bash
veramo credential create --json
```

### 3. Select a credential proof format

You'll be presented with an option to choose between `jwt` (JSON-based JWT) or `lds` (JSON-LD).

Currently, only `jwt` proof formats are supported in Veramo CLI with cheqd.

### 4. Specify the Issuer DID for the credential

Select which Issuer DID stored in your local storage you'd like to issue the credential from, e.g., `did:cheqd:mainnet:zAXwwqZzhCZA1L77ZBa8fhVNjL9MQCHX`

The assumption here is that you've either [created the issuer DID](../../did-operations/) or [stored the DIDDoc by querying it](../../did-operations/query-did.md).

### 5. Specify the subject DID (credential holder's DID)

Select which subject DID from your local agent storage the credential should be issued to, e.g., `did:key:z6Mkktr27VZ7TTFoTsD9p79JwtGnQDgJWKGrxJ79quE7M5Yx`.

This could the subject DID created in Step 1.

### 6. Set the Verifiable Credential type

This is currently free-text. However, you can hit _Enter_ to use `VerifiableCredential` as the default credential context.

### 7. Specify claim type, i.e., a "field" in the credential

The claim type can be thought of as the _label_ in a form field. It can denote the purpose/attribute being stored in the credential.

For example, the claim type `name` could refer to the name of an individual.

### 8. Enter value for claim type

Enter the _value_ of the claim type. For example, you specified `name` as the claim type, the value could be `Alice`.

In this tutorial, we will create a credential with only a single name-value pair.

### 9. Define if the credential is revocable or not

JSON/JWT credentials anchored on cheqd are currently _not_ revocable, so please choose `No` at this step.

[Privacy-preserving credential revocation](https://product.cheqd.io/updates/roadmap/identity) is a major element of cheqd's roadmap.

### 10. Verifiable Credential is generated

At this last step, Veramo CLI generates the credential based on the inputs above.

_Note_: The human-readable JSON body below is purely for easier parsing by developers/applications. The proof is encoded as JWT at the bottom of the credential, which can be decoded to reconstruct the entire credential.

#### Example Verifiable Credential

```json
{
  "credentialSubject": {
    "name": "Alice",
    "id": "did:key:z6Mkktr27VZ7TTFoTsD9p79JwtGnQDgJWKGrxJ79quE7M5Yx"
  },
  "issuer": {
    "id": "did:cheqd:mainnet:zAXwwqZzhCZA1L77ZBa8fhVNjL9MQCHX"
  },
  "type": [
    "VerifiableCredential",
    "Profile"
  ],
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://veramo.io/contexts/profile/v1"
  ],
  "issuanceDate": "2022-07-28T15:25:10.000Z",
  "proof": {
    "type": "JwtProof2020",
    "jwt": "eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJ2YyI6eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvMjAxOC9jcmVkZW50aWFscy92MSIsImh0dHBzOi8vdmVyYW1vLmlvL2NvbnRleHRzL3Byb2ZpbGUvdjByb2ZpbGUvdjEiXSwidHlwZSI6WyJWZXJpZmlhYmxlQ3JlZGVudGlhbCIsIlByb2ZpbGUiXSwiY3JlZGVudGlhbFN1YmplY3QiOnsibmFtZSI6IkFsaWNlIn19LCJzdWIiOiJkaWQ6a2V5Ono2TWtrdHIyN1ZaN1RURm9Uc25RRGdKV0tHc0Q5cDc5Snd0R25RRGdKV0tHcnhKNzlxdUU3TTVZeCIsIm5iZiI6MTY1OTAyMTkxMCwiaXNzIjoiZGlkOmNoZXFkOm1haW5uZXQ6ekFYd3dxWnpoQ1pBMUw3N1pCYThmaFZOakw5TVFDSFgifQ.MRqlKuFQzpjLvsW3C2ZSBEf5jfvJCPQBwl-gP1P8bRfNSvjxj9H3eDgDmEf5jfvJCPQBwUDltBr-ZQ3Q7SKVSvCaJHV8TnUzBA"
  }
}
```

## Next steps

You can save the generated Verifiable Credential, as you can later use it to [verify the credential cryptographically](verify-jwt-vc.md) or [generate _presentations_ to show the credential](verifiable-presentations.md) to others.
