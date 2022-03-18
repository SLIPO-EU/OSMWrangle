# OSMWrangle

### Welcome to OSMWrangle: An open-source tool for transforming geospatial features from OpenStreetMap into CSV files and RDF triples.

## Overview

OSMWrangle is a utility developed by the [Information Management Systems Institute](http://www.imis.athena-innovation.gr/) at [Athena Research Center](http://www.athena-innovation.gr/en.html) under the EU/H2020 Research Project [SmartDataLake: Sustainable Data Lakes for Extreme-Scale Analytics](https://smartdatalake.eu/). This generic-purpose, open-source tool can be used for extracting features from [OpenStreetMap](https://www.openstreetmap.org/) files and transforming them into records in a delimited CSV file.


OSMWrangle draws much of its functionality from the source code of [TripleGeo](https://github.com/SLIPO-EU/TripleGeo), an Extract-Transform-Load tool developed in the context of the EU/H2020 Innovation Action [SLIPO: Scalable Linking and Integration of big POI data](http://slipo.eu). TripleGeo extracts spatial features and their thematic attributes from a variety of geographical files and spatial DBMSs and transforms them into RDF triples.

## Quick start

### Installation

OSMWrangle is a command-line utility and has several dependencies on open-source and third-party, freely redistributable libraries. The `pom.xml` file contains the project's configuration in Maven.

Building the application with maven:

```sh
$ mvn clean package
```

results into the executable `osmwrangle-2.0-SNAPSHOT.jar` under directory `target` according to what has been specified in the `pom.xml` file.


### Execution

OSMWrangle supports transformation of geospatial features from two file formats (namely, XML and PBF) widely used for storing OpenStreetMap data. [Sample OSM datasets](test/data/) for testing are available in both file formats.

OSMWrangle also supports _classification_ of input OSM features into user-assigned categories. In the current version, such indicative classification is made available for two feature types:

- *Points of Interest (POI)*: a two-tier [POI classification scheme](test/conf/OSM_tags_POI_classification.yml) distinguishes possible amenities for Points of Interest into broader _categories_ (e.g., accommodation, eat/drink, tourism) and more specific _subcategories_ (e.g., hotel, motel, hostel, pub, bar, restaurant, museum, theater).

- *Road Network*: a two-tier [road classification scheme](test/conf/OSM_road_network_classification_tags.yml) distinguishes road network features in _categories_ (e.g., highway, road) and more specific _subcategories_ (e.g., motorway, primary, secondary, pedestrian). 

Indicative configuration files for extracting [POIs](test/conf/OSM_PBF_POI_options.conf) and [Road Network features](test/conf/OSM_PBF_road_network_options.conf) from OpenStreetMap PBF files are available to be used 'as-is' or to assist you when preparing your own.

Most importantly, if _no classification_ is required, then no filtering based on tag values will be applied and _every OSM entity_ can be extracted, provided that it can be parsed and has a valid WKT geometry. Indicative configurations are available for exporting [OSM PBF](test/conf/OSM_PBF_CSV_only_options.conf) and [OSM XML](test/conf/OSM_XML_CSV_only_options.conf) data to CSV format. This [configuration](test/conf/OSM_PBF_POI_options.conf) can be used to concurrently extract every parsable OSM entity both in CSV tuples and RDF triples (in separate files).

To execute this application, depending on the input data size, you should allocate enough memory to JVM (with the `-Xmx` directive) in order to boost performance; otherwise, OSMWrangle would rely on disk-based indexing and may run less efficiently. 


### Examples

- Let the input OpenStreetMap data are contained in *XML format* (i.e., the file has `.osm` extension) as specified in the user-defined configuration file like [this](./test/conf/OSM_XML_CSV_only_options.conf). Given that binaries are bundled together in `/target/osmwrangle-2.0-SNAPSHOT.jar`, execute this command:

```sh
$ java -Xmx2g -cp ./target/osmwrangle-2.0-SNAPSHOT.jar eu.smartdatalake.athenarc.osmwrangle.Extractor ./test/conf/OSM_XML_CSV_only_options.conf
```

- Let the input OpenStreetMap data are contained in *PBF format* (i.e., the file has `.osm.pbf` extension) as specified in the user-defined configuration file like [this](./test/conf/OSM_XML_PBF_only_options.conf). Given that binaries are bundled together in `/target/osmwrangle-2.0-SNAPSHOT.jar`, execute this command:

```sh
$ java -Xmx2g -cp ./target/osmwrangle-2.0-SNAPSHOT.jar eu.smartdatalake.athenarc.osmwrangle.Extractor ./test/conf/OSM_PBF_CSV_only_options.conf
```

Wait until the process gets finished, and verify that the resulting output files are according to your specifications.

Executing the command with another configuration will extract the specified OSM features. For instance, this [configuration](test/conf/OSM_PBF_road_network_options.conf) will extract only _road network_ features, whereas this [configuration](test/conf/OSM_PBF_POI_options.conf) will extract only Points of Interest (POIs).

*NOTE:* All execution commands and configurations refer to the current version (OSMWrangle ver. 2.0).


## License

The contents of this project are licensed under the [GPL v3 License](LICENSE).
