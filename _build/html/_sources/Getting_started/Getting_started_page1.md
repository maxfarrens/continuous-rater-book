# Downloading the app

Link to Github repo: [github.com/maxfarrens/continuous-rater](https://github.com/maxfarrens/continuous-rater)

1. If you do not already have it installed, install [Node.js](https://nodejs.org/en/)



2. Clone the `continuous-rater` repository locally. Go to your command line and enter:

	```
	git clone https://github.com/maxfarrens/continuous-rater.git
	```
	

3. Install the dependencies using npm (from within the local repository):

	```
	cd continuous-rater
	npm install
	```
	
	
4. Start Rollup:

	```
	npm run dev
	```
	
	
5. Navigate to [localhost:5000](localhost:5000) in your browser of choice. You should see a developer version of your app running. **Initially, it should just display a white screen with no content**. In order to make the app functional for your experiment, you need to set up and link a Firebase backend (see Setting up firebase), and edit some variables in the `src/utils.js` file within the `continuous-rater` repository (see Customizing the app). 

