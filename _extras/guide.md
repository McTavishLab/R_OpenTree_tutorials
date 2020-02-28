---
title: "Instructor Notes"
---

### 6.1 Get source trees with branch lengths

> > ## Extra! Try it yourself.
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
