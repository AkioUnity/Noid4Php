Noid for php
============

[Noid for Php] is a tool to create and manage temporary or permanent nice opaque
identifiers ([noid]) for physical or digital objects or anything else like "12025/654xz321".

This is the php version of a [perl tool] of 2002-2006, still largely used in
libraries, museums and any institution that manage collections and archives, for
example the University of California, the Internet Archive, or the National Library of France.

The main goal of this version is for web services, that can create and manage
standard noids without any dependancy. All commands and functions can be used as
in the Perl version, via the command line or via the web.

It is already implemented in [Omeka], an open source CMS designed to expose
digitalized documents, via the plugin [Ark & Noid for Omeka].

Other versions exist in [Java] and [Ruby].


Noid Overview
-------------

The noid utility creates minters (identifier generators) and accepts commands
that operate them. Once created, a minter can be used to produce persistent,
globally unique names for documents, databases, images, vocabulary terms, etc.
Properly managed, these identifiers can be used as long term durable information
object references within naming schemes such as ARK, PURL, URN, DOI, and LSID.
At the same time, alternative minters can be set up to produce short-lived names
for transaction identifiers, compact web server session keys, and other
ephemera.

In general, a noid minter efficiently generates, tracks, and binds unique
identifiers, which are produced without replacement in random or sequential
order, and with or without a check character that can be used for detecting
transcription errors. A minter can bind identifiers to arbitrary element names
and element values that are either stored or produced upon retrieval from
rule-based transformations of requested identifiers; the latter has application
in identifier resolution. Noid minters are very fast, scalable, easy to create
and tear down, and have a relatively small footprint. They use BerkeleyDB as the
underlying database.

Identifiers generated by a noid minter are also known as "noids" (nice opaque
identifiers -- rhymes with void). While a minter can record and bind any
identifiers that you bring to its attention, often it is used to generate,
bringing to your attention, identifier strings that carry no widely recognizable
meaning. This semantic opaqueness reduces their vulnerability to era- and
language-specific change, and helps persistence by making for identifiers that
can age and travel well.

See the full description, tutorial and usage on [metacpan], and the list
of available [commands].


Installation
------------

This tool requires the php extension "dba", that is installed by default with
Php, and the BerkeleyDB library, that is installed by default too on standard
web servers and any Linux distribution (package libdb5.3 on Debian), because it
is used in many basic tools.

Simply include the file "lib/Noid.php" in your project and the class "Noid" will
be available. The command line tool is available via "noid.php" or the symbolic
link "noid", but is not required.

This tool has been tested on php 5.6 and php 7.

[PhpUnit] can be used to check the installation via the command `phpunit tests`
at the root of the tool.


About the port
--------------

* Main purpose

The main purpose of this first conversion is the fidelity to the structure of
the original Perl script: each method has the equivalent code in perl and php
scripts. Furthermore, no dependency is added: the script can be run directly. Of
course, the performance is lower, mainly because the access to the database is
slower: only the generic functions are used, not the ones specific to Berkeley.

* Random noids

When the template is a random one, the order of generated noids is different
between the perl and the php script, because the pseudo-random generated by
these languages are different and in some cases dependent of the platform too.
In Perl, before 5.20.0 (May 2014), rand() calls drand48(), random() or rand()
from the C library stdlib.h in that order. Since 5.20.0, drand48() is
implemented. In Php, rand() uses the underlying C library's rand (see [rosettacode.org]).

So, to produce the same sequence, the default random generator is the perl one,
run from the command line. This is not an issue in most of the cases, because
perl is installed by default on all major distributions and servers.

* Database

 - Only generic features of database are managed: no environment, no alarm for
 locking. Furthermore, the locking mechanism is different between perl and php,
 so don't use them at the same time.
 - Php can read and write databases created with the perl script, but the perl
 script cannot access php ones. There is a workaround: copy the three files
 "__db.001", "__db.002" and "__db.003" in the directory "NOID". They come from a
 freshly created minter and are provided with the module in the directory
 "bdbfiles". No advanced check had been done for cross-writing databases.
 - If php and perl are used, the template must not be a random one, because
 seeds are not the same and generated ids are ordered differently.
 - In the php version, the elements can't be duplicated: dba_replace() is
 systematically used instead of dba_insert(). Anyway, this feature is not
 available in the perl script.
 - In Perl script, $noid is a pointer to data managed from $dbname. To avoid
 this direct access to memory, harder to maintain in a high level language,
 $noid is the $dbname, that points internally to the db handle.

* To do

  - Port the pseudo-generator processor from perl (c) into php.
  - Optimize structure and process, but keep inputs, calls, and outputs.
  - Seperate creation of noids and management of bindings.
  - Use the perl pseudo-random generator to create identical noids via php.
  - Use other standard or flat db engines (mysql and simple file).
  - See other todo in the perl or php scripts.


Warning
-------

Use it at your own risk.

It's always recommended to backup your files and database regularly so you can
roll back if needed.

For long term noids, all events are saved in a log too.


Troubleshooting
---------------

See online issues on the [issues] page on GitHub.


Copyright
---------

- Author:  John A. Kunze, jak@ucop.edu, California Digital Library
  - Originally created Nov. 2002 at UCSF Center for Knowledge Management
- Ported to php by Daniel Berthereau for Mines ParisTech

* Copyright (c) 2002-2006 UC Regents
* Copyright (c) 2016 Daniel Berthereau for Mines ParisTech


License
-------

* The original tool has been published by the University of California under the
BSD licence.

Permission to use, copy, modify, distribute, and sell this software and its
documentation for any purpose is hereby granted without fee, provided that (i)
the above copyright notices and this permission notice appear in all copies of
the software and related documentation, and (ii) the names of the UC Regents and
the University of California are not used in any advertising or publicity
relating to the software without the specific, prior written permission of the
University of California.

THE SOFTWARE IS PROVIDED "AS-IS" AND WITHOUT WARRANTY OF ANY KIND, EXPRESS,
IMPLIED OR OTHERWISE, INCLUDING WITHOUT LIMITATION, ANY WARRANTY OF
MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE.

IN NO EVENT SHALL THE UNIVERSITY OF CALIFORNIA BE LIABLE FOR ANY SPECIAL,
INCIDENTAL, INDIRECT OR CONSEQUENTIAL DAMAGES OF ANY KIND, OR ANY DAMAGES
WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER OR NOT ADVISED
OF THE POSSIBILITY OF DAMAGE, AND ON ANY THEORY OF LIABILITY, ARISING OUT OF OR
IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

* The php version is published under the [CeCILL-B v1.0], compatible with the
BSD one.

The exercising of this freedom is conditional upon a strong obligation of giving
credits for everybody that distributes a software incorporating a software ruled
by the current license so as all contributions to be properly identified and
acknowledged.

In consideration of access to the source code and the rights to copy, modify and
redistribute granted by the license, users are provided only with a limited
warranty and the software's author, the holder of the economic rights, and the
successive licensors only have limited liability.

In this respect, the risks associated with loading, using, modifying and/or
developing or reproducing the software by the user are brought to the user's
attention, given its Free Software status, which may make it complicated to use,
with the result that its use is reserved for developers and experienced
professionals having in-depth computer knowledge. Users are therefore encouraged
to load and test the suitability of the software as regards their requirements
in conditions enabling the security of their systems and/or data to be ensured
and, more generally, to use and operate it in the same conditions of security.
This Agreement may be freely reproduced and published, provided it is not
altered, and that no provisions are either added or removed herefrom.


Contact
-------

Current maintainers:
 * Daniel Berthereau (see [Daniel-KM] on GitHub)


[Noid for Php]: https://github.com/Daniel-KM/Noid4Php
[noid]: https://wiki.ucop.edu/display/Curation/NOID
[perl tool]: http://search.cpan.org/~jak/Noid-0.424/
[Omeka]: https://www.omeka.org
[Ark & Noid for Omeka]: https://github.com/Daniel-KM/ArkAndNoid4Omeka
[Java]: https://confluence.ucop.edu/download/attachments/16744482/noid-java.tar.gz
[Ruby]: https://github.com/microservices/noid
[metacpan]: https://metacpan.org/pod/distribution/Noid/noid
[commands] https://metacpan.org/pod/Noid
[PhpUnit]: https://phpunit.de
[rosettacode.org]: https://rosettacode.org/wiki/Random_number_generator_%28included%29#Perl
[issues]: https://github.com/Daniel-KM/Noid4Php/issues
[CeCILL-B v1.0]: https://www.cecill.info/licences/Licence_CeCILL-B_V1-en.txt
[Daniel-KM]: https://github.com/Daniel-KM "Daniel Berthereau"
