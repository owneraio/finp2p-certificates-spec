# FinP2P Certificates specifications
## Overview
This specification intended to provide a standardized definition for finp2p certificate object structures.
Certificates are properties of a FinP2P profile (a user or asset), which are “given” to the profile by a trusted third party.
The certificate is bounded for a specific time period, by which it will have to be renewed.
This specification defines the types of certificates with the data structure for each type.
The certificate data structure is a JSON object, and the specification below include a JSON schema for each type of certificate.

## Type of Certificates
There are two major use cases for Certificates:
1. Provide independent verification to an owner that it has certain qualities. For example, a KYC is a Certificate, a document that verifies that a user is an “Accredited Investor” is also a Certificate.
2. Enable the KYA (Know your Asset) model on an Asset, KYA is a set of documents that describe the asset and the exact ownership rights of token holders (such as what happens in the event of an exit, or eligibility to receive dividends).

### Owner certificates
#### Owner Holding Details
Information about the Owner of the investment account or, when the ownership is split among several owners, the primary owner is the one giving its address and account details for the registration.
This certificate is interchangable with the Nominee Info certificate, which is used to acknowledge the owner being a nominee owner.
There are two types of owners: individuals and companies. Each type has a corresponding certificate schema.

#### Individual Owner

`type: "individualInfo"`

[Individual Owner Info Schema][spec_individual_info]

Sample data structure:
```json
{
  "name": "John Doe",
  "id": {
    "idType": "Passport",
    "idNumber": "123456789"
  },
  "dateOfBirth": "1980-01-01",
  "nationality": "American",
  "email": "johndoe@example.com",
  "address": {
    "line1": "123 Main St",
    "line2": "Apt 4B",
    "buildingNumber": "10",
    "city": "New York",
    "postalCode": "10001",
    "country": "US",
    "state": "NY"
  }
}
```

#### Company Owner

`type: "companyInfo"`

[Company Owner Info Schema][spec_company_info]
Sample data structure:
```json
{
  "legalBusinessName": "Tech Solutions Inc",
  "businessName": "Tech Solutions",
  "businessRegisteredCountry": "US",
  "registeredBusinessAddress": {
    "line1": "456 Technology Ave",
    "line2": "Suite 800",
    "buildingNumber": "200",
    "city": "San Francisco",
    "postalCode": "94105",
    "country": "US",
    "state": "CA"
  },                                  
  "registrationNumberIncorporation": "12345678",
  "companiesHouseRegistrationNumber": "12345678",
  "employerIdentificationNumber": "12-3456789",
  "beneficialOwners": [
    {
      "name": "Jane Smith",
      "id": {
        "idType": "Passport",
        "idNumber": "A12345678"
      },
      "dateOfBirth": "1975-05-15",
      "nationality": "American",
      "email": "jane.smith@example.com",
      "address": {
        "line1": "123 Elm St",
        "line2": "Apt 4",
        "buildingNumber": "10",
        "city": "New York",
        "postalCode": "11111",
        "country": "US",
        "state": "NY"
      }
    }
  ],
  "industryCode": "5732",
  "vatNumber": "US123456789",
  "businessWebsite": "https://www.techsolutions.com",
  "statementDescriptor": "Tech Solutions Payment"
}

```

#### Nominee Account
Acknowledgement of the owner is registered under a nominee account.
The nominee account linked by the nomineeId field, it maintains it's own certificates such as ownerInfo, KYC etc... and can be used to verify the registered owner's information and documents.

`type: "nomineeInfo"`

[Nominee Schema][spec_nominee]

Sample data structure:
```json
 {
    "nomineeId": "123",
    "info": [
      { "type": "text", "name": "Certificate ID", "value": "123" }
    ]
}
 ```

#### KYC/AML
Acknowledgement of the owner KYC and AML verification

`type: "KYC/AML"`

[KYC Schema][spec_kyc]

Sample data structure:
```json
 {
    "name": "AML/KYC Compliance",
    "country": "usa",
    "info": [
      { "type": "text", "name": "Certificate ID", "value": "123" },
      { "type": "text", "name": "Country", "value": "usa" }
    ]
}
 ```

#### Accreditation
Acknowledgement of the owner being an accredited investor

`type: "Accreditation"`

[Accreditation Schema][spec_accreditation]

Sample data structure:
```json
{
    "name": "Certificate of Accreditation",
    "country": "usa",
    "info": [
      { "type": "text", "name": "Certificate ID", "value": "123" },
      { "type": "text", "name": "Country", "value": "usa" }
    ]
}
```

### Asset certificates
#### KYA

KYA describes the asset and the exact ownership rights of token holders (such as what happens in the event of an exit, or eligibility to receive dividends).
According to the FinP2P model, only the primary node of each asset (or entities approved on its behalf) are allowed to update the object and add documents on the KYA certificate. This means potential owners who wish to invest can always know what they are buying, and exactly who verified that information (i.e. a regulated financial node).
The KYA includes informative information about the asset in the `info` structure, ownership rights can be added as documents attached to the certificate.

`type: "KYA"`

[KYA Schema][spec_kya]

Sample data structure:
```json
{ 
  "info": [
            { "type": "text", "name": "Issuer", "value": "Issuer name" },
            { "type": "text", "name": "Headquarters", "value": "New York" },
            { "type": "text", "name": "Industry", "value": "Finance" },
            { "type": "link", "name": "Website", "value": "company-Y-asset.com" }
        ]
}
```
#### NAV 

The NAV is a financial measure used to determine the value of assets, NAV per share is calculated as the total value of the asset's holdings minus its liabilities devided by the number of ourstanding shares. 

`type: "NAV"`

[NAV Schema][spec_nav]

Sample data structure:
```json
{ 
  "currentNAV": { "date": 1684217201, "navValue": 50000000, "description": "some description"}
  "history" : [
  { "date": 1684217201, "navValue": 50000000, "description": "some description" },
  { "date": 1681625201, "navValue": 45000000 },
  { "date": 1678946801, "navValue": 42000000 }  
  ]
}
```

[spec_kyc]: schemas/user/kyc.schema.json
[spec_company_info]: schemas/user/company_info.schema_v1.0.json
[spec_individual_info]: schemas/user/individual_info.schema_v1.0.json
[spec_nominee]: schemas/user/nominee.schema.json
[spec_accreditation]: schemas/user/accreditation.schema.json
[spec_kya]: ./schemas/asset/kya.schema.json
[spec_nav]: ./schemas/asset/nav.schema.json
