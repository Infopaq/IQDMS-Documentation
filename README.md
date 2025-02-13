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
### Search TIN
This API is used to search TIN

<details>
<summary><code>GET</code></code><code><b>/api/e-ivnoice/SearchTIN?companyID={companyID}&appName={appName}</b></code></summary>

#### Parameters
> | name | data type | description | value example | rquirement |
> | -------------- | ---- | ----------- | ------------- | ----------- |
> | companyID | String | Client ID Provided by VSS | | Mandatory |
> | appName | String | Client Secret Provided by VSS | | Mandatory |
> |  | String | Client Secret Provided by VSS | | Mandatory |
> | appName | String | Client Secret Provided by VSS | | Mandatory |
> | appName | String | Client Secret Provided by VSS | | Mandatory |
> | appName | String | Client Secret Provided by VSS | | Mandatory |
> 
#### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`                | `Signature Token`                                |
> | `400`         | `application/json`                | [Standard Response Model](#General)                       |

##### Signature Token
> | name | data type | description | value example |
> | -------------- | ---- | ----------- | ------------- |
> | access_token | JWT Token | Encoded JWT token structure that contains the fields of the issued token, token protection attributes | |
> | token_type | String | Solution in this case returns only Bearer authentication tokens | Bearer |
> | expires_in | Number | The lifetime of the access token defined in seconds | 3600 |
> | submitDate | DateTime | Token Issued Date Time in UTC | 2025-02-12 13:00 |
> | expiredDate | DateTime | Token Expired Date Time in UTC  | 2025-02-12 14:00 |

</details>

