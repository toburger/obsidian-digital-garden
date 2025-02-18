# Obsidian Digital Garden
Publish your notes in your own personal digital garden. 
An example can be found [here](https://ole.dev/garden)

## Configuration
It's a bit of work to set this all up, but when you're done you'll have a digital garden in which you are in control of every part of it, and can customize it as you see fit. Which is what makes digital gardens so delightful.
Lets get started:

1. First off, you will need a GitHub account. If you don't have this, create one [here](https://github.com/signup).
2. You'll also need a Netlify account. You can sign up using your GitHub account [here](https://app.netlify.com/)
3. Open [this repo](https://github.com/oleeskild/digitalgarden), and click the green "Deploy to netlify" button. This will open netlify which in turn will create a copy of this repository in your GitHub accont. Give it a fitting name like 'my-digital-garden'. Follow the steps to publish your site to the internet.
4. Now you need to create an access token so that the plugin can add new notes to the repo on your behalf. Go to [this page](https://github.com/settings/tokens/new?scopes=repo) while logged in to GitHub. The correct settings should already be applied. If you don't want to generate this every few months, choose the "No expiration" option. Click the "Generate token" button, and copy the token you are presented with on the next page. 
5. Open Obsidian and the settings for "Digital Garden" and fill in your github username, the name of the repo with your notes which you created in step 3, and lastly paste in your token. 
6. Now, let's publish your first note! Create a new note in Obsidian. And add this to the top of your file

```
---
dg-home: true
dg-publish: true
---
```

**This does two things:**

* The dg-home setting tells the plugin that this should be your home page or entry into your digital garden. (It only needs to be added to _one_ note, not every note you'll publish).

* The dg-publish setting tells the plugin that this note should be published to your digital garden. Notes without this setting will not be published. (In other terms: Every note you publish will need this setting.)


7. Open your command pallete by pressing CTRL+P on Windows/Linux (CMD+P on Mac) and find the "Digital Garden: Publish Single Note" command. Press enter.

8. Go to your site's URL which you should find on [Netlify](https://app.netlify.com). If nothing shows up yet, wait a minute and refresh. Your note should now appear.

Congratulations, you now have your own digital garden, hosted free of charge! 
You can now start adding links as you usually would in Obisidan, with double square brackets like this: [[Some Other Note]]) to the note that you just published. Remember to also publish the notes your are linking to as this will not happen automatically. This is by design. You are always in control of what notes you actually want to publish. If you did not publish a linked note, the link will simply lead to a site telling the user that this note does not exist. 

## Commands
There are two commands available for publishing notes.

1. The "Digital Garden: Publish Single Note" command will publish the currently active note, and only this.
2. The "Digital Garden: Publish Multiple Notes" command will publish all notes in your vault that have the dg-publish setting set to true. This way you can easily keep track of which notes you have published.
(Protip: You can use the "DataView" plugin to list all published notes with this query: "list where dg-publish=true")

## Modifying the template/site
The code for the website is available in the repo you created in step 3, and this is yours to modify however you want. If you know some css I encourage you to change the default styling to make the site your own. Please modify the custom-style.scss when doing so to avoid
future conflict when updating the template. Netlify should automatically update your site when you make changes to the code.

## Updating the template
In the setting menu for the plugin there is, in addition to the previously mentioned settings, a setting with the name "Update site to latest template" with a button saying "Create PR". Whenever this template receives any updates, this button can be used to update your site. It will create a new branch in your repo with the changes and create a Pull Request to your main branch. The plugin will present you with this URL in the setting view. 

If you used the "Deploy to Netlify" button, a Netlify bot will build a preview version of your site which you can visit to see that the changes does not contain any breaking changes. The URL should be visible in the PR. 
When you are ready you can use the "Merge pull request" button on the pull request page to merge the changes into your main branch and make the changes go live.

In the future you will be notified with a visual cue whenever there is an update ready. For now you will need to manually check. If you have the latest version, you will be told so.

## Content support
The plugin currently supports rendering of these types of note contents:
* Basic Markdown Syntax
* Links to other notes
* Code Blocks
* Admonitions
* MathJax
* Embedded/Transcluded Images
* Transcluded notes
* Highlighted text
* Footnotes

## Advanced usage
### Permalinks
This plugin also supports setting your own links to a note, if you prefer something else than the default behaviour.
This is done by adding a dg-permalink attribute to the frontmatter of your file. 
As an example, the top of your file could look like this:

```
---
dg-publish: true
dg-permalink: "mynote"
---
```


This will make the URL to you note be "{Your-Garden-Name}.netlify.app/mynote/". You can still use normal obsidian links as before to link to it. These will be automatically corrected once you publish a note with the permalink attribute. 
Same goes for deleting the attribute. Doing so will result in the note using the default URL. All links in other notes should automatically be corrected and still work.  

The permalinks can be an arbitrary level of folders deep, such as:

```
---
dg-permalink: "category/2022/mynote/"
---
```

### Transclusion
By default, transclusion of other documents just renders the content as is. If you want to also include a heading on top of the transclusion you can do so by using the pipe character:
```
![[Some Other Note|Heading]]
```

This will add a h1 header with the value "Heading" at the start of your transclusion.

If you want the header to be equal to the title of the transcluded document, you can use this custom syntax:
```
![[Some Other Note|{{title}}]]
```
This will replace the heading with the title of the transcluded document when the note is published.

You can also use the title syntax inside other text:
```
![[Some Other Note|This is a {{title}}]]
```

#### Specifying heading level
You may also specify what heading level you want your transclusion to have. If you want the header to be a h2, you can use this syntax:
```
![[Some Other Note|##Heading]]
```

h4 would look like this:
```
![[Some Other Note|####Heading]]
```

#### Default behaviour
By just using regular translucion, no header will be added:
```
![[Some Other Note]]
```

It's also worth noting that transclusions *do not need* the dg-publish attribute. They behave the same as an image. If you transclude something into a document, and publish that document, everything that is transcluded in it will be published as if it was part of that note. 
