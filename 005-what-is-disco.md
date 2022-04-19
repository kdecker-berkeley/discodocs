# (PART) Preliminaries {-}

# What is the Disco Engine? {#what-is-disco}



If you happen to have an APRA account, this [article in APRA Connections magazine](http://connections.aprahome.org/blog/building-the-discovery-engine) is a good introduction. If you don't have an APRA account, then read on . . .

The Discovery Engine, or Disco Engine, is a prospecting tool that empowers Prospect Development staff to precisely define (and pull together) constitutencies for their fundraising clients. It does so by providing two complementary resources: a *vocabulary and grammar* for specifying constituencies, and tools to facilitate *discovery* of constituencies. 

## Vocabulary and Grammar

The Disco Engine is based on the idea that a constituency can be defined using one or more *predicates* (such as "majored in math" or "attended a Discover Cal event"), where multiple predicates are combined using "and", "or", or "but not." To illustrate, here is a sample constituency definition for the department of mathematics:

> *majored in mathematics* **or** *has given to the math department*

The constituency is then just any individual who fits the definition. 

The most basic tool that the Disco Engine provides is called a [widget](#working-with-widgets), which is a tool for generating a simple predicate. For instance, the widget `majored_in` allows us to build the first part of our example definition:


```r
majored_in(mathematics)
```

Furthermore, the Disco Engine provides the following connectors to *combine* predicates in order to create more complex definitions:

- `%and%`
- `%or%`
- `%but_not%`

So in the language of the Disco Engine, our definition would look like:


```r
majored_in(mathematics) %or% gave_to_department(mathematics)
```

There is no limit to the number of predicates that can be strung together to form a definition:


```r
majored_in(mathematics) %or%
    gave_to_department(mathematics) %and%
    lives_in_msa(san_francisco)
```

These definitions can quickly become very complex. To manage that complexity, the Disco Engine provides the ability to *assign* names to definitions, that can then be used to construct more complex definitions:


```r
is_math_constituent = majored_in(mathematics) %or% 
    gave_to_department(mathematics)
```

```
## Warning: `tbl_df()` was deprecated in dplyr 1.0.0.
## Please use `tibble::as_tibble()` instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was generated.
```

```r
local_math_constituent = is_math_constituent %and%
    lives_in_msa(san_francisco)

parent_of_local_math_constituent = parent_of(local_math_constituent)
```

Together, the collection of predicates, the logical operators, and the ability to assign names form an expressive language for describing custom constituencies for a variety of purposes. Best of all, the Disco Engine gives us the ability to pull the IDs of a defined constituency from the database!


```r
display(parent_of_local_math_constituent)
```

```
## # A tibble: 622 x 1
##    entity_id
##        <dbl>
##  1      1168
##  2      6322
##  3      6326
##  4      7644
##  5     11512
##  6     22093
##  7     24151
##  8     24247
##  9     26492
## 10     26968
## # ... with 612 more rows
```

## Facilitating discovery

Having a clear language for prospecting and defining constituencies is nice, but Prospect Development is increasingly being asked to do affinity-based prospecting, which often involves defining new constituencies that don't have clear-cut coding associated with them yet, or digging into student activity, interest, affiliation, and other codes to figure out which ones might fit the constituency to be defined. That need motivates the Disco Engine's bots, such as the [brainstorm bot](#brainstorm-bot) and the [matrix bot](#matrix-bot). We've already seen examples of using predicates to find individuals, but bots allow us to search for predicates.


```r
brainstorm_bot("robot*", "artificial intelligence", "AI")
```

```
## Rows: 360 Columns: 3
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (3): INTEREST_CODE, SHORT_DESC, INTEREST_GROUP_CODE
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```
## Warning in stri_replace_first_regex(string, pattern,
## fix_replacement(replacement), : argument is not an atomic vector; coercing

## Warning in stri_replace_first_regex(string, pattern,
## fix_replacement(replacement), : argument is not an atomic vector; coercing
```

```
## Committee categorization comes from the Center for Responsive Politics (www.OpenSecrets.org)
## Rows: 360 Columns: 3-- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (3): INTEREST_CODE, SHORT_DESC, INTEREST_GROUP_CODE
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```
## attended_event 
##     9136: AI Getting Smart about Start-Ups
##     9138: AI Learn how to Write a Powerful Resume
##     9431: SVF: Robots on the Edge
##     9147: AI Coaching with Niandong Wang MBA '04
##     10956: Panel Guiding UC's Responsible Use of AI
##     6724: Haas East Bay Chp Robotic Event 06-18-14
##     9140: AI Career Coach Information Session
##     9325: BARS 2017: Bay Area Robotics Symposium
##     9361: Inclusive AI: Technology and Policy
##     10752: Career Connections: AI & Data Analytics
##     9498: WIT: Future of AI
##     10043: Discover Cal: Artificial Intelligence
##     8759: Fireside Chat w/Tom Siebel:  AI and IoT
##     8760: Robots on the Edge:Intelligent Machines
##     8761: Silicon Valley Forum: Medical Robotics
##     9139: AI Energy Alumni Panel & Firm Night
##     10154: Career Connections: AI & Data Analytics
##     9141: AI Business Behaving Well: Social Resp.
##     9280: AL SD Content AI Impact on Life
##     9497: Putting AI to Work: Tech & Policy
## attended_hs 
##     471828: Cb-Ai Operations S&L
## fec_gave_to_candidate 
##     H0NY14353: ROBOT, RALLY
##     P00015610: ROBOT, RALLY
## fec_gave_to_committee 
##     C00582767: CAMPAIGN FOR A FEDERAL ROBOTICS COMMISSION
##     C00717074: RALLY ROBOT 2020
##     C00635748: AI PAC
##     C00521765: LIQUID ROBOTICS INC POLITICAL ACTION COMMITTEE (LIQUID ROBOTICS PAC)
##     C00596908: ROBOTECHNOLOGY INC C.R.
##     C00144261: APPRAISAL INSTITUTE PAC (AI PAC)
##     C00750026: C3.AI INC. PAC
##     C00717249: RALLY ROBOT ZAPS AOC
## has_interest 
##     ROB: Robotics
##     AIN: Artificial Intelligence
## has_philanthropic_interest 
##     II: Data Science & Artificial Intelligence
## participated_in 
##     ENRO: Cal Robotics
##     UROB: Robotmedia Presents
##     RENY: Robotics & Engineering for Youth
## sec_filed 
##     1528557: Corindus Vascular Robotics, Inc.
##     1409269: Restoration Robotics Inc
##     1577526: C3.ai, Inc.
##     1409269: Restoration Robotics, Inc.
```
