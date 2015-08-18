DocPu, Document publishing plugin
=================================

This plugin enables PDF export and generation for Redmine wiki pages using the LaTeX typesetting system. But it is even more than an simple PDF exporter since it uses the LaTeX text and figure layout engine.

Originally this redmine plugin was created by Christian Raschko (sponsored by [ATV-Elektronik](http://atv-elektronik.co.at/)). It was intended to be used for 1.1.x verisions of redmine and below.
Since this plugin creates eye catching documents and of cause can be extended a little more I decided to get the code and make it valid for use in __redmine 2.6.x__ versions.

Mainly all features as described below should work. You may find formatting in Redmine's wiki-syntax which isn't covered yet. If so-feel free to contact me.

Features
========

* Merge multiple wiki pages to one single PDF document
* Automatically generate title page and table of content
* Automatically generate an index from all strong or italic words
* Wiki page references are translated to PDF links
* Code highlighting is supported for Redmine code block (uses minted + pygments)
* Solid table boarders __can__ be drawn
* Hard linebreaks are possible in tables (like it is supported in redmines wiki-syntax)
* Add chapter for every wiki page
* Remove Redmine macros, eg. {{toc}}
* Convert wiki footnotes to LaTeX footnotes
* Use floating or fixed images
* support of three languages (plugin gui) => english, german, bulgarian
* ...

Dependencies
============

DocPu uses and extends the [RedCloth4](http://redcloth.org/) LaTeX export module, which is a converter for the Textile markup language. The system which hosts redmine should also have an working installation of LaTeX. Since the code highlighting is done by pygments, this python package should be installed too. This version of the plugin is made to work with __redmine 2.6.x__. This indicate a dependency to [rails 3.2](http://guides.rubyonrails.org/v3.2.21/3_2_release_notes.html) as you can see [here](http://www.redmine.org/projects/redmine/wiki/RedmineInstall#Ruby-interpreter). For older redmines you can refer to this [version](http://www.redmine.org/plugins/redmine_doc_pu) of the plugin.

Installing RedCloth4
--------------------

Currently Redmine uses RedCloth3, so you have to additionally install RedCloth4.
The simplest solution is by typing gem install RedCloth, but this only works if you have a compiler set-up, since some parts are written in C code.
For windows users a pre compiled gem package can be downloaded from the repository. Download it first and install it via eg. gem install RedCloth-4.2.2-x86-mswin32-60.gem

LaTeX
-----

### Installing LaTeX


On Windows machines you can download and install the [MikTex](http://miktex.org/) LaTeX distribution. Four all other platforms see http://www.latex-project.org/ftp.html.

DocPu Requires the following LaTeX packages, be sure you have them installed as well:

* tabularx
* listings
* ulem
* graphicx
* float
* multirow
* makeidx
* hyperref
* minted
* caption

### Testing LaTeX

It's a good idea to test the LaTeX installation before using it with DocPu. Create an empty folder and copy the article_en_tcfi.tex file from the plugin template directory(redmine_doc_pu/templates) to it. Now create an empty file named document.tex, afterwards you can compile the document with pdflatex article_en_tcfi.tex (or path_to_latex/pdflatex). The file should compile successful with the message:

```
Output written on article_en_tcfi.pdf (1 page, 28874 bytes).
```

Installing DocPu plugin
-----------------------

Please refer this [Guide](http://www.redmine.org/projects/redmine/wiki/Plugins) to learn how to install an redmine Plugin. Note that you should follow the steps for the 2.x version.

### Configure DocPu plugin

Goto Redmine administration and select Plugins. You should see the DocPu plugin installed. Now select Configure and fill in the correct LaTeX and makeindex binary paths.

![no image there](https://github.com/vvmruder/redmine_doc_pu/blob/master/doc/doc_pu_settings.png "settings dialog")

First you should try to test around a little to check if the set paths are ok and useable for the plugin. You doing so by entering the correct path in the field and clicking the "test" link. A routine will be called by that which try to use the given parameter. Please make sure you do that for all three paths. If everything is ok you should recive the following answers:

for pdflatex binary: ![no image there](https://github.com/vvmruder/redmine_doc_pu/blob/master/doc/doc_pu_settings_latex_test.png "answer for pdflatex binary test")

for makeindex binary: ![no image there](https://github.com/vvmruder/redmine_doc_pu/blob/master/doc/doc_pu_settings_makeindex_test.png "answer for makeindex binary test")

for templates folder: ![no image there](https://github.com/vvmruder/redmine_doc_pu/blob/master/doc/doc_pu_settings_templates_test.png "answer for templates binary test")

Most problems are access related. So make sure the redmine running user has the correct rights to read the files/folders.

Please note also that it is possible to set the binarys to the PATH environment variable. If you won't do this. Point the path to the exact place where the binaries are stored.

Now it's time to set Roles and permissions for the DocPu plugin in Redmine. Therefore, goto administration and select Roles and permissions. You will see a new category called Document publishing, within that category there are 3 different security settings:


* __View documents:__ Shows all documents assigned to a project, if disabled the project menu Publish is not displayed.
* __Build documents:__ Build a document, enables the permission to execute the LaTeX binary.
* __Edit documents:__ Edit document, this also involves creating, editing and deleting documents.

![no image there](https://github.com/vvmruder/redmine_doc_pu/blob/master/doc/doc_pu_permissions.png "the publishing permission settings")

The last step is to enable the Document publishing module in the project settings.

Now the plugin is ready to go for publishing documents. This plugin is intended to be used in project scope. So you can have generated documents per project => wiki. If you enbled the module in a project you get a new __publish__ tab. Here you have a list of all your created documents and you can start the creating process itself.

![no image there](https://github.com/vvmruder/redmine_doc_pu/blob/master/doc/doc_pu_publishing.png "the project's document list view")

Once you created a new document (fill in all data and check all items you want to be used for this document) you get a new view. In this you can add wiki pages (only from this project) to the document (please note that the order in the list of wiki pages is the order in the document).

After adding some content to the document you can try to build it by click in the upper left. In the following gui you have a kind of an log window. If you start the build the error are pushed in this window. Normally there should be some "Warnings" or "Bad Box(es)". This isn't tragic. If you want to have an index at the end of your document and enabled it in the document settings-it could be needfully to hit the build process several times (1-3). You should see a decreasing amount of "Warnings".

After building the doc you can check if it fits your expactations by click on "View document" in the upper left. There you can go back to the documents settings or view the generated LaTeX code also. View the LaTeX code is very helpful if there are some errors. This way you can copy the code and build it by hand on your computer to test whats going on (only recommendable for experienced users).
