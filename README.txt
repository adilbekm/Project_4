===============================================================================
OVERVIEW
===============================================================================

This is a README document for Project 4 of Udacity's FS nanodegree program. 
This document explains how to run the application and provides implementation
details on all relevant parts of the project.

STUDENT: Adilbek Madaminov
EMAIL: adilbekm@yahoo.com
APP ENGINE ID: acoustic-atom-98417
WEB_CLIENT_ID:
'863650903089-fpu3n4149emc4gf959v2jjrq25e4iu1q.apps.googleusercontent.com'

===============================================================================
HOW TO RUN THIS APPLICATION
===============================================================================

-- Update the value of "application" in "app.yaml" to App Engine ID
   provided above.

-- Update the values at the top of "settings.py" to
   reflect the respective client IDs you have registered in the
   Developer Console (use WEB_CLIENT_ID from above)

-- Update the value of CLIENT_ID in "static/js/app.js" to the Web client ID

-- (Optional) Mark the configuration files as unchanged as follows:
   "$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js"

-- Run the app with the devserver using "dev_appserver.py DIR", and ensure it's
   running by visiting your local server's address (by default localhost:8080)

-- (Optional) Generate your client library(ies) with the endpoints tool.

-- Deploy your application.

===============================================================================
TASK 1 IMPLEMENTATION NOTES
===============================================================================

This task calls for implementation of Session and SessionForm classes. Session
class has the following properties and their types:

	name            = ndb.StringProperty(required=True)
    conferenceId    = ndb.StringProperty()  
    highlights      = ndb.StringProperty()
    speaker         = ndb.StringProperty()
    duration        = ndb.IntegerProperty() 
    typeOfSession   = ndb.StringProperty()
    date            = ndb.DateProperty()
    startTime       = ndb.TimeProperty() 

Property "speaker" is implemented as a string as this appears sufficient for
all queries in this project. To accomodate the needs of subsequent queries,
property "duration" is defined as type integer, "date" as date, and "startTime"
as time.

The followint Endpoints methods are created as part of this Task 1:

createSession(SessionForm, websafeConferenceKey)
getConferenceSessions(websafeConferenceKey)
getConferenceSessionsByType(websafeConferenceKey, typeOfSession)
getSessionsBySpeaker(speaker)

===============================================================================
TASK 2 IMPLEMENTATION NOTES
===============================================================================

In this part, two additional Endpoints methods are created:

addSessionToWishlist(sessionKey)
removeSessionFromWishlist(sessionKey)
getSessionsInWishlist()

Also defined is method _sessionWishlist to be used by the first two methods -
this method accomodates both adding and removing a session from the wishlist.

===============================================================================
TASK 3 IMPLEMENTATION NOTES
===============================================================================

The following two queries are created and implemented as Endpoints methods:

SpecialQuery1: Return all sessions user has in wishlist but not registered to 
               attend. 

SpecialQuery2: Return all sessions longer than 60 minutes in duration.

Note, when SpecialQuery1 is executed in App Engine and there are no matching
results, it returns with error instead of returning no data. It returns data
normally when at least one result is found. The error only happens when
deployed to App Engine - not on local host.


TASK 3: QUERY-RELATED PROBLEM (SpecialQuery3)

The query problem given in this task occurs due to the Datastore limitation for
allowing only one inequality filter per query. In present form, the query calls
for two inequalities:

typeOfSession != "Workshop"
startTime < 19:00

One possible workaround that I implement in SpecialQuery3 is convert the first
inequality to a combination of equality filters as follows:

typeOfSession = "Lecture" or
typeOfSession = "Keynote" or
and so forth.

Please see Endpoints method "SpecialQuery3" to fully understand how it works.

===============================================================================
TASK 4 IMPLEMENTATION NOTES
===============================================================================

Here, I implement a push queue task to set a speaker as Featured Speaker in
memcache if the speaker has more than one sessions at the given conference.

For details see:

_createSessionObject (conference.py)
_createFeaturedSpeakerAd (conference.py)
getFeaturedSpeaker (conference.py)
SetFeaturedSpeaker (main.py)

