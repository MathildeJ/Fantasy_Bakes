# Blog post draft

We designed and created an example meant to explain how Surfly can be integrated into your own website and most importantly to show the different customisation possibilities it provides.
If you are thinking about using Surfly or are a new user, you can follow this step by step example to understand how Surfly can be used.
In particular, we decided to highlight the main features that we offer and show you how you can male Surfly as visible or invinsible as you wish depending on your needs and preferences.

#### First step: our website
We decided to build an example website to illustrate the changes that can be made and the variety of ways in which Surfly can be integrated into a website. We chose to design a website for a cake shop which makes personalized cakes and put emphasis on helping their clients and guiding them through the website.
Here are a few screenshots of the website before we integrate Surfly:



As you can see, it it a standard website with different pages and possible actions. We would now like to add Surfly to this website so that we can use the co-browsing functionality it provides.


#### Second step: adding Surfly 
First of all, we have to add Surfly to our website. This is easily achieve by logging in to your surfly.com account and navigating to the 'Settings' panel. You can then find the snippet code that you need to copy and paste into the source code of your website in the 'Integration' tab.
It should look something like the following:
```
<script type="text/javascript">(function(){window['_surfly_settings']=window['_surfly_settings']||{
widgetkey:"**your api key**",
/*
add your custom options here
*/
};
var e=document.createElement("script");e.type="text/javascript";e.async=!0;e.src="https://surfly.com/static/js/widget.js";var n=document.getElementsByTagName("script")[0];n.parentNode.insertBefore(e,n); })();</script>
```

You should also specify the domain name of your website so that you can accept requests made from it.

As you can see below, a red button appeared and allows us to start a session. Surfly works straight away: we can instantly start a session and receive calls without any further configuration required. 

#### Third step: widget options
We are now able to start co-browsing sessions but the overall design doesn't really suit our website. Indeed, the bright red theme color of Surfly stands out and we would prefer to use our own theme color. This can be easily achieved by setting a few options in the widget we previously added.
In our case, we simply used a few custom options:
```
 drawing_mode: "permanent", // change drawing mode so that the drawings last
	chat_box_color: "#87cefa", // change color of chat box so that it suits our website's theme
	theme_font_background: "#87cefa", // change color of button 
	videochat: false // remove videochat feature (not needed)
	
```
On the image below, you can now see that the button and the chat box are in our website's theme color. We also chose to disable the video chat feature that is included by default because we have no use for it. Finally, we decided to make the drawings permanent to facilitate communication.
You can find an extensive list of widget options here(https://www.surfly.com/cobrowsing-api/).

#### Fourth step: creating your own button
Even though Surfly is now customised to our needs and preferences, we would like to create our own button to start a co-browsing session so that we can customise it and control its behaviour more easily.

It doesnt sit as well on the website as they wanted so they experiment with the colors and options
They then decide to remove the button entirley and have people use a cake instead
 
Second step:
 - change color of chatbox/ drawing mode/ disable a few things **third commit - comments detailing exact changes** 
 - then create own button (a cake?) **fourth commit**

They decide that the red banner is unsuitable, so remove it and then create a landing page, with a popup. They say that some of their clients still ask for help over the phone, so an agent can direct them to a Surfly session and then join them with the session id from the 'start' panel in the admin page. 

Third step 
 - remove the red banner **fith commit**
 - create landing page with own popup window (cake and name of cake shop, give a session id - if on the phone to the agent please pass this over to them now, otherwise please wait for an agent to join your session - automatically close popup when follower joins?) **sixth commit**

Then we decide, we are happy with the integration, and want to test it, so enter Mrs flour and Mrs Frosting. Mrs Frosting starts a session, and then Mrs Flour joins. During the session, the developers notice a few things they want to change when Mrs Frosting placed her order. 

Fourth step (ordering a cake in the session)
 - show a form -> the agent can see payment details - enable field masking **seventh commit** 
 - disable the buy button **eigth commit**

Then they want feedback from the session, so they add a popup window, and Mrs Frosting fills this in, 
explain the changes to the widget. 

Fith step 
 - show a popup and form at the end for feedback (and then show the results on another page) **ninth commit**

Finally, we retrieve Mrs Frostings order after the popup has closed. 
 
Sixth step
 - we want to show a reciept at the end -> use session continuation **tenth commit**

#### Advanced options

The developers are happy with the flow and wantto make some small adjustments - they decide to log the sales made by Mrs FLour. (maybe they work on commission). 

Seventh step
- logging sales made by the agent (surfly.log()) -> write as csv to an excel spreadsheet **eleventh commit**

Realise that they need to restrict access to certain pages

Eigth step
 - add a blacklist for a seperate business (baking shop with ingredients) - maybe the agents are only working for the cake shop and don't understand the baking side so well **twelfth commit**

Then they have repeat customers/ loyal customers that they want to give a more personal experience to -> these customers have login details and pass names and email data to the agent(s). 

Ninth step 
 - add metadata **thirteenth commit**

At this point they decide that the functionality is not needed, and strip everything down to purely co-browsing

Final step

- We realsied that we don't need all the functionality e.g document sharing, so we will instead integrate zoopim, and just have the co-browsing functionality. Also allows us to have own control option/ exit button. 

 - totally invisable 
 - remove UI and put own chat box and buttons **fourteenth commit** 
 - show stealth mode (don't even need a button) remove everything. (not a commit, just demonstration)
 - Create exit button **fifteenth commit**
 - Integrate Zoopim **sixteenth commit**
