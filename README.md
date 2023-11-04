# MXXX-project-template
This is a standardized repo template for jelinski lab new project repos which follows our lab-specific open science guide procedures [insert link here to guide]. 

## Registering a unique project ID on @jelinski-lab-project-registry
When using this template to set up a new project repo, please first obtain a new project ID by adding your project as a new row to [@jelinski-lab-project-registry](https://github.com/orgs/jelinski-lab-pedology/projects/1). **NOTE: If you are not in my lab group, feel free to use this template repo and name it whatever the heck you want!** Simply add a row with the next number in sequence (i.e. if the last registered project is M059-jelinski-permafrost-table-id, then your project should be M060-xxxxx) followed by your last name (assuming you are the project lead/owner) and then a 2-4 word descriptive title for your project (note - spend at least a bit of time thinking about this as this is what will be used on all other platforms (your personal computer, GitHub, GoogleDrive, etc...) as your project name.

## Cloning the template to your personal GitHub page
Assuming that you are the project lead/owner, the first step is to clone this template to your personal GitHub page and set it as private initially. Any manuscripts/proposals in development (i.e. not published or awarded) should be set as private by default. Once manuscripts are in press or published then you can flip the repo to public.

## Rename the template repo 
Now, rename the template repo using your registered project ID.

## Default repo directory and subdirectory structure
Use the default standardized folder structure unless there is an explicit reason to do something different!

This standardized structure is my own, modified and inspired from many excellent sources [^1][^2][^3][^4][^5][^6]. This is likely a living template and will change/improve as we adapt to diverse projects. Here is an example of the template repo structure for an arbitrary project [M009-jelinski-permafrost-id].

-   ./M009-jelinski-permafrost-table-id
    -   /00-data
        -  /raw
        -  /prepared 
    -   /01-code
        -   \[M009-jelinski-permafrost-table-id.Rproj\] *will initiate in section X*
        -   \[00_example_script\]
    -   /02-pipeline
        -  /00_example_script
            -  /tmp
            -  /store
            -  /out
    -   /03-output
    -   /04-project-cache
    -   /04-project-temp
    -   /05-manuscript
        -   /drafts
        -   /submitted
        -   /final-and-proofs
        -   /figures
        -   /tables
        -   /refs
    -   /06-presentations
    -   /07-log
        -   /analysis-log
        -   /writing-log
    -   /08-archive
    -   /docs
    -   \[readme.txt\]
    -   \[TODOs.txt\]
    -   \[project-metadata.json\]

*Note that what I dodn't use here was -lib or -libs and -tests*

## What Do Each of These Folders Mean and What Should I Put in Them?

Although this list may seem daunting at first, each of these folders has a purpose, and this structure allows for deep integration with transparent data curation and data analysis workflows using R. Some of these folders may remain empty for the duration of your project and that is just fine! Note that each of the folders have a number convention for quick association once you start using this standardized structure. The numbers also allow for the folders to be arranged in a logical order, rather than alphabetically.

### /00-data

This folder contains all of the input data being used in this project. EVERY dataset (whether external or project generated) should have an associated metadata file! For complex projects with many datasources, each dataset should live in its own subfolder.

#### /00-data/raw
The raw data: unmodified, comprehensive, containing outliers, missing values, imperfections and other items that may be removed in data pre-processing. *Insert link to data pre-processing chapter.* It also contains any foundational geospatial data from other authors/sources that you did not generate but that you are pulling in for the purposes of analysis **and that needs further processing to be utilized in your workflow**. For example if an external data source came in a proprietary format that needed to be exported and modified. *Note - need to create a subfolder separate for this, and also need to have a way to track origin and authorship - probably a readme or metadata file .txt or something - can be a running open file that is added to - maybe a markdown or something?*

Any raw data that you generated should be in open and easily readable formats. Specifically:

-   Tabular data should be in .csv format
-   Spatial vector data (points, lines or polygons) should be in [OGC geopackage](https://en.wikipedia.org/wiki/GeoPackage) format.

Any data in raw data that is not your own should be in an /external-sources subdirectory which contains a metadata file that details the source of each of the external data sources.

#### /00-data/prepared

This folder is the sink where all of your post-processed data will live. It also contains any foundational geospatial data from other authors/sources that you did not generate but that you are pulling in for the purposes of analysis following any necessary processing to change formats - note that this can include reprojected data, etc? *actually maybye it shouldn't because all of that can go into an R script - hmm need to think about this*. *Note - need to create a subfolder separate for this, and also need to have a way to track origin and authorship - probably a readme or metadata file .txt or something - can be a running open file that is added to - maybe a markdown or something?* Data should be in open and easily readable formats. Specifically:

-   Tabular data should be in .csv format
-   Spatial vector data (points, lines or polygons) should be in [OGC geopackage](https://en.wikipedia.org/wiki/GeoPackage) format.
-   Spatial raster data should be in [GeoTIFF](https://en.wikipedia.org/wiki/GeoTIFF) format.

### /01-code

This folder contains all of your source code and R scripts that you use to conduct your data processing and analysis. *Note link to how to actually write and breakdown and organize scripts and analysis in projects - this can be in your R style guide or as a separate section - talk about sequential numbering, a master script, functions script, global script, readme, and how to renumber or keep your growing scripts squared away.* This folder should always contain a .Rproj file to initialize the environment for running your R scripts. **NOTE: even though this contains a .Rproj file, you should always use the here::here function in the [here package in R](https://here.r-lib.org/) when referencing file locations in your project repo - there are excellent reasons for using both .Rproj and here::here - when combined these are a powerful way to guarantee there won't be any issues with other users running your code due to directory issues. [Malcom Barrett](https://www.rstudio.com/authors/malcolm-barrett/) gave an an excellent talk at the [useR! 2020 conference](https://user2020.r-project.org/program/contributed/) entitled ["Why use the *here* package when you're already using projects?"](https://m.youtube.com/watch?v=QYrdsjBvZN4). The .Rproj file contained in this tenplate is pre-configured to start with a *completely clean workspace EVERY TIME* by selecting in *Project Options > General* "Restore .RData into workspace at startup" = NO and "Save workspace to .RData on exit" = NO. These two options combined will also ensure that others running your code (or even you at a later date) don't experience errors or conflictions due to workspace-specific background[^1] - this is often the cause of the following issue: "I swear...my code worked the last time I ran it and I didn't change anything!".

### /02-pipeline

The idea for this folder comes from Ties de Kok[^2] and I like it. Every script file in 01-code has a subfolder in 02-pipeline. Within each subfolder are three other directories: *tmp*, *store*, and *out*. Per Ties[^2]:

> *02_pipeline/sub-folder/out* contains files that you save with the intention of loading them in a future code file. These are usually the “end-products” of the current code file.

*02_pipeline/sub-folder/store* contains files that you save with the intention of loading them in the current code file. This is, for example, used in scenarios where it takes a while to run parts of your code and to avoid having to re-run these parts every time you might want to intermittently save the progress of your generated data to the store folder.

*02_pipeline/sub-folder/tmp* contains files that you save for inspection purposes or some other temporary reason. The basic principle is that you should not have to worry about anything in the tmp folder being deleted as it only serves a temporary purpose.

### /03-output
This folder contains final, polished figures and tables or other products that will go in the manuscript. Note - although the /06-manuscript folder has subfolders for tables and figures, these are not necessarily the same thing. Any figures and tables in /03-output are generated from your code and source data. It is likely/possible that you may also end up with manuscript figures and tables that are not generated from your source data (conceptual figures, logic models, etc).

### /04-project-cache

Your cache folder may not be used often. However it is there to provide a storage place for any large objects or data that take a very long time to generate initially in a script. When these are generated once, they can be exported to this folder - this provides a shortcut for subsequent running of the scripts that saves time. People can still run your original scripts to generate these objects if they so choose.

### /04-project-temp

The temp folder is basically to hold any temporary non-important files, such as tests script outputs, test figures, etc. These are not important to the overall analysis. This folder should be not be included in your final project using \[.gitignore\] file.

### /05-manuscript

This folder houses everything related to manuscripts or reports resulting from the project. It has subfolders for holding drafts, submitted versions (inclduign subsequent revisions), final and proofs, final figures & tables, and references (which contains a single .bib file - ideally exported from your Zotero project folder which *matches the name of this project*).

### /06-presentations

This folder should contain subfolders for each presentation realted to the topic. Subfolders should start with "C0X" and also include the year and venue. So if I gave a presentation at the Soil Science Sciety of America meetings in 2017 on a particular project, and it was the first presentation that I gave under that project, my subdirectory containing all of the relevant things for that presentation would be "C01-2017-SSSA".

### /07-log

This folder contains logs related to your project, specifically your analysis log and writing log. You can choose to make these public or not. *See this section for how to write and construct logs - do it at the end of every day/session - take the 5 MINUTES!!!*

### /08-archive

This folder contains anything that is not relevant to the current workflow but that you are not ready to delete permanently *yet*. Items can be moved to the archive folder at any time. Consider including in git-ignore.

### /docs

If you choose to create a Quarto e-book to consolidate your metadata and readmes as well as project workflow and potentially manuscript, all relevant files should go in this subdirectory. More information on generating an html book from separate markdown files *here*. NOTE - the only way to get GitHub Pages to find the files and render the htmls together is to have them in a /docs subdirectory. There is no other options currently. That is why /docs doesn't have a number before it.

### readme.txt

This is arguably one of the most important features of your entire project. The readme should always be in plain text .txt format so that anyone on any system can read it. Spending time on your readme file is extremely important:

-   It is the first writing you will do on your project and helps you to describe at a high level what your project is about
-   It is the file that tells everyone the who, what, when where, why, and how of this project:
    -   who created this project?
    -   what is in this project?
    -   when was it created?
    -   where can i find relevant data, files, and documents within the project structure?
    -   why was this project done; what was the motivation?
    -   how should I use these project files; what are the file formats, what programs do I need to run or access them; how were these files created and using what tools?

A readme is a living document. You should begin any project with a draft readme, which, provided you use this workflow and file structure can be mostly copied from a suggested template located in this section. However, as your project grows and changes, your readme should be updated. Be as thorough as possible. Note that this is a project level readme. Although not required, you should strongly consider including sub readmes at within multipl subfolders where necessary. Conversely, you can create a project level book in Quarto that contains all readmes and documentation (see section X). *More information on how to use and write a readme in section X*.

### TODOs.txt

This is a running list of TODOs - actually this shouldn't be a thing. You should use GitHub Issues to track this? Not sure need to look into this more.

### project-metadata.json

This is your metadata file for the entire project (note that all datasets should also have associated metadata files as well in the 00-data directory. This metadata file should be formatted according to our general conventions on metadata *here*.

[^1]: [Alex Douglas::Setting up a reproducible project in R](https://alexd106.github.io/intro2R/project_setup.html)
[^2]: [Ties de Kok::How to keep your research projects organized: folder structure](https://towardsdatascience.com/how-to-keep-your-research-projects-organized-part-1-folder-structure-10bd56034d3a)
[^3]: [Kenyon White::ProjectTemplate](https://github.com/KentonWhite/ProjectTemplate)
[^4]: [Project Template](http://projecttemplate.net/index.html)
[^5]: [Anna Krystalli::Projects in R Studio](http://projecttemplate.net/index.html)
[^6]: [Matt Dray & Anna De Palma - rostrum.blog::A GitHub repo template for R analysis](https://www.rostrum.blog/posts/2019-06-11-r-repo-template/)
