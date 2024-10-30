---
tags:
  - "#research/literature"
liTags: "{{allTags}}"
puplished: 
created: '{{importDate|format("YYYY-MM-DD")}}'
up:
---
## Summary
- *Summarize* the paper without rereading it and explain it *in your own words*.
- If you can't summarize it in your own words, read it again.
- This summary will make you not read again what you have already read.

> [!Cite]
> {{bibliography}}

>[!Synth]
>**Related**:: {% for relation in relations | selectattr("citekey") %} [[@{{relation.citekey}}]]{% if not loop.last %}, {% endif%} {% endfor %}

>[!md]
{% for type, creators in creators | groupby("creatorType") -%}
{%- for creator in creators -%}
> **{{"First" if loop.first}}{{type | capitalize}}**::
{%- if creator.name %} {{creator.name}}  
{%- else %} {{creator.lastName}}, {{creator.firstName}}  
{% endif %}{% endfor %}{% endfor %}> **Title**:: {{title}}  
> **Year**:: {{date | format("YYYY")}}   
> **Citekey**:: {{citekey}} {%- if itemType %}  
> **itemType**:: {{itemType}}{%- endif %}{%- if itemType == "journalArticle" %}  
> **Journal**:: *{{publicationTitle}}* {%- endif %}{%- if volume %}  
> **Volume**:: {{volume}} {%- endif %}{%- if issue %}  
> **Issue**:: {{issue}} {%- endif %}{%- if itemType == "bookSection" %}  
> **Book**:: {{publicationTitle}} {%- endif %}{%- if publisher %}  
> **Publisher**:: {{publisher}} {%- endif %}{%- if place %}  
> **Location**:: {{place}} {%- endif %}{%- if pages %}   
> **Pages**:: {{pages}} {%- endif %}{%- if DOI %}  
> **DOI**:: {{DOI}} {%- endif %}{%- if ISBN %}  
> **ISBN**:: {{ISBN}} {%- endif %}    

> [!LINK] 
> {%- for attachment in attachments | filterby("path", "endswith", ".pdf") %}
>  [{{attachment.title}}](file://{{attachment.path | replace(" ", "%20")}})  {%- endfor -%}.

{%- set colorCategories = {'Yellow': [], 'Red': [], 'Green': [], 'Blue': [], 'Purple': [], 'Magenta': [], 'Orange': [], 'Gray':[]} -%}
{%- set colorDescriptions = {'Yellow': ['Abstract'], 'Red': ['Introduction'], 'Green': ['Methods'], 'Blue': ['Applications'], 'Purple': ['Conclusions'], 'Magenta':['Questions '], 'Orange':['Orange'], 'Gray':['Gray']} -%}
{%- set imagesProp =[] -%} {%- set memo = [] -%}
{%- for annotation in annotations -%}
	{%- if annotation.annotatedText -%}
		{%- if annotation.color == "#a28ae5"-%}
			{%- set category = "Purple" -%}
		{%- else -%}
			{%- set category = annotation.colorCategory -%}
		{%- endif-%}
		{%- set text = annotation.annotatedText -%}
		{%- set page = annotation.page -%}
		{%- set key = annotation.attachment.itemKey -%}
		{%- set id = annotation.id -%}
		{%- set textComments = annotation.comment -%}
		{%- set _ = colorCategories[category].push([text, page, key, id, textComments]) -%}
	{%- elif annotation.imageRelativePath -%}
		{%- set imagePath = annotation.imageRelativePath -%}
		{%- set imageComments = annotation.comment -%}
		{%- set _ =imagesProp.push([imagePath, imageComments]) -%}
	{%- elif annotation.comment.length >0 -%}
		{%- set otherMemo = annotation.comment -%}
		{%- set _ = memo.push([otherMemo]) -%}
	{%- endif -%}
{%- endfor %}
## Annotations
{% for category, properties in colorCategories -%}
	{%- if properties.length > 0 -%}
>[!annotation-{{category}}] {{colorDescriptions[category]}} {% for property in properties %} 
>- {{property[0]}} ([{{property[1]}}](zotero://open-pdf/library/items/{{property[2]}}?page={{property[1]}}&annotation={{property[3]}})){% if property[4].length >0 %}
> **comments:**  
>>  {{property[4]}}{% endif %}
>>  {% endfor %}{% endif %}

{% endfor %}
> [!Image] {% for imag in imagesProp %}
>![[{{imag[0]}}]] {% if imag[1].length > 0 %}
> **Comments**:
>>*{{imag[1]}}* {% endif %}
>
>{%- endfor %}

>[!note] Memo
{% for mem in memo %}
> **Comments**:
>>*{{mem}}* {% endfor %}

****