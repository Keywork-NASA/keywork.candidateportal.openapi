# Keywork.CandidatePortal.Openapi
This documentation will help you to identify all available endpoints, all expecting or returning contracts, as well as, creating a client for the CandidatePortal.API.
<br><br>
The CandidatePortalAPI is a service available for our clients that want to use their own Career Portal integrated with their Keywork subscrition.

# Creating a client
You can create a client following all endpoints and contracts documentation, or simple using the NswagStudio with the provided configuration file.
<br><br>

## Using NSwagStudio
1. Follow instalation steps provided by [NswagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).
2. Download and open the file [CandidatePortalApi.nswag](https://github.com/Keywork-NASA/keywork.candidateportal.openapi/blob/master/CandidatePortalApi.nswag)
3. On the Outputs column:
    1. Choose your client language
    2. Adjust the settings as your requirements. For more information, follow the information provided.
    3. Generate your client.
<br><br>

## Api Documentation
Every request, expects the header: tenant-descriptor. If it is not sent, a HTTP 500 will be returned.
<br><br>

### POST: **/api/account/signup**
- Allows to create a new candidate on the system.
- Expected payload:
```json
{
  "FirstName": "string",
  "LastName": "string",
  "Phone": "string",
  "Linkedin": "string",
  "Email": "string",
  "BirthDate": "0001-01-01T00:00:00.000Z",
  "IdentificationNumber": "string",
  "IdentificationTypeId": 0,
  "SocialSecurityNumber": "string",
  "TaxNumber": "string",
  "CivilStatusId": 0,
  "StudyLevelId": 0,
  "StreetName": "string",
  "PostalCode": "string",
  "City": "string",
  "Country": "string",
  "District": "string",
  "County": "string",
  "PortalUrl": "string", //Required
  "Errors": [
    "string"
  ],
  "Attachment": "string",
  "AttachmentName": "string",
  "AttachmentType": "string"
}
```
- Returned Responses:
    - HTTP 200 - For Success
    - HTTP 206 - For Partial Success
    - HTTP 400 - For Validation Errors
- Returned Type:
(Same as for input)
<br>

### PUT:  **/api/account/validate**
- Validates if a token stills active, if not a new token will be sended to the candidate email.
- Expected payload: 
```json
{
  "Token": "string", // Required
  "PortalUrl": "string" //Required
}
```
- Returned Responses
    - HTTP 200
        - NOT_EXISTS_OR_DELETED
        - SENT_NEW
        - VALID
        - EXPIRED_OR_USED

### PUT: **/api/account/confirm**
- Confirms the candidate account
- Expected payload: 
```json
{
  "Token": "string", // Required
  "Password": "string" // Required
}
```
- Returned Responses
    - HTTP 200
    - HTTP 400 - USED_OR_EXPIRED_TOKEN

### POST: **/api/account/login**
- Logins the candidate
- Expected payload: 
```json
{
  "Email": "string", // Required
  "Password": "string" // Required
}
```
- Returned Responses
    - HTTP 400 - INVALID_USER_OR_PASSWORD
    - HTTP 200 
- Return Type
    ```json
    {
        "UserId": "Guid",
        "Token": "string"
    }
    ```

### -> POST: **/api/Account/password/reset**
- Requests a password reset for the candidate. A email will be sent to the candidate email address.
- Expected payload: 
```json
{
  "Email": "string",    // Required
  "PortalUrl": "string" // Required
}
```
- Returned Responses
    - HTTP 202 
    - HTTP 404

### POST: **/api/Account/password/reset**
- Performs a password reset on the candidate account.
- Expected payload: 
```json
{       
  "Token": "string",        // Required
  "NewPassword": "string"   // Required
}
```
- Returned Responses
    - HTTP 202 
    - HTTP 400

### PUT: **/api/Candidate/{candidateId}**
- Updates candidate personal information
- Expected:
    - Parameter: CandidateId: Returned on the login.
    - payload:

```json
{
  "FirstName": "string",
  "LastName": "string",
  "Phone": "string",
  "Linkedin": "string",
  "Email": "string",
  "BirthDate": "0001-01-01T00:00:00.000Z",
  "IdentificationNumber": "string",
  "IdentificationTypeId": 0,
  "SocialSecurityNumber": "string",
  "TaxNumber": "string",
  "CivilStatusId": 0,
  "StudyLevelId": 0,
  "StreetName": "string",
  "PostalCode": "string",
  "City": "string",
  "Country": "string",
  "District": "string",
  "County": "string",
  "PortalUrl": "string",    // Required
  "Errors": [
    "string" // Used to return error messages
  ]
}
```
- Returned Responses:
    - HTTP 200 - For Success
    - HTTP 206 - For Partial Success
    - HTTP 400 - For Validation Errors
- Returned Type:
(Same as for input)

### GET: **/api/Candidate/{candidateId}**
- Retrieves candidate information
- Expected parameter: candidateId -> Returned after a success login.

- Returned Responses:
    - HTTP 200
    - HTTP 404

- Returned Type:
```json
{
  "FirstName": "string",
  "LastName": "string",
  "Phone": "string",
  "Linkedin": "string",
  "Email": "string",
  "BirthDate": "0001-01-01T00:00:00.000Z",
  "IdentificationNumber": "string",
  "IdentificationTypeId": 0,
  "SocialSecurityNumber": "string",
  "TaxNumber": "string",
  "CivilStatusId": 0,
  "StudyLevelId": 0,
  "StreetName": "string",
  "PostalCode": "string",
  "City": "string",
  "Country": "string",
  "District": "string",
  "County": "string",
  "CandidateId": "Guid",
  "GDPR": "bool",
  "Name": "string",
  "Resumees": [
      {
          "Id": "int",
          "FileExtension": "string",
          "FileName": "string",
          "FileSize": 0,
          "Description": "string",
          "CreationDate": "0001-01-01T00:00:00.000Z",
      }
  ],
  "AppliedOpportunities": [
      {

      }
  ]
}
```

### PUT: **/api/Candidate/{candidateId}/resumee**
- Adds a new resumee to the candidate account.
- Expected:
    - Parameter: CandidateId: Returned on the login.
    - payload:
```json
{
  "Attachment": "string",
  "AttachmentName": "string",
  "AttachmentType": "string",
  "Errors": [
    "string"
  ]
}
```
- Returned Responses
    - HTTP 200
    - HTTP 206 - When there are some errors. The input will payload will be returned with the errors on the errors property. 

### DELETE: **/api/Candidate/{candidateId}/forgetme**
- Deletes all the candidate information.
- Expected Parameter: CandidateId: Returned on the login.
- Returned Responses
    - HTTP 200
    - HTTP 400 
    - HTTP 404

 ### GET: **/api/Oportunity**
- Retrieves all open oportunities.
- QueryString parameters (not mandatory):
    - County - countryCode
    - Search - raw text to search for.
    - Skip - Number of oportunities to ignore from the begining of the list
    - Take - Number of oportunities to be returned.
    - CandidateId - Logedin candidate (if any)
- Returned Responses
    - HTTP 200
    - HTTP 400 
    - HTTP 404   
- Returned Type:
```json
[
  {
    "Id": 0,
    "Name": "string",
    "Location": "string",
    "Area": "string",
    "Specializations": [
      "string"
    ]
  }
]
```

 ### GET: **/api/Oportunity/{oportunityId}**
- Retrieves all the details of a given oportunity.
- Expected parameter: oportunityId.
- Returned Responses
    - HTTP 200
    - HTTP 404   
- Returned Type:
```json
{
  "Id": 0,
  "Name": "string",
  "Location": "string",
  "Area": "string",
  "Specializations": [
    "string"
  ],
  "Description": "string",
  "ExperienceYears": 0,
  "OportunityType": "string",
  "Duration": 0,
  "AnswearDeadline": "2021-12-03T00:08:37.217Z"
}
```

 ### POST: **/api/Oportunity/{oportunity}/apply**
- Aply the candidate to a specific oportunity
- Expected: 
    - parameter: oportunityId.
    - payload:
    ```json
    {
  "AttachmentId": 0,
  "CandidateId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "CoverLetter": "string"
}
    ```
- Returned Responses
    - HTTP 200
    - HTTP 400   
- Returned Type:
```json
{
  "Id": 0,
  "Name": "string",
  "Location": "string",
  "Area": "string",
  "Specializations": [
    "string"
  ],
  "Description": "string",
  "ExperienceYears": 0,
  "OportunityType": "string",
  "Duration": 0,
  "AnswearDeadline": "2021-12-03T00:08:37.217Z"
}
```

### GET: **/api/Configuration/candidate**
- Return all the configured fields for the candidate signup/update
- Returns: HTTP 200
- Return Type
```json
[
  {
    "Id": 0,
    "Name": "string",
    "IsRequired": true
  }
]
```

### GET: **/api/Configuration/countries**
- Return all the configured countries
- Returns: HTTP 200
- Return Type
```json
[
  {
    "Key": "string",    // Country Code
    "Value": "string"   // Country Name
  }
]
```

### GET: **/api/Configuration/portal**
- Return the configuration of the candidate portal
- Returns: HTTP 200
- Return Type
```json
{
  "CompanyLogoUrl": "string",
  "BannerImageUrl": "string",
  "BannerVideoUrl": "string",
  "BannerMobile": "string",
  "FaviconUrl": "string",
  "MenuTitle": "string",
  "BannerTitle": "string",
  "BannerDescription": "string",
  "BackgroundMenuColor": "string",
  "BackgroundColor": "string",
  "FontColor": "string",
  "PrimaryColor": "string",
  "ButtonFontColor": "string",
  "BrowserTitle": "string",
  "Linkedin": "string",
  "Instagram": "string",
  "Twitter": "string",
  "Facebook": "string",
  "MetaDescription": "string",
  "GDPR": true,
  "OpportunityShareUrl": "string",
  "PortalIsEnabled": true,
  "DefaultLanguage": "string",
  "OrderByProfileSkill": true,
  "Favicon": "string",
  "BannerImage": "string",
  "Logo": "string",
  "BannerVideo": "string",
  "WebSite": "string",
  "BannerMobileUrl": "string",
  "GDPRLink": "string",
  "OpportunityShareImage": "string"
}
```
