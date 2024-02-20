<!-- This liquid script creates a US Core scope requirements table using input data from input/data/scopes.csv 
include parameters: conformance='SHALL'|'SHOULD' see below for how used, and crud='cruds' not currently used -->

{% assign rows = site.data.scopes -%}
{%- assign scopes = '' -%}
{%- for item in rows -%}
{%- unless forloop.first -%}
{%- unless item.add_scope == "FALSE" -%}
{% for i in (1..6) %}
{%- assign category =  'category_' | append: i -%}
{%- assign category_conformance =  'category_' | append: i |append: '_conformance' -%}
{%- if item[category] and item[category_conformance] == include.conformance -%}
{%- assign resource_type = item.resource_type | strip | replace: ',' | append: '?category=' -%}
{%- assign category = item[category] | strip | append: ',' -%}
{%- assign scope =  resource_type | append: category -%}
{%- assign scopes =  scopes | append: scope -%}
{%- endif -%}
{%- endfor -%}
{%- endunless -%}
{%- endunless -%}
{%- endfor -%}
{%- assign scopes = scopes | split: "," | uniq | sort  %}

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
<td>{{scope | split: "?" | first }}</td>
<td><code>{{ scope }}</code></td>
</tr>
{% endfor %}
</tbody>
</table>