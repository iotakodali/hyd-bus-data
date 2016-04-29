#TSRTC Expected Time of Arrival (ETA) API's for Hyderabad City Buses

##Warning:

If you ever use the following data, API's. Be warned you may be voilating parts of the Indian IT act. Better consult a lawyer if you are unsure. 

Is there any another way to source data?  
I feel the only way to source reliable transit information is through the agency. 


##Background:

Hyderabad Buses have GPS units installed in them like all the other city buses in the country. The project was implemented by CMC limited (A TCS firm). The real-time arrival information (ETA) is displayed on LED sign boards at few bus-stops. At the same time they publish the information through an android app https://play.google.com/store/apps/details?id=com.tsrtc.bats

##API:

There are 2-3 API's to get bus information by route/stop etc. I use the API which gives me ETA's by stop_id. It is a simple GET request with a stop_id in the query params. A sample implementation in python is shown below

```
import requests

def call(stop_id):
	url = "http://125.16.1.204:8080/vtpis/appQuery.do?query="+stop_id+",0,67&flag=6"
    response = requests.get(url)
    data = response.content #that's it, you get ETA's for the stop_id you need

```

The response is neither json/xml, but a string & semi-colon seperated text. [Click me](http://125.16.1.204:8080/vtpis/appQuery.do?query=348,0,67&flag=6) to find out what buses are running at the Koti Bus stop

```
data = "AP11Z6881-METRO EXPRESS,222A,KOTI,15:02:51;AP11Z6882-METRO EXPRESS,222A,KOTI,15:02:53;AP11Z7159-METRO EXPRESS,1P/25I,KOTI,15:11:09;AP11Z7298-METRO EXPRESS,222A,KOTI,15:15:49"

buses = data.split(";")

for bus in buses:
	variables = bus.split(",")
	vehicle = variable[0]
	route = variable[1]
	direction = variable[2]
	eta = variable[3] #this basically say when the bus arrives. The estimate changes as the bus moves, so you will have to request data as you need it 

```

##Data & FAQ's:

You will need all the stop_id's & route_id's & the position of stops in the route. All the information is supplied in the respective folders. You can have a look at all the bus stops on a [map](http://bl.ocks.org/anonymous/raw/e95f49cc8009cf28cb060d6a0bc864cd/)

* Hey, but wait a minute. Are their only around 1500 bus stops in Hyderabad?
> Nope. I bileve there are more un-offcial stops which the contractor didn't mind mapping out.

* Looks like the android app is an HTML based app and not a native one?
> Yes. The contractor seem to have chosen the least time-taking process to just finish up the work. Be optimistic and use the API's to write an app for yourself.

* So, I need to constantly hit the URL to get the latest information?
> Yes, But I recommend you be responsible and don't do a million requests/sec. It could break the server and all of us could loose access.

* Can't the govt. provide a better API with API key for me alone?
> Yes. They can and should. Un-fortunately I am not the government. 

* Again. I don't get gps location of the moving bus through the API. Is the ETA really accurate?
> No. I assume they simply used an avg. speed, distance time formula to tell you when the bus might arrive. ETA constantly changes. For example at 1.00 p.m the API may suggest bus arriving at 1.20 p.m at 1.15 p.m it might say the same bus will come at 1.40 p.m due to traffic.

* What. That is simply fooling me?
> Traffic is a serious issue. On the bright side, they already have GPS units in the buses and if they share that data on a google map with traffic that will be a real help for all commuters. 


* I need GPS data of the buses, Shouldn't they provide me the data if they have it already?
> It depends on the contract the pvt. firm has with tsrtc. Most of the time, pvt. firms keep the data rights even though the govt. is already paying for the implementation. This happened in the past in chennai, mumbai and potentially with all other past contracts related to Intelligent Transportation Systems in the country.

* But I really need the data for research, improving hyderabad or just to get reliable information about my bus. What do I do?
> The telangan govt. has come up with a recent ICT policy with inclinations for open-data. Write to the [IT secretary](secy_itc@telangana.gov.in) requesting the change you want to see 

* Can I buzz you for any further help?
> Please do. My details are available on my [webpage](www.lostprogrammer.com)

##License:

The code & data is licensed under Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.