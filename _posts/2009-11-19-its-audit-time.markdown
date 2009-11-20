---
layout: post
---
no no. not that kind of audit :p.  I've been spending the last couple of days
going over different versioning/audit gems to find the best for my project, and
this post is to compare and comment on my findings. currently this goes over
`vestal_versions`, `paper_trail`, and `acts_as_audited`. if you know of any
others i should add please let me know.

lets get down to brass tax. here is a simple table of comparison, then i will
go into detail for each gem. also if you see any features that should be added
to the list, let me know, i will add them and compare each gem.

<table style="width: 100%; text-align: right;">
  <tr style="text-align: right;">
    <td></td>
    <td><b>Vestal Versions</b></td>
    <td><b>Paper Trail</b></td>
    <td><b>Acts as Audited</b></td>
  </tr>
  <tr>
    <td style="text-align: left;">Popularity</td>
    <td colspan="3"></td>
  </tr>
  <tr>
    <td>Watchers</td>
    <td>359</td>
    <td>156</td>
    <td>253</td>
  </tr>
  <tr>
    <td>Forks</td>
    <td>19</td>
    <td>4</td>
    <td>24</td>
  </tr>
  <tr><td colspan="4"> <hr /> </td></tr>
  <tr>
    <td style="text-align: left;">Features</td>
    <td colspan="3"></td>
  </tr>
  <tr>
    <td>Storage</td>
    <td>Serialized</td>
    <td>Yaml</td>
    <td>Yaml</td>
  </tr>
  <tr>
    <td>History</td>
    <td>Changes</td>
    <td>Full</td>
    <td>Full</td>
  </tr>
  <tr>
    <td>Diff</td>
    <td>-/Fields/-*</td>
    <td>-/Old/All</td>
    <td>All/Fields/All</td>
  </tr>  
  <tr>
    <td>Log Action</td>
    <td>No</td>
    <td>Yes</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>Log User</td>
    <td>No</td>
    <td>Yes</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>Version Numbers</td>
    <td>Yes</td>
    <td>No</td>
    <td>Yes*</td>
  </tr>
  <tr>
    <td>Include/Exclude Fields</td>
    <td>Yes</td>
    <td>Partial*</td>
    <td>yes</td>
  </tr>
  <tr>
    <td>Reconstruct</td>
    <td>No</td>
    <td>Yes</td>
    <td>Yes*</td>
  </tr>
  <tr>
    <td>Association Access</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Yes</td>
  </tr>
  <tr><td colspan="4"> <hr /> </td></tr>
  <tr>
    <td style="text-align: left;">Test Suite</td>
    <td colspan="3"></td>
  </tr>
  <tr>
    <td>Test Count</td>
    <td>35</td>
    <td>44</td>
    <td>69</td>
  </tr>
  <tr>
    <td>Assertions</td>
    <td>154</td>
    <td>65</td>
    <td>46</td>
  </tr>
  <tr>
    <td>Failures</td>
    <td>0</td>
    <td>5</td>
    <td>0</td>
  </tr>
  <tr>
    <td>Errors</td>
    <td>0</td>
    <td>11</td>
    <td>0</td>
  </tr>
  <tr>
    <td>Coverage</td>
    <td>76.69%</td>
    <td><em>72.35% ?</em></td>
    <td>92.68%</td>
  </tr>
</table>

<p></p>
**_Vestal Versions_**
=====================

as a side note, this might not belong here as it may be more aimed toward 'versioning'
and not exactly as an auditing solution. but i put it here anyway.

im slightly put off by vestal. It has a lot of nifty features but a few of the
big things really make it a non-solution (at least for me.)  note for the rcov
coverage i had to manually place an Rcov task in Rakefile, but it worked seemlessly.

Pros
----
* easy to revert to older versions
* able to revert by datetime or version number

Cons
----
* never stores full object in history
* no action/user tracking
* destroys history on record removal

Now for me that last one is a definite deal breaker.  even when using is_paranoid with vestal the
history is removed (yet you still have a primary record.) though i have talked to the author of
vestal_versions and this is something that will be fixed/optional in the 1.0 release so thats good.
as for the action tracking you can technically check if the `changes` field is null or not (it will
be null on the create version)

<p></p>
**_Paper Trail_**
=================

paper trail as of right now, I feel is one of the best for this.  though it is also missing a few minor
things that would make it a lot better. and the testing suite needs and overhaul.  one 'minor' thing
about paper_trail is that it only has an :ignore option for ignoring fields to be audited.  this i
personally don't find too much of a problem though, and im sure the other could easily be implemented.

Pros
----
* full history always
* reify (revert) for record reconstruction is awesome!
* reconstructed records keep id & timestamps
* able to turn paper_trail off temporarily

Cons
----
* no time based reify (revert)
* no version tracking
* poor test coverage

for me version tracking isn't to bad.  a version # really isn't that important as long as the history
is actually there. and given my project people are going to worry about time more than a version number.

<p></p>
**_Acts as Audited_**
=====================

aaa has a few nice features though its a bit quirky around the edges.  a few things i think are important
to an auditing plugin just don't exist or act really funny with aaa.

Pros
----
* stores less in database than paper_trail
* cleaner interface for doing 'as_user' stuff than paper trail

Cons
----
* reconstruct gives record a new ID and timestamps
* whodunnit limited to 'username' attribute on current_user

the new id & timestamps on record reconstruction im not a big fan of. as if you have associations that
also need to be reconstructed this will put a hamper into the entire operation.