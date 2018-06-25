---
title: "Blog using Hugo on Github"
date: 2018-06-24T16:59:11-05:00
draft: false
---

### How to Create a Personal Blog on Github using Hugo

Github is a popular free static hosting service for personal and project pages. Hugo is a framework for building websites. More information about Hugo can be got at https://gohugo.io/hosting-and-deployment/hosting-on-github/

The Hugo website also has instructions for creating a personal website on Github. Instructions can be got here https://gohugo.io/hosting-and-deployment/hosting-on-github/

### Step by Step
	
	* Download and intall Git (https://gitforwindows.org/)

	* Sign up for a Github account
	* Create a <BLOG> repository on Github. This repository will contain Hugo's content and other source files.
	* Create a <USERNAME>.github.io repository. This repository will hold the fully rendered version of the Hugo website.
	* Instructions for downloading and installing Hugo can be found at https://gohugo.io/getting-started/installing/#windows
	
Directory Structure:

\Hugo
  |
  |--- bin (contains the Hugo executable)
  |--- Sites
         |--- <BLOG> (contains the blog)
         	    |--- themes (contains the theme folder)
         	    |--- public (the rendered version of the blog)


	* (at the command prompt, change directory 'Sites')
	* Give the following commands:
```
		hugo new site <BLOG>
```
	* Change directory to <BLOG> directory
	* Edit the config.toml file so that
			baseurl = "https://<USERNAME>.github.io/"
	* Edit other parameters as necessary

	


 


