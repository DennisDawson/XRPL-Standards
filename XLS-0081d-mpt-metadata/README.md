<pre>
Title:       <b>Multi-Purpose Token Metadata Schema</b>
Type:        <b>draft</b>

Author:  
             <a href="mailto:shawnxie@ripple.com">Shawn Xie</a>
             <a href="mailto:gtsipenyuk@ripple.com">Greg Tsipenyuk</a>
             <a href="mailto:smittal@ripple.com>">Shashwat Mittal</a>
Affiliation: <a href="https://ripple.com">Ripple</a>
</pre>

#  XLS-0081d Multi-Purpose Token Metadata Schema

## 1. Abstract

This document describes a Ripple-recommended metadata format for the XRPL community to follow standardized practices that allow interoperability among XRPL applications and cross-chain apps. To ensure successful adoption, the proposal focuses on the following:

1. Taking inspiration from metadata standards prevalent in other chains with significant RWA volumes.
2. Communicating metadata schemas along with MPT Devnet launch.
3. Focusing on highest-priority asset types (for example, Bonds and PE Funds).

### 1.1. Approach

MPTs include a 1024-byte field for arbitrary metadata. The metadata field is part of a hybrid approach of storing essential information on the ledger, while additional information can be stored off the ledger using an external URI in the metadata field. Advantages to this approach include:

1. Metadata can be accessed and verified on the ledger without having to fetch data from an external URI.
2. Provides increased reliability while decreasing dependency on external systems. Relying solely on external sources for metadata would introduce a dependency where services might become unavailable or unreliable, disrupting token operations and the user experience.
3. Avoids the risk of a centralized hosting service provider becoming compromised or unavailable.

## 2 Schemas

These are proposed schemas for common financial instruments that might be stored as MPTs.

### 2.1 Bond Schema

| Field Name              | Required           | Description                  |
|-------------------------|--------------------|------------------------------|
| `Name`                  | :heavy_check_mark: | The name or title of the 
bond. |
| `Identifier`                | :heavy_check_mark: | A unique identifier or 
abbreviation representing the bond, similar to stock tickers |
| `Issuer`                | :heavy_check_mark: | Entity or organization 
issuing the bond |
| `IssueDate`             | | Date the bond is issued in yyyy-mm-dd format   |
| `MaturityDate`          | :heavy_check_mark: | Date the bond matures in 
yyyy-mm-dd format |
| `FaceValue`             | :heavy_check_mark: | Face value of each bond 
token |
| `InterestRate`          | :heavy_check_mark: | Annual interest rate of the 
bond |
| `InterestFrequency`     | :heavy_check_mark: | Frequency at which interest 
payments are made |
| `Collateral`            | | Collateral backing the bond                     |
| `Jurisdiction`          | | Legal jurisdiction governing the bond           |
| `RegulatoryCompliance`  | | Compliance status with relevant regulators      |
| `SecurityType`          | | Type of security (for example, _bond_)          |
| `ExternalUri`           | | URI that points to additional metadata or 
resources related to the bond  |

#### 2.1.1 Example: US Treasury Bill

##### First class fields

```
AssetScale: 18
MaximumAmount: 1000000
```

##### Metadata schema

```json
{
    "InterestFrequency": "enum": ["Monthly", "Quarterly", "Semi Annual", "Annual"]
}
```

##### Metadata example
```json
{
  "Name": "US Treasury Bill Token",
  "Identifier": "USTBT",
  "Issuer": "US Treasury",
  "IssueDate": "2025-03-25",
  "MaturityDate": "2026-03-25",
  "FaceValue": 1000,
  "InterestRate": 2.5,
  "InterestFrequency": "Quarterly",
  "Collateral": "US Government",
  "Jurisdiction": "United States",
  "RegulatoryCompliance": "SEC Regulations",
  "SecurityType": "Treasury Bill",
  "ExternalUri": "https://example.com/t-bill-token-metadata.json"
}
```

### 2.2 Alternative Fund Tokens Schema

| Field Name              | Required           | Description                  |
|-------------------------|--------------------|------------------------------|
| `Name`                  | :heavy_check_mark: | The name or title of the 
private equity token |
| `Identifier`                | :heavy_check_mark: | A unique identifier or 
abbreviation representing the private equity token |
| `Issuer`                | :heavy_check_mark: | Entity or organization 
issuing the private equity token |
| `IssueDate`             | | Date the private equity token is issued in 
yyyy-mm-dd format   |
| `FundType `             | | Type of private equity fund (for example,
_Venture Capital_)   |
| `FundSize`              | :heavy_check_mark: | Total size of the fund      |
| `InvestmentPeriod`      | | Duration of the investment period              | 
| `LockupPeriodDate`      | :heavy_check_mark: | End date of the lockup 
period |
| `ManagementFee`         | :heavy_check_mark: | Percentage of assets charged
as a management fee |
| `Carry`                 | :heavy_check_mark: | Percentage of assets charged
as carried interest |
| `Jurisdiction`          | | Legal jurisdiction governing the private 
equity |
| `RegulatoryCompliance`  | | Compliance status with relevant regulators (for
example, SEC Regulations)     |
| `InvestorRights`        | | Rights granted to investors (for example, voting 
rights, distributions) |
| `DividendPolicy`        | :heavy_check_mark: | Policy for distributing 
dividends or profits     |
| `InvestmentStrategy`    | Strategy for investing in assets (for example, 
_Early-stage tech startups_) |
| `ExternalUri`           | URI pointing to additional metadata or resources
related to the private equity token |

#### Example Alternative Fund Token

```json
{
  "Name": "Venture Fund Token",
  "Identifier": "VFT",
  "Issuer": "ABC Capital Partners",
  "IssueDate": "2025-03-25",
  "FundType": "Venture Capital",
  "FundSize": "10000000",
  "InvestmentPeriod": "5 years",
  "LockupPeriodDate": "2027-03-25",
  "ManagementFee": 2,
  "Carry": 20,
  "Jurisdiction": "United States",
  "RegulatoryCompliance": "SEC Regulations",
  "InvestorRights": ["voting rights", "distributions"],
  "DividendPolicy": ["Reinvestment"],
  "InvestmentStrategy": "Early-stage tech startups",
  "ExternalUri": "https://example.com/venture-fund-metadata.json"
}
```
### 2.3 Generalized Metadata Standard

This is the standard for MPT issuers of assets other than bonds and private equity funds.

| Field Name              | Required           | Description                  |
|-------------------------|--------------------|------------------------------|
| `Name`                  | :heavy_check_mark: | The name or title of the 
token. |
| `Description`           | | Description of the token.                       |
| `Image`                 | | URI to an image that represents the token.      |
| `FallbackImage`         | | URI to alternate image for the token.           |
| `ExternalUri`           | | URI pointing to additional metadata or resources
related to the private equity token. |
| `GroupKey`              | | Custom taxonomic indicator.                     |
| `Keywords`              | | List of keywords for search and classification. |
| `Media`                 | | Array of associated media objects.              |
| `Localization`          | | Array of files that support translations.       |
| `AssetType`             | | Type of asset.                                  |
| `Attributes`            | | Array of objects consisting of _type_ and 
_value_. Additional optional fields can be recommended. |

## 2.4 External URI Format

These are the recommended formats for additional information to be stored outside of the XRP Ledger.

### 2.4.1 Localization

| Field Name              | Sub Fields | Description                           |
|-------------------------|------------|---------------------------------------|
| `locales`               | no         |  Array of available locales for all 
metadata. |
| `primary_uri`           | no         | The primary URI to retrieve localized 
versions of metadata. |
| `secondary_uris`        | no         | An array of secondary URIs to retrieve 
localized versions of metadata. |

### 2.4.2 Rights

| Field Name              | Sub Fields | Description                           |
|-------------------------|------------|---------------------------------------|
| `fee_split`             | no         | The holder's rights to any portion of 
a transfer fee represented as a number referring to the percentage. |
| `agreements`            | yes        | An array of file objects representing
any agreement among token holders. |

### 2.4.3 Files

| Field Name       | Sub Fields | Description                                  |
|------------------|------------|----------------------------------------------|
| `type`           | no         | The MIME type of the file.                   |
| `description`    | no         | A description of what the file's purpose.    |
| `encrypted`      | yes        | Encryption object which specifies encryption 
type. |
| `primary_uri`    | no         | The primary method to be used to  retrieve the 
file. |
| `secondary_uris` | no      | An array of secondary URIs to retrieve the file.|

### 2.4.4  Extended Attributes

| Field Name       | Sub Fields | Description                                  |
|------------------|------------|----------------------------------------------|
| `attribute_name` | no         | Name for the attribute.                      |
| `value_type`     | no	        | Type for the value (for example _string_, 
_decimal_, _int_, or _vector_). |
| `value`          | no         | Value of the attribute.                      |

## Security Considerations

<!--
  All XLS documents must contain a section that discusses the security implications/considerations relevant to the proposed change. Include information that might be important for security discussions, surfaces risks, and can be used throughout the lifecycle of the proposal. For example, include security-relevant design decisions, concerns, important discussions, implementation-specific guidance and pitfalls, an outline of threats and risks, and how they are being addressed. Submissions missing the "Security Considerations" section may be rejected.

  The current placeholder is acceptable for a draft.

  TODO: Remove this comment before submitting
-->