# Undesign the Redline @ Barnard

[![Gem Version](https://badge.fury.io/rb/wax_theme.svg)](https://badge.fury.io/rb/wax_tasks)
[![Build Status](https://travis-ci.com/minicomp/wax.svg?branch=main)](https://travis-ci.com/minicomp/wax)
[![Join the chat the minicomp-wax channel of the Code4Lib Slack](https://img.shields.io/badge/Slack-%23minicomp--wax-brightgreen.svg)](https://docs.google.com/forms/d/e/1FAIpQLSeD77mBp0Y13mFePF8UmDwFrlbxNx3VttEjz_3dgglJeK-Zbg/viewform?c=0&w=1)


[_Undesign the Redline_ @ Barnard](https://undesign.dhcbarnard.org/) is an interactive exhibition that combines history, art, and storytelling with community outreach and collaboration, in order to reckon with systemic racism through an exploration of the legacy of redlining in Barnard and Columbia's neighborhood. This digital scholarly exhibition will highlight _Undesign the Redline_ activities at Barnard and host the #Undesign@Barnard syllabus, a public and collaborative learning tool. The digital _Undesign the Redline_ @ Barnard presence will use the __Wax__ workflow which follows minimal computing principles. Below is some important information about __Wax__.

Please see the original __Wax__ readme and installation instructions in the [final section](#Wax) when first
working with this repository.

## Table of Contents
1. [Undesign directory structure](#Undesign-Directory-Structure)
2. [Undesign digital collection](#Undesign-Digital-Collection)
3. [Original Wax readme](#Wax)

<br>

# Undesign Directory Structure

The __Wax__ exhibition site works with a file of metadata records (in `CSV` format) and a directory of image files (either in `png` or `jpeg` format). The CSV spreadsheet containing the Undesign syllabus metadata records can be found at [`\undesign-barnard\_data\undesign.csv`](https://github.com/dhc-barnard/undesign-barnard/blob/main/_data/undesign_faulty.csv). This is how the spreadsheet looks like.

<br>

![CSV Spreadsheet](/img/undesign_spreadsheet.png)

<br>

The most important fields here are `pid` and `label`. These fields are required. The `pid` field should follow the naming convention already present (`obj1`, `obj2`, `obj3`, ...). The `label` field should be the title or simply description of the collection item. For other fields, please keep in mind to avoid whitespaces and special characters and to keep field names in lowercase. You can change how these field names are displayed on the website.

_The following field names will throw errors: `id`, `date` (use `_date` instead), `thumbnail`, `full`, and `manifest`._

The directory of image files to tag visuals to each collection item can be found in [`\undesign-barnard\_data\raw_images\undesign`](https://github.com/dhc-barnard/undesign-barnard/tree/main/_data/raw_images/undesign). To match an image to its object in the spreadsheet, the files must be named to match the `pid`. So for example, an image attached to `obj1` should be named `obj1.png` or `obj.jpeg` depending on file type.

# Undesign Digital Collection

With those two essential pieces, the spreadsheet and directory of image files, you can generate the
collection by running the main tasks. Please make sure you are comfortable with using Wax in a Docker container. You can see steps on how to create and access that container in the [__Wax__ section](#Wax).

Once you have your container running, you can run the following to see available tasks:
```
bundle exec rake --tasks
```

The three tasks of importance are `wax:derivatives`, `wax:pages`, and `wax:search`.

### wax:derivatives

The task `wax:derivatives:simple` generates the collection's image derivates.

```
bundle exec rake wax:derivatives:simple undesign
```

Running the above command will:
1.  Look for the directory of image files at [`\undesign-barnard\_data\raw_images\undesign`](https://github.com/dhc-barnard/undesign-barnard/tree/main/_data/raw_images/undesign)
2. Generate two copies of each image (full size and thumbnail, with 1140 px and 250 px widths, respectively) into the directory `img/derivatives/simple`.
3. Automatically add two fields (`full` and `thumbnail`) to the metadata records with paths to those image derivatives.

### wax:pages

Running the following will generate the pages for each collection item.

```
bundle exec rake wax:pages undesign
```
This will:
1. Look for the metadata spreadsheet at [`\undesign-barnard\_data\undesign.csv`](https://github.com/dhc-barnard/undesign-barnard/blob/main/_data/undesign_faulty.csv)
2. Generate the `_undesign` directory with `.md` files that will generate pages with the metadata information

### wax:search

 Running the following will generate the search index for the Undesign site.
 ```
 bundle exec rake wax:search undesign
 ```

 _To Alicia: I can't figure out how to fix the thumbnail images path for the search_

# Wax

__Wax is an extensible workflow for producing scholarly exhibitions with minimal computing principles.__<br>
It's comprised of: __a few Ruby gems__ for processing image data and associated metadata ([wax_tasks](https://github.com/minicomp/wax_tasks/), [wax_iiif](https://github.com/minicomp/wax_iiif/)), __a Jekyll theme__ ([wax_theme](https://github.com/minicomp/wax/)), and (hopefully soon!) a lot of __documentation and recipes__ for creating, deploying, and maintaining digital exhibitions.


- [Prerequisites](#Prerequisites)
- [Getting Started](#Getting-Started)
- [Using Docker](#Using-Docker)
- [Contributing](#Contributing)

<br>


## Prerequisites


You'll need `git` and `ruby >= 2.4` with `bundler` installed.
These dependencies can either be installed natively on your system or within a [Docker environment](#Using-Docker). For instructions, check the Wiki's [Installation Guide](https://minicomp.github.io/wiki/wax/system-requirements/installation-guide/).

Check your versions with:

```bash
$ ruby -v
  ruby 2.4.5p335 (2018-10-18 revision 65137) [x86_64-darwin18]

$ bundler -v
  Bundler version 2.0.1
```

To process images, you will also need to have ImageMagick and Ghostscript installed and functional. You can check to see if you have ImageMagick by running:

```bash
$ convert -version
  Version: ImageMagick 6.9.9-20 Q16 x86_64 2017-10-15 http://www.imagemagick.org
  Copyright: (c) 1999-2017 ImageMagick Studio LLC
```

... and check Ghostscript with:
```bash
$ gs -version
  GPL Ghostscript 9.21 (2017-03-16)
  Copyright (C) 2017 Artifex Software, Inc.  All rights reserved.
```

## Getting Started

__There are a few ways to get started with Wax, depending on your needs.__ Downloading the demo is suggested for new users so you can see how a full Wax site would work. __Advanced Jekyllers__ can start from a clean Jekyll install. To start with the demo:

1. Change directory into where you'd like your site, e.g., your Desktop:
    ```sh
    cd ~/Desktop
    ```
2. Download the zip file from the [wax github repository](https://github.com/minicomp/wax/). The option to download the zip file should be on the button labeled "Code." Your browser will save the file where it normally saves downloads.

3. Move the zip file to the location you will use. In our example, to the Desktop.

4. Unzip the file. This can be done through your operating system graphic user interface, or in the terminal:
    ```sh
    unzip wax-master.zip
    ```
    You can delete the zip file once you're done.

5. Rename the directory and go inside the project folder:
    ```sh
    mv wax-master my-project
    cd wax-master
    ```

6. Install the gems (not required if using Docker):
    ```sh
    bundle install
    ```

7. Run the demo site:
    ```sh
    bundle exec jekyll serve
    ```

After the last step, the terminal will provide you with a localhost URL for you to see your local copy of the site on your browser. This is the template site you will make changes to in order to make your own exhibition. For more, check out the [Minicomp/Wax Wiki](https://minicomp.github.io/wiki/wax/).


## Using Docker

To use Wax in a container, make sure you are familiar with Docker and have [Docker installed](https://docs.docker.com/get-docker/).

Run the "Getting Started" steps 1-5 above to copy and `cd` into the repo.  

Next, build the `wax_image` base image:
```
$ docker build -t ubuntu/wax .
```

You will run all of the Wax tasks and commands within an interactive bash container, which you can create and access by running:
```
$ docker run -it --rm -v "$PWD":/wax --name wax -p 4000:4000 ubuntu/wax bash
```

To serve the site, you can run the following command in the guest container and view it in your host browser:
```
$ bundle exec jekyll serve --host 0.0.0.0 --incremental --force-polling
```

You can exit the container at any time with `$ exit`, which will automatically stop and remove the container.

## Contributing

We welcome contributions to Wax, including bug reports and feature requests (submitted as [Issues](https://github.com/minicomp/wax/issues)), code contributions (submitted as [Pull Requests](https://github.com/minicomp/wax/pulls)), and documentation updates (submitted however!) Not sure where to start? Feel free to get in touch via [GitHub issue](https://github.com/minicomp/wax/issues) or grab an invite to join the conversation on the `#minicomp-wax` channel of the [Code4Lib Slack](https://docs.google.com/forms/d/e/1FAIpQLSeD77mBp0Y13mFePF8UmDwFrlbxNx3VttEjz_3dgglJeK-Zbg/viewform?c=0&w=1).
