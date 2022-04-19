# (PART) Basics {-}

# An introductory example {#intro-example}

Before diving in to details, let's work through a simple real-life example. We'll explain the functions used in more detail in later sections, but here you can see what the tool looks like in action.

Here is our scenario: A fundraiser just called, and is trying to organize an event in San Francisco that will feature a well-known technologist. She wants you to help her find prospects to invite to the event. We start out by loading the discoveryengine:


```r
library(discoveryengine)
```

```
## Welcome to Disco Engine version 0.4.2
## Cheat sheet: https://cwolfsonseeley.github.io/discodocs/cheat-sheet.html
## News and bugfixes: https://github.com/cwolfsonseeley/discoveryengine/blob/master/NEWS.md
```

## Develop a strategy

You might already be thinking of what sorts of prospects we should look for. That's good! There are countless ways to respond to this request, and you'll have to rely on your own expertise in order to know who to include on a potential invite list. For this example, I'll use the following definition of a good prospect:

- has demonstrated an interest in technology, and ...
- lives or works in/near San Francisco

Note that we now have a pretty clear idea of who to look for, but we need more precision. For instance, what constitutes a "demonstrated interest in technology?" What does it mean to be "in/near San Francisco?" In day-to-day conversations, we don't necessarily need that level of precision to understand one another, but since we'll need to translate this request into language a computer can understand, the more precision the better. I'll define a "demonstrated interest in technology" as having either an interest code or a philanthropic affinity related to technology, and "in/near San Francisco" as anywhere that falls into the San Francisco Metropolitan Statistical Area (MSA).

## Create the definition {#intro-example-create-def}

So now that we have a precise idea of who we want to find, we can create a definition that the computer will understand. This is important, since our computer is the only link we have to the CADS Data Warehouse. We can give it a precise definition in a language that it understands, and it will then relay our request to the CADS Data Warehouse. The CADS data warehouse can then search for people who meet our definition, and send them back to the computer, which will then display the list to us. 

Ideally, our definition would look like this: `has_tech_interest %and% is_in_sf`. Of course, we haven't defined those pieces yet, so just typing that in will result in an error


```r
has_tech_interest %and% is_in_sf
```

```
## Error in operate(block1, block2, "intersect"): object 'has_tech_interest' not found
```

But still we've made progress. We broke a big problem ("define a prospect for this SF tech event") into two smaller problems ("define having a tech interest" and "define being in San Francisco"). Let's tackle each of these one by one. We start by defining `has_tech_interest`. Recall that we decided to use interest codes and philanthropic affinities here. I happen to already know that there is an interest code for "technology", but I'm less familiar with the philanthropic affinities area. So I'll use the search feature that is built into most widgets, accessed by entering a question mark followed by a search term into the widget:


```r
has_philanthropic_affinity(?tech)
```

```
## Warning: `tbl_df()` was deprecated in dplyr 1.0.0.
## Please use `tibble::as_tibble()` instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was generated.
```

```
## Regular codes and synonyms:
##             synonym code
##  science_technology   ST
```

Ah, ok. I can use either the code `ST` or the synonym `science_technology`.


```r
has_tech_interest = has_interest(technology) %or%
    has_philanthropic_affinity(science_technology)
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

We use `%or%` as the connector here because we're interested in anyone who has either one of these characteristics. We now move to defining `is_in_sf`. We'd like to look for anyone who lives or works in San Francisco:


```r
is_in_sf = lives_in_msa(san_francisco) %or%
    works_in_msa(san_francisco)
```

We are now able to create our full definition by combining the pieces:


```r
event_prospect = has_tech_interest %and% is_in_sf
```

## Send the definition to the CDW

As we discussed in the previous section, now that we have an official definition we can send it to the data warehouse, and see if the data warehouse finds anyone who fits. You can see the definition for yourself by just typing the name of it, though it is now written in a language for the computer to understand and may look intimidating to us humans:







