# Camelot-Wheel-Notation
[![version][version_badge]][changelog]
[![CodeFactor][codefactor_badge]](https://www.codefactor.io/repository/github/regorxxx/Camelot-Wheel-Notation/overview/main)
[![Codacy Badge][codacy_badge]](https://www.codacy.com/gh/regorxxx/Camelot-Wheel-Notation/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=regorxxx/Camelot-Wheel-Notation&amp;utm_campaign=Badge_Grade)
![GitHub](https://img.shields.io/github/license/regorxxx/Camelot-Wheel-Notation)  
Javascript implementation of the Camelot Wheel, ready to use "harmonic mixing" rules and translations for standard key notations. The Camelot Wheel is a representation where musical keys are displayed as ‘hours’ on a clock followed by a letter (A, B). Its main use is mixing songs following a set of rules named "harmonic mixing" or "mixing in key".

## Usage
### Working with keys
Lets say you have a currently working code to retrieve a list of keys from some files then, to translate Standard Keys like ('G#m' or 'Abm') into the Camelot Wheel Notation, you would do:
```javascript
let objKey = camelotWheel.getKeyNotationObjectCamelot('Abm'); // {hour: 1, letter: 'A'} 
let objKeyOpen = camelotWheel.getKeyNotationObjectOpen('Abm'); // {hour: 6, letter: 'm'}
```
Can also translate back such object into Standard notation. Although note there are 2 ways to represent the same key, that is: Sharp and Flat. Both are equivalent.
```javascript
... // where objKey = {hour: 1	, letter: 'A'}
let flatKey = camelotWheel.getKeyNotationFlat(objKey); // 'Abm'
let sharpKey = camelotWheel.getKeyNotationSharp(objKey); // 'G#m'
```
To simply test if the keys exist in the wheel (i.e. are valid):
```javascript
camelotWheel.hasKey('G#m') // True
camelotWheel.hasKey('11A') // True
camelotWheel.hasKey('11C') // False
camelotWheel.hasKey(objKey) // True. Note objKey = {hour: 1, letter: 'A'} -> '1A'
```
Finally key objects may be translated from Camelot notation to Open Key notation and vice-versa.
```javascript
// '{hour: 1, letter: 'A'} -> {hour: 6, letter: 'm'}'
objKeyOpen = camelotWheel.translateObjectCamelotToOpen(objKey);
```

### Harmonic mixing
Once you got your keys translated into something usable within the wheel (objects), you can find harmonically compatible keys using the built-in methods:
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
let objKey = camelotWheel.getKeyNotationObjectCamelot('Abm'); // {hour: 1, letter: 'A'}
let nextObjKey = camelotWheel.energyBoost(objKey); // {hour: 2	, letter: 'A'}
```
The same for a mood boost:
```javascript
//  Note it works the same on both camelot and open key objects
let objKey = camelotWheel.getKeyNotationObjectOpen('C#m'); // {hour: 5, letter: 'm'}
let nextObjKey = camelotWheel.moodBoost(objKey); // {hour: 8, letter: 'm'}
```
While summing and subtracting is a pretty easy operation, note the hour it's a cyclic value from 1 to 12, therefore using the built-in methods is much easier than working manually with the objects. Also, it allows to easily create patterns to call the methods on successive movements:
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
	pattern = pattern.concat(
		Array(Math.ceil(playlistLength * movements[key] / 100)
	).fill(key));
});
// Sort randomly
let last = pattern.length;
let n;
while (last > 0) {
	n = Math.floor(Math.random() * last);
	--last;
	[pattern[n], pattern[last]] = [pattern[last], pattern[n]];
}
// And apply to your tracks
for (let i = 0; i < playlistLength; i++) {
  keyCurrent = yourOwnTagFunc();
  camelotKeyCurrent = camelotWheel.getKeyNotationObject(keyCurrent);
  nextKeyObj = camelotKeyCurrent ? camelotWheel[pattern[i]](camelotKeyCurrent) : null;
  ...
 }
```
Alternatively, a pattern like that can be created with this method:
```javascript
const playlistLength = 50;
const pattern = camelotWheel.createHarmonicMixingPattern(playlistLength);
```
... and applied directly to a key object to get the list of keys which would give such pattern:
```javascript
...
let objKey = camelotWheel.getKeyNotationObjectOpen('C#m'); // {hour: 5, letter: 'm'}
// Works with both Camelot and Open Keys objects
const pattern = camelotWheel.applyPattern(objKey, pattern); // An array of key objects
```


## Real World Implementation
As is, the Camelot Wheel only translates keys and outputs the next key when using one of the "harmonic mixing" rules. But to get it working with real files, you either need some library which reads tags from files or some implementation within a music player.  
You can find such examples in these foobar2000's scripts:

 1. [Search-by-Distance-SMP](https://github.com/regorxxx/Search-by-Distance-SMP): creates playlists with similar tracks by multiple methods and following "harmonic mixing" rules.  
 2. [Playlist-Tools-SMP](https://github.com/regorxxx/Playlist-Tools-SMP): takes an already existing playlist as pool and creates a new one following "harmonic mixing" rules.  

![harmonicmix](https://user-images.githubusercontent.com/83307074/163871234-0388e995-95ea-4fb5-b286-2462f5dcc462.gif)

[changelog]: CHANGELOG.md
[version_badge]: https://img.shields.io/github/release/regorxxx/Camelot-Wheel-Notation.svg
[codacy_badge]: https://api.codacy.com/project/badge/Grade/1e7a52c1cd0e406f9c46357d21f7bfac
[codefactor_badge]: https://www.codefactor.io/repository/github/regorxxx/Camelot-Wheel-Notation/badge/main
