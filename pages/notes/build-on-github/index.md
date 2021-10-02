For the last year this site has been working on a simple platform: statics, GitHub and my domain. All the pages were pre-generated and placed in a GitHub's [repository](https://github.com/vkostyanetsky/kostyanetsky.me-static) with [GitHub Pages](https://pages.github.com/) enabled.

I collected statics using a script. It was working with a list of text files, pedantically arranged in folders. The script rummaged through them and generated HTML files. Then I manually pushed them to the repository.

This scheme worked well, but the number of clicks annoyed me. Here is the script pull, and there is the script pull, and then you have to tinker with the git. I wanted it to be simpler.

At some point, I figured out that not only the deployment of statics, but also the assembly can be shifted onto the shoulders of GitHub. I put together some thoughts and added two more repositories:

1. A [repository](https://github.com/vkostyanetsky/kostyanetsky.me) of initial data. Here I put the content of the site: the text files and a bit of metadata (page titles, dates of pages creation, tags for notes, and so on).
2. A [repository](https://github.com/vkostyanetsky/BlogBuilder) of the script for generating statics. In addition to the script itself, I put various assets here (icons, styles, manifests — in general, everything that doesn't need to be generated every time, but you have to “put” it next to the resulting HTML).

Then I wrote an [action](https://github.com/vkostyanetsky/kostyanetsky.me/blob/main/.github/workflows/main.yml) that wakes up with every push to the repository with the initial data. Shortly, its logic:

1. Clone the repository with statics and the repository with the generating script;
2. Update the repository with statics using the generating script;
3. Push changes of the repository with static to the main branch;
4. Notify the owner (me) via Telegram.

Voilà! Now, whenever the repository with the original data changes, the GitHub immediately (well, as soon as possible — within a minute) updates the repository with the site and deploys it via GitHub Pages. A bonus is a web interface to manage the site (in fact, the GitHub site). No Code. :-)

While I'm at it, I added links for editing pages directly to the site (the pencil icon in the upper-right corner). This is intended as a convenience for me, but anyone who finds a typo can send a PR. Thank you in advance!