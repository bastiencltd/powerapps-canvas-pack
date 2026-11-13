---
ontology: true
type: reference
domain: powerapps-canvas
status: active
summary: Catalog of the 19 operations of the audited app's sole Office365Users API, compact schemas copied verbatim, 0 data source, reported case inconsistencies
tags: [power-apps, connectors, office365users, power-fx, canvas]
related:
  - validation-rules
---

# Connector catalog — audited app

Source: `03-connectors-apis.json` (extraction 2026-11-13, agent-3, status complete). Audited app: `app_id of the audited session`, environment `environment_id of the audited session`.

## Current state

- **1 sole connected API**: `Office365Users` (raw output: `Available APIs: 1 - Office365Users`), **19 operations**.
- **0 data source** in the audited app (`Available Data Sources: 0 / No data sources found.`).

**Limit to flag**:

- `get_data_source_schema` **not testable under real conditions** (0 data source, call not usefully executable): this catalog documents only the APIs, not Dataverse/env entity schemas.
- The scope is **the audited app only**: the listed connector APIs are not the complete Power Platform catalog (no Sharepoint, SQL, Dataverse, etc. API in this app).
- The return schemas are copied **as returned by describe_api** (compact notation, no escaping added).

## Legend of the compact schema notation

| Token | Meaning |
|---|---|
| `s` / `n` / `b` / `d` | field of type text / number / boolean / date |
| `*[...]` | table (list of records) |
| `![...]` | single record (record) |
| nested `![...]` | record or table inside a record/table |

## Office365Users — 19 operations

| Operation | Return type | Parameters (raw) | Return schema (verbatim copy) |
|---|---|---|---|
| DirectReports | Table | [required] userId: String — User principal name or id. | `*[AccountEnabled:b, BusinessPhones:*[Value:s], City:s, CompanyName:s, Country:s, Department:s, DisplayName:s, GivenName:s, Id:s, JobTitle:s, Mail:s, MailNickname:s, OfficeLocation:s, PostalCode:s, Surname:s, TelephoneNumber:s, UserPrincipalName:s, mobilePhone:s]` |
| DirectReportsV2 | Record | [required] id: String — User principal name or id.<br>[optional] $select: String — Comma separated list of fields to select. Example: surname, department, jobTitle<br>[optional] $top: Number — Limit on the number of results to return. By default returns all entries. | `![value:*[aboutMe:s, accountEnabled:b, birthday:d, businessPhones:*[Value:s], city:s, companyName:s, country:s, department:s, displayName:s, givenName:s, hireDate:d, id:s, interests:*[Value:s], jobTitle:s, mail:s, mailNickname:s, mobilePhone:s, mySite:s, officeLocation:s, pastProjects:*[Value:s], postalCode:s, preferredLanguage:s, preferredName:s, responsibilities:*[Value:s], schools:*[Value:s], skills:*[Value:s], state:s, streetAddress:s, surname:s, userPrincipalName:s, userType:s]]` |
| HttpRequest **[behavior only]** | UntypedObject | [required] Uri: String — The full or relative URI. Example: https://graph.microsoft.com/{version}/{resource}.<br>[required] Method: String — The HTTP method (default is GET).<br>[required] file: Blob<br>[optional] ContentType: String — The content-type header for the body (default is application/json).<br>[optional] CustomHeader1..5: String — Custom header N. Specify in format: header-name: header-value | **none** (no `Return schema:` line in the raw output) |
| Manager | Record | [required] userId: String — User principal name or id. | `![AccountEnabled:b, BusinessPhones:*[Value:s], City:s, CompanyName:s, Country:s, Department:s, DisplayName:s, GivenName:s, Id:s, JobTitle:s, Mail:s, MailNickname:s, OfficeLocation:s, PostalCode:s, Surname:s, TelephoneNumber:s, UserPrincipalName:s, mobilePhone:s]` |
| ManagerV2 | Record | [required] id: String — User principal name or id.<br>[optional] $select: String — Comma separated list of fields to select. Example: surname, department, jobTitle | `![aboutMe:s, accountEnabled:b, birthday:d, businessPhones:*[Value:s], city:s, companyName:s, country:s, department:s, displayName:s, givenName:s, hireDate:d, id:s, interests:*[Value:s], jobTitle:s, mail:s, mailNickname:s, mobilePhone:s, mySite:s, officeLocation:s, pastProjects:*[Value:s], postalCode:s, preferredLanguage:s, preferredName:s, responsibilities:*[Value:s], schools:*[Value:s], skills:*[Value:s], state:s, streetAddress:s, surname:s, userPrincipalName:s, userType:s]` |
| MyProfile | Record | **no parameter listed in the raw output** (parameters not invented) | `![AccountEnabled:b, BusinessPhones:*[Value:s], City:s, CompanyName:s, Country:s, Department:s, DisplayName:s, GivenName:s, Id:s, JobTitle:s, Mail:s, MailNickname:s, OfficeLocation:s, PostalCode:s, Surname:s, TelephoneNumber:s, UserPrincipalName:s, mobilePhone:s]` |
| MyProfileV2 | Record | [optional] $select: String — Comma separated list of fields to select. Example: surname, department, jobTitle | `![aboutMe:s, accountEnabled:b, birthday:d, businessPhones:*[Value:s], city:s, companyName:s, country:s, department:s, displayName:s, givenName:s, hireDate:d, id:s, interests:*[Value:s], jobTitle:s, mail:s, mailNickname:s, mobilePhone:s, mySite:s, officeLocation:s, pastProjects:*[Value:s], postalCode:s, preferredLanguage:s, preferredName:s, responsibilities:*[Value:s], schools:*[Value:s], skills:*[Value:s], state:s, streetAddress:s, surname:s, userPrincipalName:s, userType:s]` |
| MyTrendingDocuments | Record | [optional] $filter: String — An OData filter to filter the resources selected. Filter selected resources on ResourceVisualization/Type or ResourceVisualization/containerType<br>[optional] extractSensitivityLabel: Boolean<br>[optional] fetchSensitivityLabelMetadata: Boolean | `![value:*[id:s, resourceVisualization:![containerDisplayName:s, containerType:s, containerWebUrl:s, mediaType:s, previewImageUrl:s, previewText:s, title:s, type:s], sensitivityLabelInfo:*[color:s, displayName:s, isEnabled:b, isEncrypted:b, isParent:b, name:s, parentSensitivityLabelId:s, priority:n, sensitivityLabelId:s, tooltip:s], weight:n]]` |
| RelevantPeople | Record | [required] userId: String — User principal name or id. | `![value:*[birthday:s, companyName:s, department:s, displayName:s, givenName:s, id:s, imAddress:s, isFavorite:b, jobTitle:s, officeLocation:s, personNotes:s, phones:*[number:s, type:s], profession:s, scoredEmailAddresses:*[address:s, relevanceScore:n], surname:s, userPrincipalName:s]]` |
| SearchUser | Table | [optional] searchTerm: String — Search string (applies to: display name, given name, surname, mail, mail nickname and user principal name).<br>[optional] top: Number — Limit on the number of results to return. By default returns all entries. | `*[AccountEnabled:b, BusinessPhones:*[Value:s], City:s, CompanyName:s, Country:s, Department:s, DisplayName:s, GivenName:s, Id:s, JobTitle:s, Mail:s, MailNickname:s, OfficeLocation:s, PostalCode:s, Surname:s, TelephoneNumber:s, UserPrincipalName:s, mobilePhone:s]` |
| SearchUserV2 | Record | [optional] searchTerm: String<br>[optional] top: Number — Minimum value is 1. Default value is 1000.<br>[optional] isSearchTermRequired: Boolean<br>[optional] skipToken: String — Skip token to get next users. | `!['@odata.nextLink':s, value:*[AccountEnabled:b, BusinessPhones:*[Value:s], City:s, CompanyName:s, Country:s, Department:s, DisplayName:s, GivenName:s, Id:s, JobTitle:s, Mail:s, MailNickname:s, OfficeLocation:s, PostalCode:s, Surname:s, TelephoneNumber:s, UserPrincipalName:s, mobilePhone:s]]` |
| TrendingDocuments | Record | [required] id: String — User principal name or id.<br>[optional] $filter: String (same text as MyTrendingDocuments)<br>[optional] extractSensitivityLabel: Boolean<br>[optional] fetchSensitivityLabelMetadata: Boolean | `![value:*[id:s, resourceVisualization:![containerDisplayName:s, containerType:s, containerWebUrl:s, mediaType:s, previewImageUrl:s, previewText:s, title:s, type:s], sensitivityLabelInfo:*[color:s, displayName:s, isEnabled:b, isEncrypted:b, isParent:b, name:s, parentSensitivityLabelId:s, priority:n, sensitivityLabelId:s, tooltip:s], weight:n]]` |
| UpdateMyPhoto **[behavior only]** | Boolean | [required] Content-Type: String — Image content type (like 'image/jpeg')<br>[required] file: Blob | **none** (no `Return schema:` line in the raw output) |
| UpdateMyProfile **[behavior only]** | Boolean | [optional] aboutMe: String<br>[optional] birthday: DateTime<br>[optional] interests: Table<br>[optional] mySite: String<br>[optional] pastProjects: Table<br>[optional] schools: Table<br>[optional] skills: Table | **none** (no `Return schema:` line in the raw output) |
| UserPhoto | Image | [required] userId: String — User principal name or id. | **none** (no `Return schema:` line in the raw output) |
| UserPhotoMetadata | Record | [required] userId: String — User principal name or id. | `![ContentType:s, HasPhoto:b, Height:n, ImageFileExtension:s, Width:n]` |
| UserPhotoV2 | Image | [required] id: String — User principal name or id. | **none** (no `Return schema:` line in the raw output) |
| UserProfile | Record | [required] userId: String — User principal name or id. | `![AccountEnabled:b, BusinessPhones:*[Value:s], City:s, CompanyName:s, Country:s, Department:s, DisplayName:s, GivenName:s, Id:s, JobTitle:s, Mail:s, MailNickname:s, OfficeLocation:s, PostalCode:s, Surname:s, TelephoneNumber:s, UserPrincipalName:s, mobilePhone:s]` |
| UserProfileV2 | Record | [required] id: String — User principal name or id.<br>[optional] $select: String — Comma separated list of fields to select. Example: surname, department, jobTitle | `![aboutMe:s, accountEnabled:b, birthday:d, businessPhones:*[Value:s], city:s, companyName:s, country:s, department:s, displayName:s, givenName:s, hireDate:d, id:s, interests:*[Value:s], jobTitle:s, mail:s, mailNickname:s, mobilePhone:s, mySite:s, officeLocation:s, pastProjects:*[Value:s], postalCode:s, preferredLanguage:s, preferredName:s, responsibilities:*[Value:s], schools:*[Value:s], skills:*[Value:s], state:s, streetAddress:s, surname:s, userPrincipalName:s, userType:s]` |

## Operations without Return schema (5)

5 operations have **no `Return schema:` line** in the raw output of describe_api ; `return_schema_raw` is recorded there as `null` (nothing invented) :

1. **HttpRequest** (UntypedObject)
2. **UpdateMyPhoto** (Boolean)
3. **UpdateMyProfile** (Boolean)
4. **UserPhoto** (Image)
5. **UserPhotoV2** (Image)

> Internal inconsistency in the source : the `anomalies` list of 03-connectors-apis.json reports « **6** operations sans ligne Return schema » but **enumerates only 5**, and the actual tally of the 19 operations confirms **5**. It is the number 5 that is exact.

## Operations [behavior only] (3)

3 operations are marked `[behavior only]` (usable as an event behavior, not as a typed-value expression) :

- **HttpRequest** → UntypedObject
- **UpdateMyPhoto** → Boolean
- **UpdateMyProfile** → Boolean

## Reported case inconsistencies

- **V1 schemas** (DirectReports, Manager, MyProfile, SearchUser, UserProfile, SearchUserV2.value) : fields in **upper** initial case (`AccountEnabled`, `BusinessPhones:*[Value:s]`, `City:s`...) **except** `mobilePhone:s` in **lowercase**. The nested container is named `BusinessPhones` with a member `Value` (both uppercase) while `mobilePhone` is entirely lowercase.
- **V2 schemas** : consistent in camelCase (`accountEnabled`, `businessPhones`...) but the array member stays `Value` with an initial capital (`businessPhones:*[Value:s]`, `interests:*[Value:s]`) and `mobilePhone:s` keeps its original case.
- Consequence for formulas : write `BusinessPhones` vs `businessPhones` **and** `Value` vs `value` depending on the V1/V2 version of the called operation ; never transfer the case from one version to the other.

## Other recorded anomalies (source 03)

- Typo recorded as-is in the description of HttpRequest : « 1st **segement** » (instead of « segment »).
- MyProfile : no parameter listed in the raw output (invent nothing in its place).
- The two "trending documents" schemas (MyTrendingDocuments / TrendingDocuments) and the three V2 profile schemas are **identical field for field** between their operations.
