---
title: "Instructor Notes"
---

### 1.1.

> ## Note: Going from a common name to a scientific name
>
>  TNRS only deals with scientific names. If you want to work with common names, you will have to use a service that can get the scientific name of a list of common names. There are no functions in `rotl` to deal with this. We know of at least two places that have implemented this otherwise. The [OneZoom](https://github.com/OneZoom/OZtree) project has developed a service that provides all scientific names associated to common names in the Encyclopedia of Life database.
> The phylotastic project has implemented a [common name to scientific name service](https://github.com/phylotastic/phylo_services_docs/tree/master/ServiceDescription#common-name-to-scientific-name) that is also available in the r package [rphylotastic](https://github.com/phylotastic/rphylotastic).
>
{: .discussion}


### 6.1 Getting source trees with branch lengths

> >
> >
> > ~~~
> > chronograms <- rotl::studies_find_trees(property = "ot:branchLengthMode", value = "ot:time", verbose = TRUE, detailed = TRUE)
> > ~~~
> > {: .language-r}
> >
> > ~~~
> > class(chronograms)
> > names(chronograms)
> > ~~~
> > {: .language-r}
> > We should be able to use `list_trees()` to get all trees matching our criteria.
> >
> > ~~~
> > rotl:::list_trees(chronograms)
> > ~~~
> > {: .language-r}
> >
> > Except, it does not really work.
> {: .solution}
{: .testimonial}

<!-- {% include links.md %} -->
