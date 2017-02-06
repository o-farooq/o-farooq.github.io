---
layout: post
title: "Setting up NightScout with Glimp and Abbott freestyle libre using github, pushover and heroku"
date: 2017-02-06 -0800
comments: true
tags: [nightscout, abott freestyle libre, heroku, glimp, pushover, type1 diabetes, T1 diabetes]
---

My son who is type-1 diabetic is turning 5 next week. He is really excited to join school. We are excited too but at the same time worried about his diabetes management. 
We are already using Abott FreeStyle Libre, it is not generally available in New Zealand, getting it through YouShop UK. I had heard about some awesome open source work being 
 done in this space specially OpenAPS and NightScout. Its time to set this up for my son. The process was quite easy but might be little bit tricky 
 for non-technical people. Here I am listing the steps for a non-technical person.
 
 1. Signup for an account at Github if you dont have already one
    Signing up is easy, go to [Github](https://github.com/join) and enter some of your details and choose the free plan
   
    
 2. Signup for an account at [Heroku](https://signup.heroku.com/login). 
 You can select Node.js as primary development language
 
 
 
 3. If you want notifications 
    * Signup for an account at [Pushover](https://pushover.net/login).
    * Once logged in go to home page and note your user key, we will use that later on. 
    
         ![Pushover User Key]({{sie.url}}/images/20170206-push-over-app-key.png)
    
    * Click Create New Application link (https://pushover.net/apps/build) and submit the form after entering the new app name.
    Note the app key.
    
 4. Go to Nightscout repository and click fork 
         ![Forking Nightscout]({{sie.url}}/images/20170206-fork-cgm-remote-monitor.png)
 
 5. Go to forked repository and click on deploy to Heroku link
        ![Deploy to Heroku Link]({{sie.url}}/images/20170206-nightscout-deploy-to-heroku.png)
 
     * Enter app name, the nightscout url depends on that, URL after deployment will be https://{app-name}.herokuapp.com/
     * Enter API Secret, remember this because that is needed in Glimp
     * In enable tab enter "cors careportal"
     * Enter pushover_api_token (app key) and pushover_user_key (user key)
     * Click the deploy button
     * Go to https://{app-name}.herokuapp.com/



 6. In glimp settings (remote monitoring) , enter the herokuapp url and API secret.
    Test the connection.
    
Now when freestyle libre is scanned, you should be able to see in realtime the readings on your nightscout website. More on how to modify different settings in next post :). 
Setting this up by usually would not take more than 1 hour and everything is free.
Thanks Nightscout contributors for making T1s life easy.


	