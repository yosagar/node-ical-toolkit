#ICal Toolkit NodeJS#
ICal generator/updater/parser with Timezone/DST, Alams, Organizers, Events, etc. support.

**Module under dev, update existing icals under dev**

##Install##
```bash
> npm install ical-toolkit
```

##Builder##
Bad quick documentation, but covers all and will get you going quick!
```javascript
var icalToolkit = require('../lib/main');

var builder = icalToolkit.createIcsFileBuilder();

/*
 * Settings (All Default values below)
 * */
builder.spacers = true; //Add space in ICS file, better reading
builder.NEWLINE_CHAR = '\r\n'; //Newline char to use
builder.throwError = false; //If true throws errors, else returns error
builder.ignoreTZIDMismatch = true; //If TZID is invalid, ignore or not to ignore!


/**
 * Build ICS
 * */
builder.calname = 'Yo Cal';
builder.timezone = 'asia/kalcutta';
builder.tzid = 'america/new_york';

builder.events.push({
  //Required: type Date()
  start: new Date(),
  //Required: type Date()
  end: new Date(),
  //Required: type String
  summary: 'Test Event',

  //All Optionals Below
  alarms: [15, 10, 5], //Alarms, array in minutes
  additionalTags: {
    'SOMETAG': 'SOME VALUE'
  }, //Optional
  uid: null, //Optional, default autogenerated
  sequence: null, //The sequence number in update, Default: 0
  repeating: {
    freq: 'DAILY',
    count: 10,
    interval: 10,
    until: new Date()
  },
  allDay: true,
  stamp: new Date(),
  floating: false,
  location: 'Home',
  description: 'Testing it!',
  organizer: {
    name: 'Kushal Likhi',
    email: 'test@mail'
  },
  method: 'PUBLISH',
  status: 'CONFIRMED',
  url: 'http://google.com'
});

builder.additionalTags = {
  'SOMETAG': 'SOME VALUE'
};

var icsFileContent = builder.toString();

if (icsFileContent instanceof Error) {
  console.log('Returned Error, you can also configure to throw errors!');
  //handle error
}

console.log(icsFileContent);

```
####Output####
```
BEGIN:VCALENDAR
CALSCALE:GREGORIAN
VERSION:2.0
X-WR-CALNAME:Yo Cal
METHOD:PUBLISH
PRODID:node-ical-toolkit
X-WR-TIMEZONE:asia/kalcutta

BEGIN:VTIMEZONE
TZID:America/New_York
X-LIC-LOCATION:America/New_York

BEGIN:DAYLIGHT
TZOFFSETFROM:-0500
TZOFFSETTO:-0400
TZNAME:EDT
DTSTART:19700308T020000
RRULE:FREQ=YEARLY;BYMONTH=3;BYDAY=2SU
END:DAYLIGHT


BEGIN:STANDARD
TZOFFSETFROM:-0400
TZOFFSETTO:-0500
TZNAME:EST
DTSTART:19701101T020000
RRULE:FREQ=YEARLY;BYMONTH=11;BYDAY=1SU
END:STANDARD

END:VTIMEZONE


BEGIN:VEVENT
UID:49635088
DTSTAMP:20150307T131646Z
DTSTART;VALUE=DATE:20150307
DTEND;VALUE=DATE:20150307
SUMMARY:Test Event
SEQUENCE:0
LOCATION:Home
DESCRIPTION:Testing it!
URL;VALUE=URI:http://google.com
STATUS:CONFIRMED
ORGANIZER;CN="Kushal Likhi":mailto:test@mail

BEGIN:VALARM
TRIGGER:-PT15M
ACTION:DISPLAY
END:VALARM


BEGIN:VALARM
TRIGGER:-PT10M
ACTION:DISPLAY
END:VALARM


BEGIN:VALARM
TRIGGER:-PT5M
ACTION:DISPLAY
END:VALARM

RRULE:FREQ=DAILY;COUNT=10;INTERVAL=10;UNTIL=20150307T131646Z
SOMETAG:SOME VALUE
END:VEVENT

SOMETAG:SOME VALUE
END:VCALENDAR
```

##Parser##
Parse the ics files to JSON structures
```javascript
    it('should parse the content sync', function (done) {
      var json = icalToolkit.parseToJSON(icsContent);
      assert(json);
      assert(!(json instanceof Error));
      assert(json.VCALENDAR);
      done();
    });

    it('should parse the content async', function (done) {
      icalToolkit.parseToJSON(icsContent, function (err, json) {
        if (err) throw err;
        assert(json);
        assert(!(json instanceof Error));
        assert(json.VCALENDAR);
        done();
      });
    });

    it('should parse the file', function (done) {
      icalToolkit.parseFileToJSON(sampleFilePAth, function (err, json) {
        if (err) throw err;
        assert(json);
        assert(!(json instanceof Error));
        assert(json.VCALENDAR);
        done();
      });
    });
 
    it('should parse the file sync', function (done) {
      var json = icalToolkit.parseFileToJSONSync(sampleFilePAth);
      assert(json);
      assert(!(json instanceof Error));
      assert(json.VCALENDAR);
      done();
    });        
    
```
