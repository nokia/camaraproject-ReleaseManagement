# API Readiness Checklist

## Purpose

The API readiness checklist defines the set of assets that eed to be delivered with an API version to be release-ready.

## The API readiness checklist

The following list describes the release assets to be provided and their expected location.

| Release asset | Description                                  | Location         |
|---------------|----------------------------------------------|------------------|
| Release Plan | The `release-plan.yaml` file must be updated to reflect the targeted release | [relative link](/release-plan.yaml) |
| API Definition(s) | There must be one API definition file {api-name}.yaml per API in the repository. The API must follow the applicable CAMARA Commonalities and ICM Guidelines. | [relative link](/code/API_definitions/{api-name}.yaml) |
| API Documentation | The API documentation must be provided inside the API definition file. Additional documentation may be provided as well. |  [link](/code/API_definitions/{api-name}.yaml) or [link](/documentation/API_documentation/*.md (optional)) |
| User Stories | There must be at least one user story file in the repository. One user story may cover a single or multiple APIs. Each API needs to be covered by at least one user story. | [relative link](/documentation/API_documentation/{api-name}_User_Story.md) |
| Test Cases (basic) | Basic API test cases must be provided covering sunny day scenarios and main error cases, along with documentation. There must be at least one feature file per API, or optionally, one for each operationId of an API. | [relative link](/code/Test_definitions/{api-name}.feature) or [relative link](/code/Test_definitions/{api-name}-{operationId}.feature) |
| Test Cases (enhanced) | Enhanced API test cases must be provided covering rainy day scenarios, along with documentation. | [relative link](/code/Test_definitions) |
| API Description | A link to the API description on the CAMARA Wiki must be provided for each API for marketing purposes. | [relative link](/README.md) |

The following table presents the API readiness checklist for an API version. 

The required assets depend on 

- the API version type: alpha, release-candidate or public
- for public API versions, in addition, depending on its target status: initial or stable

In the table, 

- "M" indicates a mandatory release asset
- "O" indicates an optional release asset which may be provided with the release, if available.  

**Assets per target_release_type**

| Nr | API release asset          | alpha | release-candidate |  initial<br>public | stable<br> public |
|---|----------------------------|:-----:|:-----------------:|:------------------:|:------:|
| 1 | Release Plan               |   M   |         M         |          M         |    M   |
| 2 | API Definition(s)          |   M   |         M         |          M         |    M   |
| 3 | API documentation          |   M   |         M         |          M         |    M   |
| 4 | User Stories               |   O   |         O         |          O         |    M   |
| 5 | Test Cases (basic)         |   O   |         M         |          M         |    M   |
| 6 | Test Cases (enhanced)      |   O   |         O         |          O         |    M   |
| 7 | API Description            |   O   |         O         |          M         |    M   |

## Readiness review

The automated release process will check the presence of the files indicated as mandatory for the API version in the readiness checklist.

The appropriateness of the content of these files need to be checked by release reviewers.

## See also

- [../README.md](../README.md) for documentation index
- [readiness-model.md](readiness-model.md) for the conceptual model
- [../deprecated/API-Readiness-Checklist.md](../deprecated/API-Readiness-Checklist.md) for the legacy template
