# Blog post draft

We designed and created an example meant to explain how Surfly can be integrated into your own website and most importantly to show the different customisation possibilities it provides.
If you are thinking about using Surfly or are a new user, you can follow this step by step example to understand how Surfly can be used.
In particular, we decided to highlight the main features that we offer and show you how you can male Surfly as visible or invinsible as you wish depending on your needs and preferences.

#### Cake shop website
We decided to build an example website to illustrate the changes that can be made and the variety of ways in which Surfly can be integrated into a website. We chose to design a website for a cake shop which makes personalized cakes and put emphasis on helping their clients and guiding them through the website.
Here are a few screenshots of the website before we integrate Surfly:


As you can see, it is a standard website with different pages and possible actions. We would now like to add Surfly to this website so that we can use the co-browsing functionality it provides.


#### Integrate Surfly 
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


#### Widget options
We are now able to start co-browsing sessions but the overall design doesn't really suit our website. Indeed, the bright red theme color of Surfly stands out and we would prefer to use our own theme color. This can be easily achieved by setting a few options in the widget we previously added.
In our case, we simply used a few custom options:
```
drawing_mode: "permanent", // change drawing mode so that the drawings last
chat_box_color: "#87cefa", // change color of chat box so that it suits our website's theme
theme_font_background: "#87cefa", // change color of button 
videochat: false // remove videochat feature (not needed)
```
On the image below, you can now see that the button and the chat box are in our website's theme color. We also chose to disable the video chat feature that is included by default because we have no use for it. Finally, we decided to make the drawings permanent to facilitate communication.
You can find an extensive list of widget options [here](https://www.surfly.com/cobrowsing-api/).


#### Create your own button
Even though Surfly is now customised to our needs and preferences, we would like to create our own button to start a co-browsing session so that we can customise it and control its behaviour more easily.
First, we have to hide the default button since we will use our own. To do so, we simply set the 'hidden' option to 'true':
```
hidden: true, // hide Surfly's default button
```
Then, we simply need to add the #surflystart anchor to our custom button (get_help_button in our example):
```
<a href="#surflystart"> <button class="button" id="get_help_button"></button></a>
```
For instance, we have chosen to use the image of a cake as a get help button for our customers:

#### Build your own landing page
You might have noticed that when a visitor wishes to start a session they are put in a queue and, by default, have to wait for an agent to take the call to be able to navigate within the session. We would like our customer to be aware that they are in the queue (a red banner automatically displays this information) but also for a session to start automatically on our own customised page.

In order to use such a page, we first need to remove the red banner blocking the session:
```
block_until_agent_joins: false, // remove red banner
```
Once this is done, we have to move the snippet code to our landing page (since it will be the page from which sessions start) and to add the auto start option so that a session will start automatically as soon as the button is clicked:
```
auto_start: true, // session will start automatically
```
We already have our own button which starts a Surfly session from the landing page so we simply need to remove the #surflystart anchor and add an onclick function which redirects the user to our new landing page:
```
<button class="button" id="get_help_button" onclick="landing()"></button>

<script>
    // the get help button redirects the user to the landing page (if they're not already in a session)
    function landing(){
	if(!window.__surfly){
		window.location.href = '/landing_page';
	}
    }
</script>
```
Finally, we need to display the Queue ID on the landing page when a session starts so that the customer is aware that they are in the queue and, in some cases, so that they can communicate it to an agent they were already in contact with (over the phone for example). To do this, we use the REST API to get some information about the session (more information on how to use the REST API can be found [here](https://www.surfly.com/cobrowsing-api/)):
```
<script>
 	// using the REST API to get information about the session
	var request = new XMLHttpRequest();

	request.open('GET', 'https://api.surfly.com/v2/sessions/?api_key=**your api key**&active_session=true');

	request.onreadystatechange = function () {
	  if (this.readyState === 4) {
	    if(window.__surfly){
		    var body = this.responseText; 
		    // we extract the queue_id from the string we get from the request
		    var index = body.indexOf("queue_id");
		    var id = body.substring(index+10, index+14);
		    // we display this id on the button
		    document.getElementById("id_button").innerHTML=id;
	    }
	  }
	};

	request.send();
</script>
```


As you can see above, we now have our own personalised landing page to greet our customers.


#### Session behaviour
We have already managed to integrate Surfly in a way that suit our needs but there are other use cases that we still haven't covered. In particular, when a client places an order while they are in a session, we do not want the agent to be able to see their payment details or to click the 'Order' button for them if they are in control of the session. 
This can be easily achieved by using some of the built-in options provided with Surfly.

To enable field masking (the follower will not see the leader's input), we can simply add the 'surfly_private' attribute to fields containing sensitive information:
```
<span>Card Number</span>
<input type="text" size="20" data-stripe="number" surfly_private>
```
In our example, we will use this option on the three last fields of our order form as they contain information about the client's card.


As for the 'Order' button, we can easily add an eventListener in order to catch the 'surflycontrolchange' event which is fired every time the control is switched within a Surfly session. Then, we check whether or not the leader is in control and disable the order button if they are not.
```
<script>
// when the leader is in control then the 'buy' button is clickable otherwise, it is disabled
window.addEventListener('surflycontrolchange', function (event) {
    var element = document.getElementById("order_button");
    if (event.leaderHasControl) {
        element.disabled = false;
    } else {
        element.disabled = true;
    }
});
</script>
```


#### Ask for feedback
Since customer service is very important to us, we also would like to be able to ask for feedback at the end of a session so that we can improve our website and offer the smoothest co-browsing experience to our clients.
We have already created a survey page so we only needed to use the 'end_of_session_popup_url' option:
```
end_of_session_popup_url: "https://example.com/survey",
```
Please note: you might need to set the 'hidden' option to 'false' for this option to work correctly




#### Receipt
Finally, we would also like to be able to show a receipt of the order a customer has placed. Therefore, we have to make sure that this information will be passed on even if the client ends the session before getting their receipt. In order to do so, we can use soft session continuation.

The first thing we need to do is add the snippet code to all the pages we wish to transfer cookies from. We also have to set two cookies options to ensure session continuation:
```
<script type="text/javascript">(function(){window['_surfly_settings']=window['_surfly_settings']||{
widgetkey:"**your api key**",
hidden: true,
cookie_transfer_enabled: true,
cookie_transfer_proxying: false
};
var e=document.createElement("script");e.type="text/javascript";e.async=!0;e.src="https://surfly.com/static/js/widget.js";var n=document.getElementsByTagName("script")[0];n.parentNode.insertBefore(e,n); })();</script>
```
After that, we need to set the cookies when we submit the form. In our example, we chose to only store the name, email and address of the client:
```
document.getElementById("order").submit();
var get_name = document.getElementById("name").value;
var get_email = document.getElementById("email").value;
var get_address = document.getElementById("address").value;
document.cookie = 'order: name='+get_name+',email='+get_email+',address='+get_address;
```
Finally, we have to get the cookies and display the retrieved data (parsed according to the syntax previously used when we set the cookies):
```
var cookie = document.cookie;
var index_order = cookie.indexOf("order");
cookie = cookie.substring(index_order, cookie.length);
cookie = cookie.substring(7, cookie.indexOf(";"));
var info = cookie.split(",");
```




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
