# Import OAS API

Add the `db_id` field if importing to dashboard. Else remove it.

Deployment samples can be seen from the `import_oas_api` folder:
- `/api_and_pol_definition`
- `/import_oas_api`


## Schema

* type: `apidef` or `oas`. `oas` can only import, convert or publish oas spec into tyk spec. It doesn't allow syncing.
* files: array of an object api definition information. Contains the following:
  - file: input the path to the spec. Can be relative.
  - api_id: input a custom `api_id` or allow tyk to generate one when omitted.
  - db_id: input a custom `db_id` or allow tyk to generate one. Only used when publichsing or syncing with dashboard with db accesss. 
  - org_id: input a custom `org_id`. The `org_id` based on the dashboard API credentials will be used when dashboard is present.
  - oas: setup custom proxy settings for the imported v2 or v3 oas spec. Omit or leave empty `{}` when type is `apidef`
    + ovveride_listen_path: input a custom `listen_path`. Omit to keep value from OAS spec
    + override_target: input a custom `target_url` or upstream path. Omit to keep value from OAS spec
    + strip_listen_path: `true` or `false`. Decide if the listen path should be striped from the final call upstream
    + version_name: input a custom `version_name`. Omit to disable versioning on Tyk API spec 
* policies: 
  - file: input the path to the policy definition. Can be relative.
  - id: input a policy definition id. Ensure the `allow_explicit_policy_id` field in your dashboard or gateway is set to **true**. Check enable slugs on dashboard is set to **true**.

```json
{
  "type": "type_of_spec",
  "files": [
    {
      "file": "path_to_apidef.json",
      "api_id": "custom_api_definition_id",
      "db_id": "database_base_id(which is usually the `id` field in the api definition)",
      "org_id": "organisation_id",
      "oas": {
        "override_target": "upstream or target url",
        "ovveride_listen_path": "",
        "version_name": "Default or v1",
        "strip_listen_path": true
      }
    },
    {
      "file": "path_to_apidef.json",
      "org_id": "organisation_id",
      "oas": {
        "override_target": "http://target_or_upstream.url",
        "override_listen_path": "/new_listen_path/",
        "version_name": "version_name"
      }
    }
  ],
  "policies": [
    {
      "file": "path_to_policy.json",
      "id": "policy_id"
    }
  ]
}
```