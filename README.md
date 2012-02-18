# Mormon Primary Sources

With the technology of the twenty-first century, it's time to build an open source database that serves historians and other researchers in their study of primary source artifacts and documents. This project seeks to combine the advantages of document-based databasing, software versioning, and open source collaborative repositories to provide a scalable, searchable, and comprehensive record of the known body of primary sources. It takes Mormonism as its unifying topic, and will include a running collection of citations and meta-information for individual works and artifacts relating to this religious movement. The principal discipline around which this collection will be oriented is history, though other disciplines ought to benefit from the arrangement of these citations and whatever additional commentary that will be included with them. This collection will rely on guiding conventions to maintain the data and allow them to scale with new and changing methods of cataloging, systematizing, indexing, and searching. Contributors and users will need to be proficient in these conventions, which are outlined below.

## Collection Structure

This database is based on the document-oriented storage model (as opposed to the record-oriented or relational-oriented models). This means that the data is stored following a document structure: each item or record in the database is represented by a file housed within a folder, just like on a desktop platform. To facilitate browsing the collection, this project adopts a folder structure that behaves as a governing framework to the data store. Instead of a running list of records, a folder structure ordered by centuries, decades, and years breaks up historical time along a semantic trajectory. Arranging the folders this way also ensures that the date, whether a file is approximated to a year, decade, or century or not, behaves as a unique identifier to prevent data convergence or redundancy.

Some files will not be dated to a specific year; in these cases, the record would be housed in a corresponding decade. At worst, the record will be placed within an approimate century. Most often, database records will be placed in a corresponding year of its production and named in accordance with the file naming convention.

## Folder and File Naming Conventions

To ensure that each document in the database represents an actual source, the following naming conventions are adhered to when committing records to the database. These naming conventions also allow for software indexers to be applied for abstracted searching and ordering of the database contents.

These records are primarily ordered by their date of production. The database, therefore, is only browsable by the chronological order of source materials. File names begin with the date then follow with the author, title, and in some cases the recipient. Each element of the filename is delimited by the underscore character. Multiple values within a naming element are delimited by the dash character. No spaces are allowed in the filename, and words are capitalized according to the Chicago Style parameters for headlines. For example:

`1829-10-22_Smith-Joseph-Jr_Letter_Cowdery-Oliver`

This file corresponds to the October 22, 1829 letter of Joseph Smith to Oliver Cowdery. The filename begins with the date, arranged first with the year, followed by the two-digit value for the month and the two-digit value for the day. Dashes separate the year from the month, and the month from the day. The underscore character delimits the date from the next element, which is the author. All personal names follow the `lastname-firstname-additionalNames` convention. In this case, the author is Joseph Smith. We enter his full name as `Smith-Joseph-Jr` and follow with another underscore to set off the next element. The title of the work is simply `Letter` following editorial convention. The final element in the filename is the recipient of the letter. By following this naming convention, researchers can easily identify the document with the source it represents.

Folders follow three levels: century, decade, and year. Where records can only be associated with a decade or a century, files may be housed within the first two levels. Usually files will be housed at the year level. Occasionally it may be necessary to add another level of folders within the year level. These should correspond to the month at the fourth and the day at the fifth, and final, folder levels.

When naming fourth- or fifth-level folders, use the three-character month abbreviation and the two-digit day value. Month or day levels should only be created when the number of files for a given year exceeds 500. Let's say that we have succeeded at committing all the documents for 1839. This number should be very high, considering the volume of documents within and without the Mormon community relating to the history of the Missouri period. We would create folders for `Jan` through `Dec` and within those folders place the documents that represent sources produced in each month.

Where documents are produced over the course of several days or months, the convention will be to assume the latest date in the production process for how to name and store the record.

## Indexing Schema

The contents of each record file contain cataloging and researching information relating to the source. The following list of fields are used across the database. Additional keys may be used and not all keys will be present in each document. This facilitates scaling the data as the repository grows and the variability between sources increases. The document-oriented structure of this system encourages providing as much data as desired, but each data value must have a key. The Indexing Syntax section describes how the information is to be encoded to allow for easy parsing by search indexers.

    /* Primary Information */
    author:
    title:
    subtitle:
    series:
    date:
    location:
    editors:
    illustrators:
    translators:
    affiliation:
    number:
    edition:
    city:
    country:
    state:
    volume:
    issue:
    pages:
    firstEdition:
    volumes:
    
    /* Catalog Information */
    synopsis:
    description:
    notes:
    submitted:
    reviewed:
    keywords:
    label:
    url:
    doi:
    isbn:
    language:
    status:
    kind:
    copyright:
    loc:
    access:
    id:
    license:
    box:
    file:
    folio:

## Indexing Syntax

Within a document-record file, a simple syntax method is used to set off data values. Keys are entered in lowercase, and if multiple words form a key, then CamelCasing is used (the first character still being rendered in lowercase). The syntax also conforms to a key-colon-value arrangement. The outer-most layer of keys are delimited only by new lines. It is possible to assign multiple values to a key. In these cases, curly braces are used to enclose a multiple-value block. Keys with values are delimited within mutliple-value blocks by commas. Commas may not appear within the value itself. To escape a comma, use the backlash character immediately before the comma.

Multiple-lined, paragraph level values should be set off using three braces, like so:

    notes: {{{
    	
    	This broadside is damaged from ink and water stains.
    	
    }}}

Comments are discouraged and should only be used for coding maintenance, not as data descriptors. These are set off using an opening `/*` and a closing `*/`, conforming to the Java, JavaScript, and PHP standards.

This overall indexing syntax convention loosely follows the JSON format. When parsing the file contents, it may be necessary to add opening and closing braces before processing.

### Syntax Example

    author: { last: Smith, first: Joseph, suffix: Jr. }
    title: Letter
    date: { year: 1829, month: 10, day: 22 }
    recipient: { last: Cowdery, first: Oliver }
    
    venues: {
        CHL: {
        	institution: Church History Library,
        	city: Salt Lake City,
        	state: Utah,
        	collection: Joseph Smith Letterbook,
        	collection: Joseph Smith Papers,
        	box: 1,
        	folio: 9
        },
        EMD: {
        	editor: { last: Vogel, first: Dan },
        	author: { last: Vogel, first: Dan },
        	title: Early Mormon Documents,
        	volume: 1,
        	volumes: 5,
        	city: Salt Lake City,
        	publisher: Signature Books,
        	year: 1996,
        	pages: 7-8
        }
    }
