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

> [!NOTE]
> This website is intended as a living document and to be improved over time! If
> you find some data or pages that you can make easier to edit for further
> years, please do so and adapt any sections in this manual as you see fit.

## Getting Started

Before you can start working on the website, a few introductory remarks. This
website is hosted in a GitHub repository. This means that all changes can be
retained and your changes go on top of the existing revision history. To get
started with your iteration of IC2S2, the very first steps are the following:

> [!NOTE]
> You will need a terminal or command-line to manage the website.

1. First, make sure that you have a GitHub account. This can either be an
   organization or just a personal account (like with IC2S2 2025).
2. Then, clone the repository onto your computer
   (`git clone https://github.com/nathanlesage/ic2s2-2025` for the 2025
   edition). This will create a folder named after the repository (e.g.,
   `ic2s2-2025`) on your computer. For a clean slate, remove the hidden `.git`-
   folder now from the downloaded repository.
3. Create a new repository in whichever GitHub account you choose, and name it
   `ic2s2-YYYY`, replacing `YYYY` with the correct year.
4. Follow GitHub's instructions to add the files you just cloned on your
   computer (remember to initialize a new, empty git repository since you just
   removed the `.git`-folder).

From now on, you have all changes on GitHub, and can use this code also to use
Netlify or whichever provider you want to use to actually host your website.

> [!TIP]
> The rest of this manual assumes that you have created a repository on GitHub.
> We highly recommend you do so, because this will make hosting and adapting the
> website much simpler than if you have a collection of files on some computer
> and uploading them using, say, FTP. Also, this way, any improvements you make
> will be passed on to future conference organizers, ensuring that we
> collectively improve the website experience.

## Using Jekyll

This site is running atop of Jekyll, a rock-solid static site generator. What
Jekyll will do is take the contents of this folder and combine everything and
compile the various parts into a single, static HTML folder, called `_site`.
This is what you can then upload to whatever hosting provider you choose. In the
following, I will run you through the steps to get fully startd.

### Installing Jekyll on your Computer

In order to preview the changes, however, you should have Jekyll installed on
your computer. To do so, please
[head over to their manual](https://jekyllrb.com/docs/) which outlines the steps
necessary to set everything up.

### Previewing the Page Locally

Once you are set, you should be able to preview the website on your computer.
There are two important commands that you can run inside this folder (both
require a command line/terminal):

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

### Using GitHub Actions for Automated Builds

Traditionally, IC2S2 has been using Netlify for deploying its conference
websites. This works really simple: You register for a free account with Netlify
and connect it to the repository that contains the IC2S2 website source. Netlify
will then react to any changes you make to the website and redeploy it
automatically for you.

While this works well if the websites are small and do not have too many
visitors, Netlify has a very strict bandwidth limit. This has caused IC2S2 2025
to very quickly reach the bandwidth limitations of the free plan. After a short
discussion, it was decided to host the website on a different server for the
duration of the conference so that we are not affected by these bandwidth
limitations.

> [!TIP]
> We recommend you start with Netlify, as this is simple to set up, and only
> switch to GitHub Actions if Netlify warns you of low bandwidth quota. On the
> first read of this manual, you can skip the rest of this section and return to
> it if needed.

For that, we have set up a so-called "GitHub Actions Workflow". In this
repository you can see a folder `.github`. It includes another folder called
`workflows` which contains a file `jekyll.yml`. A "GitHub Workflow" is a set of
instructions that tells GitHub to automatically build and deploy the website
whenever anything changes. Essentially, it uses the same technology as Netlify,
but allows you to customize where the website will be uploaded to.

> [!WARNING]
> If you use Netlify, you need to disable the workflow on the GitHub user
> interface. Otherwise you will receive hundreds of emails from GitHub that your
> workflow failed (because you haven't set it up).

Currently, the workflow is set up to always re-build the website on any change
and immediately deploy this using SSH. This means that, whenever you change
something in the website source, and commit this to GitHub, GitHub will
automatically rebuild and redeploy the website, meaning any changes are
reflected on the actual website within minutes.

It is currently set up to utilize SSH for this. SSH, or "Secure SHell", is a
small command-line program that logs into a webserver and transfers some data.
SSH is available on almost all webservers by default. In order to utilize this
workflow, you need to provide a few repository secrets (please refer to the
GitHub documentation on how to set these). The following are required:

* `SSH_PRIVATE_KEY`: This needs to be the private key from a public-private SSH
  key. Please refer to the SSH documentation on how to generate such a key-pair.
  To set this secret, copy the entire file contents into the web interface
  (including the "BEGIN" and "END" lines).
* `REMOTE_HOST`: This is the host/domain where your webserver sits at. Can be
  something like `https://www.example.com:22`. (The port `:22` should be omitted
  unless SSH listens at a different port for you.)
* `REMOTE_USER`: This is the username that GitHub should authenticate as. This
  must be the user for which SSH accepts the private key above.
* `TARGET`: This is the target directory of your webserver. Should be an
  absolute path (e.g., `/var/www/html/ic2s2`).

> [!TIP]
> Alternatively, you can also use GitHub pages, which makes deployment a bit
> simpler. We have opted against it, as we were not certain if hosting a large
> conference website on GitHub pages conforms to their Terms of Service. For
> more information on using GitHub pages, please refer to the GitHub
> documentation. An example workflow that you can adapt can be found
> [here](https://github.com/nathanlesage/openkit/blob/main/.github/workflows/jekyll.yml).
> Should you adopt GitHub pages, please do not delete the `jekyll.yml` workflow
> but add a second workflow file instead. This way, future conference organizers
> have the choice.

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
re-use that, with merely exchanging the values.

> [!WARNING]
> Always pay attention to the indentation of the YAML files. YAML does not like
> tabulator characters, so use spaces. Also, indentation is important, so make
> sure that everything is aligned properly. If in doubt, reference the existing
> data files to see how it works. If you are unsure what is wrong, you can try
> using a [YAML checker](https://www.yamllint.com/) which can help you find
> errors.

### `_includes`

This folder contains HTML files that will be included in various places. You
should not need to edit these files, but should you have to, two remarks: First,
it's standard HTML with so-called [Liquid](https://jekyllrb.com/docs/liquid/)
template syntax. And second, I have tried to keep the naming scheme sensible so
that you hopefully are able to see where these includes are included. There are
also comments inside the files to explain what they do.

> [!TIP]
> If you choose to adopt `Conferia.js` (see below for more info), there is one
> file you need to adopt: `head.html`. In there, you should set the Conferia
> scripts and styles to the most recent version of Conferia at the time you host
> IC2S2. To do so, exchange the version number in the links. For example, if the
> most recent Conferia version is version 1.43.2, turn
> `https://cdn.jsdelivr.net/gh/nathanlesage/conferia@0.18.0/dist/conferia.js`
> into
> `https://cdn.jsdelivr.net/gh/nathanlesage/conferia@1.43.2/dist/conferia.js`.

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
working with the files in VS Code is easier. If you use a different code editor,
this won't be helpful to you.

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
directs to "www.ic2s2-2025.org/venue". These files you probably need to edit as
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
holds the primary configuration. I have documented all sections in this file for
you so that you can infer its meaning.

The most important section for you are the global variables. I have tried to
come up with an exhaustive list of global variables that may need to change
between conferences, but you can add more if you spot a need. (Just remember
that you'll have to restart the Jekyll server if you change this file.) You can
access these global properties in any HTML file using `site.[property name]`.
For example, to insert the general inquiries email address, use
`{{ site.emails.general }}`.

The second-most important section for you are the `exclude`s. Specifically, this
property is a list of HTML- and Markdown-files that Jekyll should explicitly
ignore. We have set this to ignore the README and this MANUAL, but especially if
the pages aren't ready for publication yet, you should un-comment their lines so
that Jekyll doesn't include, say, the "Venue" page when it still shows the old
Norrköping information.

### The main folder: Other files

Now quickly to the other files in the main folder:

* `Gemfile`: This tells Jekyll what types of plugins we use. You can probably
  ignore this.
* `Gemfile.lock`: Please ignore this file entirely.
* `MANUAL.md`: This file
* `README.md`: The general readme file

## Workflows / How to Create an IC2S2 Website

Here I outline a set of workflows that I believe are useful to work with. At
least this is how I got started. I also include "lessons learned" from after the
conference.

### To Get Started

Before anything else, fork the IC2S2 repository to some GitHub account. Then,
clone it to your computer and set up Jekyll, and verify that the website works
and looks the same as the previous year by running `bundle exec jekyll serve`
and navigating to `http://localhost:4000`.

### First Things First

To begin adapting the website to your likings, first do the following steps:

1. Adapt the `_config.yml` accordingly. If you don't know all the details yet,
   it makes sense to add placeholders in the according places that you can
   identify as such, and adapt later on.
2. Remove the pound-sign (`#`) in front of all lines in the `exclude` section.
   This will prevent Jekyll from including the pages in the output, meaning you
   can keep it and adapt it later. Once you are happy with a page, you can then
   un-comment the corresponding navigation entry (see next point) and comment
   out the page's entry in this exclude-section to have it show up. Note that
   the landing page (`index.html`) is not in this list, so it will never be
   ignored.
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
  open the video in VLC and take a snapshot.
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
* (Student) Volunteers

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

### Adding New Pages

Should the need occur to add entirely new pages, here's how you should do it.
First, create a new HTML-file in the main folder and give it a readable name
that should only include letters and dashes. This name will also become the URL.
So, if you want a new page titled "My New Awesome Page!" you should name it
"my-new-awesome-page.html" — it will then be reachable at
www.example.com/my-new-awesome-page.

At the top, add a frontmatter, e.g.,

```yaml
---
title: My New Awesome Page!
layout: default
---
```

Then, proceed to add the HTML code. Most pages use one of two or three different
structures, but it doesn't follow a specific structure. My tip: Copy any
existing HTML page that you like, and simply adapt it.

Next, you should add a navigation entry to `_data/navigation.yml` at the
appropriate place. Simply copy an existing entry to the correct location, and
adapt it.

Lastly, you should add that new page to the "exclude" section in the
`_config.yml`, but prepended with a pound-sign (`#`) so that Jekyll actually
includes it. This way, upcoming IC2S2 organizers can quickly remove the file if
they don't need it by uncommenting it here.

### Creating a schedule

The last important part of organizing an IC2S2 instance concerns setting up the
schedule. One big issue with IC2S2 is that it is very large, and as such a
proper interactive agenda is needed. IC2S2 2023 in Copenhagen featured a custom-
built solution by Laura Allessandretti that managed to visualize the huge agenda
in an efficient way. IC2S2 2024 in Philadelphia used Excel spreadsheets to
display the agenda.

IC2S2 2025 in Norrköping wanted to have a custom solution like Copenhagen, but
without Excel spreadsheets, so I, Hendrik, decided to write an entirely new
framework called [Conferia.js](https://github.com/nathanlesage/conferia). This
framework aims to make creation of interactive agendas for IC2S2 as simple as
possible, and we invite all future IC2S2 to use it as well. The benefits are:

* No custom apps that people must download
* Works across browsers and devices
* Has some nice quality of life features for the conference
* Is very easy for the Conference Chairs to handle

To adopt Conferia.js yourself, its manual is a good place to start. In terms of
integrating it into the website, here's what you need to do.

First, create the agenda and export a CSV file as outlined in the Conferia
manual. If you prepared the schedule accordingly, it should already be in the
correct format after the schedule has been created. Save this file into the
`files` folder, and give it the name `ic2s2_<year>_schedule.csv`. So for IC2S2
use `ic2s2_2026_schedule.csv`.

Next, open the file `_includes/head.html`. At the bottom, you can see the
Conferia scripts. Exchange the version number with the newest one available.
At the time of writing, the version is `conferia@0.8.0`. You can find the most
up-to-date version [here](https://github.com/nathanlesage/conferia/tags). Simply
use the newest version number, but without the `v` at the front, e.g.,
`conferia@1.2.0`. Doing so ensures that each conference iteration uses the most
up-to-date version of Conferia, which means more features, less bugs, and that
it works as easy as possible.

Finally, open the file `schedule.html`. At the top, you can adapt the intro as
you like (e.g., indicate if it is only a tentative schedule). The important bit
is the `<div id="conferia"></div>`, which is the place where the schedule will
appear.

At the bottom of the file you can see the initialization script for Conferia. In
it, you probably want to adapt the `src` to have it point to your new schedule,
edit the `timeZone` according to where your conference is happening, and adapt
any other configuration values as they are outlined in the Conferia.js manual.

Then, whenever something in the schedule changes, simply export it to CSV once
more, and replace the file.

> [!TIP]
> If you discover a bug, or have a good idea of how to improve Conferia.js, I am
> [very happy about your feedback](https://github.com/nathanlesage/conferia/issues)!

## Final Thoughts

I hope this manual helps you organize the best possible IC2S2 experience. I know
that it is a lot of work, and this is why I hope that my additions and changes
to the structure may help you to reduce the amount of work you have to spend
figuring out the website, and that you can instead use this time to do great
research.

If you have any additional questions, or want to just get in touch, I'd love to
hear from you! My email is <hendrik.erz@liu.se>, but in case I switch
affiliations, head over to my website, <https://www.hendrik-erz.de/> to see all
ways to get in touch.

Happy organizing!

— Hendrik
