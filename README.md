# Kabap Test Suite

This repository contains a collection of platform-independent resources which can be used to validate and exercise an implementation of Kabap to ensure correctness and compliance with the language definition.  It is designed to be embedded in to the platform specific tests to compliment and complete them -- this test suite is not very useful on its own (other than perhaps a learning tool for the language).

Platform specific tests will be found in the relevant project for the platform you wish to test.  This includes things such as tests for creating, recycling and destroying a Kabap instance; working with extensions; and integrating it with your project on the relevant platform.

It is important that you ensure your platform specific tests are successfully passing before running this test suite as a faulty implementation may produce false positives and false negatives with the language tests themselves.

>**N.B.:** The test suite may take a few minutes to run and may not display any output when busy.


## What's included
1. test_list.json
    Defines the files to test, the way the platform should test them, and the expected results.
2. test_scripts/
    This directory contains scripts to test in human readable format.
3. test_tokens/
    This directory contains scripts to test in tokenised format.


## A note on tokens
Kabap loads and parses human readable scripts to an internal token format for execution.  For greater speed efficiency and reduced memory consumption this tokenised representation of a script can be saved and loaded directly in the future to remove the expensive parsing process and associated overheads.

Developers using the token format will most likely do so for scripts that are frequently run; therefore quick time-to-execution will be a primary requirement.  To help achieve this speed the token format file is loaded **without** much validation or sanity checking.  It is a design decision in all platforms to assume that an imported token format file is always correct (as only correctly parsed human readable scripts can be exported tokenised in the first place).  It is the responsibility of the developer to maintain the integrity of the token format file between saving and subsequent future loading.

>**N.B.:** Because some tests are invalid by design they could not have equivalent tokenised versions because they would likely just crash the Kabap implementation trying to load them.

It is recommended to treat the token format file as a binary file due to the ease it could be corrupted if processed incorrectly.


## File names, prefixes and extensions
All test files are prefixed with a character followed by an underscore.  This character relates to the general area of the implementation which is being tested and map as follows:

	1  	parser
	2  	tokeniser
	3  	reserved (for optimiser/minifier)
	4  	executor
	5  	resolver
	6  	language (flow, conditionals, comparators)
	7  	language (operators, assignment, variables)
	8  	language (references/extensions)
	9  	language (complex examples)
	a  	re-runs, reuses and recycles

Human readable script files always end with the file-extension suffix *.kabap* and tokenised files always end with the suffix *.kat*


## Test guidelines
When using or creating tests please ensure they follow the following guidelines.

### Unicode
Kabap is fully Unicode and uses UTF-8 by default, without the Byte Order Mark (BOM).  All test files, strings and implementations should expect and require this encoding.

### White space
Kabap is a free-form language where white space is optional and ignored.  Unless the test is specifically exercising the processing of white space (usually tests prefixed with *1_*) then all tests use the original authors' preference of a single space separating the different components of a statement; and conditions and loops are indented by tabs.

### Line endings
All tests that span multiple lines use UNIX line endings ``\n (LF, ASCII 10)`` unless they are specifically exercising the processing of Windows and classic Mac line endings.

All tests that are not considered a complex example (tests prefixed *9_*) will **not** end with a trailing newline character unless it is trailing white space which is being tested.  This makes *NIX systems report the files as having incomplete lines.  As line endings are something the parser has to process it was felt that removing everything unnecessary except that which is being tested would be beneficial to create minimal tests.  Be sure to test if your editor automatically adds a trailing line ending.


## Testing the test suite
Each platform specific unit tester should include a test to ensure that every *.kabap* and *.kat* file found in this test suite is included somewhere in the list of tests specified in the JSON file.  This is to prevent the accidental addition of any non-exercised tests, as well as to prevent the accumulation of file cruft from any removed tests.

The JSON test list includes a UTF-8 hint called "utf8" as the first key.  The value of this key is the Unicode tick/check mark U+2713: ``✓``
Usually editors will automatically detect this and default to UTF-8 encoding, but please check this whenever editing the file.  Some test results have non-ASCII characters and editing the test list in another encoding will likely corrupt these and produce false-negative failed tests.


## Thank you
Thank you for choosing to use Kabap in your project.  We kindly accept contributions to the test suite via a pull request.  When reporting an issue or bug please create a minimal test which exhibits the problem.  It is helpful (but not essential) when submitting a ticket to reference it to your new test.


## License
Copyright 2017-18 White Label Dev Ltd, and contributors.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use these files except in compliance with the License.  You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the License for the specific language governing permissions and limitations under the License.