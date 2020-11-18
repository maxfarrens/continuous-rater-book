# Creating the website

Once you are satisfied with how your customized app looks in development mode and want to use it to collect data (via Amazon Mechanical Turk or on local subjects in a lab), you need to build and host the website. 

## Using Netlify

I recommend using [Netlify](https://www.netlify.com/) to host your version of `continuous-rater`. It is free and simple to use. For a more in depth explanation of why this is a good choice (and a list of alternative options) read this [excellent post](https://eshinjolly.com/svelteturk/#/custom-experiments) by Eshin Jolly. 

Start by creating a free account on Netlify. Next, link Netlify with your Github account (if it has not done so automatically). If you do not have a Github, you will need to create one [here](https://github.com/join?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home) for this to work. Netlify works by building a site from a Github repository, so you will also need to ensure your local repository is connected to your account (see below for directions). Once you have setup your Netlify account and have it connected to your Github, click on the **New site from Git** button and then select the **Github** option. 

A new window will pop up, and you should be able to select the `continuous-rater` repository from your Github. After selecting this, you will need to fill in details about **Deploy settings**. Leave the **Owner** field with its default, and ensure that the **Branch to deploy** is set to master (or whatever branch you are working with). Under **Basic build settings** input 

```
npm run build
```
as the **Build command** and 

```
public
```
as the **Publish directory**. Then press **Deploy site** to build your production-ready `continuous-rater` website! 


```{note}
If you visit this site outside of an MTurk context, it will not display any content. This is on purpose: it protects your site and database from undesired participation. Rest assured, if you use this url with MTurk, everything will display properly.  
```

If you want to update your website build, you simply need to push or pull from git, and Netlify will auto-detect changes and rebuild your site (this may take a few minutes). 

In order to execute a git push or git pull after changing and saving files locally, run the following lines in your command line:

```
git add .
git commit -m "YOUR COMMIT MESSAGE HERE"
git push
git pull
```



## Setting up your Github repo

First, ensure that you have git installed on your computer (click [here](https://git-scm.com/downloads) for help with downloads).

Then, `cd` into your local `continuous-rater` folder and run the following line from your command line to initialize it as a new repository and add all current files:

```
git init
git add .
git commit -m "first commit"
```

For more information, see [here](https://kbroman.org/github_tutorial/pages/init.html). After these steps are complete, login to your Github and click the green **New** button to initialize a new remote repository. Name it accordingly, select Public or Private based on your needs and click **Create repository**.

On the next page, follow the second set of instructions, and enter the following lines in your command line (with the relevant parts changed to fit your specific case):

```
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME
git push -u origin master
```
Your repository is now set up and ready to work with Netlify! 