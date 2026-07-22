---
title: "Admin GA4 with Google Spreadsheet"
date: 2022-04-12 16:59:00.005
tags: ["Google Analytics", "Google Apps Script", "ga4"]
category: professional
---

Yesterday I tweet that I saw that recently Google Spreadsheet added the [Analytics Admin service](https://developers.google.com/apps-script/advanced/analyticsadmin?authuser=0) for Google Analytics 4 (GA4). This built-in service help us to code easier with other Google Products([1](https://developers.google.com/apps-script/guides/services/)).

[![](https://blogger.googleusercontent.com/img/a/AVvXsEh0SiK0jTRmY0ReBvvzLdR1zSEJdhtA-UNrEir-QOasDi6M4yeCDXTcygTYNdYBO4TC7VijHKLwIfOiRGCoY-VpuoAnwTjgsp1vrSqpsz2wRTltEYkS5PeeQTxDIRpcJh1YyWskr16OGzxL04-MKhsGTvtEoXZ2elrBxeSRbEziPq-IPj4lPw)](https://blogger.googleusercontent.com/img/a/AVvXsEh0SiK0jTRmY0ReBvvzLdR1zSEJdhtA-UNrEir-QOasDi6M4yeCDXTcygTYNdYBO4TC7VijHKLwIfOiRGCoY-VpuoAnwTjgsp1vrSqpsz2wRTltEYkS5PeeQTxDIRpcJh1YyWskr16OGzxL04-MKhsGTvtEoXZ2elrBxeSRbEziPq-IPj4lPw)

  

What this means? That we can use the API of Google in our Google Spreadsheet. This bring us a lot of versatility to, for example, list all the GA4 Accounts, or all the GA4 properties... Or even create new ones. In this example I'm only going to show you how to list the GA4 Accounts and the GA4 Properties that you have read access, but you can do anything that the API allows you. And it can be coded in our Google App Script and output in our Google Spreadsheet. If you want to have a look about all the possibilities, here's the [official documentation](https://developers.google.com/analytics/devguides/config/admin/v1/rest/v1alpha/accountSummaries).

So, first we need to create a new Google Spreadsheet, and click in Extensions --> App Script

[![](https://blogger.googleusercontent.com/img/a/AVvXsEhMIHa3fOCvxzevfX0QzAKWvRfnjOgGRaPX3uF5P1NxZluX2Lm6Ap02cxhQdEAySPMqPvT9kDjKRb5xT1P9ZDR__W-DKiP-wdrIvRzBS-tH2AoK0lWJdtxqbMB6DliBrO414wTkhTJVTdcif_7d_wfUf62ZGxoQQuSnf7l3zp2XxVL2Xje4zQ)](https://blogger.googleusercontent.com/img/a/AVvXsEhMIHa3fOCvxzevfX0QzAKWvRfnjOgGRaPX3uF5P1NxZluX2Lm6Ap02cxhQdEAySPMqPvT9kDjKRb5xT1P9ZDR__W-DKiP-wdrIvRzBS-tH2AoK0lWJdtxqbMB6DliBrO414wTkhTJVTdcif_7d_wfUf62ZGxoQQuSnf7l3zp2XxVL2Xje4zQ)

Once we are there we can rename the project with something that identifies it, in our case it can be ga4Management, for example.

[![](https://blogger.googleusercontent.com/img/a/AVvXsEj07S6ek2LBSoa3Cs9sqrD6_D-JUmKxv_abBbcjVfPU6V_EBpJ6Tc1LpTDRqYMWd-RpUBcvlMLZMHe7NEsN9wJQfVSYpUt90SvjjYJq2f9F_aAgGLMDGEkh2R2vTgXqbK8CzY98vaQJyeDLmVK579UN-kxZPhjUR-BX5R75W04R_1npr1SVag)](https://blogger.googleusercontent.com/img/a/AVvXsEj07S6ek2LBSoa3Cs9sqrD6_D-JUmKxv_abBbcjVfPU6V_EBpJ6Tc1LpTDRqYMWd-RpUBcvlMLZMHe7NEsN9wJQfVSYpUt90SvjjYJq2f9F_aAgGLMDGEkh2R2vTgXqbK8CzY98vaQJyeDLmVK579UN-kxZPhjUR-BX5R75W04R_1npr1SVag)

  
  
Now we need to add the built-in service we have been talking about. For that, we click on the + button (Add a service).

[![](https://blogger.googleusercontent.com/img/a/AVvXsEhT9nKaKJ8OHi3xeu1nLdY5ftunGGpNMW6S7vBoLEcMEyY3ADiHSpiCoH68TIf-76kkqAJCjzKHksQd0Ic1KBZDSVfPi3DQaCCBHeDNfwPxPScT8aveeprh-dWeK9OKA53wW01sySgcNO1GMfGojotaflupzVli_CPMfADaVqWLeVfId0jMEg)](https://blogger.googleusercontent.com/img/a/AVvXsEhT9nKaKJ8OHi3xeu1nLdY5ftunGGpNMW6S7vBoLEcMEyY3ADiHSpiCoH68TIf-76kkqAJCjzKHksQd0Ic1KBZDSVfPi3DQaCCBHeDNfwPxPScT8aveeprh-dWeK9OKA53wW01sySgcNO1GMfGojotaflupzVli_CPMfADaVqWLeVfId0jMEg)

  
And we click on Google Analytics Admin API, and press the add button.

[![](https://blogger.googleusercontent.com/img/a/AVvXsEgs5IOG8gSfHsS8hDEgo2LcHEu-_brNl1KcsVtXRhUS2aQTBP5wkBgbAJykMMqY5VcvezlT7_b-lSNpWrSHlDylDy7AxufCjaB_rWOYZhoxceTsQrdcM0LTh5WSm6Lo_hjPuGqZuNV-rB59dHUg35BxZIB2MzP_CNo6tjCwJiDkeuFJJlSt1w)](https://blogger.googleusercontent.com/img/a/AVvXsEgs5IOG8gSfHsS8hDEgo2LcHEu-_brNl1KcsVtXRhUS2aQTBP5wkBgbAJykMMqY5VcvezlT7_b-lSNpWrSHlDylDy7AxufCjaB_rWOYZhoxceTsQrdcM0LTh5WSm6Lo_hjPuGqZuNV-rB59dHUg35BxZIB2MzP_CNo6tjCwJiDkeuFJJlSt1w)

Besides, we are also going to add another built-in service that will help us to create new sheets on the spreadsheet and other features related with the spreadsheet itself.

The Service is called Google Sheets Api, and for invoque at it, instead of using the object Sheet, I renamed by SpreadsheetApp

[![](/img/Screen Shot 2022-04-12 at 18.24.53.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7uVIlmr5BAAcfEJ6n6gvHyhZXQOul0e5rQ0AqbWlyCQbTum1vFdrvDSC7PDqD15mUyXEI11iZxkzrmh4QMfwo4MEKgztJk1gTkUWTPqbI-KflRdtSN_b3-TvvhEjrvFPqxOagchvkFlWKP-OJYDFv6q5zrhZ3ehsTu-cddrk82vJZw58oAg/s1794/Screen%20Shot%202022-04-12%20at%2018.24.53.png)

  

After that, the rest is coding, but don't worry readers, at the end of the post, I'll share with you a copy of the Spreadsheet and you can make a copy of it in your own Google Drive. I've tried to commented it and also include console.logs in case you want to debug it.

For last, just mention that in the beginning of the code there's an onOpen function that runs when we load the spreadsheet but as we are creating it now, we are going to force this method to arrange the proper permissions.

Et voila, first time you click on run, for the onOpen function.

[![](/img/Screen Shot 2022-04-12 at 18.46.55.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhbptuq0GovmesmM3LeVurXPSLFJIB114rAsF4jOmO6q1TMHr37hU8fbs70_m998_HKHLjjv6qynG3iZH4w2-hswG5QuHzxs1Nh6PsAklTIcEh0u9JEKXQKSsUXpZb3NMfSmVx_l_CFOT0HYjhkw61dpKPvOGC1VSelvAbDJBf00wlY3t3sng/s1432/Screen%20Shot%202022-04-12%20at%2018.46.55.png)

And it will ask you for permissions:

[![](/img/Screen Shot 2022-04-12 at 18.32.16.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhbnXeHqTERdbdug6SY4nJoZxsvs8gwmfsFxNmml6Z_DV8nFt3kc5rpceEbdbj4qeMfcAJUCE_PxqIUePhIBKuWVNC0CLVvE1hHkyDK94EPc1HLkqNd6WFN2ZmAa1yenrtKZ8nCV2MlUWXl2EeiL9bqJayIoku2NmjizrGSG1QTB8brXyka5Q/s1738/Screen%20Shot%202022-04-12%20at%2018.32.16.png)

  

So you will have to click in advance

[![](/img/Screen Shot 2022-04-12 at 18.34.02.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj2uHFQchOg9KoaTURUHVPeWsxbf7tWnAzavkT3rEgDKDMrlpLH23EtOwOJX8ydsbIdAp54grV_XxGOaJZ0NTCWrudYkiKm8M0FdNgcFxi2q74cl78co-dai-0oCdP9JHOk6xWFbTZl-vxblCdl15kbXRCKrZPbBr5SF-xfaNW9Zmj2x2puEw/s1302/Screen%20Shot%202022-04-12%20at%2018.34.02.png)

And then select the option that ends with (unsafe).

[![](/img/Screen Shot 2022-04-12 at 18.43.50.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgD7Y9cQ_C1O-z3GN55XhFF5zvfAKadz3cTDbXI0g1v4f0yEpXC1P8eN0f-Yy6GDeWdvrHGD4n6NH70RwfF8g7ogvP1mHYNufs4X22iSYtATCr6aBeXgrRijA7HG0GUvX9Ohmmdw0Fs4ubFCN7GKVM02TRnWaxkLfYF_wb4KQV8p2K-fGRfkg/s1298/Screen%20Shot%202022-04-12%20at%2018.43.50.png)

Next window will inform us about the permissions we are going to grant to this app using our gmail account, and click on Allow

[![](/img/Screen Shot 2022-04-12 at 18.48.59.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiOeiZ745_iPfFml9jAdDQkHUsTwNoQcFpLL29ExEZFuJnVKjfbnFoMtiHG9cpClMpZJcvTqB5hJ7uBiiTzdc215nwH7YTLzlL6MZRGn3vRUGbIekKpSo6onEQUsl95aRQUhmJLufZglw-YaLbjjhwgTlNmoCzJqbUvw4zTr-I7PXHmk3oJLA/s1330/Screen%20Shot%202022-04-12%20at%2018.48.59.png)

  

We click on Run again with the difference that it will appear execution completed instead of asking us for permissions.

[![](/img/Screen Shot 2022-04-12 at 18.51.53.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhNqW_yqKlVJHBs9gvJ4Sa7d9-4gHHx022RTvQ4elyb1Q88EWyJO446olz5-fh6AwWXLjuCw5Z_F-XBWS31P57QjgYqpva304RzgIwUj8bljBBxD7HSHiLIOTsfgN6GvlxNKKti-T2EDgcTz-MhZs8KH7e0XzVViW5rEyReReRQn6JvRfuKsw/s1222/Screen%20Shot%202022-04-12%20at%2018.51.53.png)

  

Piece of cake, once the execution is completed, a new menu on our spreadsheet called "GA Management" will appear

[![](/img/Screen Shot 2022-04-12 at 18.53.53.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhyhdSB6IrNVuKyvgcrZxc4JacWp96ylj9z-tIIRB1ijm5PklqoQ3ijOCo6_xuj0KPsEE5IUMmgJ-CfSgs6E0WD2JdDV4g5EZjprO5SHAYyE5_NtmCPOAx46ThkEZfO0eVrelwgmz9h_d0fNAXjQNMw23ZzMSq2zDHzoyPA224NZ8SPQ-qtfQ/s1736/Screen%20Shot%202022-04-12%20at%2018.53.53.png)

  
Using this menu, now we can list all our GA4 Accounts and all our GA4 properties.  
  
And that's all folks !!!  
  
[This is the link of the Google Spreadsheet.](https://docs.google.com/spreadsheets/d/1hFfqZKagYYB8gomiYhHcmTmQ-8VkAINOm6-Xm7xJPvk/edit?usp=sharing)
