---
layout: default
title: "High-Performance Local File XML Sitemap Generator Guide"
date: 2026-11-04
categories: [shell, productivity]
---

# High-Performance Local File XML Sitemap Generator Guide

This guide describes a high-performance Bash script designed to **generate structured, split XML sitemaps directly from millions of local static files** while complying strictly with Google Search Console validation rules.

Standard HTTP web crawlers crash or run out of memory when crawling millions of URLs. This script bypasses the network entirely by scanning your local filesystem, formatting paths into production URLs, automatically escaping illegal characters, and splitting them into Google-compliant chunks.

## 🚀 The Script

Save the following code as `generate_sitemaps.sh`:

```bash
#!/bin/bash

# ==============================================================================
# CONFIGURATION
# ==============================================================================
# Change these variables to match your setup
WEB_ROOT="/var/www/html"
DOMAIN="https://yourwebsite.com"

# ==============================================================================
# STEP 1: DISCOVER FILES AND CHUNK THE PATHS
# ==============================================================================
# 1. Finds all static .htm(l) files (including query strings like .html?id=123).
# 2. Strips the local absolute disk root and swaps it with your production domain.
# 3. Splits the raw list into manageable text files capped at 45,000 URLs each.
find "$WEB_ROOT" -type f -name "*.htm*" | sed "s|$WEB_ROOT|$DOMAIN|" | split -l 45000 - sitemap_chunk_

# Count how many chunks were actually generated
chunk_count=$(ls sitemap_chunk_* 2>/dev/null | wc -l)

if [ "$chunk_count" -eq 0 ]; then
    echo "No HTML files found. Skipping sitemap generation."
    exit 1
fi

# ==============================================================================
# STEP 2: CONVERT CHUNKS INTO GOOGLE-COMPLIANT SUB-SITEMAPS
# ==============================================================================
for file in sitemap_chunk_*; do
    
    # If there is only 1 chunk, name it sitemap.xml. Otherwise, keep the suffix.
    if [ "$chunk_count" -eq 1 ]; then
        target_name="sitemap.xml"
    else
        target_name="sitemap-${suffix}.xml"
    fi

    # Initialize the child sitemap file with the exact XML namespace Google requires
    suffix=${file#sitemap_chunk_}
    echo '<?xml version="1.0" encoding="UTF-8"?><urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">' > "$target_name"
    
    # Process each line and replace raw '&' with '&amp;' to prevent strict XML parser breakage
    # Using -v rep avoids the backslash warnings cleanly while keeping awk happy
    awk -v rep="&amp;" '{gsub(/&/, rep); print "<url><loc>"$0"</loc></url>"}' "$file" >> "$target_name"
    
    # Close the XML block
    echo '</urlset>' >> "$target_name"
    
    rm "$file" # Clean up temporary text chunks
done

# ==============================================================================
# STEP 3: CREATE THE MASTER SITEMAP INDEX REGISTRY
# ==============================================================================
# Google demands a formal index wrapper schema if you manage more than 50,000 links.
# Create the sitemap index ONLY if there are multiple sitemaps
if [ "$chunk_count" -gt 1 ]; then
    echo "Multiple sitemaps created. Generating sitemap-index.xml..."
    
    echo '<?xml version="1.0" encoding="UTF-8"?><sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">' > sitemap-index.xml

    for file in sitemap-*.xml; do
    if [ "$file" != "sitemap-index.xml" ]; then
        echo "<sitemap><loc>$DOMAIN/$file</loc></sitemap>" >> sitemap-index.xml
    fi
    done

    echo '</sitemapindex>' >> sitemap-index.xml
else
    echo "Only one sitemap was needed. Skipping sitemap-index.xml."
    # Clean up an old index file if it exists from a previous run
    rm -f sitemap-index.xml
fi

echo "Sitemaps generated successfully!"
```

## 🛠 How to Use It

### 1. Make the Script Executable
Give the script permission to execute on your machine:
```bash
chmod +x generate_sitemaps.sh
```

### 2. Set Your Target Settings
Open the script and change the configuration variables at the very top:
*   `WEB_ROOT`: The absolute pathway where your files live on disk (e.g., `/var/www/html` or `/home/user/public_html`).
*   `DOMAIN`: Your production domain profile (e.g., `https://example.com`).

### 3. Run It
Run the generation pipeline:
```bash
./generate_sitemaps.sh
```

### 4. Deploy and Verify
1. Move all generated `sitemap-*.xml` and `sitemap-index.xml` files to your production website's root folder.
2. Submit **only** the `sitemap-index.xml` or `sitemap.xml` URL to Google Search Console or Bing Webmaster Tools. The search engines will automatically read the index and crawl all sub-sitemaps linked inside.

## 🧠 Why This Approach Works Best for Millions of Pages

*   **Valid Namespaces:** Uses `xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"` which aligns exactly with official search engine specifications, preventing validation failures.
*   **Zero RAM Overhead:** Instead of loading millions of file entities directly into system memory, this script uses **Unix streams (`find` and `awk`)**. It processes links on-the-fly, so it runs flawlessly on basic cloud servers.
*   **Complies with Google Limitations:** Search engines enforce a strict cap of 50,000 URLs or 50MB per sitemap file. This script caps blocks safely at **45,000 lines**, leaving a comfortable buffer for size limits.
*   **XML Sanitized:** Raw parameters inside URLs containing special query characters like `&` will break XML parsers. The embedded `awk` stream translates those instances into secure `&amp;` strings seamlessly.
