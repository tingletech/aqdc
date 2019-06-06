# Arbitrarily Qualified Dublin Core


**Arbitrarily Qualified Dublin Core** Element Names are based on [RFC 5013 Element Names](https://tools.ietf.org/html/rfc5013#section-6) prefixed with the String `aqdc_`.

```
curl -s "https://tools.ietf.org/html/rfc5013" \
| grep "Element Name" \
| sed -e 's/^Element Name:   /Element Name:   aqdc_/'
Element Name:   aqdc_title
Element Name:   aqdc_creator
Element Name:   aqdc_subject
Element Name:   aqdc_description
Element Name:   aqdc_publisher
Element Name:   aqdc_contributor
Element Name:   aqdc_date
Element Name:   aqdc_type
Element Name:   aqdc_format
Element Name:   aqdc_identifier
Element Name:   aqdc_source
Element Name:   aqdc_language
Element Name:   aqdc_relation
Element Name:   aqdc_coverage
Element Name:   aqdc_rights

```
Elements names in Arbitrarily Qualified Dublin Core are the Element Names from RFC5013 prefixed with the string `aqdc_`.  
[Content is as described in the RFC](https://tools.ietf.org/html/rfc5013#section-6), and is encoded as per this specification.

## `aqdc_` Values

Dublin core values require a simple string which may optionally be qualified by any or all of the following:
 * `value_uri` : linked data style URI representing the same value as the value string.
 * `qualifier_uri` : URI to a qualifier, such as `dcterms:` or other mapping analog from the source metadata
 * `qualifier_string` : arbitrarty label displayed to the end user along with this field

### Simple String AQDC Value

```js
aqdc_creator: [ 'Brian Tingle' ]
```

### AQDC Value with String and URI


```js
aqdc_creator: [
  'Brian Tingle', {
    'value_uri': 'http://orcid.org/0000-0002-5309-7921'
  }
]
```
(question, is authority source/type needed, or can it be infered by the URI value?)

### AQDC Value with LC Relators terms

```js
aqdc_creator: [
  'Brian Tingle', {
    'qualifier_uri': 'http://id.loc.gov/vocabulary/relators/ppt',
    'qualifier_string': 'Puppeteer'
  }
]
```

### AQDC Value with no labels

```js
aqdc_creator: [
  { 'value_uri': 'http://orcid.org/0000-0002-5309-7921' }
]
```

### AQDC Value Unknown

should this be valid?
```js
aqdc_creator: [
  { 'qualifier_uri': 'http://id.loc.gov/vocabulary/relators/ppt' }
]
```

Fully normalized for indexing, this might look like
```js
  aqdc_creator: [ '[not supplied]', {
    'qualifier_uri': 'http://id.loc.gov/vocabulary/relators/ppt',
    'qualifier_string': 'Puppeteer'
  }]
```

Which might display along the lines of
```
Creator (Puppeteer): [not supplied]
```
### full example

```json
aqdc_creator: [
  'Brian Tingle', {
    'value_uri': 'http://orcid.org/0000-0002-5309-7921'
    'qualifier_uri': 'http://id.loc.gov/vocabulary/relators/ppt',
    'qualifier_string': 'Puppeteer'
  }
]
```

## File Format for sending to 

Create one JSON-L file per record?  Probably not.  Could we use `\n\n` as the record seperator, and a single `\n` as a field seperator?

```json
'aqdc_creator':['Brian Tingle',{'value_uri':'http://orcid.org/0000-0002-5309-7921','qualifier_uri':'http://id.loc.gov/vocabulary/relators/ppt','qualifier_string':'Puppeteer'}]
```

A file with metadata for a set of records.  `\n\n` is at the start of every record.
```js

'aqdc_title':['Record 1']
'aqdc_identifier':['id:01']

'aqdc_title':['Record 2']
'aqdc_identifier':['id:02']
```


## Special rule when AQMD Value String can be parsed as URI?

A special parser option that will normalize input values such as:

```
'aqdc_creator':['http://orcid.org/0000-0002-5309-7921']
```

## Special rule for link to reference image

For Calisphere/DPLA use case, need to create thumbnail/reference image

## Special rule for link to full text

Want to full text index everything

## Special rule for link to the page for record in its primary DAMS

This will have links to the best viewers for the content described in the record.
