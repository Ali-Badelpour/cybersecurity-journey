# Chapter 2: Basic SPL (Search Processing Language)

**Objective:** Learn the fundamentals of Splunk's Search Processing Language (SPL) to search, filter, analyze, and visualize your log data. By the end of this chapter, I will be able to write powerful searches that extract meaningful insights from my authentication logs.

**Date:** July 2026

**Prerequisites:**
- Splunk Enterprise installed and running on Windows 11
- Splunk Universal Forwarder installed on Kali Linux
- Data flowing from `/var/log/auth.log` into Splunk

---

## Table of Contents
1. [What is SPL?](#1-what-is-spl)
2. [The Anatomy of a Search](#2-the-anatomy-of-a-search)
3. [Core Filtering Commands](#3-core-filtering-commands)
4. [Transforming Commands: stats](#4-transforming-commands-stats)
5. [Visualizing Data: timechart](#5-visualizing-data-timechart)
6. [Creating New Fields: eval](#6-creating-new-fields-eval)
7. [Enriching Data: lookup](#7-enriching-data-lookup)
8. [Correlating Events: subsearch](#8-correlating-events-subsearch)
9. [Practice Exercises](#9-practice-exercises)
10. [Troubleshooting Reference](#10-troubleshooting-reference)

---

## 1. What is SPL?

SPL (Search Processing Language) is Splunk's proprietary query language. It is **pipeline-based**, meaning data flows from one command to the next, with each command transforming the data in some way.

**Key Concept:** The pipe (`|`) character separates commands. Data flows from left to right through the pipeline.

```
index=main | stats count | sort -count
```

Think of it like an assembly line:
1. **First**, you retrieve raw events (`index=main`)
2. **Then**, you calculate statistics (`stats count`)
3. **Finally**, you sort the results (`sort -count`)

**SPL vs SQL:** If you know SQL, SPL is similar but operates on events rather than tables.

---

## 2. The Anatomy of a Search

Every search in Splunk has three main components:

### 2.1 The Generating Command
This is the first part of my search. It tells Splunk **what data** to look at.

**Most common:**
```
index=main
```
or
```
index=main sourcetype=auth
```

I can also use:
- `index=*` → All indexes (slow, use sparingly)
- `source=/var/log/auth.log` → Specific file path
- `host=kali` → Specific host

My examples:

```
index=main sourcetype=auth
index=main source=/var/log/auth.log
index=main host=kali
```

![Search 1](pictures/splunk-windows-spl-1.png)
![Search 2](pictures/splunk-windows-spl-2.png)
![Search 3](pictures/splunk-windows-spl-3.png)



### 2.2 The Time Range
Always specify a time range! This is the #1 performance optimization.

- **In the UI:** I can use the time picker in the top-right corner.
- **In the search:** `earliest=-1h` (last hour), `latest=now`

**Example:**
```
index=main earliest=-24h
```

My example:
```
index=main host=kali earliest=-144h
```

![Time range example](pictures/splunk-windows-spl-4.png)

### 2.3 The Pipeline (Commands after `|`)
Each pipe (`|`) transforms your results.

**Simple pipeline:**
```
index=main sourcetype=auth | head 10
```
This returns only the first 10 events.

My example:
```
index=main host=kali earliest=-144h | head 3
```

![Pipeline example](pictures/splunk-windows-spl-5.png)


---

## 3. Core Filtering Commands

These commands help you narrow down your results.

### 3.1 `search` (Implicit)
The first part of your search is always a search command. You don't need to type `search`—it's implicit.

```
index=main "Accepted password"
```
This finds all events containing the phrase "Accepted password".
![Search implicit](pictures/splunk-windows-spl-6.png)

My example:
```
index=main "ssh"
```

![Search SSH](pictures/splunk-windows-spl-7.png)

### 3.2 `fields`
Keeps or removes specific fields to reduce data volume.

```
index=main sourcetype=auth | fields host, user, src_ip
```
This shows only the `host`, `user`, and `src_ip` fields.

My example:
```
index=main sourcetype=auth | fields host
```

![Fields command](pictures/splunk-windows-spl-8.png)

### 3.3 `where`
Filters events based on a condition.

```
index=main sourcetype=auth | where like(user, "kali")
```
This shows only events where the `user` field contains "kali".

My example:
```
index=main source=/var/log/auth.log | where like (host, "kali")
```

![Where command](pictures/splunk-windows-spl-9.png)
### 3.4 `head` / `tail`
Limits results to the first or last N events.

```
index=main | head 20
```

My example:
```
index=main | tail 5
```

![Tail command](pictures/splunk-windows-spl-10.png)

### 3.5 `dedup`
Removes duplicate events based on a field.

```
index=main sourcetype=auth | dedup user
```
Shows only the most recent event for each unique user.

**⚠️ Warning:** `dedup` uses a lot of your computer's memory because it has to hold all those unique values in RAM to compare them. Only use it on smaller result sets (under 500,000 events) or make sure to limit your time range, otherwise Splunk might complain about memory usage!


My example:
```
index=main sourcetype=auth | dedup host
```

![Dedup command](pictures/splunk-windows-spl-11.png)

---

## 4. Transforming Commands: `stats`

The `stats` command calculates aggregate statistics—this is where I start getting real answers from my data.

### 4.1 Basic `stats` (Without `BY`)
Without a `BY` clause, `stats` returns **one row** summarizing all events.

```
index=main sourcetype=auth | stats count
```
**Result:** Total number of authentication events.

![Stats count](pictures/splunk-windows-spl-12.png)

### 4.2 `stats` with `BY`
With a `BY` clause, you get **one row per distinct value**.

```
index=main sourcetype=auth | stats count BY user
```
**Result:** Number of authentication events per user.

My example:
```
index=main sourcetype=auth | stats count BY host
```

![Stats by host](pictures/splunk-windows-spl-13.png)
### 4.3 Common Stats Functions

| Function | Description | Example |
| :--- | :--- | :--- |
| `count` | Number of events | `stats count BY host` |
| `avg(field)` | Average value | `stats avg(response_time)` |
| `sum(field)` | Total value | `stats sum(bytes)` |
| `min(field)` | Minimum value | `stats min(latency)` |
| `max(field)` | Maximum value | `stats max(latency)` |
| `distinct_count(field)` | Unique values | `stats distinct_count(user)` |
| `values(field)` | List of all values | `stats values(user) BY host` |

### 4.4 Advanced `stats` Example

```
index=main sourcetype=auth "Accepted password"
| stats count AS total_logins, distinct_count(user) AS unique_users BY host
| sort -total_logins
```
**Result:** For each host, shows total logins and unique users, sorted by most logins.
![Advanced stats](pictures/splunk-windows-spl-14.png)

---

## 5. Visualizing Data: `timechart`

The `timechart` command creates charts that show trends over time. The x-axis is always time.
Note: We need to select `Visualization` to see the chart.
### 5.1 Basic `timechart`

```
index=main sourcetype=auth | timechart count
```
**Result:** A chart showing authentication events over time.


My example:
```
index=main error | timechart count
```
**Result**: I can see the numbers and a chart for `errors`.
![Timechart basic](pictures/splunk-windows-spl-15.png)

### 5.2 `timechart` with `BY`

```
index=main sourcetype=auth | timechart count BY user
```
**Result:** Multiple lines showing login activity per user over time.

My example:
```
index=main error | timechart count by user
```

![Timechart by user](pictures/splunk-windows-spl-16.png)

### 5.3 Controlling Time Intervals with `span`

```
index=main sourcetype=auth | timechart span=1h count
```
**Result:** Events grouped by hour.

Common spans: `span=1m` (minute), `span=1h` (hour), `span=1d` (day).

My example:
```
index=main auth | timechart count span=1d by user
```

![Timechart span](pictures/splunk-windows-spl-17.png)

### 5.4 Security Use Case: Detecting Brute-Force Attacks

```
index=main sourcetype=auth "Failed password"
| timechart span=5m count BY user
```
**Result:** A spike in failed password attempts per user—potential brute-force attack!

![Brute force detection](pictures/splunk-windows-spl-18.png)


---

## 6. Creating New Fields: `eval`

The `eval` command creates new fields by calculating values from existing fields.

### 6.1 Basic `eval`

```
index=main sourcetype=auth
| eval is_root = if(user=="root", "YES", "NO")
| table user, is_root
```
**Result:** A table showing whether each login was by the root user.

![Eval basic](pictures/splunk-windows-spl-19.png)

My example:
```
index=main source="/var/log/auth.log" | eval full_message = if(user == "root", "Logged in as Root", "Logged in as Kali") | table full_message
```

![Eval if](pictures/splunk-windows-spl-20.png)
### 6.2 String Concatenation

```
index=main sourcetype=auth
| eval full_message = "User " . user . " logged in."
| table full_message
```

![Eval concatenation](pictures/splunk-windows-spl-21.png)

### 6.3 `eval` with `stats` and Mathematical Operations (Powerful Combination)

```
index=main sourcetype=auth
| stats earliest(_time) as start_time, latest(_time) as end_time by user
| eval session_duration_sec = end_time - start_time
| eval session_duration_min = session_duration_sec / 60
| eval session_duration_hr = session_duration_sec / 3600
| table user, session_duration_sec, session_duration_min, session_duration_hr
```
**Result**: Making a table with 3 different durations of users logged as. 
![Eval stats 1](pictures/splunk-windows-spl-22.png)
![Eval stats 2](pictures/splunk-windows-spl-23.png)



```
index=main sourcetype=auth
| stats count(eval(user=="root")) AS root_logins, count(eval(user!="root")) AS user_logins
```
**Result:** Counts root logins versus non-root logins.
![Eval count](pictures/splunk-windows-spl-24.png)


### 6.5 Common `eval` Functions

| Function | Description | Example |
| :--- | :--- | :--- |
| `if(condition, true, false)` | Conditional logic | `eval status=if(bytes>1000, "large", "small")` |
| `like(field, pattern)` | Pattern matching | `eval is_ssh=if(like(process, "ssh%"), 1, 0)` |
| `upper(field)` / `lower(field)` | Case conversion | `eval user_upper=upper(user)` |
| `now()` | Current timestamp | `eval age_seconds=now() - _time` |

---

## 7. Enriching Data: `lookup`

Lookups enrich your events with external data from CSV files or other sources.

### 7.1 Creating a Lookup File

1. Create a CSV file called `users.csv`:
```csv
username,department,role
kali,IT,Security
admin,IT,Administrator
```

![CSV file](pictures/splunk-windows-spl-25.png)

2. Upload it to Splunk:
   - **Settings** → **Lookups** → **Lookup table files**
   - Click **Add new** and upload `users.csv`
![Lookup upload 1](pictures/splunk-windows-spl-26.png)
![Lookup upload 2](pictures/splunk-windows-spl-27.png)
![Lookup upload 3](pictures/splunk-windows-spl-28.png)


2. Create the lookup definition:
   - **Settings** → **Lookups** → **Lookup definitions**
   - Click **Add new**
   - Name: `user_lookup`
   - Lookup file: `users.csv`
![Lookup definition](pictures/splunk-windows-spl-29.png)
### 7.2 Using the Lookup in a Search

```
index=main sourcetype=auth
| lookup user_lookup username AS user OUTPUT department, role
| table user, department, role
```
**Result:** Each event now shows the user's department and role.
![Lookup search 1](pictures/splunk-windows-spl-30.png)
![Lookup search 2](pictures/splunk-windows-spl-31.png)
### 7.3 Security Use Case: IP Geolocation

Upload a geolocation CSV and use it to map IP addresses to countries:

```
index=main sourcetype=auth
| lookup geo_ip ip AS src_ip OUTPUT country_name
| stats count BY country_name
```

In the part 7.1, I learned that I can create my own `.CSV` file and use it in Splunk. For 7.3, I will create my imaginary geolocation CSV file:

my_geo_lookup.csv
``` csv
src_ip,location
192.168.106.133,Kali VM (LAN)
127.0.0.1,Localhost Loopback
8.8.8.8,Google DNS (Test)
0.0.0.0,Unknown
```

![Geo CSV](pictures/splunk-windows-spl-32.png)
![Geo upload 1](pictures/splunk-windows-spl-33.png)
![Geo upload 2](pictures/splunk-windows-spl-34.png)
![Geo upload 3](pictures/splunk-windows-spl-35.png)
![Geo upload 4](pictures/splunk-windows-spl-36.png)
![Linux lookup 1](pictures/splunk-linux-spl-1.png)
![Linux lookup 2](pictures/splunk-linux-spl-2.png)
![Linux lookup 3](pictures/splunk-linux-spl-3.png)
![IP Geolocation Result](pictures/splunk-windows-spl-37.png)

## What is `rex`? (Learned From DeepSeek AI and Used in My Search)

**`rex`** is Splunk's **Regular Expression** extraction command. It lets you pull out specific pieces of text from the raw log (`_raw`) and save them as new fields that you can use in searches, lookups, or tables.

---

## Breaking Down the Regex (Step-by-Step)

Let's cut it into pieces:

|Part of Regex|What it means|
|---|---|
|`(?<client_ip>`|**Start capturing.** This tells Splunk: "Find whatever matches next, and store it in a new field called `client_ip`."|
|`\d+`|**Digit(s).** `\d` means any number (0-9). `+` means "one or more times". So `\d+` matches `192`, `127`, `8`, etc.|
|`\.`|**Literal dot.** In regex, a plain `.` means "any character". To match an actual period (`.`), we have to "escape" it with a backslash (`\`). This matches the dots between IP octets.|
|`\d+`|(Same as above) Matches the second octet (e.g., `168`, `0`, `8`).|
|`\.`|Another literal dot.|
|`\d+`|Matches the third octet (e.g., `106`, `0`, `8`).|
|`\.`|Another literal dot.|
|`\d+`|Matches the fourth octet (e.g., `133`, `1`, `8`).|
|`)`|**Stop capturing.** End of the `client_ip` field definition.|

**In plain English:**  
"Look anywhere in the log for a pattern that looks like `number.number.number.number`. When you find it, save that exact text (e.g., `127.0.0.1`) into a new field called `client_ip`."


---

## 8. Correlating Events: `subsearch`

A subsearch uses the results of one search as input for another.
## The Super Simple Analogy

Imagine you walk into a giant library (your Splunk index). You ask a librarian (the subsearch):

- **You say:** _"Go find the person who has failed to log in the most times, and bring me their name."_
    
- **The Librarian goes, counts everything, comes back and says:** _"It is 'kali'."_
    
- **Now YOU say (the outer search):** _"Great. Now show me EVERYTHING that 'kali' did."_
    

That is **exactly** what a subsearch does.
### 8.1 Basic Subsearch Syntax and Security Use Case

Enclose the subsearch in square brackets `[ ]`:

```
index=main sourcetype=auth* [
  search index=main sourcetype=auth* "Failed password"
  | stats count BY host
  | sort -count
  | head 1
  | fields host
]
| stats count BY host
```
Here is what Splunk does behind the scenes:

### Step 1: Splunk runs everything inside the `[ ]` brackets FIRST.

Splunk runs this part:

``` spl
search index=main sourcetype=auth* "Failed password" 
| stats count BY host 
| sort - count 
| head 1 
| fields host
```

**Result of Step 1:** Splunk gets a single piece of text: **`kali`**. (Because `host="kali"` has the most failed logins).

### Step 2: Splunk takes that result and stuffs it into the outer search.

Splunk literally deletes the `[ ... ]` brackets and **replaces them** with a search filter.

- Your outer search was: `index=main sourcetype=auth* [ ... ] | stats count BY host`
    
- Splunk changes it to: **`index=main sourcetype=auth* host="kali" | stats count BY host`**
    

### Step 3: Splunk runs this final, rewritten query.

Now it's just a simple search for `host="kali"`. It counts all the events where `host` is `kali`, and you get **948**.

![Subsearch result](pictures/splunk-windows-spl-38.png)

## The Only Two Things You Need to Remember

1. **A subsearch is just a shortcut** to avoid typing a specific value (like `kali`) manually. It figures out that value for you.
    
2. **You can only use it with fields that already exist in your raw data** (like `host`, `user`, `sourcetype`, `rhost`). If you create a field with `rex`, you **cannot** use it in a subsearch filter.

### 8.2 Important Subsearch Limitations

- Subsearches return a maximum of **50,000 results** by default
- They can be slow on large datasets—use them sparingly
- They cannot be used with `timechart` directly

---

## 9. Practice Exercises

Try these exercises using your own `auth.log` data.

### Exercise 1: Count Logins by Host
**Goal:** Show how many times each host logged in.

```
index=main sourcetype=auth* "Accepted password"
| stats count by host
| table host, count
```

![Exercise 1](pictures/splunk-windows-spl-39.png)

### Exercise 2: Login Activity Over Time
**Goal:** Show login attempts per hour for the last 24 hours.

```
index=main sourcetype=auth* earliest=-744h
| timechart span=1h count
```

![Exercise 2](pictures/splunk-windows-spl-40.png)
### Exercise 3: Failed vs Successful Logins
**Goal:** Compare failed and successful login counts.

```
index=main sourcetype=auth* earliest=-720h
| stats count(eval(like(_raw, "%Accepted%"))) AS successful, count(eval(like(_raw, "%Failed%"))) AS failed
```

![Exercise 3](pictures/splunk-windows-spl-41.png)

### Exercise 4: Find Suspicious Activity
**Goal:** Find hosts with more than 3 failed logins.

```
index=main sourcetype=auth* "Failed password"
| stats count AS failures BY host
| where failures > 3
| sort -failures
```

![Exercise 4](pictures/splunk-windows-spl-42.png)
### Exercise 5: Root Activities
**Goal:** Show all activities of root.

```
index=main sourcetype=auth* "Accepted password" root
| table _time, host, sourcetype
```

![Exercise 5 1](pictures/splunk-windows-spl-43.png)
![Exercise 5 2](pictures/splunk-windows-spl-44.png)

---

## 10. Troubleshooting Reference

### Issue: "No results found"

**Solutions:**
1. Check your time range—are you looking at the right period?
2. Verify your index name: `index=*` to see all indexes
3. Check sourcetype: `| stats count BY sourcetype` to see available types

### Issue: Search is very slow

**Solutions:**
1. Always specify an index: `index=main` not `index=*`
2. Use the smallest time range possible
3. Filter early: Put restrictive conditions before the first pipe
4. Use `fields` to reduce data volume

### Issue: Stats aren't grouping correctly

**Solution:** Verify field names are spelled correctly:
```
| fields
```
This shows all available fields in your events.

### Issue: Eval expressions not working

**Solution:** Check data types:
```
| eval type = typeof(field_name)
```
This shows whether a field is a string, number, etc.

---

## ✅ Milestone Achieved

At this point, I can:
- ✅ Write basic SPL searches with filters
- ✅ Use `stats` to aggregate and summarize data
- ✅ Create time-based visualizations with `timechart`
- ✅ Create new fields with `eval`
- ✅ Enrich data with `lookup` tables
- ✅ Correlate events with `subsearch`
- ✅ Apply these skills to real security use cases

**Next Steps:** Continue to Chapter 3: Installing Sysmon on Windows for Advanced Logging, where I will collect Windows Security events and apply these SPL skills to detect threats in a Windows environment.

---

## 📚 References

- [Splunk Search Reference](https://help.splunk.com/en/splunk-cloud-platform/search/search-reference/10.4.2604/introduction/welcome-to-the-search-reference)
- [Get Started with Search](https://help.splunk.com/en/splunk-enterprise/search/search-manual/10.4/search-overview/get-started-with-search)
- [stats Command Documentation](https://help.splunk.com/en/splunk-enterprise/spl-search-reference/10.4/search-commands/stats)
- [timechart Command Examples](https://docs.splunk.com/Documentation/SCS/latest/SearchReference/TimechartCommandExamples)
- [lookup Command Documentation](https://help.splunk.com/en/splunk-enterprise/spl-search-reference/9.4/search-commands/lookup)
- [Subsearch Documentation](https://help.splunk.com/en/splunk-enterprise/search/search-manual/9.2/subsearches/use-subsearch-to-correlate-events)
- [eval Command Documentation](https://help.splunk.com/en/splunk-enterprise/spl-search-reference/10.4/search-commands/eval)
- `DeepSeek AI`
