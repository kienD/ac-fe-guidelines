## Language Keys

When a static string needs to be displayed to the end user, we should be using a language key.

### How to use a language key.
`Liferay.Language.get('this-is-my-language-key')`

### Types of language keys
We have separated language keys into 3 categories:
* Title
	* Before creating a title language key, we will need to check to see if the title capitalization is correct.
	* We generally use [Capitalize My Title](https://capitalizemytitle.com) with Chicago format.
	* e.g. `a-language-key-title=A Language Key Title`

* Fragment
	* Fragments are languages keys that are used with other language keys to create a complete key.
	* Fragment language keys should be appended with the word "fragment".
	* e.g. `title-fragment=title`
* Sentence
	* Sentences are self explanatory.
	* This should just be in a sentence format.
	* e.g. `this-is-my-cool-sentence=This is my cool sentence.`

You can also use [this](https://github.com/kienD/language-key-creator) to assist with language key creation.
