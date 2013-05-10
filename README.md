# MiniProfiler data structures

This document describes the MiniProfiler data structures.

## Profile

The root element.

 - **Id** (string) - a unique identifier, like a guid
 - **Name** (string) - request name, generally `Controller/Route`
 - **Started** (number) - request start time, in milliseconds since Unix epoch
 - **DurationMilliseconds** (number) - duration of request in milliseconds
 - **MachineName** (string) - name of the server
 - **CustomLinks** (object) - object of links; keys are names, values are URLs
 - **Root** (Timing) - root timing object
 - **ClientTimings** (ClientTimings) - client timings object

## Timing

 - **Id** (string) - a unique identifier, like a guid
 - **Name** (string) - step name
 - **StartMilliseconds** (number) - step start time, in milliseconds since start of request
 - **DurationMilliseconds** (number) - duration of step in milliseconds (including children)
 - **Children** (array of Children, optional) - list of Child steps
 - **CustomTimings** (object, optional) - object with keys as call type ("redis", "sql", etc.) and values as arrays of CustomTiming of recorded calls (does not include child step's custom timings)

## CustomTiming

 - **Id** (string) - a unique identifier, like a guid
 - **ExecuteType** (string, optional) - execute type of call ("read", "write", "query", etc.)
 - **CommandString** (string) - HTML-escaped command; rendered in a pre, so supports newlines
 - **StackTraceSnippet** (string) - shortened, one-line stack trace; generally all function names on the stack separated by a space
 - **StartMilliseconds** (number) - call start time, in milliseconds since start of request
 - **DurationMilliseconds** (number) - duration of call in milliseconds
 - **FirstFetchDurationMilliseconds** (number, optional) - duration in milliseconds of the time to first result

## ClientTimings

This data should be recorded by the browser and reported back to MiniProfiler.

 - **RedirectCount** (number) - redirect count
 - **Timings** (array of ClientTiming)

## ClientTiming

 - **Name** (string)
 - **Start** (number)
 - **Duration** (number)

## Example

```
{
    "Id": "934a392e-9669-48f2-2797-3f88ae87566e",
    "Name": "goapp.Main",
    "Started": 1368209567000,
    "MachineName": "mjibson-mbp.local",
    "Root": {
        "Id": "b78bf93b-2ed4-43ab-235c-9bc25d7f2c0a",
        "Name": "GET http://localhost:8080/",
        "DurationMilliseconds": 4.55,
        "StartMilliseconds": 0,
        "Children": null,
        "CustomTimings": {
            "memcache": [
                {
                    "Id": "",
                    "ExecuteType": "Get",
                    "CommandString": "Get\n\nkey:&#34;agtkZXZ-Z28tcmVhZHIcCxIBVSIVMTg1ODA0NzY0MjIwMTM5MTI0MTE4DA&#34; for_cas:true ",
                    "StackTraceSnippet": "Call Call GetMulti Get includes Main",
                    "StartMilliseconds": 0.346,
                    "DurationMilliseconds": 2.592,
                    "FirstFetchDurationMilliseconds": 0
                }
            ]
        }
    },
    "User": "",
    "HasUserViewed": true,
    "ClientTimings": {
        "RedirectCount": 0,
        "Timings": [
            {
                "Name": "Domain Lookup",
                "Start": 3,
                "Duration": 1
            },
            {
                "Name": "Connect",
                "Start": 4,
                "Duration": 0
            },
            {
                "Name": "Request",
                "Start": 4,
                "Duration": -1
            },
            {
                "Name": "Response",
                "Start": 21,
                "Duration": 1
            },
            {
                "Name": "Unload Event",
                "Start": 22,
                "Duration": 0
            },
            {
                "Name": "Dom Content Loaded Event",
                "Start": 180,
                "Duration": 63
            },
            {
                "Name": "Load Event",
                "Start": 312,
                "Duration": 2
            }
        ]
    },
    "DurationMilliseconds": 4.55,
    "CustomLinks": {
        "appstats": "http://localhost:8080/_ah/stats/details?time=879202000"
    }
}
```
