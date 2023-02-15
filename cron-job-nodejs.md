# Membuat Aplikasi Cronjob menggunakan NodeJS
1.  Create a NodeJs application
    ```
    npm init
    
    npm i express
    ```

2. Set up the Express server

    Create the `app.js` file (note that some people prefer to call this file server.js Configure the port and set up the listener

    ```
	const express = require("express");
	const app = express();
	app.set("port", process.env.PORT || 3000);

	app.listen(app.get("port"), () => {
	  console.log("Express server listening on port " + app.get("port"));
	});
    ```


3. Add the node-cron npm package to the application
    ```
    npm i node-cron
    ```
4. Set up a folder for your scheduled jobs

   Create a folder called "scheduledFunctions" or similar


5. Define the logic to run your scheduled jobs
   
    In the folder you just set up, create a file to hold your scheduled function logic. You can also use different files for each task if you prefer.

    Configure scheduled logic:
    ```
    const CronJob = require("node-cron");

	exports.initScheduledJobs = () => {
	  const scheduledJobFunction = CronJob.schedule("*/5 * * * *", () => {
	    console.log("I'm executed on a schedule!");
	    // Add your custom logic here
	  });

	  scheduledJobFunction.start();
	}
    ```

    The execution frequency of your custom function is defined as the first argument to the Cron.schedule function. Note that this argument is written as a Cron Schedule Expression or crontab. I find Crontab Guru a really useful resource to play around and get comfortable with schedule expressions so you can easily set up your own.


6. Configure the Express app to initialise and run the job schedules
    In the app.js file add a call to the init function(s) from your scheduled file(s).

    ```
	const express = require("express");
	// DEFINE the path to your scheduled function(s)
	const scheduledFunctions = require('./scheduledFunctions');
	const app = express();
	app.set("port", process.env.PORT || 3000);

	// ADD CALL to execute your function(s)
	scheduledFunctions.initScheduledJobs();

	app.listen(app.get("port"), () => {
	  console.log("Express server listening on port " + app.get("port"));
	});

    ```

    Start the app to generate the functions and their schedules in the Cron scheduler. Your custom logic will be executed to schedule as long as the app is running.


Reference: 

- https://www.mickpatterson.com.au/blog/how-to-setup-scheduled-functions-with-nodejs-express-and-node-cron

- https://medium.com/javascript-indonesia-community/penggunaan-cron-jobs-di-node-js-c986bbaaf7f2
