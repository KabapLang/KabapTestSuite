# Kabap Test Suite

This repository contains a collection of platform-independent resources which can be used to validate and exercise an implementation of Kabap to ensure correctness and compliance with the language definition.  It is designed to be embedded in to the platform specific tests to compliment and complete them -- this test suite is not very useful on its own (other than perhaps a learning tool for the language).

Platform specific tests will be found in the relevant project for the platform you wish to test.  This includes things such as tests for creating, recycling and destroying a Kabap instance; working with extensions; and integrating it with your project on your chosen platform.

It is important that you ensure your platform specific tests are successfully passing before running this test suite as a faulty implementation may produce false positives and false negatives with the language tests themselves.

>**N.B.:** Some tests may take a few minutes to run and may not display any output when busy.


## What's included
1. test_list.json
    Defines the files to test, the way the platform should test them, and the expected results.
2. test_scripts/
    This directory contains tests in human readable format.
3. test_tokens/
    This directory contains tests in tokenized format.
4. [CHANGELOG.md](CHANGELOG.md)
    The list of changes to the test suite.


## A note on tokens
Kabap loads and parses human readable scripts to an internal token format for execution.  For greater speed, efficiency and reduced memory consumption, this tokenized representation of a script can be saved and loaded directly in the future to remove the expensive parsing process and associated overheads.

Developers using the token format will most likely do so for scripts that are frequently run; therefore quick time-to-execution will be a primary requirement.  To help achieve this speed the token format file is loaded **without** much validation or sanity checking.  It is a design decision in all platforms to assume that an imported token format file is always correct (as only correctly parsed human readable scripts can be exported tokenized in the first place).  It is the responsibility of the developer to maintain the integrity of the token format file between saving and subsequent future loading.

>**N.B.:** Because some tests are invalid by design, they could not have equivalent tokenized versions because they would likely just crash the Kabap implementation trying to load them.

It is essential to treat the .kat token format files as binary files due to the ease they could be corrupted if processed incorrectly.  Token files include a mechanism to hint to various programs (such as FTP or editors) that they should not be treated as text, but this cannot be relied on so exercise caution when saving and loading them in your implementation.


## File names, prefixes and extensions
All test files are prefixed with a character followed by an underscore.  This character relates to the general area of the implementation which is being tested and map as follows:

	0  	Sanity
	1  	Parser
	2  	Tokenizer
	3  	Optimizer/Minifier
	4  	Executor
	5  	Resolver
	6  	Language (flow, conditionals, comparators)
	7  	Language (operators, assignment, variables)
	8  	Language (references/extensions)
	9  	Language (complex examples)
	a  	Standard library (file and net)

Human readable script files always end with the file-extension suffix *.kabap* and tokenized files always end with the suffix *.kat*


## Test guidelines
When using or creating tests please ensure they follow the following guidelines.

### Unicode
Kabap is fully Unicode and uses UTF-8 by default, without the Byte Order Mark (BOM).  All test files, strings and implementations should expect and require this encoding.

### Whitespace
Kabap is a free-form language where whitespace is optional and ignored.  Unless the test is specifically exercising the processing of whitespace (usually tests prefixed with *1_* or *9_*) then all tests use the original authors' preference of no space separating the different components of a statement, unless required to distinguish different operators.  Each statement is on its own line as defined in the next section.  Indentations for conditionals and blocks are not used.

### Line endings
All tests that span multiple lines use UNIX line endings ``\n (LF, ASCII 10)`` unless they are specifically exercising the processing of Windows and classic Mac line endings.

All tests that are not considered a complex example (tests prefixed *9_*) will **not** end with a trailing newline character unless it is trailing whitespace which is being tested.  This makes *NIX systems report the files as having incomplete lines.  As line endings are something the parser has to process it was felt that removing everything unnecessary except that which is being tested would be beneficial to create minimal tests.  When making tests be sure to check if your editor adds a trailing line ending automatically and disable or remove them.


## Testing the test suite
Each platform specific unit tester should include a test to ensure that every *.kabap* and *.kat* file found in this test suite is included somewhere in the list of tests specified in the JSON file.  This is to prevent the accidental addition of any non-exercised tests, as well as to prevent the accumulation of file cruft from any removed tests.

The JSON test list includes a UTF-8 hint called "utf8" as the first key.  The value of this key is the Unicode tick/check mark U+2713: ``✓``
Usually editors will automatically detect this and default to UTF-8 encoding, but please check this whenever editing the file.  Some test results have non-ASCII characters and editing the test list in another encoding will likely corrupt these and produce false-negative failed tests.


## Thank you
Thank you for choosing to use Kabap in your project.  We kindly accept contributions to the test suite via a pull request.  When reporting an issue or bug please create a minimal test which exhibits the problem.  It is helpful (but not essential) when submitting an issue to reference it to your new test.


## License
Copyright 2017-22 Unify Process Magic Ltd

Licensed under the Apache License, Version 2.0 (the "License"); you may not use these files except in compliance with the License.  You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the License for the specific language governing permissions and limitations under the License.