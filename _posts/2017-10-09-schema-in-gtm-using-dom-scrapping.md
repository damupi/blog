---
title: "Schema in GTM using DOM scrapping"
date: 2017-10-09 06:24:00
tags: ["gtm", "google tag manager", "DOM", "Schema"]
category: professional
---

It seems it has been ages since the last time I post here. Well, not ages, but last year at least. Even this time I'll 'try' (and I mean try because my English is not so good) to write it in English as I moved a time to another country where they speak in English. So, to my Spanish readers, my apologies in advance, if you need something, I bet you know how to catch me and I'll be please to explain it in Spanish too.  
  
Anyway, intro apart, this time I will write about something we did in the actual Company I work for. The post is not new and it's based on this big one: [Using Gooble Tag Manager to dynamically generate a Schema Tag](https://moz.com/blog/using-google-tag-manager-to-dynamically-generate-schema-org-json-ld-tags). I will write down more because, as the silly person I am, the most difficult part from me, was to extract the data from the DOM elements that were already on the page to create the JSON schema on the fly.  
  
Speaking of DOM scrapping, it also has to be with [my last post on how to retrieve values from that DOM](http://blog.damupi.com/2017/02/insertando-un-pixel-de-facebook-dom.html), but this time, using GMT built-in variable, instead of querySelector javascript function. One thing that helps me so from a better understanding was the mozilla developer page [attribute selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors) . But all of you that know me a bit, know that I prefer to show it to you with examples, so, here we are.  
  
By example we are going to use the same example I used last year, so if we go to <http://www.damupi.com/test/damupi/index_en.html> we will find this page:  
  
  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgW-gTyr-1Ap9pWkV6uP0WDHKVfPIMYumPanDwlq5bL_hXShyxTUFVf-4ocmR3M1Um9zz7JAghBS7l7BOXuZXJjUAll7hUfmFS1R33QCm_ExciQMOwhy40lYhGssFJiQuwKq_as/s400/Screen+Shot+2017-09-17+at+11.26.44.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgW-gTyr-1Ap9pWkV6uP0WDHKVfPIMYumPanDwlq5bL_hXShyxTUFVf-4ocmR3M1Um9zz7JAghBS7l7BOXuZXJjUAll7hUfmFS1R33QCm_ExciQMOwhy40lYhGssFJiQuwKq_as/s1600/Screen+Shot+2017-09-17+at+11.26.44.png)

And this is the source code:  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjWK165hF68Ya9SmudtMLLBLHnEpX4GDIAn0zNRv32-R1SpWS7ugn2dGKYUcVp4kX9eNSWECx-0Mm5QV2y4brB2Wi7xVxsvBYcwcL69Mvo1X8ej-FPzX1eM-IjsFNRweehqnPHM/s400/Screen+Shot+2017-09-17+at+12.52.43.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjWK165hF68Ya9SmudtMLLBLHnEpX4GDIAn0zNRv32-R1SpWS7ugn2dGKYUcVp4kX9eNSWECx-0Mm5QV2y4brB2Wi7xVxsvBYcwcL69Mvo1X8ej-FPzX1eM-IjsFNRweehqnPHM/s1600/Screen+Shot+2017-09-17+at+12.52.43.png)

Imagine that I want to create an schema like the following one, and you want to retrieve all the data from the page to create the JSON file on the fly:  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgQ_TVGQ7s-XpHM4-eRG6n4_HlVj-Y7gqAZUsC3oX2ZwSVLFmbh4Bpqmo3gY41LT6ol4El-xpi3ZhE7YiJp753qnaEdFP2vbK_7xKzBurL3wE7EdPjcYZQCul5x5uaLPvxFmG-Y/s320/Screen+Shot+2017-09-17+at+12.56.27.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgQ_TVGQ7s-XpHM4-eRG6n4_HlVj-Y7gqAZUsC3oX2ZwSVLFmbh4Bpqmo3gY41LT6ol4El-xpi3ZhE7YiJp753qnaEdFP2vbK_7xKzBurL3wE7EdPjcYZQCul5x5uaLPvxFmG-Y/s1600/Screen+Shot+2017-09-17+at+12.56.27.png)

  
  
And you want to retrieve all the data from the page to create the JSON file on the fly.  
  
Let's start by getting the name "Suzuki". In GTM, go to variables and create a new variable like and, for example, call it "Schema product name - DOM":  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgwSA5buzVJcCQKQ6ILwwZzuLU-uhx58PQ-ipRQ0c_a2WGa8QgNz1FnAEzxFFksEYQrxn8SU4oJDPBhSxrCKSLg0fNc1OCDoJbdOQ9qYr20EWbx_MgpoIymsRLWo_eh_rXNDma0/s400/Screen+Shot+2017-09-17+at+13.07.34.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgwSA5buzVJcCQKQ6ILwwZzuLU-uhx58PQ-ipRQ0c_a2WGa8QgNz1FnAEzxFFksEYQrxn8SU4oJDPBhSxrCKSLg0fNc1OCDoJbdOQ9qYr20EWbx_MgpoIymsRLWo_eh_rXNDma0/s1600/Screen+Shot+2017-09-17+at+13.07.34.png)

  
  
As you see on the source code I start looking for a div with a specific class and then I drill down till the next element, which in our case is an img, with the attribute "type" equals to "brand". Let's say this is an example an in this particular case we could just get the value directly from "img[type="brand"]" but I have preferred to write you down like this because normally, all the pages have a lot of source code and the only thing for drilling down is to do it like this.  
  
Check GTM preview mode to test if your expression is right or not  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgtZjIxfQQGZeUBBmswYeJEpsqBI4znxOutWcCgjuGaB4z18tlGEKz0CdMIU0-VN3a831kOLpwmuIzWH1AIvTBK3KLF3k9U3fWqqZi0PsBz162i-BmPIHEpLS7CKKuoEE_2zNGk/s400/Screen+Shot+2017-09-17+at+13.05.03.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgtZjIxfQQGZeUBBmswYeJEpsqBI4znxOutWcCgjuGaB4z18tlGEKz0CdMIU0-VN3a831kOLpwmuIzWH1AIvTBK3KLF3k9U3fWqqZi0PsBz162i-BmPIHEpLS7CKKuoEE_2zNGk/s1600/Screen+Shot+2017-09-17+at+13.05.03.png)

  
  
Let's do the same for getting image value for building our Schema JSON file. In this case we are going to retrieve the image value for the same element but a different attribute, so the only thing we are going to replace is "alt" per "scr".  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhChK39wYRh6EjZTE0HRk8oERJA4z2EQzuw0aE7pqCMaOr3a7cn2lBdaZ8rrya7ORLJsjqlDfp3ctFS5W_OXK-oe35i-7N2XYuPRLA3-6MHNQVlCL_sauVy7kds1KZd6iEuNBuS/s400/Screen+Shot+2017-09-17+at+13.13.02.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhChK39wYRh6EjZTE0HRk8oERJA4z2EQzuw0aE7pqCMaOr3a7cn2lBdaZ8rrya7ORLJsjqlDfp3ctFS5W_OXK-oe35i-7N2XYuPRLA3-6MHNQVlCL_sauVy7kds1KZd6iEuNBuS/s1600/Screen+Shot+2017-09-17+at+13.13.02.png)

  
  
Test it again in GTM preview mode:  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiQFsY9yY4TFCrD9f-uR701wIcXO3CX97WlvjGxQggub3UXo-mg3pCjyko3i_rnmziOnbpuBrfEzORyLEfEn8DctpFAHEDoOurOXB_E6cHSzHfJrnBMxMULu_aakJckeG_H1PkP/s400/Screen+Shot+2017-09-17+at+13.15.39.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiQFsY9yY4TFCrD9f-uR701wIcXO3CX97WlvjGxQggub3UXo-mg3pCjyko3i_rnmziOnbpuBrfEzORyLEfEn8DctpFAHEDoOurOXB_E6cHSzHfJrnBMxMULu_aakJckeG_H1PkP/s1600/Screen+Shot+2017-09-17+at+13.15.39.png)

  
  
And finally, following the instructions of [Chris Goddard](https://moz.com/community/users/4315837) we can build the json we want to.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhynw_HWVg5x40RtGcgpo7gjEK4SSvrxRvuoF3khBjx0CMCz-0pFBs3a-zMyQ3R1fVAUgShYiFU4UcUq0Ld6bzjjvlL43F_pwdEQm_D5cU96xvV8gkqAiyTpXaNBrx6M77stmTQ/s400/Screen+Shot+2017-10-08+at+11.02.47.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhynw_HWVg5x40RtGcgpo7gjEK4SSvrxRvuoF3khBjx0CMCz-0pFBs3a-zMyQ3R1fVAUgShYiFU4UcUq0Ld6bzjjvlL43F_pwdEQm_D5cU96xvV8gkqAiyTpXaNBrx6M77stmTQ/s1600/Screen+Shot+2017-10-08+at+11.02.47.png)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgXMSVAS-Rnz5LhIRYXhnf4uIDqPBvqoo3i-axWdSaE47pygHwbGxDRYIUINqzcGZlB5GJTkD_gLAsAnB7-vj_c2_arLaM-JmOEmHsRaFA0BBcOpolqvZYyv0xoryUdEpbvKTzv/s400/Screen+Shot+2017-10-08+at+11.05.58.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgXMSVAS-Rnz5LhIRYXhnf4uIDqPBvqoo3i-axWdSaE47pygHwbGxDRYIUINqzcGZlB5GJTkD_gLAsAnB7-vj_c2_arLaM-JmOEmHsRaFA0BBcOpolqvZYyv0xoryUdEpbvKTzv/s1600/Screen+Shot+2017-10-08+at+11.05.58.png)
