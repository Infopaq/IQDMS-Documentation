# IQDMS Documentation

## Revision History

| Name | Date       | Reason For Changes  | Version   |
| ---- | ---------- | ------------------- | --------- |
| Infopaq | 2025-02-13 | First Publish       | 1.0       |
| Infopaq | 2025-03-19 | Fixed typo and Update [Cancel Document](#cancel-document) API calling | 1.1 |

## General
### Standard Response Model for most request when Success(200)/BadRequest(400) occur.

##### StandardResponseModel
> | name | data type | description | value example |
> | -------------- | ---- | ----------- | ------------- |
> | isError | Boolean | False if error occurs | `false` |
> | error | String | Error that occurs | `"invalid_client"` |
> | error_description | String | Details of error occurs | `Client ID or Secret not found` |
> | uuid | String | Result of the API | `UUID of E-Invoice` |

### Authenticated Call
The header parameter apply to API Calls that required authentication.

> | header Parameter | data type | description | value example |
> | -------------- | ---- | ----------- | ------------- |
> | Authorization | String | Must contain access token issued prior to this call by [Get Token](#get-token) | `Bearer <Token Value>` |

### Document Type
The document type that used to determine e-invoice type.
> | name | value |
> | ---- | ----  | 
> | INVOICE | `INV` |
> | CREDIT_NOTE | `CN` |
> | DEBIT_NOTE | `DN` |
> | REFUND_NOTE | `RN` |
> | SELF-BILLED INVOICE | `SBINV` |
> | SELF-BILLED CREDIT_NOTE | `SBCN` |
> | SELF-BILLED DEBIT_NOTE | `SBDN` |
> | SELF-BILLED REFUND_NOTE | `SBRN` |

### Date Time Format
The system recognize only YYYY-MM-DD HH:MM date time format in UTC+8.

`Example : 2025-02-15 14:05:26`

## Authorization
### Get Token
This API is used to get token for API's that need authorization

<details>
<summary><code>POST</code></code><code><b>/api/Token/GetToken</b></code></summary>

#### Body Parameters
> | name | data type | description | value example | requirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | client_id | String | Client ID Provided by VSS | | Mandatory |
> | client_secret | String | Client Secret Provided by VSS | | Mandatory |
> | onbehalfof | String | Company ID provided by VSS | | Mandatory |
> | grant_type | String | Must be ‘client_credentials’ | | Mandatory |
> | scope | String | Optional, can be leave blank. | | Optional |

##### JSON Sample
```
{
  "client_id": "your-client-id-here",
  "client_secret": "your-client-secret-here",
  "grant_type": "client_credentials",
  "onbehalfof": "your-companyid-here",
  "scope": ""
}
```


#### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`                | `Signature Token` |
> | `400`         | `application/json`                | [Standard Response Model](#general) |

##### Signature Token
> | name | data type | description | value example |
> | -------------- | ---- | ----------- | ------------- |
> | access_token | JWT Token | Encoded JWT token structure that contains the fields of the issued token, token protection attributes | |
> | token_type | String | Solution in this case returns only Bearer authentication tokens | Bearer |
> | expires_in | Number | The lifetime of the access token defined in seconds | 3600 |
> | submitDate | DateTime | Token Issued Date Time in UTC | 2025-02-12 13:00 |
> | expiredDate | DateTime | Token Expired Date Time in UTC  | 2025-02-12 14:00 |

</details>

## IRB Functions
All the API call in this section need to contains [Authorization Header](#authenticated-call)

## TIN

### Search TIN
This API is used to search TIN

<details>
<summary><code>GET</code></code><code><b>/api/e-invoice/SearchTIN?companyID={companyID}&appName={appName}&type={type}&value={value}</b></code></summary>

#### Parameters
> | name | data type | description | value example | requirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | companyID | String | Company ID Provided by VSS | `GV` | Mandatory |
> | appName | String | Application Name Provided by VSS | `GV` | Mandatory |
> | type | String | NRIC, PASSPORT, BRN, ARMY | `BRN` | Mandatory |
> | value | String | The actual value of the ID Type selected. For example, if NRIC selected as ID Type, then pass the NRIC value here. | `2014087894` | Mandatory |

#### Responses

> | http code     | content-type | response |
> | --- | ---- | ---------- |
> | `200` | `application/json` | `{"isError":"false","uuid":"C25845632020"}` |
> | `400` | `application/json` | [Standard Response Model](#general) |

</details>

### Validate TIN
This API is used to validate TIN

<details>
<summary><code>GET</code></code><code><b>/api/e-invoice/ValidateTIN?companyID={companyID}&appName={appName}&tin={tin}&type={type}&value={value}</b></code></summary>

#### Query Parameters
> | name | data type | description | value example | requirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | companyID | String | Company ID Provided by VSS | `GV` | Mandatory |
> | appName | String | Application Name Provided by VSS | `GV` | Mandatory |
> | tin | String | The Tax Identification Number to get the validity of the tin. | `C25845632020` | Mandatory |
> | type | String | NRIC, PASSPORT, BRN, ARMY | `BRN` | Mandatory |
> | value | String | The actual value of the ID Type selected. For example, if NRIC selected as ID Type, then pass the NRIC value here. | `2014087894` | Mandatory |

#### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`                | `{"isError":"false","error_description":"{TIN is valid.}"}` |
> | `400`         | `application/json`                | [Standard Response Model](#general) |

</details>

## Documents

### Submit Document
This API is used to submit E-Invoice

<details>
<summary><code>GET</code></code><code><b>/api/e-invoice/SubmitDocument?companyID={companyID}&appName={appName}&appVersion={appVersion}&isSubmit={isSubmit}&isAutoSubmit={isAutoSubmit}</b></code></summary>

#### Query Parameters
> | name | data type | description | value example | requirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | companyID | String | Company ID Provided by VSS | `GV` | Mandatory |
> | appName | String | Application Name Provided by VSS | `GV` | Mandatory |
> | appVersion | String | Application Version | `1.0` | Mandatory |
> | isSubmit | String | (1, 0) Used to control if a user wants to submit to the IRB or just for testing purposes. Default is 1, which will submit to the IRB, where 0 is not. | `1` | Optional |
> | isAutoSubmit | String | (1,0) Used to control if a user wants to auto submit an invoice for more than 3 days. Default is 1, for invoice that is more than 3 days, the system will use the current date time as the e-invoice date time. | `1` | Optional |

Body of the request need to have a [document](#document) below, all the field length follow [IRB Standard](https://sdk.myinvois.hasil.gov.my/documents/invoice-v1-1/).

#### Document

* Supplier
> * Field for supplier information
* Customer
> * Field for buyer information
* Delivery
> * Field for delivery information

> | name | data type | description | value example | requirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | SupplierID | String | Supplier ID (for internal used only, won't be able to check at MyInvois Portal) | | Mandatory |
> | SupplierName | String | Name of business or individual who will be the supplier providing the goods / services in a commercial transaction. | | Mandatory |
> | SupplierTIN | String | Supplier’s TIN assigned by LHDNM | `C2584563222` | Mandatory |
> | SupplierSchemeType | String | NRIC/BRN/ARMY/PASSPORT | `BRN` | Mandatory |
> | SupplierSchemeID | String | Scheme Type Value | `202001234567` | Mandatory |
> | SupplierSSTNo | String | Supplier's SST No. | | Mandatory for SST-registrant |
> | SupplierTTXNo | String | Supplier's Tourism Tax No. | | Mandatory for tourism tax registrant |
> | SupplierAddress | String | Supplier's Address | | Mandatory |
> | SupplierCity | String | Supplier's City | | Mandatory |
> | SupplierState | String | Supplier's State | | Mandatory |
> | SupplierPostal | String | Supplier's Postal | | Mandatory |
> | SupplierCountry | String | Supplier's Country | | Mandatory |
> | SupplierPhone | String | Supplier's Phone | | Mandatory |
> | SupplierEmail | String | Supplier's Email | | Optional |
> | MSICCode | String | 5-digit numeric code that represent the supplier’s business nature and activity | `01111` | Mandatory |
> | BusinessActivity | String | Description of the Supplier’s business activity | `01111` | Mandatory |
> | CustomerID | String | Customer ID (for internal used only, won't be able to check at MyInvois Portal) | | Mandatory |
> | CustomerName | String | Name of recipient of goods / services who is the issuer of the self-billed e-Invoice in a commercial transaction | | Mandatory |
> | CustomerTIN | String | Recipient’s TIN assigned by LHDNM | `C2584563222` | Mandatory |
> | CustomerSchemeType | String | NRIC/BRN/ARMY/PASSPORT | `BRN` | Mandatory |
> | CustomerSchemeID | String | Scheme Type Value | `202001234567` | Mandatory |
> | CustomerSSTNo | String | Customer's SST No. | | Mandatory for SST-registrant |
> | CustomerTTXNo | String | Customer's Tourism Tax No. | | Mandatory for tourism tax registrant |
> | CustomerAddress | String | Customer's Address | | Mandatory |
> | CustomerCity | String | Customer's City | | Mandatory |
> | CustomerState | String | Customer's State | | Mandatory |
> | CustomerPostal | String | Customer's Postal | | Mandatory |
> | CustomerCountry | String | Customer's Country | | Mandatory |
> | CustomerPhone | String | Customer's Phone | | Mandatory |
> | CustomerEmail | String | Customer's Email | | Optional |
> | JobSheetNo | String | Internal Invoice Reference No. | | Mandatory |
> | BillNo | String | Internal Invoice Reference No. | | Mandatory |
> | BillDate | DateTime | Original Invoice Date | | Mandatory |
> | DocType | String | Document Type For Submit | [Document Type](#document-type) | Optional |
> | FrequencyOfBilling | String | Frequency of the invoice (e.g., Daily, Weekly, Biweekly, Monthly, Bimonthly, Quarterly, Half-yearly, Yearly, Others / Not Applicable) | `Daily` | Optional |
> | BillStartDate | DateTime | start date of the transaction interval | `2025-02-01` | Mandatory |
> | BillEndDate | DateTime | end date of the transaction interval | `2025-02-28` | Mandatory |
> | MatchedBillNo | String | Bill No. of the matched Invoice | | Mandatory if Credit Note/Refund Note for E-Invoice with UUID |
> | MatchedUUID | String | UUID of the matched Invoice | | Mandatory if Credit Note/Refund Note for E-Invoice with UUID |
> | MatchedBillDate | String | Internal Invoice Reference No. | | Mandatory if Credit Note/Refund Note for E-Invoice with UUID |
> | ServiceTax | Number | Service Tax Amount of the Invoice | | Mandatory |
> | ServiceTaxRate | Number | Service Tax Rate of the Invoice | | Mandatory |
> | DiscountAmount | Number | Discount Amount of the Invoice | | Mandatory |
> | AmountB4GST | Number | Total Amount before Tax of the Invoice | | Mandatory |
> | Amount | Number | Total Amount after Tax of the Invoice | | Mandatory |
> | RoundingAmount | Number | Rounding Amount of the Invoice | 0 | Mandatory |
> | VoucherAmount | Number |Voucher Amount of the Invoice | 0 | Mandatory |
> | [DocReference](#docreference) | object | Used to specify additional document reference like K1 Form | | Optional |
> | [JSDet](#jsdet) | object | Invoice line items | | Mandatory |
> | DeliveryID | String | Delivery ID (for internal used only, won't be able to check at MyInvois Portal) | | Optional |
> | DeliveryName | String | Name of shipping recipient of the products included in the e-Invoice in a commercial transaction | | Optional |
> | DeliveryTIN | String | TIN of the shipping recipient assigned by LHDNM | `C2584563222` | Optional |
> | DeliverySchemeType | String | NRIC/BRN/ARMY/PASSPORT | `BRN` | Optional |
> | DeliverySchemeID | String | Scheme Type Value | `202001234567` | Optional |
> | DeliveryAddress | String | Delivery's Address | | Mandatory |
> | DeliveryCity | String | Delivery's City | | Mandatory |
> | DeliveryState | String | Delivery's State | | Mandatory |
> | DeliveryPostal | String | Delivery's Postal | | Mandatory |
> | DeliveryCountry | String | Delivery's Country | | Mandatory |

##### DocReference
> | name | data type | description | value example | requirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | ID | String | Unique identifier assigned on the Declaration of Goods Imported. Multiple reference numbers can be separated by commas (,) without space. | `FTA` | Mandatory |
> | DocumentType | String | Type of the specified reference | `FreeTradeAgreement` | Mandatory |
> | DocumentDescription | String | Details of the reference | `ASEAN-Australia-New Zealand FTA (AANZFTA)` | Mandatory |

##### JSDet
> | name | data type | description | value example | requirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | ItemNo | String | Sequence of the item, will be sorted when submit. | `0010` | Mandatory |
> | Description | String | Description of the item. | `FreeTradeAgreement` | Mandatory |
> | Quantity | Number | Quantity of the item. | `1` | Mandatory |
> | [UOMID](https://sdk.myinvois.hasil.gov.my/codes/unit-types/) | String | Standard unit or system used to measure the product or service. | `XUN` | Mandatory |
> | UnitPrice | String | Unit Price of the item | `10` | Mandatory |
> | TotalPriceB4GST | Number | Total Price before tax of the item. | `10` | Mandatory |
> | TotalPrice | String | Total Price after tax of the item. | `0010` | Mandatory |
> | DiscountType | String | Discount Type of the item (%/RM) | `%` | Mandatory |
> | DiscountAmount | Number | Discount Amount of the item. | `1` | Mandatory |
> | ServiceTax | Number | Service tax of the item. | `0010` | Mandatory |
> | [JSDetTax](#jsdettax) | object | Tax detail | `FreeTradeAgreement` | Mandatory |
> | Remark | String | Remarks field |  | Optional |
> | [ClassificationCode](https://sdk.myinvois.hasil.gov.my/codes/classification-codes/) | String | Category of products or services being billed as a result of a commercial transaction. More than 1 classification codes can be added for goods / services included in the e-Invoice. | `001` | Mandatory |

##### JSDetTax
> | name | data type | description | value example | requirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | [TaxType](https://sdk.myinvois.hasil.gov.my/codes/tax-types/) | String | Tax types. | `06` | Mandatory |
> | TaxRate | Number | Tax Rate for the tax. | `0` | Mandatory |
> | TaxAmount | Number | Tax Amount. | `0` | Mandatory |
> | TaxableAmount | String | Taxable Amount for this tax of the given item | `10` | Mandatory |
> | AmountExemptTax | String | Unit Price of the item | `0` | Optional |
> | TaxExemptAmount | String | Unit Price of the item | `0` | Optional |

#### Responses
> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`                | `{"isError":"false","uuid":"Unique ID of the submission."}` [Standard Response Model](#general)                            |
> | `400`         | `application/json`                | [Standard Response Model](#general)                       |

</details>

### Get Document
This API allows caller to get full details of the document when requested by unique ID assigned to document by MyInvois System.

Details include original XML or JSON submission and additional tax authority metadata i.e., additional ID, and current status. As the document is submitted in XML or JSON, this API returns document information also in either XML or JSON.

Document with invalid status will not be returned. Details of the invalid document can be fetched by Get Document Details API.

<details>
<summary><code>GET</code></code><code><b>/api/e-invoice/GetDocumentcompanyID={companyID}&appName={appName}&uuid={uuid}</b></code></summary>

#### Parameters
> | name | data type | description | value example | requirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | companyID | String | Company ID Provided by VSS | `GV` | Mandatory |
> | appName | String | Application Name Provided by VSS | `GV` | Mandatory |
> | uuid | String | Unique ID of the document to retrieve.  | `F9D425P6DS7D8IU` | Mandatory |

#### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`                | [GetDocument](https://sdk.myinvois.hasil.gov.my/einvoicingapi/07-get-document/#successful-response) |
> | `400`         | `application/json`                | [Standard Response Model](#general) |

</details>

### Get Document Details
This API allows caller to get full details of the document when requested by unique ID assigned to document by MyInvois System including the detailed validation results

<details>
<summary><code>GET</code></code><code><b>/api/e-invoice/GetDocumentDetails?companyID={companyID}&appName={appName}&uuid={uuid}</b></code></summary>

#### Parameters
> | name | data type | description | value example | requirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | companyID | String | Company ID Provided by VSS | `GV` | Mandatory |
> | appName | String | Application Name Provided by VSS | `GV` | Mandatory |
> | uuid | String | Unique ID of the document to retrieve.  | `F9D425P6DS7D8IU` | Mandatory |

#### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`                | [GetDocumentDetails](https://sdk.myinvois.hasil.gov.my/einvoicingapi/08-get-document-details/#successful-response) |
> | `400`         | `application/json`                | [Standard Response Model](#general) |

</details>


### Cancel Document
This API allows issuer to cancel previously issued document, either self-induced cancellation or by accepting a rejection request made by the buyer.

<details>
<summary><code>PUT</code></code><code><b>/api/e-invoice/CancelDocument?companyID={companyID}&appName={appName}&uuid={uuid}</b></code></summary>

#### Query Parameters
> | name | data type | description | value example | requirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | companyID | String | Company ID Provided by VSS | `GV` | Mandatory |
> | appName | String | Application Name Provided by VSS | `GV` | Mandatory |
> | uuid | String | Unique ID of the document to retrieve.  | `F9D425P6DS7D8IU` | Mandatory |

### Body Parameters
> | name | data type | description | value example | requirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | Reason | String | ReasonReason for cancelling the document. (The length of the reason would be limited to 300 chars) | `Customer Cancel Order` | Mandatory |

#### Sample JSON
```
{
    "Reason": "Customer Cancel Order"
}
```

#### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`                | [CancelDocument](https://sdk.myinvois.hasil.gov.my/einvoicingapi/03-cancel-document/#outputs) |
> | `400`         | `application/json`                | [Standard Response Model](#general) |

#### Cancel Document Response Parameters
> | name | data type | description |
> | -------------- | ---- | ----------- |
> | uuid | String | UUID of the document |
> | status | String | Status of the document |
> | badRequestModel | [Standard Response Model](#standardresponsemodel) | Details of error if the request failed. |

##### Sample Response
```
{
  "uuid": "NM9WK1N89S6RJF3GCH5ZJ2KB45",
  "status": null,
  "error": {
    "propertyName": "",
    "propertyPath": "",
    "errorCode": "",
    "error": "",
    "errorMs": "",
    "innerError": ""
  },
  "badRequestModel": {
    "isError": true,
    "error": "",
    "error_description": "Documents cannot be canceled after 72 hours of submission.",
    "error_uri": "",
    "uuid": ""
  }
}
```

</details>
