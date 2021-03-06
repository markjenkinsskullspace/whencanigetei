This software was written by Mark Jenkins and Sara Arenson for the Canadian Open Data Experience hackathon
https://canadianopendataexperience.com/

Premise: You are working as a temp or have otherwise insecure unemployement. For the sake of your prayers you ask, "How much longer do I have to work until I have the security of EI elegibility?"
("merciful god! May this job last at least that long")

In our EI system, the answer can be complicated depending on your work history and history with the EI system.

But, in the simple cases it comes down to how many hours you've worked in the last year, how many hours a week you are working now, and where in the country you are. In places with high unemployment rates, less EI eligible hours are required, in places with low unemployment rates, more are hours required.

If you're a time traveler trying to answer this for some poor soul in the past, you can find lots of historic unemployment rates for the different regions here:

http://data.gc.ca/data/en/dataset/aad2bcd4-9f45-4013-b2a6-8367106dc0b2

If you care about the rates of today, you should refer to this Service Canada table:

http://srv129.services.gc.ca/eiregions/eng/rates_cur.aspx

which I have copy-pasted to unemployement_rates_feb9_march8_2014.csv

The initial versions of this application will be based on that. Time permitting, I may also add a time-travel feature so that the data from data.gc.ca can have a purpose.

You can see a copy of this up and running at:
http://www.when-ei.ca

-----------
So, how do you use this?
You need Django 1.7 (which is currently in alpha stage)
https://www.djangoproject.com/download/
and Python 2.7
http://python.org/downloads/

You can read all about how to install Django and use Django sites and apps 
through the docs
https://docs.djangoproject.com/en/dev/

The there are three top level directories:
 * /html HTML and gif files statically served up
 * /whenei -- a django app for EI data
 * /wheneisite -- a django site for all of this

You need both the top level and /wheneisite sub directories to be in your PYTHON_PATH so that both the whenwi and wheneisite/wheneisite packages can be imported

```
import whenei
import wheneisite
```

Install it somewhere that's in your default sys.path or set PYTHON_PATH to include it and this source code directory.

load the database]

$ cd wheneisite
$ ./manage.py syncdb

load the CSV file

$ cd wheneisite
DJANGO_SETTINGS_MODULE=wheneisite.settings python ../load_unemployement_rates.py unemployement_rates_feb9_march8_2014.csv 2014-2-9 2014-3-8

load some old rates
DJANGO_SETTINGS_MODULE=wheneisite.settings python ../load_old_unemployment_rates.py ../old_unemp_rates/20121207-01-unemploymtrate-eng.csv

---------

It's possible to skip the web parts of this and just run this as a command
line application. For this, its useful to have to have the django-admin program
available in PATH . You also don't need /wheneisite in your PYTHON_PATH

Customize sqlite_test_settings.py to your likeing and put DJANGO_SETTINGS_MODULE=sqlite_test_settings in your environment.

create your database

$ django-admin syncdb

load the CSV file

python load_unemployement_rates.py unemployement_rates_feb9_march8_2014.csv 2014-2-9 2014-3-8

Now we can interactivly look stuff up:

python interactive_when_ei.py

```
AB: Alberta
BC: British Columbia
YT: Yukon
ON: Ontario
NL: Newfoundland and Labrador
MB: Manitoba
NB: New Brunswick
SK: Saskatchewan
QC: Quebec
PE: Prince Edward Island
NS: Nova Scotia
NT: Northwest Territories
NU: Nunavut
When province are you from? > MB

39: Winnipeg
40: Southern Manitoba
41: Northern Manitoba
And where exactly? > 41

you need 420 hours in Northern Manitoba with it's 32% unemployment rate
```

check out the SQL schema this has created

$ django-admin sqlall

Look at the pretty data dump

$ django-admin dumpdata

And well that's all we've got so far. Happy hacking.
