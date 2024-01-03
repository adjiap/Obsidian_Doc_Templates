---
year: '{{ date | format("YYYY") }}'
author: "{{ authors }}{{ directors }}"
tags:
  - zoteroreference
  {% for tag in tags -%}
  - {{ tag.tag }}
  {% endfor %}
imported on: {{ importDate | format ("YYYY-MM-DD HH:mm:ss")}}
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
{%- if not annotation.imageRelativePath and annotation.comment and annotation.annotatedText -%}
### {{ annotation.comment }}
{% endif %}
{%- if annotation.annotatedText and annotation.comment %}
> [!quote] {{ annotation.annotatedText }} [Page {{ annotation.page }}](zotero://open-pdf/library/items/{{ annotation.attachment.itemKey}}?page={{ annotation.page }}&annotation={{ annotation.id }})

Tags:
{% if	annotation.color -%}
- #{{ annotation.type | lower }}_{{ annotation.colorCategory | lower }}
{% else -%}
- #{{ annotation.type | lower }}
{% endif %}
{%- for tags in annotation.tags -%}
- #{{ tags.tag }}
{% endfor %}
{%- endif -%}
{% endfor -%}

## Direct Quotes without Comments

{% for annotation in annotations -%}
{%- if not annotation.imageRelativePath and not annotation.comment and annotation.annotatedText -%}
> [!quote] {{ annotation.annotatedText }} [Page {{ annotation.page }}](zotero://open-pdf/library/items/{{ annotation.attachment.itemKey}}?page={{ annotation.page }}&annotation={{ annotation.id }})

Tags:
{% if	annotation.color -%}
- #{{ annotation.type | lower }}_{{ annotation.colorCategory | lower }}
{% else -%}
- #{{ annotation.type | lower }}
{% endif %}
{%- for tags in annotation.tags -%}
- #{{ tags.tag }}
{% endfor %}
---
{% endif -%}
{% endfor -%}

## My Direct Notes
{% for annotation in annotations -%}
{%- if not annotation.imageRelativePath and not annotation.annotatedText and annotation.comment -%}
> [!paraphrase] {{ annotation.comment }} [Page {{ annotation.page }}](zotero://open-pdf/library/items/{{ annotation.attachment.itemKey}}?page={{ annotation.page }}&annotation={{ annotation.id }})

Tags:
{% if	annotation.color -%}
- #{{ annotation.type | lower }}_{{ annotation.colorCategory | lower }}
{% else -%}
- #{{ annotation.type | lower }}
{% endif %}
{%- for tags in annotation.tags -%}
- #{{ tags.tag }}
{% endfor %}
---
{% endif -%}
{% endfor -%}

## Tables & Images

{% for annotation in annotations -%}
{%- if annotation.imageRelativePath and annotation.comment -%}
### {{ annotation.comment }}
![[{{annotation.imageRelativePath}}]]

{% endif -%}
{% endfor -%}
