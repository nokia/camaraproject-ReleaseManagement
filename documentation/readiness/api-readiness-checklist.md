# API Readiness Checklist

## Purpose

The API readiness checklist defines the set of assets that need to be provided with an API version to be release-ready.

## The API readiness checklist

The following table presents the API readiness checklist for each API version type: alpha, release-candidate or public.
For public API versions, in addition, the required assets depend on its target status: initial or stable.
In the table, 

- "M" indicates a mandatory release asset
- "O" indicates an optional release asset which may be provided with the release, if available.  

Release assets required to release an API version type:

| Nr | API release asset         | alpha | release-candidate |   initial public   | stable public |
|---|----------------------------|:-----:|:-----------------:|:------------------:|:------:|
| 1 | Release Plan               |   M   |         M         |          M         |    M   |
| 2 | API Definition(s)          |   M   |         M         |          M         |    M   |
| 3 | API documentation          |   M   |         M         |          M         |    M   |
| 4 | User Stories               |   O   |         O         |          O         |    M   |
| 5 | Test Cases (basic)         |   O   |         M         |          M         |    M   |
| 6 | Test Cases (enhanced)      |   O   |         O         |          O         |    M   |
| 7 | API Description            |   O   |         O         |          M         |    M   |

## API release assets

The following list explains the release assets to be provided in more detail, and indicates their expected location.
,
| Release asset | Description                                  | Location         |
|---------------|----------------------------------------------|------------------|
| Release Plan | The `release-plan.yaml` file must be updated to reflect the targeted release | /release-plan.yaml |
| API Definition(s) | There must be one API definition file {api-name}.yaml per API in the repository. The API must follow the applicable CAMARA Commonalities and ICM Guidelines. | /code/API_definitions/{api-name}.yaml |
| API Documentation | The API documentation must be provided inside the API definition file. Additional documentation may be provided as well. |  /code/API_definitions/{api-name}.yaml or /documentation/API_documentation/*.md (optional) |
| User Stories | There must be at least one user story file in the repository. One user story may cover a single or multiple APIs. Each API needs to be covered by at least one user story. | /documentation/API_documentation/{api-name}_User_Story.md |
| Test Cases (basic) | Basic API test cases must be provided covering sunny day scenarios and main error cases, along with documentation. There must be at least one feature file per API, or optionally, one for each operationId of an API. | /code/Test_definitions/{api-name}.feature or /code/Test_definitions/{api-name}-{operationId}.feature |
| Test Cases (enhanced) | Enhanced API test cases must be provided covering rainy day scenarios, along with documentation. | /code/Test_definitions |
| API Description | A link to the API description on the CAMARA Wiki must be provided for each API for marketing purposes. | /README.md |

## Readiness review

The automated release process will check the presence of the files indicated as mandatory for the API version in the readiness checklist.

The content of these files needs to be checked by release reviewers.

## See also

- [../README.md](../README.md) for documentation index
- [../deprecated/API-Readiness-Checklist.md](../deprecated/API-Readiness-Checklist.md) for the legacy template
