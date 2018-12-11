# DLME & Other Data Quality Analysis Recipes & Scripts

A set of metadata harvesting and analysis scripts, largely built off of the following:

- [Metadata QA](https://github.com/cmh2166/metadataQA.git)
- [pyoaiharvester](https://github.com/vphill/pyoaiharvester)
- [metadata_breakers](https://github.com/vphill/metadata_breakers)
- [Metadata Analysis at the Command Line](http://journal.code4lib.org/articles/7818)

## Installation

Working with python 2.7:

1. Clone this repository.
2. In this repository, install requirements using ```$ pip install -r requirements.txt```
3. Run the scripts

## Examples

This all works at present by using the harvest scripts to get a data file to your computer, then running an analysis script on that file.

### Harvesting

#### Harvest OAI-PMH feed

This downloads all the MODS/XML data from the OAI feed at Florida State University, and saves it to the file 'fsuoai.mods.xml'.
```
$ python oaiharvest.py -m mods -o fsuoai.mods.xml -l https://fsu.digital.flvc.org/oai2
```

### Analysis

All of the analysis scripts run similarly to what is described by Mark Phillips here for his own work: [Metadata Analysis at the Command Line](http://journal.code4lib.org/articles/7818)

#### oai dc analysis

Works most similarly to the original script created by Mark Phillips.

To get a field report:

```
$ python oaidc_analysis.py test/output.dc.xml
```

To get all the values for the dc:creator field:

```
$ python oaidc_analysis.py test/output.xml -e creator  
```

To get all the unique values for the dc:creator field, sorted by count:

```
$ python oaidc_analysis.py test/output.xml -e creator | sort | uniq -c  
```

#### oai mods analysis

This has added support for reviewing nested MODS elements, as well as perform queries with xpath.

To print a field report:
```
python oaimods_analysis.py test/DLTNphase1.mods.xml
```

To get all the values for mods:title (this does not mean just top level mods:titleInfo/mods:title - but any mods:title element wherever it appears in the record):
```
python oaimods_analysis.py test/DLTNphase1.mods.xml -e title
```

To get all the unique values for mods:form (again, wherever it appears) sorted by count:
```
python oaimods_analysis.py test/DLTNphase1.mods.xml -e form | sort | uniq -c
```

To get all the values that fit the 'mods:mods/mods:originInfo/mods:dateCreated[@encoding="edtf"]' Xpath query (i.e., all dateCreated for the object that have edtf encoding):
```
python oaimods_analysis.py test/DLTNphase1.mods.xml -x 'mods:originInfo/mods:dateCreated[@encoding="edtf"]'   
```
