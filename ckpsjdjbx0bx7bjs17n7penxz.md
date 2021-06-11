## Build a Bitcoin Price Notification Service with Python

# Introduction

According to a [recent post](https://www.youtube.com/watch?v=srIgXeFG7nc) made by Robert Kiyosaki, There are three kinds of people in the world when it comes to money
- **Investor** - Stores value for long term
- **Trader** - Buys and sells e.g crypto
- **Speculator** - Gamblers

While the point of this article is not to tell you which category is right or wrong, like Robert, Some of us fall in the first category. We choose to keep bitcoin as a store of value and think long-term while we go about our daily business. But sometimes you want to always check the state of your flocks and see how your investments are doing. 

What if you could build a simple app that notifies you when major events happen like when your bitcoin investment crosses a threshold or when the price reaches a certain limit? - Then you've come to the right place.

In this article, you will learn how to build a bitcoin notification service using Python and IFTTT. The service will send notifications to your phone when the price of bitcoin reaches a threshold you define in the code. It will also send prices to telegram.

![telegram.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1623408376233/R2Ye1rTPJ.jpeg)

![notification.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1623408398320/stFVgOofP.jpeg)

# What is IFTT

If This Then That commonly referred to as IFTTT, is a service that allows users to program responses to events. Those programs are usually called **applets**. 

An IFTTT applet is composed of two parts: a trigger and an action. 

Examples

* If you make a call on your Android phone, then a log of that call is added to a Google spreadsheet.

* If you add a new task to your Amazon Alexa to-dos, then it will be added to your iOS Reminders app.

You will write a python service that fetches bitcoin price from [coinmarketcap](coinmarketcap.com) and send a trigger to the IFTTT applet created. The trigger will be a webhook service provided by IFTTT. You can think of webhooks as “user-defined HTTP callbacks” but in this case, IFTTT is providing the webhook. Then the webhook will send a notification to your phone and telegram in our case

There are tons of open-source applets created already. You can check them out [here](https://ifttt.com/explore)

The technology behind IFTTT is what powers most real-time software solutions today like [Zapier](https://zapier.com/), [Integromat](https://www.integromat.com/en), [Automate.io](https://automate.io/), etc

# Project Setup

Setup python and a virtual environment on your computer. If you are mac you can follow the guide [here](https://opensource.com/article/19/5/python-3-default-mac) to get started. For other operating systems you can check [here](https://realpython.com/installing-python/)

Then we will be installing two packages
- python-dotenv==0.17.0
- requests==2.18.4

I have created a Github repo for the project. You can read more about the project requirements [here](https://github.com/sannimichaelse/btc-notification-service)

# Getting Latest Bitcoin Price

In this section, you will use the python request library to get the latest prices of cryptocurrency. But before you do that you need to signup on [coinmarketcap.com](coinmarketcap.com) and obtain `COIN_MARKETCAP_API_KEY`

You also need to set two environment variable

```
COIN_MARKETCAP_API_KEY=
IFTTT_WEBHOOK_KEY=
```

You will learn more about the `IFTTT_WEBHOOK_KEY` in the coming section

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1623413499908/cNql5FF4G.png)

In the snippet above, we basically make a call to the bitcoin URL using the python request library to get the latest price of bitcoin. Then we also made sure the function returns a float value. The bitcoin price is contained in the first object of the data array

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1623414026906/1mPVcvaQR.png)

This is a sample of the response from the `coinmarketcap` API.

# Sending Test IFTTT Notification
Now we can move onto the IFTTT side of things. To use IFTTT you’ll first need to [set up a new account](https://ifttt.com/join) and install their mobile app (if you want to receive phone notifications from your Python app). Once you set that up, we’re going to [create](https://ifttt.com/create) a new IFTTT applet for testing purposes.

To create a new test applet follow these steps:

- Click on the big “this” button
- Search for the “webhooks” service and select the “Receive a web request” trigger
- Let’s name the event `testing`
- Now select the big “that” button
- Search for the “notifications” service and select the “Send a notification from the IFTTT app”
- Change the message to `I just triggered my first IFTTT action!` and click on “Create action”
- Click on the “Finish” button and we’re done

Next up, check the [webhooks page](https://ifttt.com/maker_webhooks) and click on `Documentation` to see how to use the webhook with your webhook key. 

Let's create a sample notification using `curl` to test the webhook. Make sure you've installed the IFTTT app on your phone and you've logged in with your account. You can find the app on [play store](https://play.google.com/store/apps/details?id=com.ifttt.ifttt&hl=en&gl=US) and [apple store](https://apps.apple.com/us/app/ifttt/id660944635)

```
curl -X POST https://maker.ifttt.com/trigger/{event}/with/key/{{IFTTT_WEBHOOK_KEY}}
```
where `IFTTT_WEBHOOK_KEY` is the webhook key obtained from the webhook documentation page and the event is the name of the test event we created above

So the request becomes
```
curl -X POST https://maker.ifttt.com/trigger/testing/with/key/{{IFTTT_WEBHOOK_KEY}}
```
Then you should get a notification on your phone

![first_notification.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1623424457890/DmaNbTcMK.jpeg)

# Creating Applets for Bitcoin Prices

You will create two applets, one called `bitcoin_price_emergency` and the other called `bitcoin_price_update`. The first will be used to trigger notification to your phone when the bitcoin price crosses the threshold you set in your code while the other will be used to send bitcoin prices to telegram based on the length you set in the code

#### Emergency bitcoin price notification applet:

- Choose the “webhooks” service and select the “Receive a web request” trigger
- Name the event `bitcoin_price_emergency`
- For the action select the “Notifications” service and select the “Send a rich notification from the IFTTT app” action
- Give it a title, like “Bitcoin price emergency!”
- Set the message to Bitcoin price is at ${{Value1}}. Buy or sell now! (we’ll return to the {{Value1}} part later on)
- Create the action and finish setting up the applet

#### Regular price updates applet:

- Again choose the “webhooks” service and select the “Receive a web request” trigger
- Name the event `bitcoin_price_update`
- For the action select the “Telegram” service and select the “Send message” action
- Set the message text to Latest bitcoin prices: `<br>{{Value1}}`
- Create the action and finish with the applet

**Note**: When creating this applet you will have to authorize the IFTTT Telegram bot.

# Breaking things Down

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1623425166826/MBEaULVi9.png)

In the snippet above, we make a call to the `get_latest_bitcoin_price` method we defined above to get the current bitcoin price. Then, if the price is more than the threshold set - we make a post request to IFTTT which triggers the webhook that sends a notification to your phone.  Also when the length of the bitcoin price is 2( you can set it to any value of your choice) we also send another request to IFTTT which triggers the webhook that sends a notification to telegram. Also notice we define each IFTTT request with the event name.

Then the last line in the method is to sleep for `x` number of seconds to reduce the number times we make calls to the coin API. There is actually a limit on the number of calls. You can check more about that [here](https://coinmarketcap.com/api/documentation/v1/#section/Authentication)

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1623425534533/zqLjPfFEP.png)

If you notice from the previous snippet, the `post_ifttt_webhook` was called based on the event we want to trigger a notification for. For the telegram notification, there are some couple formatting done to improve the look of the bitcoin prices so it's always coming as a formatted string. Also, you should have also taken note of the data we are posting to IFTTT, it contains an object

```
{'value1': '36,951.78'}
```

The `value1` is what we already defined up when creating the applets. It will be replaced by any value sent and should show in the notifications received

```
if isinstance(value, str) == False:
        bitcoin_value = "{:,.2f}".format(value)
```

What this line basically does it to format the bitcoin price because it's coming as float directly for the `bitcoin_price_emergency` event. The telegram event comes in as a formatted string already, so no need to format it again.

You can easily start the application by running 

```
python bitcoin_notification.py
```

You can find the full source code [here](https://github.com/sannimichaelse/btc-notification-service)

# Conclusion

There is so much that can be done with the IFTTT service. In this article, you've learned what IFTTT is and how to create applets which are the building blocks of IFTTT. You also learned how to connect different applications together with the service and how to create webhooks without writing a single line of code. All through the power of IFTTT. 

The technology behind IFTTT can be used to build very powerful applications especially in an ever-changing world where we need to respond to actions or events in real-time

You can read more about IFTTT [here](https://ifttt.com/home). 



