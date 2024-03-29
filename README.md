
# DocumentCloud Add-On

[Please see the Add-On documentation](https://github.com/MuckRock/documentcloud-hello-world-addon/wiki/)

# DocumentCloud Klaxon Add-On

This repository contains a DocumentCloud Add-On that replicates the behavior of [Klaxon](https://github.com/themarshallproject/klaxon), which allows you to monitor web pages for changes on sections of the site that might be newsworthy. It allows you to receive email alerts when a modification to the page has been made, and a snapshot of the page is made on the Internet Archive automatically. 

To get started with Klaxon, copy the following code to a new bookmark:

```javascript:(function(){document.body.appendChild(document.createElement('script')).src='https://documentcloud-klaxon.s3.amazonaws.com/inject.js';})();```

and then save it to your bookmarks. Visit a page you want to monitor and click on the bookmarklet to activate Klaxon. 

# Running your own fork of Klaxon

Fork this repository.
Generate [Internet Archive (IA) credentials](https://archive.org/developers/tutorial-get-ia-credentials.html)

Specify your IA credentials as [GitHub repository secrets](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository) in your forked repository.

The secrets should be named:
```SAVEPAGENOW_ACCESS_KEY```
```SAVEPAGENOW_SECRET_KEY```

Enable GitHub action workflows on your fork (navigate to "Actions" on the repository and enable). <br>
Follow the [DocumentCloud Add-On instructions](https://github.com/MuckRock/documentcloud-hello-world-addon/wiki/#run-your-add-on-in-documentcloud) to authorize your forked add-on within DocumentCloud. <br>

At this point, the forked add-on will appear when you browse in DC, but will be identically named; you can distinguish your version by looking at "Created By" when opening the add-on or check the URI fragment. <br>
You can manually configure monitoring tasks from within DC, which will trigger the action to run from your forked repository with your IA credentials. <br>

You will also want to modify the bookmarklet to point to your repository. <br>
Modify bookmarklet/klaxon.html to target your forked add-on when your browser session is redirected to DC to specify the rest of the monitoring parameters.  Just change the repo name on line 171 to yours. <br>

Next, publish the bookmarklet to a publicly accessible location (e.g. S3, GCS, your website) and copy the URL to the resource. <br>
Modify bookmarklet/inject.js to replace all three of the klaxon.html references at the [top of the file](https://github.com/MuckRock/Klaxon/blob/89df26a6ea4433765cc3402c76335b9209cd4e90/bookmarklet/inject.js#L2) with your own. <br>
Publish the Javascript inject.js file to a publcly accessible location. <br>

Finally, modify the bookmarklet code in your browser (or create a separate bookmarklet) to reference your custom inject.js file instead. <br> <br>

Your "Add to Klaxon" bookmarklet will now work the same but send monitoring jobs to your forked add-on instead of the default MuckRock add-on. <br> <br>

# Notes on rate-limiting

Note that Klaxon relies on [savepagenow](https://pypi.org/project/savepagenow/), a Python library that interacts with the Internet Archive's Wayback Machine. Even authenticated requests with savepagenow are limited to [6 requests/minute](https://palewi.re/docs/savepagenow/python.html#authentication). 
The MuckRock version of Klaxon has a higher rate limit, so if you plan on running your own version of Klaxon with a lot of users running hourly checks, you may eventually hit rate limits. 
Klaxon does use [tenacity](https://pypi.org/project/tenacity/) for exponential back-offs on retries for the savepagenow captures, but under high usage, it is still possible for the low default rate limits to cause issues, especially if users are monitoring sites without applying a filter for trivial changes. 

# Acknowledgements 
* The Marshall Project Team (especially Tom Meagher)
* The Internet Archive Team (especially Vangelis Banos & Mark Graham)
* Gregory Foster
