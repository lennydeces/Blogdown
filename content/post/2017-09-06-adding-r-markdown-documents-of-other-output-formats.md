---
title: Adding R Markdown documents of other output formats
date: '2017-09-06'
slug: adding-r-markdown-documents-of-other-output-formats
raw: "https://raw.githubusercontent.com/rbind/blogdown-demo/master/content/post/2017-09-06-adding-r-markdown-documents-of-other-output-formats.md"
---

This post is a minimal example to show how you can render arbitrary Rmd documents on a [**blogdown**](https://github.com/rstudio/blogdown) website. 

Here are some possible use cases:

* You have an existing Rmd file where the knitted `.html` is the version you would like to share on your website, with options (typically specified in your YAML frontmatter) not available with a **blogdown** post such as:
    * a [Bootstrap theme](http://rmarkdown.rstudio.com/html_document_format.html#appearance_and_style), 
    * [tabbed sections](http://rmarkdown.rstudio.com/html_document_format.html#tabbed_sections),
    * a [floating table of contents](http://rmarkdown.rstudio.com/html_document_format.html#floating_toc), or
    * [code folding](http://rmarkdown.rstudio.com/html_document_format.html#code_folding).
    
    
* You have an existing Rmd file where the knitted [PDF](http://rmarkdown.rstudio.com/pdf_document_format.html) is the version you would like to share on your website. For example, a resume or CV, a course syllabus, etc.


* You have an existing Rmd file where the knitted slide presentation is the version you want to share. See [Carl's issue here](https://github.com/rstudio/blogdown/issues/97).

In all of the above use cases, the underlying "raw" Rmd documents will *not* render correctly using the `blogdown::html_page()` output format (see section [1.5 in the **blogdown** book](https://bookdown.org/yihui/blogdown/output-format.html#output-format)). However, you *can* render Rmd files correctly in an output format other than `blogdown::html_page()` using **blogdown**. See the brief documentation in the [**blogdown** book](https://bookdown.org/yihui/blogdown/static-files.html). The key is that you add an R script `build.R` under the `R/` directory (in which you can use one line of code `blogdown::build_dir('static')`), and put your Rmd files under the `static/` directory. Here are the four steps:

1. Go to your blogdown project's root directory and create a new folder called `R`
2. In that `R/` directory, create a new R script called `build.R` that contains 1 line of code that reads: `blogdown::build_dir('static')`
3. Add and save Rmd file(s) to your blogdown project in the `static/` directory. 
    * In fact, you can add Rmd files within sub-directories such as `static/slides/`, `static/pdf/`, and/or `static/html/`
4. Serve your site 

It does not matter which output formats your Rmd files are generated to; `build_dir()` will call `rmarkdown::render()` to render them in the output format in the YAML header. So, Rmd files present in your `static/` directory do not need to be `knit` to html in order to be rendered on your **blogdown** site. Any time you make changes to an Rmd file, the output file will be rendered again, overwriting the previous output file each time.

Please see the Github repo [yihui/blogdown-static](https://github.com/yihui/blogdown-static) for a concrete example.
