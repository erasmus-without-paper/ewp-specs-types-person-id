Universal Person Identifiers ("UPIDs")
======================================

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

This repository describes the identifier type we plan to use in EWP APIs to
uniquely identify people. It follows the same [versioning rules][compat-rules]
as all EWP APIs do. Other projects are welcome to reuse this data type, along
with its XML namespace. We are also open to suggestions of extending this
document.


National ID numbers
-------------------

As you can learn from [this article on Wikipedia][wiki-article], in most
countries citizens are issued an identification number upon reaching legal age,
or when they are born.

UPIDs build on these numbers, because they seem to be the best we can hope
for when we're looking for a "globally unique" person identifier.


UPID structure
--------------

An UPID consists of the following pieces, concatenated into a single string:

 * **ISO 3166-1 alpha-2 code** of the country which issued the national ID
   number. These codes are always uppercase.

   Server implementers: Please note, that if your system stores national
   identifiers issued for your student by multiple countries, then it might be
   desirable to include all these identifiers when responding to your clients.

 * `-` (a dash character).

 * **The assigned national identification number**, along with all its
   punctuation and possibly even whitespace (!).

   Server implementers: If there are multiple standard ways in which an
   identifier can be formatted (e.g. with dashes, or without them), and you
   (the developer) are aware of all these standards, then you SHOULD include
   such identifier multiple times in your response, each time formatted in a
   different way. (We hope that developers from country `XY` will be aware of
   all possible formats of UPIDs beginning with `XY-`.)


### Examples

```
ZA-8001015009087
AR-20-10563145-8
AR-20105631458
PL-83120453012
NO-04128353012
FI-041283-1234
FI-0412831234
CH-756.1234.5678.97
CH-7561234567897
```


Possible problems with UPIDs
----------------------------

### Are they truly unique?

National ID numbers are not always unique. This means that **one UPID MAY be
assigned to several people**.

In many cases this problem can be ignored, because it occurs extremely rarely,
and - in general - countries are trying to fix it. However, it still might be
desirable to verify some other attributes of the person before concluding that
this indeed is the person we're looking for (i.e. date of birth).


### Multiple UPIDs per person

One person MAY be assigned more than one UPID number. There are numerous causes
for that:

 * In some countries new national ID number will be issued each time the
   person renews his ID card.

 * In some countries there are multiple identification number schemes in use,
   and it's not certain which one is the "primary" one.

 * One person MAY be concurrently regarded as a citizen of more than one
   country. Or, he/she might have been a citizen of one country in the past,
   but now is a citizen of a different country. Either way, such person can be
   identified by multiple UPIDs (issued by different countries).

All this means that it is generally desirable for computer systems to keep **a
list** of UPID identifiers for each person, including the older (possibly
obsolete) identifiers, in order to help match this person in other computer
systems.


### Uncertainty of formatting

Usually there exists one standard way of formatting these identifiers, with
spaces and punctuation in proper places. However, we cannot be 100% certain of
that. We cannot exclude the possibility of one national ID number being
formatted differently in different computer systems.

On the other hand, it seems that punctuation MAY have important meaning and
therefore SHOULD NOT be ignored during comparison (e.g. in Finland one of the
characters can be either `+`, `-` or `A`, and it indicates the century in which
the person has been born).

That's why we RECOMMEND server implementers to include all possible variants
(formats) of national IDs in their UPIDs. However, if you use other properties
for matching (e.g. date of birth), then ignoring punctuation and case during
comparison also should be acceptable (or even recommended!).


### Lack of ID type

As you might have noticed, UPID don't include any indication as to the **type**
of the national ID. That is, if there are multiple identification number
schemes in use, and it's not certain which one is the "primary" one, then
there's yet another chance of conflict.

This is caused by the fact that these types don't have any standard
identifiers themselves. In other words, we can't include the type, because we
don't know how we should name this type. We would need to keep a registry of
such identifiers (either in the schema, or on some other page).

The lack of ID type **doesn't seem to be a problem though**. As far as we know,
each type of national identifier has a unique format (i.e. length, alphanumeric
format). When someone from the issuing country sees it, he/she should be able
to deduce which type of identifier it is, just by looking at it (software
should also be able to do that). This seems to be true for all European
countries at least.

Worst case scenario: We are wrong and some country uses two separate "primary"
identifier types, which also happen to have the same format. In this case we
are indeed introducing more conflicts in UPIDs. However, even in this
pessimistic scenario, if you decide to use other attributes to identify the
person, then you should be okay.


### Conclusion

In most countries, UPIDs are safe to use. However, as you might have noticed,
there are numerous indications that UPIDs might cause problems in some other
countries (and we will notice that once such countries join the EWP Network).
When comparing UPIDs you might want to compare some other attributes too (i.e.
date of birth), in order to counteract such problems early on.

We do suspect that a better way to identify people might surface in the next
few years, and we suspect that this "better way" will be officially backed by
governments. Once such new "universal" type of identifier emerges, UPIDs will
probably be deprecated.

That said, we still encourage you to use UPIDs in your project (even outside
of EWP), if you want to. Having a universal identifier with these flaws (and
all such flaws being documented in a single place) might still be better than
not having one at all.


Privacy issues
--------------

In most countries, national ID numbers are considered private personal
information. Therefore, in general, you SHOULD NOT display UPIDs to the users
(especially users other than the user identified by the UPID).


XML Schema
----------

If you decide to use UPIDs in XML documents, then you SHOULD reuse the data
type defined in the attached [`schema.xsd`](schema.xsd) file.


[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management#statuses
[compat-rules]: https://github.com/erasmus-without-paper/ewp-specs-architecture/#backward-compatibility-rules
[wiki-article]: https://en.wikipedia.org/wiki/National_identification_number
