{% capture markdown_content %}

## testing markdown

{% endcapture %}
{{ markdown_content | markdownify }}
