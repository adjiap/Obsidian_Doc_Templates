---
Year: '{{ date | format("YYYY") }}'
Author: "{{ authors }}{{ directors }}"
tags:
  - zoteroreference
---
```toc
```
{# FYI, the shape looks weird for the template below, but that's actually intentional, because the result would be in the correct template #}

Title: {{title}}
URL: {{url}}
Zotero Link: {{pdfZoteroLink}}

---
## Direct Quotes with Comments
{% for annotation in annotations -%}
{% if not annotation.imageRelativePath and annotation.comment and annotation.annotatedText %}
### {{ annotation.comment }}
{% endif %}
{%- if annotation.annotatedText %}
{{ annotation.annotatedText }} [Page {{ annotation.page }}](zotero://open-pdf/library/items/{{ annotation.attachment.itemKey}}?page={{ annotation.page }}&annotation={{ annotation.id }})
{% if	annotation.color %}{{ annotation.type | capitalize }} (**{{ annotation.colorCategory }}**)
{%- else %}{{ annotation.type | capitalize }}
{% endif %} 
{%- endif %}
{% endfor -%}

## Direct Quotes without Comments
{% for annotation in annotations -%}
{% if not annotation.imageRelativePath and not annotation.comment and annotation.annotatedText  %}
{{ annotation.annotatedText }} [Page {{ annotation.page }}](zotero://open-pdf/library/items/{{ annotation.attachment.itemKey}}?page={{ annotation.page }}&annotation={{ annotation.id }})
{% if	annotation.color %}{{ annotation.type | capitalize }} (**{{ annotation.colorCategory }}**)
{%- else %}{{ annotation.type | capitalize }}
{% endif %} 
{%- endif %}
{% endfor -%}

## Tables & Images
{% for annotation in annotations -%}{%- if annotation.imageRelativePath and annotation.comment -%}### {{ annotation.comment }}
![[{{annotation.imageRelativePath}}]]
{%- endif %}
{% endfor -%}
