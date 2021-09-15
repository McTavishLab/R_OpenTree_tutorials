---
source: Rmd
title: "Finding your taxa in the Open Tree of Life Taxonomy"
teaching: 5
exercises: 5
questions:
- "What is the Open Tree of Life Taxonomy?"
- "What are OTT ids?"
- 'What does TNRS stand for?'
objectives:
- "Getting OTT ids for some taxa."
- "Understanding TNRS and approximate matching."
# - "Finding the utility of taxonomic contexts"
# - "Discovering functions to handle a 'match_names' object."
keypoints:
- "Open Tree of Life Taxonomy ids, or OTT ids are unique numeric identifiers for individual taxa that the Open Tree of Life project uses to handle taxonomy."
# - "taxonomic context is very important to find the correct OTT ids for our taxa."
- "You can go from a scientific name to an OTT id using TNRS matching."
- "You can not go from a common name to OTT id using the Open Tree of Life tools."
---

```
## Error in library("emo"): there is no package called 'emo'
```
<br/>
<br/>

<!-- ### The Open Tree Taxonomy and its identifiers -->

The [Open Tree of Life Taxonomy](https://tree.opentreeoflife.org/about/taxonomy-version/ott3.2) (**OTT** from now on) synthesizes taxonomic information from different sources and assigns each taxon a unique numeric identifier, which we refer to as the **OTT id**. To interact with the OTT (and any other Open Tree of Life services) using R, we will learn how to use the functions from the `rotl` package. If you don't know if you have the package installed, go to [setup](../setup.html) and follow the instructions there.

To deal with synonyms and scientific name misspellings, the Open Tree Taxonomy uses
the [Taxonomic Name Resolution Service](http://tnrs.iplantcollaborative.org/) (**TNRS**
from now on), that allows linking scientific names to a unique OTT id, while dealing
with misspellings, synonyms and scientific name variants. The functions from `rotl` that interact
with OTT's TNRS start with "tnrs_".

<br/>

### Getting OTT ids for a taxon

To get OTT ids for a taxon or set of taxa we will use the function `tnrs_match_names()`.
This function takes a character vector of one or more scientific names as main argument.

> ## Hands on! Running TNRS
>
> Do a `tnrs_match_names()` run for the amphibians (Amphibia). Save the output to an object named `resolved_name`.
>
> You can try different misspellings and synonyms of your taxon to see TNRS in action.
>
>
> 
> ```r
> resolved_name <- rotl::tnrs_match_names(names = "amphibians")
> resolved_name
> ```
> 
> ```
> ##   search_string unique_name approximate_match ott_id is_synonym flags
> ## 1    amphibians    Amphibia              TRUE 544595      FALSE      
> ##   number_matches
> ## 1              6
> ```
>
>
{: .challenge}

Ok, we were able to run the function `tnrs_match_names` successfully. Now, let's explore the structure of the output.

<br/>

### The 'match_names' object

As we can tell from the data printed to screen, the output of the `tnrs_match_names` function is some sort of a data table. In R (and all object-oriented programmming languages), defined data structures called [**classes**](https://www.datamentor.io/r-programming/object-class-introduction/) are assigned to objects. This makes data manipulation and usage of objects across different functions much easier.
Redundantly, a class is defined as a data structure that is the same among all objects that belong to the same class. However, we can do more to understadn the structure of any class,
To get the name of the class of the `tnrs_match_names()` output, we will use the function `class`.


```r
class(resolved_name)
```

```
## [1] "match_names" "data.frame"
```

<br/>

As you can see, an object can belong to one or more classes.

Indeed, R is telling us that the output of `tnrs_match_names()` is a data frame (a type of table) and a **'match_names' object**, which is in turn a data frame with exactly 7 named columns: `search_string`, `unique_name`, `approximate_match`, `ott_id`, `is_synonym`, `flags`, and `number_matches`.
<!-- **search_string**, **unique_name**, **approximate_match**, **ott_id**, **is_synonym**, **flags**, and **number_matches**. -->

Next we will explore the kinds of data that are stored in each of the columns of a 'match_names' object.

<br/>

### Kinds of data stored in a 'match_names' object

You should have a good idea by now of what type of data is stored in the `ott_ids` column.

Can you guess what type of data is displayed in the column `search_string` and `unique_name`?

How about `is_synonym`?

The column `approximate_match` tells us whether the unique name was inferred from the search string using approximate matching (TRUE) or not (FALSE).

<!-- The column `number_matches` tells us how many -->

Finally, the `flags` column tells us if our unique name has been flagged in the OTT
(TRUE) or not (FALSE). It also indicates the type of flag associated to the taxon. Flags are markers that indicate if the taxon in question is problematic and should be included in further analyses of the Open Tree workflow. You can read more about flags in the [Open Tree wiki](https://github.com/OpenTreeOfLife/reference-taxonomy/wiki/Taxon-flags).

Now we know what kind of data is retrieved by the `tnrs_match_names()` function. Pretty cool!

<br/>

> > ## Pro tip 1.1: Looking at "hidden" elements of a data object
> >
> > The 'match_names' object has more data that is not exposed on the screen and is not part of the main data structure. This "hidden" data is stored in the attributes of the object.
> > All objects have at least one attribute, the class. If an object has more attributes, these can be accesed with the function `attributes()`.
> >
> > Let's explore the attributes and class of a basic object, such as a character vector. It certainly has a class:
> >
> > 
> > ```r
> > class(c("Hello!", "my", "name", "is", "Luna!"))
> > ```
> > 
> > ```
> > ## [1] "character"
> > ```
> > But what about other attributes:
> >
> > 
> > ```r
> > attributes(c("Hello!", "my", "name", "is", "Luna!"))
> > ```
> > 
> > ```
> > ## NULL
> > ```
> >
> > As you can see, some objects have no hidden attributes.
> >
> > Let's look for hidden attributes on our 'match_names' object:
> >
> > 
> > ```r
> > attributes(resolved_name)
> > ```
> >
> > The structure of the "attributes" data is complicated and extracting it requires some exploring.
> >
> > 
> > ```r
> > class(attributes(resolved_name))
> > ```
> > 
> > ```
> > ## [1] "list"
> > ```
> > 
> > ```r
> > names(attributes(resolved_name))
> > ```
> > 
> > ```
> > ## [1] "names"              "row.names"          "original_order"    
> > ## [4] "original_response"  "match_id"           "has_original_match"
> > ## [7] "class"
> > ```
> > 
> > ```r
> > str(attributes(resolved_name))
> > ```
> > 
> > ```
> > ## List of 7
> > ##  $ names             : chr [1:7] "search_string" "unique_name" "approximate_match" "ott_id" ...
> > ##  $ row.names         : int 1
> > ##  $ original_order    : num 1
> > ##  $ original_response :List of 10
> > ##   ..$ context                     : chr "All life"
> > ##   ..$ governing_code              : chr "undefined"
> > ##   ..$ includes_approximate_matches: logi TRUE
> > ##   ..$ includes_deprecated_taxa    : logi FALSE
> > ##   ..$ includes_suppressed_names   : logi FALSE
> > ##   ..$ matched_names               :List of 1
> > ##   .. ..$ : chr "amphibians"
> > ##   ..$ results                     :List of 1
> > ##   .. ..$ :List of 2
> > ##   .. .. ..$ matches:List of 6
> > ##   .. .. .. ..$ :List of 7
> > ##   .. .. .. .. ..$ is_approximate_match: logi TRUE
> > ##   .. .. .. .. ..$ is_synonym          : logi FALSE
> > ##   .. .. .. .. ..$ matched_name        : chr "Amphibina"
> > ##   .. .. .. .. ..$ nomenclature_code   : chr "ICZN"
> > ##   .. .. .. .. ..$ score               : num 0.778
> > ##   .. .. .. .. ..$ search_string       : chr "amphibians"
> > ##   .. .. .. .. ..$ taxon               :List of 10
> > ##   .. .. .. .. .. ..$ flags                   : list()
> > ##   .. .. .. .. .. ..$ is_suppressed           : logi FALSE
> > ##   .. .. .. .. .. ..$ is_suppressed_from_synth: logi FALSE
> > ##   .. .. .. .. .. ..$ name                    : chr "Succinea"
> > ##   .. .. .. .. .. ..$ ott_id                  : int 978937
> > ##   .. .. .. .. .. ..$ rank                    : chr "genus"
> > ##   .. .. .. .. .. ..$ source                  : chr "ott3.3draft1"
> > ##   .. .. .. .. .. ..$ synonyms                :List of 12
> > ##   .. .. .. .. .. .. ..$ : chr "Amphibia"
> > ##   .. .. .. .. .. .. ..$ : chr "Amphibina"
> > ##   .. .. .. .. .. .. ..$ : chr "Arborcinea"
> > ##   .. .. .. .. .. .. ..$ : chr "Brachyspira"
> > ##   .. .. .. .. .. .. ..$ : chr "Cerinasota"
> > ##   .. .. .. .. .. .. ..$ : chr "Cochlohydra"
> > ##   .. .. .. .. .. .. ..$ : chr "Luccinea"
> > ##   .. .. .. .. .. .. ..$ : chr "Lucena"
> > ##   .. .. .. .. .. .. ..$ : chr "Succinaea"
> > ##   .. .. .. .. .. .. ..$ : chr "Succinastrum"
> > ##   .. .. .. .. .. .. ..$ : chr "Tapada"
> > ##   .. .. .. .. .. .. ..$ : chr "Truella"
> > ##   .. .. .. .. .. ..$ tax_sources             :List of 7
> > ##   .. .. .. .. .. .. ..$ : chr "worms:181586"
> > ##   .. .. .. .. .. .. ..$ : chr "ncbi:145426"
> > ##   .. .. .. .. .. .. ..$ : chr "gbif:2297197"
> > ##   .. .. .. .. .. .. ..$ : chr "irmng:1393632"
> > ##   .. .. .. .. .. .. ..$ : chr "irmng:1348813"
> > ##   .. .. .. .. .. .. ..$ : chr "irmng:1133222"
> > ##   .. .. .. .. .. .. ..$ : chr "irmng:1202351"
> > ##   .. .. .. .. .. ..$ unique_name             : chr "Succinea"
> > ##   .. .. .. ..$ :List of 7
> > ##   .. .. .. .. ..$ is_approximate_match: logi TRUE
> > ##   .. .. .. .. ..$ is_synonym          : logi FALSE
> > ##   .. .. .. .. ..$ matched_name        : chr "Amphibia"
> > ##   .. .. .. .. ..$ nomenclature_code   : chr "ICZN"
> > ##   .. .. .. .. ..$ score               : num 0.75
> > ##   .. .. .. .. ..$ search_string       : chr "amphibians"
> > ##   .. .. .. .. ..$ taxon               :List of 10
> > ##   .. .. .. .. .. ..$ flags                   : list()
> > ##   .. .. .. .. .. ..$ is_suppressed           : logi FALSE
> > ##   .. .. .. .. .. ..$ is_suppressed_from_synth: logi FALSE
> > ##   .. .. .. .. .. ..$ name                    : chr "Amphibia"
> > ##   .. .. .. .. .. ..$ ott_id                  : int 544595
> > ##   .. .. .. .. .. ..$ rank                    : chr "class"
> > ##   .. .. .. .. .. ..$ source                  : chr "ott3.3draft1"
> > ##   .. .. .. .. .. ..$ synonyms                :List of 1
> > ##   .. .. .. .. .. .. ..$ : chr "Lissamphibia"
> > ##   .. .. .. .. .. ..$ tax_sources             :List of 4
> > ##   .. .. .. .. .. .. ..$ : chr "ncbi:8292"
> > ##   .. .. .. .. .. .. ..$ : chr "worms:178701"
> > ##   .. .. .. .. .. .. ..$ : chr "gbif:131"
> > ##   .. .. .. .. .. .. ..$ : chr "irmng:1131"
> > ##   .. .. .. .. .. ..$ unique_name             : chr "Amphibia"
> > ##   .. .. .. ..$ :List of 7
> > ##   .. .. .. .. ..$ is_approximate_match: logi TRUE
> > ##   .. .. .. .. ..$ is_synonym          : logi FALSE
> > ##   .. .. .. .. ..$ matched_name        : chr "Amphibia"
> > ##   .. .. .. .. ..$ nomenclature_code   : chr "ICN"
> > ##   .. .. .. .. ..$ score               : num 0.75
> > ##   .. .. .. .. ..$ search_string       : chr "amphibians"
> > ##   .. .. .. .. ..$ taxon               :List of 10
> > ##   .. .. .. .. .. ..$ flags                   :List of 1
> > ##   .. .. .. .. .. .. ..$ : chr "sibling_higher"
> > ##   .. .. .. .. .. ..$ is_suppressed           : logi FALSE
> > ##   .. .. .. .. .. ..$ is_suppressed_from_synth: logi FALSE
> > ##   .. .. .. .. .. ..$ name                    : chr "Bostrychia"
> > ##   .. .. .. .. .. ..$ ott_id                  : int 782484
> > ##   .. .. .. .. .. ..$ rank                    : chr "genus"
> > ##   .. .. .. .. .. ..$ source                  : chr "ott3.3draft1"
> > ##   .. .. .. .. .. ..$ synonyms                :List of 1
> > ##   .. .. .. .. .. .. ..$ : chr "Amphibia"
> > ##   .. .. .. .. .. ..$ tax_sources             :List of 5
> > ##   .. .. .. .. .. .. ..$ : chr "silva:AF203893/#6"
> > ##   .. .. .. .. .. .. ..$ : chr "ncbi:103711"
> > ##   .. .. .. .. .. .. ..$ : chr "worms:143904"
> > ##   .. .. .. .. .. .. ..$ : chr "gbif:2661216"
> > ##   .. .. .. .. .. .. ..$ : chr "irmng:1282403"
> > ##   .. .. .. .. .. ..$ unique_name             : chr "Bostrychia (genus in kingdom Archaeplastida)"
> > ##   .. .. .. ..$ :List of 7
> > ##   .. .. .. .. ..$ is_approximate_match: logi TRUE
> > ##   .. .. .. .. ..$ is_synonym          : logi FALSE
> > ##   .. .. .. .. ..$ matched_name        : chr "Amphibia"
> > ##   .. .. .. .. ..$ nomenclature_code   : chr "ICZN"
> > ##   .. .. .. .. ..$ score               : num 0.75
> > ##   .. .. .. .. ..$ search_string       : chr "amphibians"
> > ##   .. .. .. .. ..$ taxon               :List of 10
> > ##   .. .. .. .. .. ..$ flags                   : list()
> > ##   .. .. .. .. .. ..$ is_suppressed           : logi FALSE
> > ##   .. .. .. .. .. ..$ is_suppressed_from_synth: logi FALSE
> > ##   .. .. .. .. .. ..$ name                    : chr "Egadroma"
> > ##   .. .. .. .. .. ..$ ott_id                  : int 732965
> > ##   .. .. .. .. .. ..$ rank                    : chr "genus"
> > ##   .. .. .. .. .. ..$ source                  : chr "ott3.3draft1"
> > ##   .. .. .. .. .. ..$ synonyms                :List of 1
> > ##   .. .. .. .. .. .. ..$ : chr "Amphibia"
> > ##   .. .. .. .. .. ..$ tax_sources             :List of 2
> > ##   .. .. .. .. .. .. ..$ : chr "ncbi:247376"
> > ##   .. .. .. .. .. .. ..$ : chr "irmng:1307131"
> > ##   .. .. .. .. .. ..$ unique_name             : chr "Egadroma"
> > ##   .. .. .. ..$ :List of 7
> > ##   .. .. .. .. ..$ is_approximate_match: logi TRUE
> > ##   .. .. .. .. ..$ is_synonym          : logi FALSE
> > ##   .. .. .. .. ..$ matched_name        : chr "Amphibia"
> > ##   .. .. .. .. ..$ nomenclature_code   : chr "ICZN"
> > ##   .. .. .. .. ..$ score               : num 0.75
> > ##   .. .. .. .. ..$ search_string       : chr "amphibians"
> > ##   .. .. .. .. ..$ taxon               :List of 10
> > ##   .. .. .. .. .. ..$ flags                   : list()
> > ##   .. .. .. .. .. ..$ is_suppressed           : logi FALSE
> > ##   .. .. .. .. .. ..$ is_suppressed_from_synth: logi FALSE
> > ##   .. .. .. .. .. ..$ name                    : chr "Stenolophus"
> > ##   .. .. .. .. .. ..$ ott_id                  : int 561664
> > ##   .. .. .. .. .. ..$ rank                    : chr "genus"
> > ##   .. .. .. .. .. ..$ source                  : chr "ott3.3draft1"
> > ##   .. .. .. .. .. ..$ synonyms                :List of 6
> > ##   .. .. .. .. .. .. ..$ : chr "Agonoderos"
> > ##   .. .. .. .. .. .. ..$ : chr "Agonoderus"
> > ##   .. .. .. .. .. .. ..$ : chr "Amphibia"
> > ##   .. .. .. .. .. .. ..$ : chr "Astenolophus"
> > ##   .. .. .. .. .. .. ..$ : chr "Egadroma"
> > ##   .. .. .. .. .. .. ..$ : chr "Stenelophus"
> > ##   .. .. .. .. .. ..$ tax_sources             :List of 3
> > ##   .. .. .. .. .. .. ..$ : chr "ncbi:177549"
> > ##   .. .. .. .. .. .. ..$ : chr "gbif:8401238"
> > ##   .. .. .. .. .. .. ..$ : chr "irmng:1330562"
> > ##   .. .. .. .. .. ..$ unique_name             : chr "Stenolophus"
> > ##   .. .. .. ..$ :List of 7
> > ##   .. .. .. .. ..$ is_approximate_match: logi TRUE
> > ##   .. .. .. .. ..$ is_synonym          : logi FALSE
> > ##   .. .. .. .. ..$ matched_name        : chr "Amphibia"
> > ##   .. .. .. .. ..$ nomenclature_code   : chr "ICZN"
> > ##   .. .. .. .. ..$ score               : num 0.75
> > ##   .. .. .. .. ..$ search_string       : chr "amphibians"
> > ##   .. .. .. .. ..$ taxon               :List of 10
> > ##   .. .. .. .. .. ..$ flags                   : list()
> > ##   .. .. .. .. .. ..$ is_suppressed           : logi FALSE
> > ##   .. .. .. .. .. ..$ is_suppressed_from_synth: logi FALSE
> > ##   .. .. .. .. .. ..$ name                    : chr "Succinea"
> > ##   .. .. .. .. .. ..$ ott_id                  : int 978937
> > ##   .. .. .. .. .. ..$ rank                    : chr "genus"
> > ##   .. .. .. .. .. ..$ source                  : chr "ott3.3draft1"
> > ##   .. .. .. .. .. ..$ synonyms                :List of 12
> > ##   .. .. .. .. .. .. ..$ : chr "Amphibia"
> > ##   .. .. .. .. .. .. ..$ : chr "Amphibina"
> > ##   .. .. .. .. .. .. ..$ : chr "Arborcinea"
> > ##   .. .. .. .. .. .. ..$ : chr "Brachyspira"
> > ##   .. .. .. .. .. .. ..$ : chr "Cerinasota"
> > ##   .. .. .. .. .. .. ..$ : chr "Cochlohydra"
> > ##   .. .. .. .. .. .. ..$ : chr "Luccinea"
> > ##   .. .. .. .. .. .. ..$ : chr "Lucena"
> > ##   .. .. .. .. .. .. ..$ : chr "Succinaea"
> > ##   .. .. .. .. .. .. ..$ : chr "Succinastrum"
> > ##   .. .. .. .. .. .. ..$ : chr "Tapada"
> > ##   .. .. .. .. .. .. ..$ : chr "Truella"
> > ##   .. .. .. .. .. ..$ tax_sources             :List of 7
> > ##   .. .. .. .. .. .. ..$ : chr "worms:181586"
> > ##   .. .. .. .. .. .. ..$ : chr "ncbi:145426"
> > ##   .. .. .. .. .. .. ..$ : chr "gbif:2297197"
> > ##   .. .. .. .. .. .. ..$ : chr "irmng:1393632"
> > ##   .. .. .. .. .. .. ..$ : chr "irmng:1348813"
> > ##   .. .. .. .. .. .. ..$ : chr "irmng:1133222"
> > ##   .. .. .. .. .. .. ..$ : chr "irmng:1202351"
> > ##   .. .. .. .. .. ..$ unique_name             : chr "Succinea"
> > ##   .. .. ..$ name   : chr "amphibians"
> > ##   ..$ taxonomy                    :List of 5
> > ##   .. ..$ author : chr "open tree of life project"
> > ##   .. ..$ name   : chr "ott"
> > ##   .. ..$ source : chr "ott3.3draft1"
> > ##   .. ..$ version: chr "3.3"
> > ##   .. ..$ weburl : chr "https://tree.opentreeoflife.org/about/taxonomy-version/ott3.3"
> > ##   ..$ unambiguous_names           : list()
> > ##   ..$ unmatched_names             : list()
> > ##  $ match_id          : int 2
> > ##  $ has_original_match: logi TRUE
> > ##  $ class             : chr [1:2] "match_names" "data.frame"
> > ```
> >
> > There are many hidden attributes on our 'match_names' object. The function `synonyms()` in the package `rotl` can extract the synonyms from the attributes of a 'match_names' object.
> >
> >
> > 
> > ```r
> > rotl::synonyms(resolved_name)
> > ```
> > 
> > ```
> > ## $Amphibia
> > ## [1] "Lissamphibia"
> > ## 
> > ## attr(,"class")
> > ## [1] "otl_synonyms" "list"
> > ```
> >
> > That's neat!
> >
> {: .solution}
{: .testimonial}

<br/>

### Getting OTT ids for multiple taxon names at a time

Now that we know about classes and the data structure of the `tnrs_match_names` output, we will learn how to use the tnrs_match_names function for multiple taxa.
In this case, you will have to create a character vector with your taxon names and use it as input for `tnrs_match_names`:

<br/>

> ## Hands on! Running TNRS for multiple taxa
>
> Do a `tnrs_match_names()` run for the amphibians (Amphibia), the genus of the dog (_Canis_),
> the genus of the cat (_Felis_), the family of dolphins (Delphinidae), and the class
> of birds (Aves). Save the output to an object named `resolved_names`.
>
> Again, you can try different misspellings and synonyms of your taxa to see TNRS in action.
>
>
> 
> ```r
> my_taxa <- c("amphibians", "canis", "felis", "delphinidae", "avess")
> resolved_names <- rotl::tnrs_match_names(names = my_taxa, do_approximate_matching = TRUE)
> ```
> 
> ```
> ## Warning in check_tnrs(res): amphibians, avess are not matched
> ```
> 
> ```
> ## Error in names(summary_match) <- c(names(tnrs_columns), "number_matches"): 'names' attribute [7] must be the same length as the vector [0]
> ```



































