# <mark>`diabetes-management`</mark>

| Metadata | Value
| ---- | ----
| specificationVersion | 1.0
| hookVersion | 2.2
| hookMaturity | [0 - Draft](../../specification/1.0/#hook-maturity-model)

## Workflow

<mark>The `diabetes-management` hook is triggered when the practitioner opens a patient record, in the diabetes tab, in the electronic health record system. These hook triggers various diabetes-related CDS services which may or may not be activated at the same time, depending on the current patient record data. Those services that are triggered but not activated, are triggered again after the patient record is filled with the CDS services results from those diabetes-related services that were activated during the first call to the CDS system. Below, we provide the specifications for the CDS service with Id `primary-prevention-review`, a service that aims to be activated during the first CDS system call and that provides support on continuous diabetes prevention therapy to patients (lipid lowering drug therapy), considering characteristics such as type of diabetes, age, risk factors.</mark>

## Context
<mark>The context contains information on the active diabetes mellitus diagnoses of the patient regarding the continuous diabetes prevention therapy.</mark>

Field | Optionality | Prefetch Token | Type | Description
----- | -------- | ---- | ---- | ----
<mark>`patientId`</mark> | REQUIRED | Yes | *string* | <mark>FHIR `patient.id` identifier of current patient.</mark>
<mark>`encounterId`</mark> | REQUIRED | Yes | *string* | <mark>FHIR `encounter.id` identifier of current encounter.</mark>
<mark>`practitionerId`</mark> | OPTIONAL | Yes | *string* | <mark>FHIR `practitioner.id` identifier of current practitioner.</mark> 
<mark>`birthDate`</mark> | REQUIRED | Yes | *string* | <mark> FHIR `patient.birthdate` string representing the date of birth of the patient in the format `yyyy-dd-mm`.</mark>
<mark>`diabetesAssessment`</mark> | REQUIRED | No | *object* | <mark>FHIR `Condition` resource that is currently active, recurrent, or relapse (Condition.clinicalStatus), has been confirmed or is provisional by clinical staff (verificationStatus.coding.code) and that is categorised as problem-list-item or encounter-diagnosis or SNOMED CT Diagnosis (category.coding.code).</mark>
<mark>`riskFactors`</mark> | OPTIONAL | No | *object* | <mark> FHIR `Bundle` resource containing potential diabetes mellitus risk factors (if any) that must be taken into consideration when identifying a suitable treatment for the current patient.</mark>

### Example
```json
{
"hookInstance": "d1577c69-dfbe-44ad-ba6d-3e05e953b2ea",
  "hook": "diabetes-assess",
  "context": {
    "birthDate": "1988-17-03",
    "diabetesAssessment": {
      "resource": {
        "resourceType": "Condition",
        "id": "DM",
        "clinicalStatus": {
          "coding": [
            {
              "system": "http://terminology.hl7.org/CodeSystem/condition-clinical",
              "code": "active"
            }
          ]
        },
        "verificationStatus": {
          "coding": [
            {
              "system": "http://terminology.hl7.org/CodeSystem/condition-ver-status",
              "code": "confirmed"
            }
          ]
        },
        "category": [
          {
            "coding": [
              {
                "system": "http://terminology.hl7.org/CodeSystem/condition-category",
                "code": "encounter-diagnosis",
                "display": "Encounter Diagnosis"
              },
              {
                "system": "http://snomed.info/sct",
                "code": "439401001",
                "display": "Diagnosis"
              }
            ]
          }
        ],
        "code": {
          "coding": [
            {
              "system": "http://snomed.info/sct",
              "code": "46635009",
              "display": "Diabetes mellitus type 1"
            }
          ]
        },
        "subject": {
          "reference": "Patient/1677163"
        }
      }
    },
    "riskFactor": {
      "resourceType": "Bundle",
      "entry": [
        {
          "resource": {
            "resourceType": "Condition",
            "id": "microalb_history",
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "473111003",
                  "display": "History of microalbuminuria"
                }
              ]
            },
            "subject": {
              "reference": "Patient/1677163"
            }
          }
        },
        {
          "resource": {
            "resourceType": "Condition",
            "id": "microalb_DM_complication",
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "18521000119106",
                  "display": "Microalbuminuria due to type 1 diabetes mellitus"
                }
              ]
            },
            "subject": {
              "reference": "Patient/1677163"
            }
          }
        }
      ]
    }
  }
}

```

## Change Log

Version | Description
---- | ----
1.0 | FHIR resource parameters
