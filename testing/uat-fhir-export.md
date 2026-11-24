# UAT Test Script: FHIR Export

**Test ID:** UAT-005  
**Feature:** FHIR R4 Patient Export  
**Version:** 1.0  
**Created:** 2026-11-16  
**Tester:** Codex (automated HTTP UAT)\
**Test Date:** 2026-11-18

---

## Test Environment

**Requirements:**
- Clinic EMR application running
- Access to http://127.0.0.1:8080/patients
- Test patients with diverse data
- JSON viewer/validator (browser extension or online tool)
- FHIR validator (optional): https://validator.fhir.org/

**Pre-Test Setup:**
- [X] Application is running
- [X] Test patients created with various data combinations
- [X] JSON parser available locally

**Execution notes**

- Tester was Codex. Execution date was 2026-11-18, Asia/Rangoon. The initial request to http://127.0.0.1:8080/patients returned HTTP 200. Testing proceeded against the already running Windows practice instance, app 0.1.0, PostgreSQL on port 54329, schema 2, source commit 8f310c1.
- Baseline was 250 patients, 228 active and 22 inactive. Existing fixtures supplied Japanese, Arabic, Thai and Burmese names, all four administrative genders, all date precisions, missing contacts and long text.
- One fictional complete patient, P00000251 / 126962ca-610d-4264-8118-e26cf4e61eaf (John Michael Smith), was registered through the application. Cases 26 and 27 edited and deactivated that fixture, then restored its original name and Active status. All 250 original patient records were confirmed unchanged. Final counts were 251 total, 229 active and 22 inactive. The new fixture remains for review, with its test revisions preserved.
- Tests followed live detail-page links, fetched attachment responses, submitted application forms, parsed actual response bytes as strict UTF-8 JSON, and compared values with read-only PostgreSQL queries. Native browser automatic-download behavior was unverified.
- Local JSON parsing replaced the public JSONLint step in Case 21. Case 22 used the installed HL7 validator CLI 7.0.3 with Java 17.0.16, -version 4.0.1 and -tx n/a. Its initial cache-permission error was resolved using a writable copy of the installed packages. Remote terminology checking and receiver-specific profiles were outside this validation.
- The test harness initially compared timestamp strings with different time-zone offsets and literal name text with escaped XHTML. Those comparisons were corrected and the affected checks were rerun before recording these results.
- Cases 3 and 4 were evaluated against the original script expectations. The current mapping contract in docs/fhir.md specifies one namespaced MRN identifier and a full name in name.text.

---

## Test Objective

Verify that FHIR R4 export:
- Produces valid FHIR R4 (4.0.1) Patient resources
- Includes all patient data correctly
- Handles Unicode names properly
- Supports partial birth dates
- Includes identifiers (UUID and MRN)
- Downloads as proper JSON file
- Follows FHIR specification

---

## Test Cases

### Test Case 1: Basic FHIR Export

**Steps:**
1. Navigate to a patient detail page (e.g., "John Smith")
2. Click "Export FHIR" or equivalent button
3. File downloads as: `patient-{UUID}.fhir.json`

**Expected Results:**
- ✅ JSON file downloads automatically
- ✅ Filename format: `patient-{UUID}.fhir.json`
- ✅ File opens in text editor or JSON viewer
- ✅ Content-Type header: `application/fhir+json`

**Actual Results:**

P00000251 detail download link returned HTTP 200, application/fhir+json, and Content-Disposition attachment; filename="patient-126962ca-610d-4264-8118-e26cf4e61eaf.fhir.json". Saved the actual UTF-8 response bytes as complete-patient-126962ca-610d-4264-8118-e26cf4e61eaf.fhir.json and parsed the saved content. Native browser automatic-download behavior was outside this HTTP execution.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 2: FHIR Resource Structure

**Steps:**
1. Export patient FHIR JSON
2. Open in JSON viewer
3. Examine top-level structure

**Expected Results:**
- ✅ `"resourceType": "Patient"`
- ✅ `"id"` field present (UUID)
- ✅ Valid JSON structure (no syntax errors)
- ✅ Properly formatted/indented

**Actual Results:**

The response parsed as an object with resourceType=Patient and id=126962ca-610d-4264-8118-e26cf4e61eaf. It used two-space indentation and a final newline.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 3: Patient Identifiers

**Steps:**
1. Export patient with known UUID and MRN
2. Examine `identifier` array

**Expected Results:**
```json
"identifier": [
  {
    "system": "urn:uuid:...",
    "value": "{patient-uuid}"
  },
  {
    "type": {
      "coding": [
        {
          "system": "http://terminology.hl7.org/CodeSystem/v2-0203",
          "code": "MR"
        }
      ]
    },
    "value": "P00000XXX"
  }
]
```
- ✅ Two identifiers present
- ✅ First: UUID with clinic namespace
- ✅ Second: MRN with type "MR" (Medical Record)
- ✅ Values match patient record

**Actual Results:**

The UUID is in resource.id. identifier contained [{"system":"urn:uuid:2fbd100a-7684-4c30-9dc7-e1a28066f93a","value":"P00000251"}]. Exactly one clinic-namespaced MRN identifier was exported, with no UUID identifier entry and no MR type coding. The two-identifier structure in the script was absent. Both exported identity values matched PostgreSQL. docs/fhir.md specifies the one-MRN-identifier mapping.

**Status:** [ ] Pass / [X] Fail

---

### Test Case 4: Patient Name (Full Name)

**Test Patient:** John Michael Smith

**Expected Results:**
```json
"name": [
  {
    "text": "John Michael Smith",
    "family": "Smith",
    "given": ["John", "Michael"]
  }
]
```
- ✅ `text` contains full name
- ✅ `family` contains last name
- ✅ `given` array contains first/middle names
- ✅ Array format correct

**Actual Results:**

P00000251 exported name=[{"text":"John Michael Smith"}]. family and given were absent, so the structured-name expectation failed. The internal record stores one full-name string and docs/fhir.md specifies name.text.

**Status:** [ ] Pass / [X] Fail

---

### Test Case 5: Patient Name (Single Name/Mononym)

**Test Patient:** Madonna (single name)

**Expected Results:**
```json
"name": [
  {
    "text": "Madonna"
  }
]
```
- ✅ Only `text` field present
- ✅ No `family` or `given` fields (optional for single names)
- ✅ Name handled correctly

**Actual Results:**

P00000009 exported name=[{"text":"Madonna"}], with no family or given properties.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 6: Unicode Name Export

**Test Patient:** 山田太郎 (Japanese)

**Expected Results:**
```json
"name": [
  {
    "text": "山田太郎"
  }
]
```
- ✅ Unicode characters preserved
- ✅ No character corruption
- ✅ Proper UTF-8 encoding

**Actual Results:**

Strict UTF-8 decoding, original byte checks, and JSON name comparison passed for P00000005 山田太郎; P00000006 محمد أحمد; P00000007 สมชาย ใจดี; P00000001 Fictional Patient — မေမေ (updated). The script’s Japanese placeholder was resolved to 山田太郎.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 7: Gender Field

**Test 7A: Male**
- Export patient with gender: male
- Expected: `"gender": "male"`

**Test 7B: Female**
- Export patient with gender: female
- Expected: `"gender": "female"`

**Test 7C: Other**
- Export patient with gender: other
- Expected: `"gender": "other"`

**Test 7D: Unknown**
- Export patient with gender: unknown
- Expected: `"gender": "unknown"`

**Expected Results:**
- ✅ Gender field matches patient record
- ✅ Value is FHIR-compliant lowercase string

**Actual Results:**

All four lowercase codes matched PostgreSQL: P00000002=male, P00000003=female, P00000018=other, P00000001=unknown.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 8: Birth Date (Full Date)

**Test Patient:** Birth date: 1985-06-15

**Expected Results:**
```json
"birthDate": "1985-06-15"
```
- ✅ Format: YYYY-MM-DD
- ✅ Matches patient record

**Actual Results:**

P00000251 exported birthDate="1985-06-15", equal to the database.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 9: Birth Date (Partial - Year-Month)

**Test Patient:** Birth date: 1990-03

**Expected Results:**
```json
"birthDate": "1990-03"
```
- ✅ Format: YYYY-MM (partial date supported in FHIR)
- ✅ Matches patient record

**Actual Results:**

P00000013 exported birthDate="1990-03" with its year-month precision unchanged.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 10: Birth Date (Partial - Year Only)

**Test Patient:** Birth date: 1975

**Expected Results:**
```json
"birthDate": "1975"
```
- ✅ Format: YYYY (year-only supported in FHIR)
- ✅ Matches patient record

**Actual Results:**

P00000014 exported birthDate="1975"; no month or day was invented.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 11: Birth Date (Empty/Missing)

**Test Patient:** No birth date

**Expected Results:**
- ✅ `birthDate` field NOT present in JSON OR
- ✅ `birthDate` field is null/empty
- ✅ Export still valid without birth date

**Actual Results:**

P00000004 exported successfully with birthDate omitted. PostgreSQL birth_date was empty.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 12: Active Status

**Test 12A: Active Patient**

**Expected Results:**
```json
"active": true
```
- ✅ Boolean value `true`

**Actual Results:**

P00000251 exported the JSON boolean active=true, matching PostgreSQL.

**Status:** [X] Pass / [ ] Fail

---

**Test 12B: Inactive Patient**

**Expected Results:**
```json
"active": false
```
- ✅ Boolean value `false`

**Actual Results:**

P00000250 exported the JSON boolean active=false, matching PostgreSQL.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 13: Telecom (Phone and Email)

**Test Patient:** 
- Phone: +1-555-0100
- Email: john@example.com

**Expected Results:**
```json
"telecom": [
  {
    "system": "phone",
    "value": "+1-555-0100"
  },
  {
    "system": "email",
    "value": "john@example.com"
  }
]
```
- ✅ Array contains both entries
- ✅ `system` field correct ("phone", "email")
- ✅ `value` matches patient record

**Actual Results:**

P00000251 exported both phone +1-555-0100 and email john@example.com, with their correct system codes and original values.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 14: Telecom (Phone Only)

**Test Patient:** Phone: +1-555-0100, No email

**Expected Results:**
```json
"telecom": [
  {
    "system": "phone",
    "value": "+1-555-0100"
  }
]
```
- ✅ Only phone entry present

**Actual Results:**

P00000020 exported exactly one phone entry. Its value matched PostgreSQL, and there was no email entry.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 15: Telecom (Email Only)

**Test Patient:** Email: test@example.com, No phone

**Expected Results:**
```json
"telecom": [
  {
    "system": "email",
    "value": "test@example.com"
  }
]
```
- ✅ Only email entry present

**Actual Results:**

P00000025 exported exactly one email entry. Its value matched PostgreSQL, and there was no phone entry.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 16: Telecom (Empty)

**Test Patient:** No phone, no email

**Expected Results:**
- ✅ `telecom` field NOT present OR
- ✅ `telecom` is empty array `[]`

**Actual Results:**

P00000004 had no phone or email. telecom was absent from the JSON.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 17: Address Field

**Test Patient:**
```
123 Main Street
Apt 4B
Springfield, IL 62701
```

**Expected Results:**
```json
"address": [
  {
    "text": "123 Main Street\nApt 4B\nSpringfield, IL 62701"
  }
]
```
- ✅ Address in `text` field
- ✅ Line breaks preserved or formatted
- ✅ Complete address present

**Actual Results:**

P00000251 exported the complete three-line address in address[0].text. Both LF line breaks were preserved after JSON decoding and matched PostgreSQL.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 18: Address (Empty)

**Test Patient:** No address

**Expected Results:**
- ✅ `address` field NOT present OR
- ✅ `address` is empty array

**Actual Results:**

P00000004 had an empty database address. The address property was omitted.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 19: Complete Patient Export

**Test Patient with ALL fields:**
- Name: John Michael Smith
- MRN: P00000123
- Birth Date: 1985-06-15
- Gender: male
- Phone: +1-555-0100
- Email: john@example.com
- Address: 123 Main St, Springfield, IL
- Active: true

**Expected Results:**
- ✅ All fields present in FHIR export
- ✅ All values match patient record
- ✅ Proper FHIR structure

**Actual Results:**

Complete fictional patient P00000251 (John Michael Smith) matched PostgreSQL for UUID, MRN and clinic namespace, name, 1985-06-15 birth date, male, phone, email, multiline address, active=true, versionId and lastUpdated. meta.profile was omitted. A generated escaped XHTML narrative was included. The script’s illustrative MRN P00000123 was replaced by the newly allocated P00000251.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 20: Minimal Patient Export

**Test Patient with ONLY name:**
- Name: Test Patient
- All other fields empty/default

**Expected Results:**
- ✅ Export succeeds
- ✅ Contains required fields: resourceType, id, name
- ✅ Optional fields absent or empty
- ✅ Still valid FHIR

**Actual Results:**

P00000004 (Jane Doe) had only the entered name plus registration defaults. Export succeeded with resourceType, id, identifier, name, active, unknown gender, meta and narrative. birthDate, telecom and address were omitted.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 21: JSON Validation

**Steps:**
1. Export any patient
2. Copy JSON content
3. Validate at: https://jsonlint.com/

**Expected Results:**
- ✅ Valid JSON (no syntax errors)
- ✅ Proper quotation marks
- ✅ Proper commas
- ✅ Proper brackets

**Actual Results:**

All 251 current patient exports parsed locally using JSON.parse and matched every stored demographic field, identity value, administrative gender, status, version and timestamp. This included active/inactive records, partial or missing dates, missing contacts, Unicode and long text. Local parsing replaced the script’s public JSONLint upload.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 22: FHIR Validation (Optional)

**Steps:**
1. Export patient
2. Go to: https://validator.fhir.org/
3. Upload/paste FHIR JSON
4. Validate against FHIR R4

**Expected Results:**
- ✅ Validates as FHIR R4 (4.0.1)
- ✅ No critical errors
- ✅ Warnings acceptable (implementation-specific)

**Actual Results:**

All five actual saved response files passed the local HL7 FHIR validator against base R4 4.0.1, with 0 errors and 0 warnings across 5 OperationOutcomes. Local Java 17 validation replaced uploading resources to the public validator. Remote terminology was disabled with -tx n/a; no receiver-specific profile was claimed.

**Status:** [X] Pass / [ ] Fail
**Notes:** Validation transcript and OperationOutcomes are retained in .local/uat005-validator-20261118.log and .local/uat005-validator-output-20261118.json. The five saved samples were validated together.

---

### Test Case 23: Content-Type Header

**Steps:**
1. Export patient
2. Check HTTP response headers (browser dev tools)

**Expected Results:**
- ✅ Content-Type: `application/fhir+json`
- ✅ Content-Disposition: `attachment; filename="patient-{id}.fhir.json"`

**Actual Results:**

Content-Type was application/fhir+json. Content-Disposition was attachment; filename="patient-126962ca-610d-4264-8118-e26cf4e61eaf.fhir.json".

**Status:** [X] Pass / [ ] Fail

---

### Test Case 24: Last-Modified Header

**Steps:**
1. Export patient
2. Check Last-Modified header

**Expected Results:**
- ✅ Last-Modified header present
- ✅ Matches patient's `updated_at` timestamp
- ✅ Proper HTTP date format

**Actual Results:**

P00000251 Last-Modified=Wed, 18 Nov 2026 00:32:47 GMT. It equaled PostgreSQL updated_at at HTTP whole-second precision. meta.lastUpdated preserved the fractional precision.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 25: Multiple Exports Same Patient

**Steps:**
1. Export patient
2. Save/note file content
3. Export same patient again
4. Compare files

**Expected Results:**
- ✅ Both exports identical (assuming no edits between)
- ✅ Consistent export output
- ✅ Deterministic (not random)

**Actual Results:**

Two consecutive exports of P00000251 were byte-for-byte identical (1058 bytes), with identical Last-Modified and meta values.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 26: Export After Edit

**Steps:**
1. Export patient, save JSON
2. Edit patient (change name)
3. Export again
4. Compare exports

**Expected Results:**
- ✅ Second export reflects new name
- ✅ Export stays current with database

**Actual Results:**

P00000251 export reflected the saved new name in name.text and its narrative. versionId increased 5 to 6, lastUpdated advanced, and UUID/MRN remained stable. The original fictional name was restored afterward.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 27: Export Inactive Patient

**Steps:**
1. Deactivate a patient
2. Navigate to patient detail
3. Export FHIR

**Expected Results:**
- ✅ Export still available
- ✅ `"active": false` in JSON
- ✅ All data present

**Actual Results:**

P00000251 was deactivated through its edit form. Its detail download link remained available and returned active=false. All demographic and identifier values matched the prior export. The fixture was reactivated afterward.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 28: Direct Export URL

**Steps:**
1. Get patient UUID
2. Navigate to: `http://127.0.0.1:8080/patients/{uuid}/export/fhir`

**Expected Results:**
- ✅ FHIR JSON downloads directly
- ✅ No HTML wrapper

**Actual Results:**

Direct GET /patients/126962ca-610d-4264-8118-e26cf4e61eaf/export/fhir returned HTTP 200 and a JSON attachment containing the Patient resource, with no HTML document wrapper. Its XHTML narrative remained inside the JSON text field.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 29: Export Non-Existent Patient

**Steps:**
1. Try to export fake UUID: 
   `http://127.0.0.1:8080/patients/00000000-0000-0000-0000-000000000000/export/fhir`

**Expected Results:**
- ✅ 404 Not Found OR
- ✅ Error message
- ✅ No crash

**Actual Results:**

The all-zero UUID returned HTTP 404 and “This patient could not be found.” Malformed UUID also returned 404. /healthz returned 200 with ok immediately afterward.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 30: File Size and Performance

**Steps:**
1. Export patient
2. Check file size
3. Note download time

**Expected Results:**
- ✅ File size reasonable (typically 1-5 KB)
- ✅ Downloads instantly (<1 second)

**Actual Results:**

Five representative patients, including 200-character name and 1000-character address fixtures, were each exported three times. P00000251: 1058 bytes (1.033 KiB), 3.019/1.755/1.382 ms; P00000004: 636 bytes (0.621 KiB), 1.341/1.367/1.58 ms; P00000005: 685 bytes (0.669 KiB), 2.82/4.024/2.231 ms; P00000010: 1020 bytes (0.996 KiB), 2.104/1.73/1.349 ms; P00000046: 2730 bytes (2.666 KiB), 1.342/1.406/1.335 ms. All 15 measured local responses completed in under one second. The 1–5 KB size range in the script is illustrative; actual sizes vary with supplied data.

**Status:** [X] Pass / [ ] Fail

---

## Edge Cases

**Test if time permits:**

- [X] Export patient with 200-character name. P00000010 exported 1020 bytes. The complete original field value matched PostgreSQL.
- [X] Export patient with 1000-character address. P00000046 exported 2730 bytes. The complete original field value matched PostgreSQL.
- [X] Export patient with all Unicode name. Case 6 verified 山田太郎, محمد أحمد and สมชาย ใจดี.
- [ ] Export during high system load. This optional load check was unverified. Case 30 measured 15 local requests under the observed conditions.
- [X] Export via curl/wget command line. P00000251 exported and matched PostgreSQL through curl.exe.

Additional character check passed for P00000008 exported 693 bytes. The complete original field value matched PostgreSQL. Case 26 also saved a name containing angle brackets, ampersand and quotation marks, then verified JSON preservation and escaped narrative output.

---

## FHIR Compliance Checklist

**Required Elements:**
- [X] resourceType = "Patient"
- [X] id present
- [X] At least one identifier (UUID or MRN)
- [X] Name present

**Recommended Elements:**
- [X] Gender
- [X] Birth date
- [X] Active status
- [X] Contact information (telecom)

**Optional but Included:**
- [X] Address

---

## Test Results Summary

**Total Test Cases:** 30 main cases  
**Passed:** 28\
**Failed:** 2\
**Not Tested:** 0

**Pass Rate:** 93.3%

Case 12A and 12B contribute one main result. All 30 main cases have recorded outcomes. Optional high-load testing was unverified.

**Saved response samples**

| Coverage | Patient MRN | Actual response file | Bytes |
|---|---|---|---|
| complete | P00000251 | [complete-patient-126962ca-610d-4264-8118-e26cf4e61eaf.fhir.json](uat-005-samples/complete-patient-126962ca-610d-4264-8118-e26cf4e61eaf.fhir.json) | 1058 |
| unicode-year-only | P00000005 | [unicode-year-only-patient-6d3b3692-3129-44cf-9def-9d2fd4debf37.fhir.json](uat-005-samples/unicode-year-only-patient-6d3b3692-3129-44cf-9def-9d2fd4debf37.fhir.json) | 685 |
| partial-year-month | P00000013 | [partial-year-month-patient-0b99d2d1-713e-462b-b7aa-deb95531b094.fhir.json](uat-005-samples/partial-year-month-patient-0b99d2d1-713e-462b-b7aa-deb95531b094.fhir.json) | 697 |
| inactive | P00000250 | [inactive-patient-41a8e44b-da7b-40d6-bbef-96345891e14b.fhir.json](uat-005-samples/inactive-patient-41a8e44b-da7b-40d6-bbef-96345891e14b.fhir.json) | 1066 |
| minimal | P00000004 | [minimal-patient-3f173b5e-e52f-4bc5-a3c4-fbacb303be0e.fhir.json](uat-005-samples/minimal-patient-3f173b5e-e52f-4bc5-a3c4-fbacb303be0e.fhir.json) | 636 |

Samples preserve the exact bytes returned at their export time. The complete sample records P00000251 before the later edit/status checks, so its version metadata precedes the fixture’s current version.

---

## Critical Checks

**Valid JSON?** [X] Yes / [ ] No\
**Valid FHIR R4?** [X] Yes / [ ] No (base R4 sample validation, Case 22)\
**All Data Present?** [X] Yes / [ ] No (all stored demographic fields matched; Cases 3 and 4 record the additional structures requested by the script)\
**Unicode Preserved?** [X] Yes / [ ] No

---

## Issues Found

| Issue # | Test Case | Description | Severity |
|---------|-----------|-------------|----------|
| UAT-005-01 | 3 | The script requires UUID and MRN identifier entries plus MR type coding. The live export has resource.id=patient UUID and one clinic-namespaced MRN identifier. Values match the database and the documented export contract. | Medium - acceptance criteria discrepancy |
| UAT-005-02 | 4 | The script requires family=Smith and given=[John, Michael]. The live export contains name.text only, matching the stored full-name model and documented mapping. | Medium - acceptance criteria discrepancy |

---

## Recommendations

**FHIR Compliance:**

The five live sample exports passed base R4 4.0.1 validation. Agree the receiving system’s profile and terminology requirements before integration. Production sign-off remains pending the written acceptance criteria and the practice edition’s planned staff authentication, permissions and HTTPS deployment.

**Data Mapping:**

Resolve Cases 3 and 4 against the agreed export contract. A change to the required identifier structure or structured names needs an explicit data and mapping decision. Preserve accurate full names, existing UUID/MRN values and date precision when revising either the implementation or the test script.

---

## Sign-Off

**Tester Name:** Codex (automated HTTP UAT)\
**Date Completed:** 2026-11-18\
**Status:** [ ] Ready for Production / [X] Needs Fixes

---

**Document Status:** UAT-005 executed and results recorded\
**Next Step:** Resolve the recorded acceptance differences and obtain production sign-off
