# Blog post draft

We designed and created an example meant to explain how Surfly can be integrated into your own website and most importantly to show the different customisation possibilities it provides.
If you are thinking about using Surfly or are a new user, you can follow this step by step example to understand how Surfly can be integrated and customised.
In particular, we decided to highlight the main features that we offer and show you how you can make Surfly as visible or invisible as you wish depending on your needs and preferences.

#### Cake shop website
We decided to build an example website to illustrate the changes that can be made and the variety of ways in which Surfly can be integrated into a website. We chose to design a website for a cake shop which makes personalized cakes and put emphasis on helping their clients and guiding them through their website.
Here is a screenshot of the home page before we integrate Surfly:

![website](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s1.png)

As you can [see](https://github.com/MathildeJ/Cake_shop_example/commits/master/4cbf4486ecc3e93a442a46488588698466891dbb), it is a standard website with different pages and possible actions. We would now like to add Surfly to this website so that we can use the co-browsing functionality it provides.


#### Integrate Surfly 
First of all, we have to add Surfly to our website. This is easily achieve by logging in to your surfly.com account and navigating to the 'Settings' panel. In the 'Integration' tab, you can then find the snippet code that you need to copy and paste into the source code of your website.
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

![Surfly widget](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s2.png)


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

![widget options 1](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s4.png) ![widget options 2](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s5.png)

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
In particular, we have chosen to use the image of a cake as a get help button for our customers:

![custom button](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s6.png)


#### Build your own landing page
You might have noticed that when a visitor wishes to start a session they are put in a queue and, by default, have to wait for an agent to take the call to be able to navigate within the session. We would like our customer to be aware that they are in the queue (a red banner automatically displays this information) but also for a session to start automatically on our own customised page.

In order to use such a page, we first need to remove the red banner blocking the session:
```
block_until_agent_joins: false, // remove red banner
```
Once this is done, we have to move the snippet code to our landing page (since it will be the page from which sessions start) and to add the auto start option so that a session will start automatically (as soon as the user is redirected to our landing page):
```
auto_start: true, // session will start automatically
```
We already have our own button which starts a Surfly session on the home page but we now what this button to redirect the user to the landing page. We simply need to remove the #surflystart anchor and add an onclick function which does just that:
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
Finally, we need to display the Queue ID on the landing page when a session starts so that the customer is aware that they are in the queue and, in some cases, so that they can communicate it to an agent they were already in contact with (over the phone for example). To do this, we use the REST API to get some information about the session and only keep the data we are interested in (more information on how to use the REST API can be found [here](https://www.surfly.com/cobrowsing-api/)):
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
![landing page](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s7.png)

As you can see above, we now have our own personalised landing page to greet our customers.


#### Session behaviour
We have already managed to integrate Surfly in a way that suits our needs but there are other use cases that we still haven't covered. In particular, when a client places an order while they are in a session, we do not want the agent to be able to see their payment details or to click the 'Order' button for them (if they are in control of the session). 
This can be easily achieved by using some of the built-in options provided with Surfly.

To enable field masking (the follower will not see the leader's input), we can simply add the 'surfly_private' attribute to fields containing sensitive information:
```
<span>Card Number</span>
<input type="text" size="20" data-stripe="number" surfly_private>
```
In our example, we will use this option on the three last fields of our order form as they contain information about the client's card. As can be seen in the image, the agent only see crosses instead of the leader's input:

![field masking](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s8.png)

As for the 'Order' button, we can easily add an eventListener in order to catch the 'surflycontrolchange' event which is fired every time the control is switched within a Surfly session. Then, we check whether or not the leader is in control and disable the order button if they are not.
```
<script>
// when the leader is in control then the 'Order' button is clickable otherwise, it is disabled
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

![survey](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s9.png)


#### Receipt
Finally, we would also like to be able to show a receipt of the order a customer has placed. Therefore, we have to make sure that this information will be passed on even if the client ends the session before getting their receipt. In order to do so, we can use soft session continuation.

The first thing we need to do is add the snippet code to all the pages we wish to transfer cookies from. We also have to set two cookies options to ensure session continuation (including on the landing page): 
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
var info = cookie.split(",");
```
In the gif below, you can see that the order details are available even if the session ends before the client get their receipt:
![receipt](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/receipt.gif)


#### Advanced options
Even though we were able to customise the integration of Surfly, we still would like to make small adjustments so that our website better suit our needs and the needs of our clients.

##### Blacklisting
Indeed, we quickly realised that visitors shouldn't be allowed to access our baking shop page while they are in a Surfly session as it is a separate activity and the agents working for our cake shop are not necessarily qualified to guide our customers through our baking shop.

In order to restrict access to this page (in our case, its path is '/about'), we can use the blacklist option:
```
blacklist: JSON.stringify([{"pattern": ".*/about.*", "redirect": "https://example.com/#restricted"}]),
```
Even though it is optional, we also decided to specify a redirect link so that we can control how the user is going to be informed. More specifically, we chose to redirect the client to the home page with a #restricted hash. We can then simply add a script to implement the desired behaviour: 
```
<script>
    if (window.location.hash == "#restricted"){
    	window.location.href = '/restricted';
    }
</script>  
```
Here, we simply decided to redirect the user to our custom restricted page which informs them that they cannot access this page while they are in a session:

![blacklist](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/blacklist.gif)


##### Queue metadata
We also have repeat customers and would like to be able to give them a more personal experience. In particular, we want to retrieve their login details and pass them on as metadata in the queue so that, for instance, our agents can call them by name.

Firstly, we need to store their information when they log in (in 'metaName' and 'metaEmail') and then we can pass this data by using the 'QUEUE_METADATA_CALLBACK' option:
```
QUEUE_METADATA_CALLBACK: new Function('return {"name": '+sessionStorage.getItem('metaName')+',"email": '+sessionStorage.getItem('metaEmail')+'}'),
```
To know more about the syntax used for this option, click [here](https://www.surfly.com/cobrowsing-api/).

As can be seen below, the agents can directly see this information from the 'Queue' panel:

![metadata](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/metadata.gif)


### Remove the UI
Finally, we wanted to completly strip everything down to co-browsing. Indeed, we realised that, by default, Surfly provides more tools and features than we need. In fact, we are only interested in the co-browsing functionality and, ideally, we wish for Surfly to be completly invisible on our website.

Fortunately, there is an option which remove the Surfly user interface (UI) and therefore allow us to use our own custom elements to control the appearance and feel of the sessions:
```
ui_off: true, // make Surfly invisible
```
##### Exit button
We already have our own start button and landing page but now that we removed the UI, we can't exit a session or use the chat. It's up to us to choose which functionality we want to add to our website and customise the way it will look.

In our example, we chose to create our own exit session button and add it to all the necessary pages. 
First of all, we have to make sure that the page we are adding the button to contains the Surfly widget and then we can simply add our custom button:
```
<button class="button" id="exit_button" style="visibility:hidden" onclick="exitSession()">Exit session</button>
```
Considering that it is an exit button, we do not want it to be shown when the customer is not in a session.  We can easily make sure that the exit button is visible only when there is an on-going Surfly session (and, analogically, we can also control the behaviour of the get help button on the home page):
```
<script>
   if(window.__surfly){
	document.getElementById('exit_button').style.visibility="visible";
	document.getElementById('get_help').style.visibility="hidden";
   } else {
	document.getElementById('exit_button').style.visibility="hidden";
	document.getElementById('get_help').style.visibility="visible";
   }
</script>
```
Finally, we have to define the action triggered by the button that is to say ending a session. To do so, we can once again use the REST API. The first request allows us to retrieve the session ID:
```
<script>
// get session ID
var request = new XMLHttpRequest();
request.open('GET', 'https://api.surfly.com/v2/sessions/?api_key=**your api key**&active_session=true');
request.onreadystatechange = function () {
	if (this.readyState === 4) {
		if(window.__surfly){
			var body = this.responseText;
			var index = body.indexOf("session_id");
			var index_end = body.indexOf("agent_id");
			var id = body.substring(index+13, index_end-3);
			sessionStorage.setItem("session_id", id);
		}
	}
};
request.send();	
</script>
```
Once we've stored the session ID, we can use a second request which will use this information to end the current session:
```
   <script>
    // end session
    function exitSession(){
	var request_end = new XMLHttpRequest();
	request_end.open('DELETE', 'https://api.surfly.com/v2/sessions/'+sessionStorage.getItem("session_id")+'/?api_key=**your api key**');
	request_end.onreadystatechange = function () {
  		if (this.readyState === 4) {
			var body = this.responseText;
			var index = body.indexOf("response");
			// make sure that the session ended
			var success = body.substring(index+11, body.length-2);
			if(success==="Session has been ended successfully"){
				// end the session
				Surfly.endSession('https://example.com');
			}
 		}
	};
	request_end.send();
  }
  </script>
```
![exit button](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s10.png)

Please note: considering how our website is built, there is a unique get help button which means that our customers can only start a session from the home page (by clicking a button which redirects them to the landing page). However, stealth mode is activated by default on all the pages containing the Surfly widget and allows to start a session instantly by pressing Ctrl + Enter.

##### Integrate an already existing chat solution
Finally, we would also like to be able to keeping chatting with our clients in a Surfly session. As a matter of fact, we wish to use Zopim which is the chat solution we were using prior to the integration of Surfly. We can simply add the Zopim snippet code to all the pages of our website and we will be able to communicate with our clients inside and outside of a Surfly session without any disturbance when we enter/exit one:
```
<!-- Adding Zopim Live Chat -->
<script>
	if(!window.__surfly){
	    window.$zopim||(function(d,s){var z=$zopim=function(c){z._.push(c)},$=z.s=
	    d.createElement(s),e=d.getElementsByTagName(s)[0];z.set=function(o){z.set.
	    _.push(o)};z._=[];z.set._=[];$.async=!0;$.setAttribute("charset","utf-8");
	    $.src="//v2.zopim.com/?40l495kTPWFGA7JFrUDzK03KARqCPsNL";z.t=+new Date;$.
	    type="text/javascript";e.parentNode.insertBefore($,e)})(document,"script");  
	}
</script>
<!--End of Zopim Live Chat Script-->
```
You will probably notice that we added a condition in the beginning of the script to make sure that a second Zopim chat window is not opened when a Surfly session starts.
