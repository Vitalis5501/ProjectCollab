# Property API Test Cases

Test Case | API Endpoint | Input Data | Expected Result | Expected Status Code | Response Time
---------|--------------|------------|-----------------|---------------------|---------------
Get Locations - Happy Path | GET /api/v1/locations | No parameters | List of all available locations | 200 OK | < 500ms
Get Locations - Empty List | GET /api/v1/locations | No locations exist | Empty array returned | 200 OK | < 500ms
Get Properties - Happy Path | GET /api/v1/properties | No parameters | List of all properties | 200 OK | < 1000ms
Get Properties - With Pagination | GET /api/v1/properties | page=1&limit=10 | Paginated property list | 200 OK | < 500ms
Get Properties - With Filters | GET /api/v1/properties | status=active&type=residential | Filtered property list | 200 OK | < 500ms
Add Property - Happy Path | POST /api/v1/properties | Valid property data | Property created successfully | 201 Created | < 1000ms
Add Property - Invalid Data | POST /api/v1/properties | Missing required fields | Validation error returned | 400 Bad Request | < 500ms
Add Property - Duplicate | POST /api/v1/properties | Existing property data | Conflict error returned | 409 Conflict | < 500ms
Get Group Report - Happy Path | GET /api/v1/properties/report/group | No parameters | Complete group report | 200 OK | < 1000ms
Get Group Report - No Data | GET /api/v1/properties/report/group | No report data exists | Empty report returned | 200 OK | < 500ms
Get Unit Tenants - Happy Path | GET /api/v1/properties/units/{unit_id}/tenants | Valid unit_id | List of unit tenants | 200 OK | < 500ms
Get Unit Tenants - Invalid Unit | GET /api/v1/properties/units/{unit_id}/tenants | Invalid unit_id | Unit not found error | 404 Not Found | < 500ms
Get Unit Tenants - Empty Unit | GET /api/v1/properties/units/{unit_id}/tenants | Valid unit_id (empty) | Empty tenant list | 200 OK | < 500ms
Get Resident Doc - Happy Path | GET /api/v1/properties/units/{unit_id}/tenants/file/download/{resident_doc_id} | Valid IDs | Document file returned | 200 OK | < 1000ms
Get Resident Doc - Invalid Doc | GET /api/v1/properties/units/{unit_id}/tenants/file/download/{resident_doc_id} | Invalid doc_id | Document not found | 404 Not Found | < 500ms
Get Resident Doc - No Permission | GET /api/v1/properties/units/{unit_id}/tenants/file/download/{resident_doc_id} | Valid IDs without access | Permission denied | 403 Forbidden | < 500ms
Upload Regulatory Agreement - Happy Path | POST /api/v1/properties/{property_id}/regulatory/upload | Valid file data | Agreement uploaded | 201 Created | < 2000ms
Upload Regulatory Agreement - Invalid File | POST /api/v1/properties/{property_id}/regulatory/upload | Invalid file format | Format error returned | 400 Bad Request | < 500ms
Upload Regulatory Agreement - Large File | POST /api/v1/properties/{property_id}/regulatory/upload | Oversized file | Size limit error | 400 Bad Request | < 500ms
Get Property - Happy Path | GET /api/v1/properties/{property_id} | Valid property_id | Property details returned | 200 OK | < 500ms
Get Property - Invalid ID | GET /api/v1/properties/{property_id} | Invalid property_id | Property not found | 404 Not Found | < 500ms
Get Property - No Permission | GET /api/v1/properties/{property_id} | Valid ID without access | Permission denied | 403 Forbidden | < 500ms
Edit Property - Happy Path | PUT /api/v1/properties/{property_id} | Valid update data | Property updated | 200 OK | < 1000ms
Edit Property - Invalid Data | PUT /api/v1/properties/{property_id} | Invalid update fields | Validation error | 400 Bad Request | < 500ms
Edit Property - Non-existent | PUT /api/v1/properties/{property_id} | Non-existent property | Property not found | 404 Not Found | < 500ms
Get Income Limit - Happy Path | GET /api/v1/properties/{property_id}/income_limit | Valid property_id | Income limits returned | 200 OK | < 500ms
Get Income Limit - No Data | GET /api/v1/properties/{property_id}/income_limit | Valid ID no limits | Empty limits returned | 200 OK | < 500ms
Get Regulatory Agreements - Happy Path | GET /api/v1/properties/{property_id}/regulatory | Valid property_id | Agreements list returned | 200 OK | < 500ms
Get Regulatory Agreements - No Data | GET /api/v1/properties/{property_id}/regulatory | Valid ID no agreements | Empty list returned | 200 OK | < 500ms
Get Rent Limit - Happy Path | GET /api/v1/properties/{property_id}/rent_limit | Valid property_id | Rent limits returned | 200 OK | < 500ms
Get Rent Limit - No Data | GET /api/v1/properties/{property_id}/rent_limit | Valid ID no limits | Empty limits returned | 200 OK | < 500ms
Get Units - Happy Path | GET /api/v1/properties/{property_id}/units | Valid property_id | List of units returned | 200 OK | < 1000ms
Get Units - With Filters | GET /api/v1/properties/{property_id}/units | status=occupied | Filtered units list | 200 OK | < 500ms
Get Units - No Units | GET /api/v1/properties/{property_id}/units | Valid ID no units | Empty units list | 200 OK | < 500ms
Get Unit Info - Happy Path | GET /api/v1/units/{id} | Valid unit_id | Unit details returned | 200 OK | < 500ms
Get Unit Info - Invalid ID | GET /api/v1/units/{id} | Invalid unit_id | Unit not found error | 404 Not Found | < 500ms
Get Unit Info - No Permission | GET /api/v1/units/{id} | Valid ID without access | Permission denied | 403 Forbidden | < 500ms

## Performance Test Cases
Test Case | API Endpoint | Input Data | Expected Result | Expected Status Code | Response Time
---------|--------------|------------|-----------------|---------------------|---------------
Concurrent Property Access | GET /api/v1/properties | 100 simultaneous requests | 95% successful responses | 200 OK | < 2000ms
Large Property List | GET /api/v1/properties | 1000+ properties | Paginated response | 200 OK | < 3000ms
Multiple File Uploads | POST /api/v1/properties/{property_id}/regulatory/upload | 10 simultaneous uploads | All files processed | 201 Created | < 5000ms
Bulk Unit Retrieval | GET /api/v1/properties/{property_id}/units | Property with 1000+ units | All units retrieved | 200 OK | < 3000ms

## Security Test Cases
Test Case | API Endpoint | Input Data | Expected Result | Expected Status Code | Response Time
---------|--------------|------------|-----------------|---------------------|---------------
Cross-Property Access | Any property endpoint | Different property's ID | Access denied error | 403 Forbidden | < 500ms
Invalid Token Access | Any property endpoint | Expired/invalid token | Authentication error | 401 Unauthorized | < 500ms
SQL Injection Attempt | GET /api/v1/properties | Malicious SQL in parameters | Injection prevented | 400 Bad Request | < 500ms
File Type Validation | POST /api/v1/properties/{property_id}/regulatory/upload | Malicious file type | Upload rejected | 400 Bad Request | < 500ms
XSS in Property Data | POST /api/v1/properties | Script in property name | XSS prevented | 400 Bad Request | < 500ms