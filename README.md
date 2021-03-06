# Blog post draft

Surfly's co-browsing technology enables you to share your browser with others. You can start a Surfly session by entering the url of the website you want to browse into the 'start session' panel on your admin page and then invite people to join you by sharing the session url with them. This is the easiest and quickest way to co-browse and doesn't require any configuration. 

However, if you wish to use Surfly as a feature on your own website, you can also add a Surfly button to your website. Simply adding this button already allows you to start co-browsing, and Surfly works without any alterations. If required, small additions to the code allow you to fully customise your session and to use as much, or as little, of Surfly's functionality within your own product. 

If you are thinking about using Surfly, or are a new user, follow our step by step guide as we integrate Surfly into our example application. The major steps and changes are illustrated by an image/gif and, for each modification, you can refer to our repository, where you'll find a corresponding commit.
In this post, we decided to highlight the main features on offer and show you how you can make Surfly as visible or invisible as you wish depending on your needs and preferences.

For further reference, please see [the repository](https://github.com/MathildeJ/Cake_shop_example) containing the code for our example website and the commits made during integration. 

**please note** if you wish to integrate Surfly, you need to give Surfly access to the server. This is especially important when you are developing locally. 

 - [Cake shop website](#website)
 - [Integrate Surfly](#integrate)
 - [Widget options](#widget)
 - [Create your own start button](#start_button)
 - [Build your own landing page](#landing)
 - [Session behaviour](#session)
 - [End of session popup](#popup)
 - [Session continuation](#receipt)
 - [Blacklisting](#blacklist)
 - [Queue metadata](#metadata)
 - [Create your own exit button](#exit_button)
 - [Integrate an already existing chat solution](#chat)
 - [Add a discrete button](#small_button)


<a name="website"></a>
#### Cake shop website

Our example application features a bespoke cake shop, specialising in personalized cakes. The shop prides themselves on their customer service, meaning that the co-browsing feature Surfly provides is an ideal addition to their website, as it can dramatically improve online communication. 
Here is a screenshot of the home page before we integrate Surfly:

![website](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s1.png)

As you can see, it is a standard website with different pages and possible actions. 
We are now going to integrate Surfly into our website, selecting the aspects of Surfly's functionality that best suit our needs.


<a name="integrate"></a>
#### Integrate Surfly 

First, add the Surfly code snippet to your website's source code. To do this, log into your surfly.com account and navigate to the 'Settings' panel. In the 'Integration' tab, you can find the snippet code that you need to copy and paste into the source code of your website.
It should look something like the following:
``` javascript
<script type="text/javascript">(function(){window['_surfly_settings']=window['_surfly_settings']||{
widgetkey:"**your api key**",
/*
add your custom options here
*/
};
var e=document.createElement("script");e.type="text/javascript";e.async=!0;e.src="https://surfly.com/static/js/widget.js";var n=document.getElementsByTagName("script")[0];n.parentNode.insertBefore(e,n); })();</script>
```

You should also specify the domain name of your website in the integrations panel so that you can accept requests made from it.


As you can see below, after adding the widget code to our website, we see a red "get live help" button.  This button is shown when an agent is logged in, and, when clicked, allows us to start a session. Surfly works straight away: we can instantly start a session and receive calls without any further configuration required. 

![Surfly widget](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s2.png)

When a client clicks on the red "get live help" button, the client is queue'd until an agent joins the session. The agent will be able to see the list of queue'd users in the Queue panel on the Surfly admin page.

![user queue'd](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s3.png)


<a name="widget"></a>
#### Widget options

The default red Surfly button doesn't match our website design, so we'd prefer to use our own theme color. You can do this by setting a few options in the Surfly widget code.
In our case, we simply used a few custom options:
``` javascript
drawing_mode: "permanent", // change drawing mode so that the drawings last
chat_box_color: "#87cefa", // change color of chat box so that it suits our website's theme
theme_font_background: "#87cefa", // change color of button 
videochat: false // remove videochat feature (not needed)
```
In the images below, you can see that the button and the chat box are now in our website's theme color. We also chose to disable the video chat feature that is included by default, as we felt that it was not required. Finally, we decided to make the drawings permanent to facilitate communication.

![widget options 1](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s4.png) ![widget options 2](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s5.png)

The API has an [extensive list of widget options](https://www.surfly.com/cobrowsing-api/).


<a name="start_button"></a>
#### Create your own button

Even though Surfly is now customised to our needs and preferences, we'd like to create our own button to start a co-browsing session so that we can customise it and control its behaviour more easily.

First, we need to hide the default button, as we'll be using our own. To do this, set the 'hidden' option to 'true':
``` javascript
hidden: true, // hide Surfly's default button
```
Then, we add the #surflystart anchor to our custom button (get_help_button in our example):
``` javascript
<a href="#surflystart"> <button class="button" id="get_help_button"></button></a>
```
In particular, we have chosen to use the image of a cake as a get help button for our customers:

![custom button](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s6-zoom.png)


<a name="landing"></a>
#### Build your own landing page

When a visitor initiates a session they are queue'd and, by default, have to wait for an agent to take the call before they can navigate within the session. The screen is blocked, and a red banner appears at the top of the screen with their queue pin. In our example application, the page that the user starts a session from is the home page, and consequently, this is the page that gets blocked. We would prefer to have our own custom landing page, where we can tell our customers that they are in the queue, and that an agent will be with them soon. 

In order to use such a page, we first remove the red banner blocking the session:
``` javascript
block_until_agent_joins: false, // remove red banner
```
Next, we move the snippet code to our landing page (since it will be the page from which sessions start) and add the auto start option so that a session will start automatically (as soon as the user is redirected to our landing page):
``` javascript
auto_start: true, // session will start automatically
```
We now want our button to redirect the user to the landing page. We simply replace the #surflystart anchor with an onclick function that does just that:
``` javascript
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
Finally, we want to display the queue ID on the landing page when a session starts. This is so that the customer is aware that they're in the queue and, in some cases, so that they can communicate the ID to an agent that they were already in contact with (over the phone for example). The agent will then be able to find the customer on the queue page, and join their session. To do this, we use the REST API to get information about the session, keeping only the data we are interested in (more information on how to use the REST API can be found in our [API](https://www.surfly.com/cobrowsing-api/) ):
``` javascript
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
![landing page](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/landing_page_flow.gif)

We now have our own personalised landing page to greet our customers.


<a name="session"></a>
#### Session behaviour

We have integrated Surfly to our satisfaction, but there are other use cases that we still haven't covered. In particular, when a client places an order during a session, we don't want the agent to be able to see their payment details or to click the 'Order' button for them. 
You can do this by using some of the built-in options provided with Surfly.

To enable field masking (the follower will not see the leader's input), add the 'surfly_private' attribute to fields containing sensitive information:
``` javascript
<span>Card Number</span>
<input type="text" size="20" data-stripe="number" surfly_private>
```
In our example, we will use field masking on the last three fields of our order form as they contain information about the client's card. As can be seen in the image, the agent only see crosses instead of the leader's input:

![field masking](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s8.png)

As for the 'Order' button, we can easily add an eventListener in order to catch the 'surflycontrolchange' event which is fired every time the control is switched within a Surfly session. Then, we check whether or not the leader is in control and disable the order button if they are not.
``` javascript
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


<a name="popup"></a>
#### End of session popup

Since customer service is very important to us, we'd also like to be able to ask for feedback at the end of a session so that we can improve our website and offer the smoothest co-browsing experience to our clients.
We use the 'end_of_session_popup_url' option to point to the url of our survey page:
``` javascript
end_of_session_popup_url: "https://example.com/survey",
```

Please note: you might need to set the 'hidden' option to 'false' for this option to work correctly

![survey](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/s9.png)


#### Advanced options

We still would like to make some small adjustments so that our website better suits our needs and the needs of our clients.

<a name="receipt"></a>
#### Session continuation

Finally, we want to show the customer their receipt. Therefore, we have to make sure that their order information will be passed on, even if the client ends the session before getting their receipt. In order to do so, we can use soft session continuation.

We need to add the snippet code to all the pages we wish to transfer cookies from. We also have to set two cookie options to ensure session continuation (including on the landing page): 
``` javascript
<script type="text/javascript">(function(){window['_surfly_settings']=window['_surfly_settings']||{
widgetkey:"**your api key**",
hidden: true,
cookie_transfer_enabled: true,
cookie_transfer_proxying: false
};
var e=document.createElement("script");e.type="text/javascript";e.async=!0;e.src="https://surfly.com/static/js/widget.js";var n=document.getElementsByTagName("script")[0];n.parentNode.insertBefore(e,n); })();</script>
```
After that, we need to set the cookies when we submit the form. In our example, we chose to only store the name, email and address of the client:
``` javascript
document.getElementById("order").submit();
var get_name = document.getElementById("name").value;
var get_email = document.getElementById("email").value;
var get_address = document.getElementById("address").value;
document.cookie = 'order: name='+get_name+',email='+get_email+',address='+get_address;
```
Finally, we have to get the cookies and display the retrieved data (parsed according to the syntax previously used when we set the cookies):
``` javascript
var cookie = document.cookie;
var index_order = cookie.indexOf("order");
cookie = cookie.substring(index_order, cookie.length);
var info = cookie.split(",");
```
In the gif below, you can see that the order details are available even if the session ends before the client get their receipt:

![receipt](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/receipt.gif)


<a name="blacklist"></a>
##### Blacklisting

We quickly realised that visitors shouldn't be allowed to access our baking shop page while they're in a Surfly session as it's a separate activity, and the agents working for our cake shop aren't necessarily qualified to guide our customers through our baking shop.

In order to restrict access to this page (in our case, its path is '/about'), we can use the blacklist option:
``` javascript
blacklist: JSON.stringify([{"pattern": ".*/about.*", "redirect": "https://example.com/#restricted"}]),
```
We also decided to specify an optional redirect link so that we can design our own restricted page. More specifically, we chose to redirect the client to the home page with a #restricted hash. We can then add a script to implement the desired behaviour: 
``` javascript
<script>
    if (window.location.hash == "#restricted"){
    	window.location.href = '/restricted';
    }
</script>
```
In our example, we decided to redirect the user to our custom restricted page which informs them that this page is restricted:

![blacklist](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/blacklist.gif)


<a name="metadata"></a>
##### Queue metadata

We'd like to be able to give our repeat customers a more personal experience. More specifically, we want to retrieve their login details and pass them on as metadata in the queue so that, for instance, our agents can greet them by name.

Firstly, we need to store their information when they log in (in 'metaName' and 'metaEmail') and then we can pass this data by using the 'QUEUE_METADATA_CALLBACK' option:
``` javascript
QUEUE_METADATA_CALLBACK: new Function('return {"name": '+sessionStorage.getItem('metaName')+',"email": '+sessionStorage.getItem('metaEmail')+'}'),
```
To know more about the syntax used for this option, see our [API](https://www.surfly.com/cobrowsing-api/).

As can be seen below, the agents can directly see this information from the 'Queue' panel:

![metadata](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/metadata.gif)


### Remove the UI

Finally, we wanted to completely strip everything down to co-browsing. By default, Surfly provides more tools and features than our example application needs. In fact, we're only interested in the co-browsing functionality and, ideally, we wish for Surfly to be completly invisible on our website.

Fortunately, there's an option which removes the Surfly user interface (UI) and therefore allows us to use our own custom elements to control the appearance and feel of the sessions:
``` javascript
ui_off: true, // make Surfly invisible
```


<a name="exit_button"></a>
##### Create your own exit button

We already have our own start button and landing page, but now that we have removed the UI, we can't exit a session or use the chat. It's up to us to choose which functionality we want to add to our website and customise the way it will look.

In our example, we chose to create our own exit session button and add it to all the necessary pages. 
First, we have to make sure that the page we are adding the button to contains the Surfly widget and then we can add our custom button:
``` javascript
<button class="button" id="exit_button" style="visibility:hidden" onclick="exitSession()">Exit session</button>
```
Considering that it's an exit button, we don't want it to be shown when the customer isn't in a session.  We can easily make sure that the exit button is visible only when there's an on-going Surfly session (in a similar manner, we can also control the behaviour of the "get help" button on the home page):
``` javascript
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
Finally, we define the action triggered by the button, in this case, ending a Surfly session. To do so, we can once again use the REST API. The first request allows us to retrieve the session ID (which we store so that it's accessible from all the pages):
``` javascript
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
``` javascript
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

Please note: considering how our website is built, there's a unique "get help" button which means that our customers can only start a session from the home page (by clicking a button which redirects them to the landing page). However, stealth mode is activated by default on all the pages containing the Surfly widget and allows to start a session instantly by pressing Ctrl + Enter. Stealth mode can also be disabled, if you prefer.


<a name="chat"></a>
##### Integrate an already existing chat solution

Finally, we'd also like to be able to continue chatting with our clients in a Surfly session. In our application, we were using Zopim prior to integrating Surfly. We can simply add the Zopim snippet code to all the pages of our website and we will be able to communicate with our clients inside and outside of a Surfly session without any disturbance when we enter/exit one:
``` javascript
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
We added a condition in the beginning of the script to make sure that a second Zopim chat window doesn't open when a Surfly session starts.


<a name="small_button"></a>
##### Adding a discrete button 

Adding Zopim to our website has made text chat the primary method of communication. Therefore, we no longer want our customers to start a Surfly session themselves, but rather that an agent directs them to one.  We decided to remove the landing page, and to add a smaller icon to the footer of our webpage. If necessary, the agent can direct the user to start a session by clicking on the small cake icon. 

![cake icon](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/cake_icon.png)

As you can see from the code below, by adding the #surflystart anchor, we ensure that a Surfly session starts when this icon is clicked:

``` javascript

<p id="idP"><a href="#surflystart"><img src="../static/cakeicon.png" id="showId"></a></p>

```

We then retrieve the queue ID and display it to the user:


``` javascript
        if(window.__surfly){
        // first check if a session has started (meaning that the icon has been clicked on)
            var request = new XMLHttpRequest();
	    request.open('GET', 'https://api.surfly.com/v2/sessions/?api_key=**your api key**&active_session=true');
            request.onreadystatechange = function () {
	        if (this.readyState === 4) {
		    var body = this.responseText; 
		    // we extract the queue_id from the string we get from the request
		    var index = body.indexOf("queue_id");
		    var id = body.substring(index+10, index+14);
                    // we hide the cake icon
                    document.getElementById("showId").style.visibility='hidden';  
                    var textId = document.createTextNode(id);
                    // replace the cake icon with the session id number
                    document.getElementById("idP").appendChild(textId);
	        } 
       	    }
	 request.send();
        };
```

The user tells the agent this ID, and the agent can use it to identify the correct customer in the queue. The co-browsing session will start, and they can continue talking via Zopim. 

![Retrieved session id](https://github.com/MathildeJ/Fantasy_Bakes/blob/master/static/session_id_number.png)
