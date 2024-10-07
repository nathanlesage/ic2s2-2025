# Manual

Hi, I'm Hendrik and I have been tasked with creating the IC2S2'25 website for
the 11th IC2S2 in Norrköping in 2025. I have inherited this website from the
previous years, and since that was essentially a set of static html pages, I set
out to make it a bit more dynamic for two reasons: First, I hope that in this
new setup, adding and modifying the changing information is much quicker and
easier as the conference approaches. Second, I hope that, by decoupling the
layout from the data, the website may become more coherent and you don't have to
deal with too much HTML code.

That being said, in this document, I have collated how I did this and how the
website is supposed to work so that you know where to start and how to adapt it.
If you have any questions, please reach out to me!

## Preconditions

Before you can start working on the website, a few introductory remarks. This
site is now running atop of Jekyll, a rock-solid static site generator. What
Jekyll will do is take the contents of this folder and combine everything and
compile the various parts into a single, static HTML folder, called `_site`.
This is what you can then upload to whatever hosting provider you choose.

In order to preview the changes, however, you should have Jekyll installed on
your computer. To do so, please
[head over to their manual](https://jekyllrb.com/docs/) which outlines the steps
necessary to set everything up.

After you are done, you should be able to preview the website. There are two
important commands that you can run inside this folder (both require a command
line):

```bash
$ bundle exec jekyll serve
```

This command starts a development server -- usually reachable in your browser by
visiting http://localhost:4000. It will continuously watch the files in this
folder for changes, and rebuild the page in the background if necessary. This
way you can quickly modify the data on the website and then refresh the website
to see how the changes look.

> [!NOTE]
> The `serve`-command does not react to changes in your `_config.yml`, so
> whenever you change this file, you will have to stop the development server
> with `Ctrl+C` (it's `Ctrl` even on macOS) and restart it by running the
> command again.

The second important command is:

```bash
$ bundle exec jekyll build
```

This works pretty much the same to `serve`, except that it does not keep a
server open. It just builds the website, puts it into `_site`, and quits. This
command is useful to just build the static pages, and this is also what GitHub,
for example, will run should you decide to host this website there.

> [!TIP]
> There are some other commands that may be useful, they can be found
> [here](https://jekyllrb.com/docs/usage/).

## Overview over the Structure

With the preconditions out of the way, the next two sections will first provide
you with an overview over the folder structure so that you know what is where,
and then an overview over the various data structures used by this website.

### `_data`

This folder contains all the data files. You will probably spend most of your
time editing the data here. All data files use the YAML format. If this sounds
unfamiliar,
[here is a quick introduction into how the format works](https://learnxinyminutes.com/docs/yaml/),
but fret not: Usually, you can just take whatever you find in the data files and
re-use this data structure.

> [!WARNING]
> Always pay attention to the indentation of the YAML files. YAML does not like
> tabulator characters, so use spaces. Also, indentation is important, so make
> sure that everything is aligned properly. If in doubt, reference the existing
> data files to see how it works. If you are unsure what is wrong, you can try
> using a [YAML checker](https://www.yamllint.com/) to test the code.

### `_includes`

This folder contains HTML files that will be included in various places. You
should not need to edit these files, but should you have to, two remarks: First,
it's standard HTML with so-called [Liquid](https://jekyllrb.com/docs/liquid/)
template syntax. And second, I have tried to keep the naming scheme sensible so
that you hopefully are able to see where these includes are included. There are
also comments inside the files to explain what they do.

### `_layouts`

This folder contains the major layouts, of which there is only one:
`default.html`. This is the default scaffolding for every page. It works like
the `_includes`, except that most includes are included here.

### `_site`

After you have built the website, this folder contains the entire website and
everything necessary so that it works. Upload this folder's contents to some
webserver.

### `.jekyll-cache`

If you can see this (hidden) folder, just ignore it. It just makes Jekyll go
faster.

### `.vscode`

You can also ignore this (hidden) folder. It contains some settings so that
working with the files in VS Code is easier.

### `assets`

This folder contains any additional files that are necessary for the website to
function, which mainly includes JavaScript (for dynamic elements) and CSS code.

### `files`

A folder for any non-HTML files that you require (such as ZIP-files with
templates and so on).

### `images`

The dreaded "big pile of pictures": This folder contains a ton of various images
that are used on the website. Most of it is pictures of the speakers and
tutorial leaders, some of it are pictures by the venue.

> [!NOTE]
> I **strongly** (double emphasis!) recommend sticking to a reasonable folder
> structure in here, otherwise this folder becomes a big mess. I myself opted
> for "people" (for the various faces we need on the website) and "venue" for
> photos of the venue, etc.

### The main folder: HTML Pages

Now to the various files in the main folder. There are many HTML files here.
Each one of this files corresponds to a sub-page, so "venue.html", for example,
directs to "www.example.com/venue". These files you probably need to edit as
well while you are preparing the conference. They work similarly to the includes
and layout HTML files above, with one exception: They start with two lines of
three dashes and this is a requirement that cannot be skipped!

```html
---
title: This will become the page's title
---
<div>
  <p>
    This is the HTML code of the page.
  </p>
</div>
```

These two lines demarcate what is known as a
[frontmatter](https://jekyllrb.com/docs/front-matter/). You can use it to define
various variables (see the Jekyll documentation for more info). Our website uses
them only to apply the page's title.

> [!TIP]
> I call the front matter a requirement since it is: Jekyll will treat any file
> without such a front matter in this folder as a simple text file, which means
> that it will not do anything to files without it. This frontmatter -- even if
> it is empty and only consists of two lines of three dashes -- signals to
> Jekyll "Hey, this is a page that you need to render properly."

### The main folder: The Config

The second important file in the main folder is the config, `_config.yml`. This
holds the primary configuration. At the top you will see some `plugins` that the
page uses.

Below that you have a set of global variables. I have tried to come up with an
exhaustive list of global variables that may need to change between conferences,
but you can add more if you need to. (Just remember that you'll have to restart
the Jekyll server if you change this file.) You can access these global
properties in any HTML file using `site.[property name]`. For example, to insert
the general inquiries email address anywhere, use `{{ site.emails.general }}`.

The `defaults` property denotes default values for the frontmatters I have
mentioned above, but which you can overwrite on a per-file basis.

### The main folder: Other files

Now quickly to the other files in the main folder:

* `Gemfile`: This tells Jekyll what types of plugins we use. You can probably
  ignore this.
* `Gemfile.lock`: Please ignore this file entirely.
* `MANUAL.md`: This file
* `README.md`: The general readme file

## Workflows

Here I outline a set of workflows that I believe are useful to work with.

### To Get Started

Before anything else, fork the IC2S2 repository to some GitHub account. Then,
clone it to your computer and set up Jekyll, and verify that the website works
and looks the same as the previous year.

### First Things First

To begin adapting the website to your likings, first do the following steps:

1. Adapt the `_config.yml` accordingly. If you don't know all the details yet,
   it makes sense to add placeholders in the according places that you can
   identify as such, and adapt later on.
2. Remove the pound-sign (`#`) in front of all lines in the `exclude` section.
   This will prevent Jekyll from including the pages in the output, meaning you
   can keep it and adapt it later. Once you are happy with a page, you can then
   un-comment the corresponding navigation entry (see next point) and comment
   out the page's entry in this exclude-section to have it show up.
3. Comment out everything in `navigation.yml`. This way, all the subpages will
   still be included in the output, but they are not linked. This way you can
   gradually adapt the pages and "unlock" them by uncommenting the corresponding
   entries of the main navigation.
4. Comment out the the dates in `dates.yml` by prepending each line with a pound
   sign so that they don't show up. If you already agreed on a set of dates, you
   can also already adapt them now. (This way the correct dates will show up
   immediately on the website).
5. In `tutorials.yml`, completely remove the `past_tutorials` section and rename
   `items` to `past_tutorials`. Then you already have that section done.
6. In `about.html`, create the link to last year's conference, and add your
   edition on top of the list.
7. Remove all the images from the `images` folder, except for the IC2S2 logos.
8. Remove all sponsors from the `sponsors.yml` so that you can add yours later.

At this point, you should have a relatively clean start and can begin adapting
the conference website to reflect your edition of IC2S2.

### Choose a Header Image

The first thing you should do is select a header image. It has become a form of
tradition for each conference to choose a custom header image. Copenhagen had a
picture of a structure in the city, for example, and Norrköping had a drone
footage video.

Once you are happy with a selection, you need to adapt the file
`_includes/header.html`. The file allows you to choose one image for the landing
page and one for all other pages. Set both to the same image or video if you do
not want to have multiple images/videos.

If you only want an image:

* Comment out the entire `<video>` element
* Exchange the `background: url()`-section on the top and adapt it to point to
  your image of choice

If you want a video instead (repeat these steps if you want two different
videos):

* Select a video to show. It should be something that can loop endlessly, but it
  will also work with any other video.
* Encode the video into both `mp4` as well as `webm` (webm is usually smaller,
  but mp4 is more compatible) and place them in the images folder. I can
  recommend the Open Source-software [Handbrake](https://handbrake.fr/) for this
  task.
* Make a screenshot of the very first frame of the video. The simplest way is to
  open the video in VLC and make a snapshot.
* Put the video files and screenshots into the `images` folder, and name it
  properly (something with `ic2s2_year_teaser` for easy identifiability).
* Adapt the `<source>`-elements within the `<video>` element to point to your
  video files.
* Set as the `cover` attribute in the `<video>` element the screenshot(s) of
  your videos.
* Set the `background: url()` at the top of the file to your screenshots, too.
  This way, the browser will show the screenshot until the video is loaded, as
  well as simply show the screenshot if the device doesn't support video.

### Adapt the main landing page

The next thing you should do is adapt the landing page. For this, open the file
`index.html` and start adapting:

* The number (e.g., `11<sup>th</sup>` to the iteration number)
* The location and the conference dates
* Add a picture of your venue
* Adapt the "About the Conference" section

Start the Jekyll development server `bundle exec jekyll serve` and take a look
at the page to verify that it looks as you want to.

At this point, you can already upload the website to the public, as this denotes
the bare minimum for a functioning website. You can gradually adapt the other
pages as you go.

### Adapt the "Important Dates" section

To adapt the important dates, head into `dates.yml` and make sure all are not
commented. Then, adapt each entry to your likings. If you need additional
entries, just create new ones.

Once a deadline approaches, you will have to manually set the `done: true` in
here to mark it as over and apply a strikethrough style to the date.

### Add the Conference Chairs

To adapt the conference chairs, you will need to adapt the file `people.yml`.
The file is grouped into groups of chairs that have a `title` property. The
following groups have been used in the past (but you may of course leave out
some or add new ones that you need):

* General Chairs
* Program Chairs
* Tutorial Chairs
* Poster Chairs
* Website & Social Media Chairs

If any of these groups has no `people`, the system will exclude it from
rendering. This way, you can just remove all people from there, keep the
category name, and gradually add chairs as you go.

Each person has a name, an affiliation, a field, an image (that you should place
in the images folder), and a URL to, e.g., their institutional or personal web
site.

### Add Registration Fees

Adding registration fees is straightforward. Open `registration.yml`, and add
the correct currency code or symbol (depending on how you want to display it).

Then, adapt the fees (include thousands-delimiter for better readability) and
any other fields that you need. This is also the location for the link to the
registration system that you use.

> [!NOTE]
> Set the value for `registration_open` to `false` initially, and set it to
> `true` only when people can register for the conference. This property shows
> or hides the registration form.

### Add Submission Links

To add and control submission, open `submission.yml` and add the corresponding
links (usually IC2S2 uses OpenReview). Set the submission properties accordingly
to enable/disable submission (this shows or hides the submission links).

### Add Keynote Speakers

Keynote speakers can be added equivalently to program chairs in the organization
page. There is just a bit more info to add.

### Add Tutorials

The tutorials page includes two pieces of information: The available tutorials
(that will fill up as you accept tutorials) and the schedule. The schedule
denotes how the tutorial day will proceed, including lunch or coffee breaks.

Then, each tutorial that you accept should have the following infos in the
`items` property:

* `title`: The tutorial's title
* `tutors`: A list of instructors, each should in turn have the following:
    * `name`: The instructor name
    * `image`: A picture (put that into the images folder)
    * `affiliation`: The instructor's affiliation
    * `url`: A link to their institutional or personal website.
* `time`: The time of the tutorial
* `room`: Which room the tutorial will be in
* `website`: Usually tutorials come with small websites for the participants.
* `abstract`: A longer description of what the tutorial will do.

### Add Sponsors

Adding sponsors works analogous to adding chairs: They are grouped in different
groups (e.g., Academic, Gold, or Bronze), and contain a name, an image, and a
URL to their website. The sponsors will show up in both the landing page and the
sponsors subpage. Edit the `sponsors.yml` file to add them.
