---
title: "Just a Test" # Title of the blog post.
date: 2023-02-15T11:07:42+08:00 # Date of post creation.
description: "This is a test article." # Description used for search engine.
featured: true # Sets if post is a featured post, making appear on the home page side bar.
# draft: true # Sets whether to render this page. Draft of true will not be rendered.
# toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
usePageBundles: true # Set to true to group assets like images in the same folder as this post.
featureImage: "images/screenshot.png" # Sets featured image on blog post.
# featureImageAlt: 'Description of image' # Alternative text for featured image.
# featureImageCap: 'This is the featured image.' # Caption (optional).
thumbnail: "images/screenshot.png" # Sets thumbnail image appearing inside card on homepage.
# shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
# figurePositionShow: true # Override global value for showing the figure label.
categories:
  - 技术
tags:
  - hugo
# comment: false # Disable comment if false.
---

# h1
## h2
### h3

```golang
package main

import (
    "fmt"
    "log"
    "github.com/PuerkitoBio/goquery"
)

func main() {
    // Make an HTTP GET request to the website
    response, err := http.Get("https://example.com")
    if err != nil {
        log.Fatal(err)
    }
    defer response.Body.Close()

    // Parse the HTML response using goquery
    doc, err := goquery.NewDocumentFromReader(response.Body)
    if err != nil {
        log.Fatal(err)
    }

    // Find and print all the links on the page
    doc.Find("a").Each(func(i int, s *goquery.Selection) {
        link, exists := s.Attr("href")
        if exists {
            fmt.Println(link)
        }
    })
}
```