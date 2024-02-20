<!-- This liquid script creates a US Core scope requirements table using input data from input/data/scopes.csv
include parameters: conformance='SHALL'|'SHOULD' and crud='cruds' not currently used -->

{% assign rows = site.data.scopes -%}
{%- assign scopes = '' -%}
{%- for item in rows -%}
{%- unless forloop.first -%}
{%- unless item.add_scope == "FALSE" -%}
{%- assign scope = item.resource_type | strip | append: ',' -%}
{%- assign scopes =  scopes | append: scope -%}
{%- endunless -%}
{%- endunless -%}
{%- endfor -%}
{%- assign scopes = scopes | split: "," | uniq | sort %}

<table class="grid">
<thead>
<tr>
<th>Resource Type</th>
<th>Resource Level Scope</th>
</tr>
</thead>
<tbody>
{% for scope in scopes -%}
<tr>
<td>{{scope}}</td>
<td><code>{{ scope | prepend: 'patient/' | append: '.rs' }}</code></td>
</tr>
{% endfor %}
</tbody>
</table>