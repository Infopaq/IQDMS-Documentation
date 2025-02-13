# IQDMS

Sandbox Base URL: https://vsstest.ddns.me:8090

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

## Authorization
### Get Token
This API is used to get token for API's that need authorization

<details>
<summary><code>GET</code></code><code><b>/api/Token/GetToken</b></code></summary>

#### Parameters
> | name | data type | description | value example | rquirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | client_id | String | Client ID Provided by VSS | | Mandatory |
> | client_secret | String | Client Secret Provided by VSS | | Mandatory |

#### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`                | `Signature Token`                                |
> | `400`         | `application/json`                | [Standard Response Model](#general)                       |

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

### Search TIN
This API is used to search TIN

<details>
<summary><code>GET</code></code><code><b>/api/e-invoice/SearchTIN?companyID={companyID}&appName={appName}&type={type}&value={value}</b></code></summary>

#### Parameters
> | name | data type | description | value example | rquirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | companyID | String | Company ID Provided by VSS | `GV` | Mandatory |
> | appName | String | Application Name Provided by VSS | `GV` | Mandatory |
> | type | String | NRIC, PASSPORTY, BRN, ARMY | `BRN` | Mandatory |
> | value | String | The actual value of the ID Type selected. For example, if NRIC selected as ID Type, then pass the NRIC value here. | `2014087894` | Mandatory |

#### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`                | `{"isError":"false","uuid":"C25845632020"}` [Standard Response Model](#general)                            |
> | `400`         | `application/json`                | [Standard Response Model](#general)                       |

</details>

### Validate TIN
This API is used to validate TIN

<details>
<summary><code>GET</code></code><code><b>/api/e-invoice/SearchTIN?companyID={companyID}&appName={appName}&tin={tin}&type={type}&value={value}</b></code></summary>

#### Parameters
> | name | data type | description | value example | rquirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | companyID | String | Company ID Provided by VSS | `GV` | Mandatory |
> | appName | String | Application Name Provided by VSS | `GV` | Mandatory |
> | tin | String | The Tax Identification Number to get the validity of the tin. | `C25845632020` | Mandatory |
> | type | String | NRIC, PASSPORTY, BRN, ARMY | `BRN` | Mandatory |
> | value | String | The actual value of the ID Type selected. For example, if NRIC selected as ID Type, then pass the NRIC value here. | `2014087894` | Mandatory |

#### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`                | `{"isError":"false","error_description":"{TIN is valid.}"}` [Standard Response Model](#general)                            |
> | `400`         | `application/json`                | [Standard Response Model](#general)                       |

</details>

### Submit Document
This API is used to submit E-Invoice

<details>
<summary><code>GET</code></code><code><b>/api/e-invoice/SubmitDocument?companyID={companyID}&appName={appName}&appVersion={appVersion}&isSubmit={isSubmit}&isAutoSubmit={isAutoSubmit}</b></code></summary>

#### Parameters
> | name | data type | description | value example | rquirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | companyID | String | Company ID Provided by VSS | `GV` | Mandatory |
> | appName | String | Application Name Provided by VSS | `GV` | Mandatory |
> | appVersion | String | Application Version | `1.0` | Mandatory |
> | isSubmit | String | (1, 0) Used to control if a user wants to submit to the IRB or just for testing purposes. Default is 1, which will submit to the IRB, where 0 is not. | `1` | Optional |
> | isAutoSubmit | String | (1,0) Used to control if a user wants to auto submit an invoice for more than 3 days. Default is 1, for invoice that is more than 3 days, the system will use the current date time as the e-invoice date time. | `1` | Optional |

Body of the request need to have a [document](#document) below.

#### Document
> | name | data type | description | value example | rquirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | client_id | String | Client ID Provided by VSS | | Mandatory |
> | client_secret | String | Client Secret Provided by VSS | | Mandatory |

#### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`                | `{"isError":"false","error_description":"{TIN is valid.}"}` [Standard Response Model](#general)                            |
> | `400`         | `application/json`                | [Standard Response Model](#general)                       |

</details>
