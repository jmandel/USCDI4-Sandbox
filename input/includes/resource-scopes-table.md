<!-- This liquid script creates a US Core scope requirements table using input data from input/data/scopes.csv
include parameters: conformance='SHALL'|'SHOULD' and crud='cruds' not currently used -->

{% assign rows = site.data.scopes -%}
{%- assign resource_scopes = '' -%}
{%- for item in rows -%}
{%- unless item.add_scope == "FALSE" -%}
{%- assign scope = item.resource_type | strip | append: ',' -%}
{%- assign resource_scopes =  resource_scopes | append: scope -%}
{%- endunless -%}
{%- endfor -%}
{%- assign resource_scopes = resource_scopes | split: "," | uniq | sort %}