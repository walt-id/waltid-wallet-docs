---
description: Verifiable Credential templates/types setup
---

# Credential Templates

### CLI

You can import a DID document using:

```bash
config --as-issuer default vc templates import --name TestCheqdCredential cheqd-credential-template.json
```

The value "default" of `--as-issuer` refers to the tenantId.

By specifying `vc templates --help` you can also view other VC template commands (besides import you can list, export and remove)

### REST API

### List templates you have

GET /issuer-api/{tenantId}/config/templates

{% tabs %}
{% tab title="CURL" %}
```bash
curl -X 'GET' \
  'http://localhost:8080/issuer-api/default/config/templates' \
  -H 'accept: application/json'
```
{% endtab %}

{% tab title="Default Response" %}
By default, only the template ID will be shown, but the full template will not be loaded.

```json
[
  {
    "mutable": false,
    "name": "Europass",
    "template": null
  },
  {
    "mutable": false,
    "name": "VerifiableAuthorization",
    "template": null
  },
  {
    "mutable": false,
    "name": "VerifiableVaccinationCertificate",
    "template": null
  },
  {
    "mutable": false,
    "name": "KycCredential",
    "template": null
  },
  {
    "mutable": false,
    "name": "DataSelfDescription",
    "template": null
  },
  {
    "mutable": false,
    "name": "VerifiableId",
    "template": null
  },
  {
    "mutable": false,
    "name": "UniversityDegree",
    "template": null
  },
  {
    "mutable": false,
    "name": "VerifiableMandate",
    "template": null
  },
  {
    "mutable": false,
    "name": "ProofOfResidence",
    "template": null
  },
  {
    "mutable": false,
    "name": "AmletCredential",
    "template": null
  },
  {
    "mutable": false,
    "name": "OpenBadgeCredential",
    "template": null
  },
  {
    "mutable": false,
    "name": "VerifiableDiploma",
    "template": null
  },
  {
    "mutable": false,
    "name": "DataConsortium",
    "template": null
  },
  {
    "mutable": false,
    "name": "DataServiceOffering",
    "template": null
  },
  {
    "mutable": false,
    "name": "VerifiableAttestation",
    "template": null
  },
  {
    "mutable": false,
    "name": "KybCredential",
    "template": null
  },
  {
    "mutable": false,
    "name": "PermanentResidentCard",
    "template": null
  },
  {
    "mutable": false,
    "name": "EuropeanBankIdentity",
    "template": null
  },
  {
    "mutable": false,
    "name": "Iso27001Certificate",
    "template": null
  },
  {
    "mutable": false,
    "name": "LegalPerson",
    "template": null
  },
  {
    "mutable": false,
    "name": "ParticipantCredential",
    "template": null
  },
  {
    "mutable": false,
    "name": "GaiaxCredential",
    "template": null
  },
  {
    "mutable": false,
    "name": "VerifiablePresentation",
    "template": null
  },
  {
    "mutable": false,
    "name": "PeerReview",
    "template": null
  },
  {
    "mutable": false,
    "name": "ServiceOfferingCredential",
    "template": null
  },
  {
    "mutable": false,
    "name": "KybMonoCredential",
    "template": null
  },
  {
    "mutable": false,
    "name": "Email",
    "template": null
  }
]
```


{% endtab %}
{% endtabs %}

### Show a VC template

GET /issuer-api/{tenantId}/config/templates/{templateId}

{% tabs %}
{% tab title="CURL" %}
```bash
curl -X 'GET' \
  'http://localhost:8080/issuer-api/default/config/templates/VerifiableId' \
  -H 'accept: application/json'
```


{% endtab %}

{% tab title="Example Response" %}
Response for "VerifiableId" credential:

```json
{
  "type": [
    "VerifiableCredential",
    "VerifiableAttestation",
    "VerifiableId"
  ],
  "@context": [
    "https://www.w3.org/2018/credentials/v1"
  ],
  "id": "urn:uuid:3add94f4-28ec-42a1-8704-4e4aa51006b4",
  "issuer": "did:ebsi:2A9BZ9SUe6BatacSpvs1V5CdjHvLpQ7bEsi2Jb6LdHKnQxaN",
  "issuanceDate": "2021-08-31T00:00:00Z",
  "issued": "2021-08-31T00:00:00Z",
  "validFrom": "2021-08-31T00:00:00Z",
  "credentialSchema": {
    "id": "https://api.preprod.ebsi.eu/trusted-schemas-registry/v1/schemas/0xb77f8516a965631b4f197ad54c65a9e2f9936ebfb76bae4906d33744dbcc60ba",
    "type": "FullJsonSchemaValidator2021"
  },
  "credentialSubject": {
    "id": "did:ebsi:2AEMAqXWKYMu1JHPAgGcga4dxu7ThgfgN95VyJBJGZbSJUtp",
    "currentAddress": [
      "1 Boulevard de la Libert�, 59800 Lille"
    ],
    "dateOfBirth": "1993-04-08",
    "familyName": "DOE",
    "firstName": "Jane",
    "gender": "FEMALE",
    "nameAndFamilyNameAtBirth": "Jane DOE",
    "personalIdentifier": "0904008084H",
    "placeOfBirth": "LILLE, FRANCE"
  },
  "evidence": [
    {
      "documentPresence": [
        "Physical"
      ],
      "evidenceDocument": [
        "Passport"
      ],
      "subjectPresence": "Physical",
      "type": [
        "DocumentVerification"
      ],
      "verifier": "did:ebsi:2A9BZ9SUe6BatacSpvs1V5CdjHvLpQ7bEsi2Jb6LdHKnQxaN"
    }
  ]
}
```
{% endtab %}
{% endtabs %}

### Add a new VC template

POST /issuer-api/{tenantId}/config/templates/{templateId}

{% tabs %}
{% tab title="CURL" %}
```bash
curl -X 'POST' \
  'http://0.0.0.0:8080/issuer-api/default/config/templates/CrudeProductCredential' \
  -H 'accept: text/plain' \
  -H 'Content-Type: application/json' \
  -d 'SEE EXAMPLE BODY'
```
{% endtab %}

{% tab title="Example Body" %}
For this demo, we will add the example w3c-ccg credential [CrudeProductCredential](https://w3c-ccg.github.io/vc-examples/crude/examples/v1.0/crude-product-verifiable-credential-v1.0.json):

```json
{
    "@context": [
        "https://www.w3.org/2018/credentials/v1"
    ],
    "id": "http://neo-flow.com/credentials/3a185b8f-078a-4646-8343-76a45c2856a5",
    "type": [
        "VerifiableCredential",
        "CrudeProductCredential"
    ],
    "name": "Heavy Sour Dilbit",
    "description": "Crude oil stream, produced from diluted bitumen.",
    "issuer": "did:v1:test:nym:z6MkmbrHuhkzwWYeJjkBhTYktabXR22ECzk1WrHJPW69EsJY",
    "issuanceDate": "2020-04-09T21:12:12Z",
    "credentialSubject": {
        "producer": "did:v1:test:nym:z6MkfG5HTrBXzsAP8AbayNpG3ZaoyM4PCqNPrdWQRSpHDV6J",
        "category": "Western Canadian Select",
        "hsCode": "270900",
        "identifier": "3a185b8f-078a-4646-8343-76a45c2856a5",
        "name": "Heavy Sour Dilbit",
        "description": "Crude oil stream, produced from diluted bitumen.",
        "volume": "10000",
        "address": {
            "address": "Edmonton, CAN",
            "latitude": "53.5461",
            "longitude": "113.4938"
        },
        "productionDate": "2020-03-30T07:23:14.206Z",
        "predecessorOf": "c98f2452-ab18-4cbe-bf89-635fb8ae7f33",
        "successorOf": "",
        "physicalSpecs": {
            "uom": "barrel",
            "minimumQuantity": "1000",
            "apiGravity": 21,
            "viscosityAt10C": "302",
            "viscosityAt20C": "157",
            "viscosityAt30C": "89.6",
            "viscosityAt40C": "55.3",
            "viscosityAt45C": "44.4",
            "pourPoint": "-30",
            "vapourPressure": "51.7",
            "density": "928",
            "naphtha": "",
            "distillateAt350To650F": "",
            "gasOilAt650To980F": "",
            "residAt980F": "41",
            "deemedButane": "1.9",
            "tan": "1.05",
            "ron": "",
            "mon": "",
            "boilingPoint": "",
            "freezingPoint": "",
            "criticalTemperature": "",
            "criticalPressure": "",
            "autoIgnitionTemperatureInAirAt1atm": "",
            "solubilityInTrichloroethylene": "",
            "penetrationAt25C100g5sec": "",
            "softeningPoint": "",
            "ductilityAt25C": "",
            "olefin": "",
            "color": "",
            "odor": "",
            "grossCalorificValueAt15C": "",
            "netCalorificValueAt15C": "",
            "airRequiredForCombustion": "",
            "copperCorrosionAt38CFor1Hour": ""
        },
        "chemicalSpecs": {
            "microCarbonResidue": "9.68",
            "aromaticsTotalBTEX": "0.23",
            "sedimentAndWater": "188",
            "liquidPhaseH2S": "",
            "mercury": "",
            "oxygenates": "",
            "filterableSolids": "",
            "phosphorousVolatile": "",
            "mediumChainTriglycerides": "",
            "benzene": "",
            "particulates": "",
            "organicChlorides": "",
            "nickel": "54",
            "vanadium": "132.5",
            "water": "",
            "molecularWeight": "",
            "sulphur": "3.66",
            "naphthenes": "",
            "chloride": "",
            "arsenic": "",
            "lead": "",
            "ethene": "",
            "propane": "",
            "isoButane": "",
            "nButane": "",
            "hydrocarbonsHeavier": "",
            "unsaturatedHydrocarbons": ""
        }
    },
    "proof": {
        "type": "Ed25519Signature2018",
        "created": "2020-04-09T21:12:28Z",
        "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..VOFp3CC42FvRBqpTNVABY0DX4TqmLrkjG20K0Zl0JtfOU28EcUkndTQm1n9NPDyh3r4XXw0l7i-hxlViCjkTCw",
        "proofPurpose": "assertionMethod",
        "verificationMethod": "did:v1:test:nym:z6MkmbrHuhkzwWYeJjkBhTYktabXR22ECzk1WrHJPW69EsJY#z6Mkmh3mQN5VNM6yXksw19kiBHLNud8vXSfHE84xtanMchbA"
    }
}
```
{% endtab %}
{% endtabs %}

### Remove the VC template

/issuer-api/{tenantId}/config/templates/{templateId}

{% tabs %}
{% tab title="CURL" %}
```bash
curl -X 'DELETE' \
'https://wallet.cheqd.walt-test.cloud/issuer-api/default/config/templates/CrudeProductCredential' \
-H 'accept: text/plain'
```
{% endtab %}
{% endtabs %}
