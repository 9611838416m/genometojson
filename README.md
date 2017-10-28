# genometojson
[![npm version](https://badge.fury.io/js/genometojson.svg)](https://badge.fury.io/js/genometojson)

[![Build Status](https://travis-ci.org/9611838416m/genometojson.svg?branch=master)](https://travis-ci.org/9611838416m/genometojson)

## Information

<table>
<tr>
<td>Package</td><td>genometojson</td>
</tr>
<tr>
<td>Description</td>
<td>Formats your commercial genome file as JSON</td>
</tr>
<tr>
<td>Node Version</td>
<td>>= 0.4</td>
</tr>
</table>

## Terminology

### SNP

A SNP (Single-Nucleotide Polymorphism - pronounced *snip*) is a variation of a single nucleotide (`A`, `G`, `C` or `T`) at some location in your genome. The presense of particular variants, or groups thereof, can tell you a lot about yourself (hair color, height, muscle type, allergies, response to disease, response to pharmaceuticals, heritage, etc.). 

Commercial genome vendors will typically produce an analysis that covers some of these attributes - but will also provide you with 'raw' data, should you wish to investigate further.Your 'raw' data from any vendor will be a list of several hundred thousand SNPs, located across the [autosomes](https://en.wikipedia.org/wiki/Autosome) (chromosomes 1-22), the [sex chromosomes](https://en.wikipedia.org/wiki/Allosome) (X and Y), and possibly the [mitochondrial chromosome](https://en.wikipedia.org/wiki/Mitochondrial_DNA). Given that your genome contains two copies of each autosome, and either XX (female) or XY (male), the data contains a 'genotype' composed of the two variants at each location.


## Vendors

| Name | Supported | Price | Sample | Autosomal SNPs | Y SNPs | X SNPs | MT SNPs | Raw Data |
|------|-----------|-------|--------|----------------|--------|--------|---------|----------|
| 23andMe | Yes | 199/99 USD<sup>*</sup> | Saliva | 967,000 | 3,089 | 26,087 | 2,737 | [Yes](https://www.23andme.com/you/download/) |
| ancestryDNA | Yes | 99 USD | Saliva | 682,549 | 885 | 17,604 | 0 | [Yes](http://ldna.ancestry.com/atFAQ.aspx#raw-3) |
| FamilyTree | Yes | 99 USD | Cheek Swab | 708,092 | 0 | 18,091 | 0 | [Yes](http://www.familytreedna.com/faq/answers.aspx?id=17#606) |

<sup>*</sup> 23andMe picing is 199 USD in the US and 99 USD abroad (the abroad test does not include Health Information).

## CLI Usage

Use this if you just want to convert your data to the correct format so you can start querying your genome.

```
$ npm install dna2json -g
$ dna2json
Usage: dna2json <input file> <output file>
$ dna2json dna.txt dna.json
This will take a while...
```

## Module Usage

```javascript
var dna = require('dna2json');
var fs = require('fs');

var txt = fs.readFileSync('dna.txt');
dna.parse(txt, function(err, snps){
  // snps = the object with your mutations
});
```

## SNP-JSON

Every vendor has their own format for your DNA. I decided to make a standard format called SNP-JSON. This library will convert custom formats to this standard.

SNP-JSON looks like this

```json
{
  "rs4477212": {
    "chromosome": 1,
    "genotype": "AA"
  },
  "rs3094315": {
    "chromosome": 1,
    "genotype": "AA"
  },
  "rs3131972": {
    "chromosome": 1,
    "genotype": "GG"
  },
  "rs12124819": {
    "chromosome": 1,
    "genotype": "AA"
  },
  "rs11240777": {
    "chromosome": 1,
    "genotype": "GG"
  },
  "rs6681049": {
    "chromosome": 1,
    "genotype": "CC"
  },
  ...
}
```

Explanation:

- The key is the RSID or vendor ID.
- Chromosome is which chromosome the data came from (1-22, X, Y, or MT).
- Genotype is the value for the RSID.

### Differences between formats

SNP-JSON, SNPedia and vendor formats use different notations to indicate indels and no-calls.

| Format        | Insertion            | Deletion | No Call | *Reference* |
|---------------|----------------------|----------|---------|-------------|
| 23AndMe       | `I`                  | `D`      | `-`     | https://customercare.23andme.com/hc/en-us/articles/212196888-What-does-not-determined-or-not-genotyped-mean- |
| AncestryDNA   | `I`                  | `D`      | `0`     | https://www.ancestry.com/dna/en/legal/us/faq#raw-6 |
| FamilyTreeDNA | `I`                  | `D`      | `-`     | https://www.familytreedna.com/learn/autosomal-ancestry/universal-dna-matching/read-family-finder-raw-data-file/ |
| SNP-JSON      | `I`                  | `-`      | `?`     | |
| SNPedia       | (the actual letters) | `-`      | N/A     | https://www.snpedia.com/index.php/Talk:Rs5030655 |

## Using your SNP-JSON

### Show me the code!!

In this example we will determine if you are immune to norovirus (Data from SNPedia):

```js
var dna = require('./dna.json');

if (dna.rs601338.genotype == 'AA') {
  // congrats you are immune!
}
```


## Example Files

The Folder Examples has a code example to use the module

The Folder Samples contains sample Genome files from different vendors and their JSON converted versions


## LICENSE

Coming Soon..
