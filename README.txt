======================================================================================

GOTH (Generate OWL to HTML) is a simple command-line tool for generating human-readable HTML documentation of the ESS RDFS/OWL vocab/schema.

Author: Thomas Davison, on behalf of Digital Commons.
Original seed for this code downloaded 28/03/2022 from: https://github.com/ldodds/dowl

This is a first version, more development is needed to meet the needs of Digital Commons.

======================================================================================

INSTALLATION INSTRUCTIONS HERE: Dependencies need installing before running goth. 

======================================================================================

The application requires three parameters. 
Parameter 1: location of the .ttl vocab/schema file to parse, it must contain an ontology.
Parameter 2: language tag indicating which language to extract from the .tll, e.g. en or fr.
Parameter 3: location of the .erb file that contains the HTML template for output.

The script sends its output to STDOUT so just pipe it into a file.

E.g:
bash$ bin/goth essglobal-vocab.ttl en esstemplate.erb > essglobal-vocab_en.html

The script will automatically include the contents of introduction.html, if found in the same directory as the schema, into the documentation. This file should contain a HTML fragment.

The script relies on a .csv file which contains translations for phrases within the HTML (in addition to translations already contained within the .ttl itself). Its directory is currently hardcoded. When generating this .csv from the .xlxs, ensure that "CSV UTF-8 (Comma delimited)" is selected.

======================================================================================

Code Overview:

bin/goth is the executable, it creates a "schema" for a "generator", and then runs the generator with the chosen "template".

html_to_owl/lib/goth/generator.rb
html_to_owl/lib/goth/schema.rb
html_to_owl/esstemplate.erb

running a generator returns the results of esstemplate.erb, the html.

schemas include models (RDF::Graph) and collections of "RDF statements" taken from these models. The collections are statements with common term type: ontology, classes, properties, ves_classes, extra_concepts.

"util" describes DocObjects and LabelledDocObjects. Ontologies are extended DocObjects. Classes, properties and extra_concepts are extended LabelledDocObjects.

lib/goth/util.rb
lib/goth/ontology.rb
lib/goth/class.rb
lib/goth/property.rb
lib/goth/concept.rb

These files contain all the methods required to extract data from the RDF statements within the .erb template.

Finally, "goth" includes helper RDF::Vocabularies, as global Namespaces, necessary for extracting this data.

lib/goth.rb

======================================================================================