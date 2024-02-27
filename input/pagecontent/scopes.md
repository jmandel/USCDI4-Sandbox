
This page is new content for US Core Version 7.0.0
{:.new-content}

## SMART on FHIR Server Capabilities

intro text  ... Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed nulla dui, dignissim sed ultrices vel, ultricies non mauris. Vestibulum sit amet lorem interdum, consequat urna eu, rutrum sapien. Morbi scelerisque, purus non volutpat maximus, orci massa congue nisl, nec lobortis nunc eros et leo.
### Servers SHALL support the following SMART on FHIR capabilities*:
html
intro text  ... Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed nulla dui, dignissim sed ultrices vel, ultricies non mauris. Vestibulum sit amet lorem interdum, consequat urna eu, rutrum sapien. Morbi scelerisque, purus non volutpat maximus, orci massa congue nisl, nec lobortis nunc eros et leo.

1. `launch-standalone`
2. `context-standalone-patient`
3. `permission-patient`
4. ...
   
\*these encompass all the capability sets listed in SMART

### Servers SHALL support Token introspection

intro text  ... Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed nulla dui, dignissim sed ultrices vel, ultricies non mauris. Vestibulum sit amet lorem interdum, consequat urna eu, rutrum sapien. Morbi scelerisque, purus non volutpat maximus, orci massa congue nisl, nec lobortis nunc eros et leo.

### SMART Scopes


The US Core API requires servers to support both patient-specific [resource level scopes] and [granular scopes] as defined in Version 2.0.0 of [SMART App Launch]. **US Core's required scopes (SHALL) are based on community-based consensus that the scope meets a system requirement, clinical need, or federal regulation. Similarly, additions to US Core's recommended scopes (SHOULD) rely on community-based consensus that the scope meets a system requirement or clinical need as a best practice.**
The US Core required scopes listed below are named in the HTA-1 final rule, which requires support for Condition and Observation category scopes, and the recommends granular scope listed below is of particular interest to patients and health systems. 
 
**Although these scopes are limited to patient-specific scopes. servers MAY support other patient-specific, user-level, and system-level scopes.** US Core clients should follow the [principle of least privilege] and access only the necessary resources. In other words, if a client needs only vital sign observations, it should request access only to Observations with a category of "vital-signs".

#### Scopes Format
Version 2.0.0 of [SMART App Launch] introduced a scope syntax of: `<patient|user|system> / <fhir-resource>. <c | r | u | d |s> [?param=value]`

For example, to limit read and search access to a specific patient's laboratory observations but not other observations, the server grants the following patient-specific scope:

`patient/Observation.rs?category=http://terminology.hl7.org/CodeSystem/observation-category|laboratory`.

#### US Core Scopes

The table below summarizes the US Core scope requirements (**SHALL**) and best practice recommendations (**SHOULD**) for both resource-level and granular scopes. The same information can be found in each US Core Profile page's "Quick Start" section.

##### The Following Resource Level Scopes **SHALL** Be Supported

{% include resource-scopes-table.md conformance="SHALL" crud='rs'%}

##### The Following Granular Scopes **SHALL** Be Supported

{% include granular-scopes-table.md conformance="SHALL" crud='rs' %}

<table class="grid">
<thead>
<tr>
<th>Resource Type</th>
<th>Granular Scope</th>
</tr>
</thead>
<tbody>
{% for scope in scopes -%}
<tr>
<td>{{scope | split: '.' | first }}</td>
<td><code>{{ scope | prepend: 'patient/' }}</code></td>
</tr>
{% endfor %}
</tbody>
</table>

##### The Following Granular Scopes **SHOULD** Be Supported

{% include granular-scopes-table.md conformance="SHOULD" crud ='rs' %}

<table class="grid">
<thead>
<tr>
<th>Resource Type</th>
<th>Granular Scope</th>
</tr>
</thead>
<tbody>
{% for scope in scopes -%}
<tr>
<td>{{scope | split: '.' | first }}</td>
<td><code>{{ scope | prepend: 'patient/' }}</code></td>
</tr>
{% endfor %}
</tbody>
</table>

### Servers SHALL support the following metadata in their `/.well-known/smart-configuration`

In addition to the capabilities defined in the server's CapabilityStatement,
servers **SHALL** document their SMART capabilities in the `.well-known/smart-configuration`.

#### Required Metadata Inherited from SMART:

The following metadata is inherited from SMART.

- `issuer` (conditional)
- `jwks_uri` (conditional)
- `authorization_endpoint`
- `grant_types_supported`
- `token_endpoint`
- `capabilities`
- `code_challenge_methods_supported`

#### Additional US Core requirements

- `scopes_supported` : Servers **SHALL** document supported scopes as an array in in the file
    - The server **SHALL** support all scopes listed in the table above for the US Core Profiles they support; additional scopes **MAY** be supported (so clients should not consider this an exhaustive list). 

    - Servers **MAY** limit clients' scopes to those configured at registration time. Servers **SHALL** allow users to select a subset of the requested scopes at the approval time. The app **SHOULD** inspect the returned scopes and accommodate the differences from the scopes it requested and registered.
- `introspection_endpoint`: Servers **SHALL** document this endpoint in the file

#### Example `.well-known/smart-configuration` File

This example `.well-known/smart-configuration` file contains all the US Core scopes listed in the capabilities array.  See the [SMART App Launch] Implementation Guide for more example and details.


~~~http
HTTP/1.1 200 OK
Content-Type: application/json
~~~

{% include granular-scopes-table.md %}

~~~json
{
  "issuer": "https://ehr.example.com",
  "jwks_uri": "https://ehr.example.com/.well-known/jwks.json",
  "authorization_endpoint": "https://ehr.example.com/auth/authorize",
  "token_endpoint": "https://ehr.example.com/auth/token",
  "token_endpoint_auth_methods_supported": [
    "client_secret_basic",
    "private_key_jwt"
  ],
  "grant_types_supported": [
    "authorization_code",
    "client_credentials"
  ],
  "registration_endpoint": "https://ehr.example.com/auth/register",
  "scopes_supported": [
    "openid",
    "profile",
    "launch",
    "launch/patient",
    "offline_access",
{% for scope in scopes -%}
    {{ scope | prepend: '    "patient/'}}"{% unless forloop.last %},{% endunless %}
{% endfor %}
  ],
  "response_types_supported": ["code"],
  "management_endpoint": "https://ehr.example.com/user/manage",
  "introspection_endpoint": "https://ehr.example.com/user/introspect",
  "revocation_endpoint": "https://ehr.example.com/user/revoke",
  "code_challenge_methods_supported": ["S256"],
  "capabilities": [
    "launch-ehr",
    "permission-patient",
    "permission-v2",
    "client-public",
    "client-confidential-symmetric",
    "context-ehr-patient",
    "sso-openid-connect"
  ]
}
~~~
 

{% include link-list.md %}




