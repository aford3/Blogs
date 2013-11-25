Getting Started With Podio (Ruby)
---------------------------------

Podio is a great collaboration tool with many options for creating your own apps inside the software to handle whatever you may need. In this post I will quickly go over the setup for how to pull data from fields in your Podio apps.

First up is to install the required gems for Podio.
```bash
gem install podio
gem install sinatra
```

Next go ahead and create your ruby script adding in the require statements 'rubygems' 'sinatra' and 'podio'.
Now add in the code to setup podio.
```bash
Podio.setup(:api_key => 'APIKEY' , :api_secret => 'SECRET')
```

If you do not have a key/secret yet just login to your Podio account, go to account settings, api keys, and generate API key. It is recommend for each to generate a new api key for each new application you make.

Now with Podio there are two ways to authenticate. Authenticate with user and with app. For this post we will be authenticating with an app. This way requires no user interaction once complete so is preferable for something that you want to run automatically, such as pulling down and logging data from your Podio apps. Now to authenticate with an app simply use.
```bash
Podio.client.authenticate_with_app('app_id','app_token')
```

To get your app id and token go to the top right corner of Podio, click the wrench, and then click Developer. Here you can see your ID and Token for this app. You can also see any field ID's in case you need them. 

Now that you are authenticated with your Podio app it's time to find the the items and data you are looking for.
```bash
Podio::Item.find_by_filter_values('app_id', filter_id, attributes)
```

For a full list of possible filters visit https://developers.podio.com/doc/filters

As an example of a filter if you wanted to only pull the items from your app that were created in the past seven days you could use the created_on filter.
```bash
Podio::Item.find_by_filter_values(app_id, created_on:{from:”-7d”,to:”+0d”})
```

This returns an array of information. To get to the first item in your array simple add on [0][0] to the end of the above statement and so on for the second and third ( [0][1] or [0][2] ). Now that you have a single item from your Podio app it's simply drilling down through the arrays and hashes to find the data you are looking for. As an example lets say I have an item with a text field as the second field and a date field as the fourth. So to access those two fields I can do something like this.
```bash
Item = Podio::Item.find_by_filter_values(app_id, created_on:{from:”-7d”,to:”+0d”})[0][1]
text = item.fields[1][“values”][0][“value”][“text”]
date = item.fields[3][“values][0][“start_date”]
```

There you have it. Now with those several lines of code you can access data from your Podio app entries to log for later, integrate with another api, or just to play around with. Podio has many more commands and options to access all other parts of it's website but I hope this tutorial is enough to get you quickly up and working!