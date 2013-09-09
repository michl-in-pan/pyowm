PyOWM usage examples
====================

Import the PyOWM library
------------------------
As simple as:

    >>> from pyowm import OWM

Create global OWM object
------------------------
Use your OWM API key if you have one (read [here](http://openweathermap.org/appid) 
on how to obtain an API key).

    >>> API_key = 'G097IueS-9xN712E'
    >>> owm = OWM(API_key)
    
Of course you can change your API Key at a later time if you need:

    >>> owm.get_API_key()
    'G09_7IueS-9xN712E'
    >>> owm.set_API_key('6Lp$0UY220_HaSB45')

Getting currently observed weather for a specific location
----------------------------------------------------------
Querying for current weather is simple: provide an _OWM_ object with the location
you want current the weather be looked up for and the job is done.
You can specify the location either by passing its toponym (eg: "London") or
its geographic coordinates (lon/lat):

    obs = owm.observation_for_name('London,uk')                     # Toponym
    obs = owm.observation_for_coords(-0.107331,51.503614)           # Lon/lat

A _Observation_ object will be returned, containing weather info about the first
location matching the toponym/coordinates you provided. So be precise when
specifying locations!
    
    
Currently observed weather extended search
------------------------------------------
You can query for currently observed weather:

+ for all the places whose name equals the toponym you provide (use _search='accurate'_)
+ for all the places whose name contains the toponym you provide (use _search='like'_)
+ for all the places whose lon/lat coordinates are in the surroundings of the lon/lat couple you provide

In all cases, a list of _Observation_ objects is returned, each one describing 
the weather currently observed in one of the places matching the search.
You can control how many items the returned list will contain by using the
_limit_ parameter.

Examples:

    # Find observed weather in all the "London"s in the world
    list_of_obs = owm.find_observations_by_name('London',search='accurate')
    # As above but limit result items to 3
    list_of_obs = owm.find_observations_by_name('London',search='accurate',limit=3)
    
    # Find observed weather for all the places whose name contains the word "London"
    list_of_obs = owm.find_observations_by_name('London',search='like')
    # As above but limit result items to 5
    list_of_obs = owm.find_observations_by_name('London',search='like',limit=5)
    
    # Find observed weather for all the places in the surroundings of lon=-2.15,lat=57
    list_of_obs = owm.find_observations_by_coords({'lon':-2.15,'lat':57})
    # As above but limit result items to 8
    list_of_obs = owm.find_observations_by_coords({'lon':-2.15,'lat':57},limit=8)

Getting data from Observation objects
-------------------------------------

_Observation_ objects encapsulate two useful objects: a _Weather_ object that
contains the weather-related data and a _Location_ object that describes the
location for which the weather data are provided.

If you want to know when the weather observation data have been queried, just
issue:

    >>> obs.get_reception_time()                           # UNIX UTC time


You can retrieve the _Weather_ object like this:

    >>> w = obs.get_weather()

and then access weather data using the following methods:

    >> w.get_reference_time()                              # get time of observation in UTC UNIXtime
    1377872206
    >>> w.get_reference_time(timeformat='iso')             # ...or in ISO 8601
    2013-08-30 14:16:46+00
    
    >>> w.get_clouds()                                     # Get cloud coverage
    12
    
    >>> w.get_rain()                                       # Get rain volume
    {'3h':0}
    
    >>> w.get_snow()                                       # Get snow volume
    {'3h': 0}
    
    >>> w.get_wind()                                       # Get wind degree and speed
    {'deg': 59, 'speed': 2.660}
    
    >>> w.get_humidity()                                   # Get humidity percentage
    99
    
    >>> w.get_pressure()                                   # Get atmospheric pressure
    {'pressure': 1030.119, 'sea_level: 1038.381}
    
    >>> w.get_temperature()                                # Get temperature in Kelvin degs
    {'temp': 293.4, 'temp_kf': -1.89, 'temp_max': 297.5, 'temp_min': 290.9}
    >>> w.get_temperature(unit='celsius')                  # ... or in Celsius degs
    >>> w.get_temperatire(unit='fahrenheit')               # ... or in Fahrenheit degs

    >>> w.get_status()                                     # Get weather short status
    'Clouds'
    >>> w.get_detailedStatus()                             # Get detailed weather status
    'Broken clouds'

    >>> w.get_weather_code()                               # Get OWM weather condition code
    803
    
    >>> w.get_weather_icon_name()                          # Get weather-related icon name
    '02d'

    >>> e.get_sunrise()                                    # Sunrise time (UTC UNIXtime or ISO 8601)
    1377862896
    >>> e.get_sunset()                                     # Sunset time (UTC UNIXtime or ISO 8601)
    1377893277

Support to weather data interpreting can be found [here](http://bugs.openweathermap.org/projects/api/wiki/Weather_Data#Description-parameters) 
and [here](http://bugs.openweathermap.org/projects/api/wiki/Weather_Condition_Codes) you can read about OWM weather condition codes and icons.

If you need a JSON/XML representation of the _Weather_ object you can benefit 
from the following methods:

    >>> w.dump_JSON()                                    # Get a JSON representation
    {'base':'gdpsstations','referenceTime':1377851530,'Location':{'name':'Palermo',
    'coordinates':{'lon':13.35976,'lat':38.115822}'ID':2523920},...}
    
    >>> w.dump_XML()                                     # Same as above, but in XML
    <weather><base>gdpsstations</base><referenceTime>1377851530</referenceTime>
    <Location><name>Palermo</name><coordinates><lon>13.35976</lon><lat>38.115822</lat>
    </coordinates><ID>2523920</ID></Location>...</weather>

As said, _Observation_ objects also contain a _Location_ objects with info about
the weather location:

    >>> l = obs.get_location()
    >>> l.get_name()
    'London'
    >>> l.get_lon()
    -0.107331
    >>> l.get_lat()
    51.503614
    >>> l.get_ID()
    2643743

The last call returns the OWM city ID of the location - refer to the 
[OWM API documentation](http://bugs.openweathermap.org/projects/api/wiki/Api_2_5_weather#3-By-city-ID)
for details.

Getting 3 hours weather forecast
--------------------------------
TBD

Getting daily weather forecast
------------------------------
TBD