# CMS Migration

1. Markdown conversions

   - Rename from mdtext to md
   - Fenced code blocks differ
   - Hyperlinks to openwebbeans should be internal
   - Fixed a bad elementid
   - Updated your relase-checklist.md

   See [changes.txt](changes.txt)

2. Theme Template

   CMS templates were converted into `base.html`

3. Configuration

   See [pelicanconf.py](../pelicanconf.py)

4. Pelican ASF plugin configuration

   [asfgenid.py](../theme/plugins/asfgenid.py)

   - 'unsafe_tags': True  # allow style, script, and iframe tags
   - 'metadata': False    # no metadata replacement in markdown files
   - 'elements': True     # CMS used mdx_elementid features
   - 'headings': True     # Fix up headings w/ permalinks
   - 'headings_re': r'^h[1-4]'
   - 'permalinks': True,
   - 'toc': False         # does not use [TOC]
   - 'toc_headers': r"h[1-4]",
   - 'tables': True       # Fix up for markdown table class
