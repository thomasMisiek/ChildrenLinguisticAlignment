# children-linguistic-alignment

children-linguistic-alignment allows to compute the semantic, lexical and syntactic similarity between couples
of consecutive utterances retrieved from the CHILDES database, as well as several other measures.
6 languages available: English, French, Spanish, German, Chinese and Japanese.

## Installation

Install the project by cloning it on your computer or by downloading a compressed version

## Usage

Use the Run notebook, where an example call is already set up

Or you can also run the script run.py

For example: python run.py French_20_25 French 20 25 True
<br />

<ul>
<li>with French_20_25 being the name of the directory where the results will be stored (can be any name) but mandatory</li>
<li>with French being the language used to retrieve data from CHILDES. Mandatory</li>
<li>with 20 being the minimal age to be considered. Mandatory</li>
<li>with 25 being the maximal age to be considered. Mandatory</li>
<li>and True stating if the program is in test mode or not. Optional, default value: False</li>
</ul>

## Contributing
Please contact thomas.misiek@gmail.com to report any bug or if you want to add something to the code

## License

## Methods and how to interpret the results

After the end of a run, all the results are stored in Databases/your_directory_name/results.
To each treated age correspond a CSV file containing similarity measurements and other relevant data.
Here is an exhaustive list of the columns of the CSV file, what is contained in them, and how it is processed.

#### condition

"rand_ex" if both utterances are taken at random in the whole CHILDES dataset (of the same language).
</br>
"rand_in" if both utterances are taken at random in the same transcript (conversation).
</br>
"chi->par" if the child speak first then the parent, and both utterances are strictly consecutive.
</br>
"par->chi" if the parent speak first then the child, and both utterances are strictly consecutive.
</br>
In "rand_in" and "rand_ex", the direction of speech make no sense, as utterances are almost never consecutive
</br>
Only single utterances are considered, groups of consecutive, uninterrupted utterances by the same speaker are not clustered,
so in all conditions, only the last and first utterance of a cluster might be taken into account.
</br>
In all conditions, both a parent and a child sentence are selected.

#### child_age

The age of the target child in the transcript, only relevant for "chi->par", "par->chi" and "rand_in".
</br>
In "rand_ex" this is the age of the target child from the transcript from which the child utterance was retrieved. In this condition, the adult might have been speaking to a child of a different age.

#### child_sex

The sex of the target child in the transcript, once again, only relevant for "chi->par", "par->chi" and "rand_in", for the same reason as for child_age.

#### child_id

The id of the target child in the transcript, once again, only relevant for "chi->par", "par->chi" and "rand_in", for the same reason as for child_age.

#### parent_sex

The sex of the target child in the transcript, once again, only relevant for "chi->par", "par->chi" and "rand_in", for the reversed reason as for child_age. This time the parent sex comes from the transcript from which the parent utterance was retrieved.

#### child_utterance_order

Order of the utterance in its transcript, ie: 0 if it is the first sentences uttered in the transcript, 1 if it is the second...
</br>
It might be irrelevant for the "rand_ex" condition as child and adult utterances do not come from the same transcript

#### adult_utterance_order

Same as child_utterance_order, but this time for the parent

#### child_transcript_id

Id of the transcript from which the child utterance was retrieved

#### adult_transcript_id

Id of the transcript from which the adult utterance was retrieved

#### child_corpus_name

Name of the corpus from which the child utterance was retrieved

#### adult_corpus_name

Name of the corpus from which the adult utterance was retrieved

#### semantic_similarity

Float comprised between 0 and 1. The closer it is to 1, the higher the semantic similarity between the child and adult utterances.
</br>
It it computed by first tokenising both utterances using a Spacy model, specific to the language selected.
</br>
Tokens unknown to the Spacy models are removed from the utterances, function words (specified by Spacy) are removed too.
</br>
Each of the remaining tokens are transformed into a 300 dimension embedding using word2vec, then each of these embeddings are
summed to create one representation of the whole sentence.
</br>
The two resulting 300 dimension vectors representing the child and the adult utterances are used to compute the cosine similarity, which is our proxy for the semantic similarity.

#### editdistance

Int that represent the Levenshtein distance between child and parent utterancse.
</br>
The Levenshtein distance is the number of deletions, insertions, or substitutions that are required to transform one string (the source) into another (the target).
</br>
Here the atomic level is the word (or the token), not the character.
