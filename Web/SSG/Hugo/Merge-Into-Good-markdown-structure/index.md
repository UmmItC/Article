---
author: "UmmIt"
title: "Structuring and Preparing Your Blog for Hugo Themes"
tags: ["SSG", "Hugo"]
date: 2023-12-26T08:44:20+0800
thumbnail: https://d33wubrfki0l68.cloudfront.net/c38c7334cc3f23585738e40334284fddcaf03d5e/2e17c/images/hugo-logo-wide.svg
---

## Introduction

When transitioning to a new Hugo theme, it's essential to have a well-organized blog structure. This guide will help you merge all your files into a format suitable for the new Hugo theme.

## Step 1: Renaming Markdown Files

Start by renaming all your Markdown files to `index.md` for a cleaner structure. Execute the following command in your blog's root directory:

```bash
find . -type f -name "*.md" -exec bash -c 'mv "$0" "${0%/*}/index.md"' {} \;
```

This simplifies your file structure and aligns it with Hugo's expectations.

## Step 2: Managing Thumbnails

For thumbnails, ensure they are named as `featured.*` for compatibility with your Hugo theme. If your `index.md` files include URLs for thumbnails, you can use the following script to download and rename them:

```bash
#!/bin/bash

find . -type f -name "index.md" -exec awk 'NR<=10 && /thumbnail: .+/{print $2, FILENAME}' {} \; | while read -r url filepath; do
    if [[ $url == https* ]]; then
        url=$(echo "$url" | tr -d '[:space:]')
        filename=$(basename "$url")
        dirpath=$(dirname "$filepath")

        cd "$dirpath" || exit
        wget "$url" -O "./featured.${filename##*.}"

        if [ $? -eq 0 ]; then
            echo "Downloaded and renamed to featured.${filename##*.}"
        else
            echo "Failed to download $filename"
        fi

        cd - || exit
    else
        echo "Skipping non-HTTPS URL: $url"
    fi
done
```

>Note: Place this script in your blog's root directory and run it to download and rename the thumbnails.

And Please check out the file, is it what you expected? Since this script might be overwrite your current file.

## Step 3: Verify and Adjust

Review your blog's content and ensure all links, images, and references are intact. Adjust any broken links or missing images.

## Step 4: Test with the New Theme

Finally, test your blog with the new Hugo theme. Use Hugo's built-in server for local testing:

```bash
hugo server --watch --loglevel debug
```

Visit `http://localhost:1313` in your browser to see how your blog looks with the new theme.


## Source code

The source code for these can also be found in my repositories! Please check out: https://codeberg.org/UmmIt/Markhugo
