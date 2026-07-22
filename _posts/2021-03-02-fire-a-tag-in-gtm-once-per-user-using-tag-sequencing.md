---
title: "Fire a tag in GTM once per user, using tag sequencing"
date: 2021-03-02 14:32:00.006
tags: ["gtm", "tag sequencing", "google tag manager"]
category: professional
render_with_liquid: false
---

  

Last week the marketing guys asked me to include a Floodlight pixel on the website. From now everything looks fine, but then taking to the agency, they told me to fire the pixel only once per user.

First thing that came to my mind was “why they need me to fire it once per user if we can handle it in the Floodlight built-in pixel itself?”

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjh-NizIoELLlK0XVMe95BoyZbwGNQfl5ki3GhFM5kyIth4kLmKGeQvvp-q3hdyQ4BfiLvO_OSAYZ9rDGXNqZiGO_-tjZte4fEjNBJx1DdOnz5LdsyQ1TAFhlJC-tFtqkUb7pEO/w400-h366/Picture+1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjh-NizIoELLlK0XVMe95BoyZbwGNQfl5ki3GhFM5kyIth4kLmKGeQvvp-q3hdyQ4BfiLvO_OSAYZ9rDGXNqZiGO_-tjZte4fEjNBJx1DdOnz5LdsyQ1TAFhlJC-tFtqkUb7pEO/s941/Picture+1.png)

  

But then, the agency said this feature only has a 24 hour’s timeframe. OK, don’t panic. We have the Google Analytics cookie (\_ga by default) so my plan B was: Check if the user has the GA cookie and if not, fire the tag.

However, life is never like is planned and there are other dependencies, in my case, the Consent Management Platform. Let me give you summarize background to explain you how it works.

First time, user lands on the site, it gets the cookie disclaimer. If the user consent, then an event, let’s call it “consent” allows you to fire the pixels from the page.

So, my problem was that at the time the pixel should fire, I already had the \_ga cookie, so I couldn’t evaluate if it was a new user or not. Then I saw [this article from Simo Ahava](https://www.simoahava.com/gtm-tips/check-for-new-user/) and got the update from dusoft and Stephen Harris talking about the set of the dataLayer, try to make it work but it didn’t.

However, I started to spike about the set method of the dataLayer and ended [in this article of Dan Wilkerson](https://www.bounteous.com/insights/2016/03/21/removing-values-datalayer-data-layer-best-practices-pt-4/) written more than 5 years ago with this beautiful code:

<script>

(function(window) {

var gtm = window.google\_tag\_manager[{{Container ID}}];

try {

gtm.dataLayer.set('balloonsPopped', undefined);

// Notifies GTM our HTML Tag has finished

gtm.onHtmlSuccess({{HTML ID}});

} catch(e) {

if({{Debug Mode}}) throw e;

// Notifies GTM our HTML Tag has failed

return gtm.onHtmlFailure({{HTML ID}});

}

})(window);

</script>

And this is where I saw the light: maybe I can’t control when something is going to fire because is asynchronous, but I can say to GTM if the pixel has succeeded or it has failed.

OK, so let’s get into the hot:

First we create the variable for reading the Google Analytics cookie (\_ga):

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiP1umfHrX3Al8bKsbcEVIHvVXu7phEDB7GoDFivS1XPRAccdMYVz8mYe0tVFaHT0pUR3dS3bWrb4pGgkW1v5eWIhNSEE-3Ec3UHjjVPBZMjoIo4JP6_exs3wWje1BD3RBFSl8p/w640-h354/Picture+15.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiP1umfHrX3Al8bKsbcEVIHvVXu7phEDB7GoDFivS1XPRAccdMYVz8mYe0tVFaHT0pUR3dS3bWrb4pGgkW1v5eWIhNSEE-3Ec3UHjjVPBZMjoIo4JP6_exs3wWje1BD3RBFSl8p/s941/Picture+15.png)

  

Then what I did was to create a custom HTML tag for evaluating if the \_ga exists, when the GTM is loaded, and therefore, before the “consent” event happens.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhmcNvOLXaCloSZQbSIzryhM_rWpGmO1oSxN4XToYpLBy7rEuXuWRaDS9vcaO9nEiasFatjjxBfH1M8HSC8n8pX6P_KU_jXnEmwhWFyL8fOQyxw8sBcZd4fK4S5YyaBAnWukdaU/w640-h468/Picture+2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhmcNvOLXaCloSZQbSIzryhM_rWpGmO1oSxN4XToYpLBy7rEuXuWRaDS9vcaO9nEiasFatjjxBfH1M8HSC8n8pX6P_KU_jXnEmwhWFyL8fOQyxw8sBcZd4fK4S5YyaBAnWukdaU/s941/Picture+2.png)

This is the code

<script>

(function() {

var gtm = window.google\_tag\_manager[{{Container ID}}];

try {

if ({{Cookie - \_ga}}) {

// if \_ga exists is NOT a new user and we fail the tag

gtm.onHtmlFailure({{HTML ID}});

return false;

} else {

// if \_ga does NOT exist is a new user and he tag succeed

gtm.onHtmlSuccess({{HTML ID}});

return true;

}

} catch(e) {

if({{Debug Mode}}) throw e;

return gtm.onHtmlFailure({{HTML ID}});

}

})();

</script>

And this is when the GTM container loads

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhNyzoTcmbFSoChuT9JEyjSP-LxbHYx29XSJMv0NMbTAbH6rRXA1thz2hTmjm-57TaglWZwyNx4VU5eO2sTr-fY4C7xmQ5hSzD6uy0zXVoZQbNawntTNQoXR7gxoTRPCqQ3iAq_/w640-h294/Picture+3.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhNyzoTcmbFSoChuT9JEyjSP-LxbHYx29XSJMv0NMbTAbH6rRXA1thz2hTmjm-57TaglWZwyNx4VU5eO2sTr-fY4C7xmQ5hSzD6uy0zXVoZQbNawntTNQoXR7gxoTRPCqQ3iAq_/s941/Picture+3.png)

Reader, one advice here: PLEASE, USE THE F\*CKING NOTES on GTM to help your mates.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhaAlSsC8n_mZvLnINWD4XcdrKOUUsXieBOsYoTWbFpKpGQlf8pBHb2vIPECnzgzhzcqckeTtU4x4Y-_S-tegMCGgmtpa5jWGpJ-1wHgXBoWkyWOyi8vLTp80EOY4pwAA4TqSvf/w640-h420/Picture+4.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhaAlSsC8n_mZvLnINWD4XcdrKOUUsXieBOsYoTWbFpKpGQlf8pBHb2vIPECnzgzhzcqckeTtU4x4Y-_S-tegMCGgmtpa5jWGpJ-1wHgXBoWkyWOyi8vLTp80EOY4pwAA4TqSvf/s941/Picture+4.png)

  

Then, once we have the 2 tags set up, we have to arrange the sequence of firing. This is where Tag sequencing come into scene:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgwD23UCV8v0IqWiceUNQzfZa14GPw8xzyEjvRtA7Z2qCTz3SEfXk_DGfIwAd-tzIpTLRcBdEJR3nneHkX9txuQ9-p7jFAzOI0TrRfkw5_KoawC2pMWurtF08v1yAXHzgsjSfw0/w640-h444/Picture+5.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgwD23UCV8v0IqWiceUNQzfZa14GPw8xzyEjvRtA7Z2qCTz3SEfXk_DGfIwAd-tzIpTLRcBdEJR3nneHkX9txuQ9-p7jFAzOI0TrRfkw5_KoawC2pMWurtF08v1yAXHzgsjSfw0/s941/Picture+5.png)

  

With these 2 checkboxes I don’t know at what exactly time the Floodlight tag is going to be fire. But, I can be 100% sure, that is going to be fired after checking if it’s a new user, and if the tag hasn’t failed.

Finally, don’t forget to include the built-in variables needed for the custom HTML tag:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEij9ot9ej5UavXKJR57u2kpV5Cy9ZvG7ZZ4IOb7pdcnGz3JkIhBh9fu3PPv2ND3r5uFkznGgUPbAQV-_Iq10GsmKwFiBjc4Jy6HdBP9QZ1HjS6ZTP8kQK3gCU1jj2u_-2cX4Uhv/w640-h198/Picture+6.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEij9ot9ej5UavXKJR57u2kpV5Cy9ZvG7ZZ4IOb7pdcnGz3JkIhBh9fu3PPv2ND3r5uFkznGgUPbAQV-_Iq10GsmKwFiBjc4Jy6HdBP9QZ1HjS6ZTP8kQK3gCU1jj2u_-2cX4Uhv/s941/Picture+6.png)

And that’s all folks. There are also a lot of things you can do like create a function name for checking if it’s a new user and call it in another tags or variables but that’s another topic.

Live long and prosper.
