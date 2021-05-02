# Camelot-Wheel-Notation
[![version][version_badge]][changelog]
[![CodeFactor][codefactor_badge]](https://www.codefactor.io/repository/github/regorxxx/Camelot-Wheel-Notation/overview/main)
[![Codacy Badge][codacy_badge]](https://www.codacy.com/gh/regorxxx/Camelot-Wheel-Notation/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=regorxxx/Camelot-Wheel-Notation&amp;utm_campaign=Badge_Grade)
![GitHub](https://img.shields.io/github/license/regorxxx/Camelot-Wheel-Notation)  
Javascript implementation of the Camelot Wheel, ready to use "harmonic mixing" rules and translations for standard key notations. The Camelot Wheel is a representation where musical keys are displayed as ‘hours’ on a clock followed by a letter (A, B). Its main use is mixing songs following a set of rules named "harmonic mixing" or "mixing in key".

## Usage
### Working with keys
Lets say you have a currently working code to retrieve a list of keys from some files then, to transalate standard keys like ('G#m' or 'Abm') into the camelot wheel Notation, you would do:
```javascript
let objKey = camelotWheel.getKeyNotationObject('Abm'); // {hour: 1, letter: 'A'}
```

Can also translate back such object into standard notation. Although note there 2 ways to represent the same key, that is: sharp and flat. Both are equivalent.
```javascript
... // where objKey = {hour: 1	, letter: 'A'}
let flatKey = camelotWheel.getKeyNotationFlat(objKey); // 'Abm'
...javascript
let sharpKey = camelotWheel.getKeyNotationSharp(objKey); // 'G#m'
```
To simply test if the keys exist in the wheel (i.e. are valid):
```javascript
camelotWheel.hasKey('G#m') // True
camelotWheel.hasKey('11A') // True
camelotWheel.hasKey('11C') // False
camelotWheel.hasKey(objKey.hour + objKey.letter) // True. Note objKey = {hour: 1, letter: 'A'} -> '1A'
```
### Harmonic mixing
Once you got your keys translated into something usable within the wheel, you can find harmonically compatible keys using the built-in methods:
 1. Perfect Match (nX -> nX): staying in the same 'hour' and letter.
 2. Energy Changes:
 	* Energy Boost (nX -> n+1X): adding one 'hour' (+1), equivalent to going up a fifth.
 	* Energy Drop (nX -> n-1X): subtracting one 'hour' (-1), equivalent to going down a fifth.
 	* Energy Switch (nA -> nB): staying in he same 'hour' but changing the letter, equivalent to going from relative minor to major (and viceversa).
 3. Mood Changes:
 	* Mood Boost (nA -> n+3B): adding three 'hours' (+3), equivalent to going from minor to major.
 	* Mood Drop (nX -> n-3B): subtracting three 'hours' (-3), equivalent to going from major to minor.
 4. Key Changes:
 	  * Dominant Key (nA -> n+1B): subtracting one 'hour' (+1) and changing the letter.
 	  * Sub Dominant Key (nB -> n-1A): subtracting one 'hour' (+1) and changing the letter.
 5. Experimental Energy Changes:
 	* Energy Boost ++ (nX -> n+2X): adding two 'hours' (+2), equivalent to going up a step.
 	* Dramatic Energy Raise (nX -> n+7X): adding seven 'hours' (+7), equivalent to going up a half step.

Which are named: perfectMatch, energyBoost, energyDrop, energySwitch, moodBoost, moodDrop, domKey, subDomKey, energyRaise

For ex. to find the next key with an energy boost:
```javascript
let objKey = camelotWheel.getKeyNotationObject('Abm'); // {hour: 1, letter: 'A'}
let nextObjKey = camelotWheel.energyBoost(objKey); // {hour: 2	, letter: 'A'}
```
The same for a mood boost:
```javascript
let objKey = camelotWheel.getKeyNotationObject('C#m'); // {hour: 12, letter: 'A'}
let nextObjKey = camelotWheel.moodBoost(objKey); // {hour: 3, letter: 'A'}
```
While summing and substracting is a pretty easy operation, note the hour it's a cyclic value from 1 to 12, therefore using the built-in methods is much easier than working with the objects. Also, it allows to easily create patterns to call the methods on sucessive movements:
```javascript
...
const playlistLength = 50;
const movements = {
		perfectMatch: 	35	, // perfectMatch (=)
		energyBoost : 	10	, // energyBoost (+1)
		energyDrop  :	10	, // energyDrop (-1)
		energySwitch:	10	, // energySwitch (B/A)
		moodBoost   :	5	, // moodBoost (+3)
		moodDrop    :	5	, // moodDrop (-3)
		energyRaise :	5	, // energyRaise (+7)
		domKey      :	10	, // domKey (+1 & B/A) = energyBoost & energySwitch
		subDomKey   :	10	, // subDomKey (-1 & B/A) = energyDrop & energySwitch
	}; // Sum must be 100%
let pattern = [];
Object.keys(movements).forEach((key) => {
	pattern = pattern.concat(Array(Math.ceil(playlistLength * movements[key] / 100)).fill(key));
});
pattern.sort(() => Math.random() - 0.5);
for (let i = 0; i < playlistLength; i++) {
  keyCurrent = yourOwnTagFunc();
  camelotKeyCurrent = camelotWheel.getKeyNotationObject(keyCurrent);
  nextKeyObj = camelotKeyCurrent ? camelotWheel[pattern[i]](camelotKeyCurrent) : null;
  ...
 }
```

## Real World Implementation
As is, the camelot wheel only translates keys and outputs the next key when using one of the "harmonic mixing" rules. But to get it working with real files, you either need some library which reads tags from files or some implementation within a music playert.  
You can find such examples in these foobar2000's scripts:

 1. [Search-by-Distance-SMP](https://github.com/regorxxx/Search-by-Distance-SMP): creates playlists with similar tracks by multiple methods and following "harmonic mixing" rules.  
 2. [Playlist-Tools-SMP](https://github.com/regorxxx/Playlist-Tools-SMP): takes an already existing playlist as pool and creates a new one following "harmonic mixing" rules.  

![Animation](https://user-images.githubusercontent.com/83307074/116748571-2bad7d00-a9ef-11eb-8c8b-6d77c797d4c8.gif)

[changelog]: CHANGELOG.md
[version_badge]: https://img.shields.io/github/release/regorxxx/Camelot-Wheel-Notation.svg
[codacy_badge]: https://api.codacy.com/project/badge/Grade/1e7a52c1cd0e406f9c46357d21f7bfac
[codefactor_badge]: https://www.codefactor.io/repository/github/regorxxx/Camelot-Wheel-Notation/badge/main
