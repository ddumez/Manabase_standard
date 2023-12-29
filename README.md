This is a fork of Chris-Cunningham works, who himself worked on the basis of Frank Karsten's seminal paper on the subject (see CFB)


Manabase
========

A simulator that runs a Magic: The Gathering deck through repeated trials to determine the effectiveness of the deck's mana.

To use the tool, you will need a Python interpreter (this was tested with Python 3.4, see https://www.python.org/download/releases/3.4.0/) and decklists in text format.

I'm not a professional programmer, so suggestions and constructive criticism about the tool are welcome!

The Output
========

The output you get will say something like: "**Siege Rhino, Turn 4: 69.3%.**"

What this means, precisely, is: 

* If you are on the play, and 
* you follow a very specific mulligan strategy, and in doing so,
* you end up with a Siege Rhino in your opening hand, 
* then you have a 69.3% chance of casting it by turn 4.

**This would actually be a very high rate** -- a deck with 24 basic lands only casts its 4-drops on time 65.5% of the time. It turns out that with only six scrylands, it is actually possible to overcome color problems and even surpass the mana of a deck full of only basics.

Features
========

* Given a decklist, the program shuffles the deck, draws an initial hand, and then simply tries all possible lines of play to see if any of them lead to spells being castable. 
* The lines of play can be printed to the screen for the user's inspection, or played silently to collect data as quickly as possible (thousands of trials per minute).
* Aggregate results are stored over multiple sets of trials and can be output in text format or HTML tables.
* A simple mulligan strategy is implemented: 7-card hands with 0, 1, 6, or 7 lands are mulliganed. 6-card hands with 0, 1, 5, or 6 lands are mulliganed. 5-card hands with 0 or 5 lands are mulliganed.
* Multicolored lands are handled properly. Fetchlands and scrylands are implemented as more branches in the lines of play. Fetchlands correctly fetch shocks if needed. Checklands work properly with basics and shocks.
* The most common corner cases in Khans of Tarkir standard, namely Urborg, Tomb of Yawgmoth, Chained to the Rocks, and Evolving Wilds, are handled appropriately. All of KTK standard works except Nykthos.
* Mana abilities of cards like Elvish Mystic, Noble Hierarch, Sylvan Caryatid, and Abzan Banner are implemented, but mana abilities that have a cost (e.g. Signets) silently give wrong results.
* Multiple spells can be cast per turn, e.g. a hand of three Plains, an Abzan Banner, an Avacyn's Pilgrim, and a Wingmate Roc successfully casts Avacyn's Pilgrim turn 3 and Wingmate Roc turn 4.
* Satyr Wayfinder is implemented, and he can even get lands that get played that turn, casting spells that turn. Courser of Kruphix works as well, and has friendly interactions with fetchlands.
* Benchmark numbers can be displayed, so you know that 70% Siege Rhino on turn 4 is a very high number.
* A comprehensive battery of tests helps ensure that the results we get here are accurate.

To-do
========

* Allow an option to scale the percentages so that not making land drops doesn’t count against a spell's castability. (First step complete -- we can now calculate benchmarks)
* Try to identify when something unsupported is in the decklist instead of silently giving nonsense answers.
* Implement card draw spells.
* Implement Nykthos.
* Track pain taken from painlands and fetches somehow; track life gained from lands and Courser.
* Do something intelligent with sideboards.

Not Planned
========

* X spells will probably always be treated as though X = 0.
* Delve and other alternate cost mechanics are probably forever ignored. If you want to check whether you have double blue by turn 5, replace Dig Through Time with Tidebinder Mage in your decklist.
* Fetching will never shuffle your library, meaning it does not exactly work correctly with scry; if it did, another fix would be needed to prevent “prescient fetching.” Courser's current implementation adds "scry 1" to every fetchland in play, which is pretty close to what it does.
* “Curving out”: If your hand is Temple, Mountain, then you can’t cast both your one-drop and your two-drop on curve, but the program notices that you can do either one, so both will probably always count as castable.

See Also
========

In case it wasn't clear, this kind of analysis is definitely inspired by Frank Karsten's Frank Analysis series, especially the article here: http://www.channelfireball.com/articles/frank-analysis-how-many-colored-mana-sources-do-you-need-to-consistently-cast-your-spells/ . 


=========================================================
Purpose of the branch : adapt this simulator to the current standard
I modified it to simulate the interactions between dragons, plaza of heroes, cavern of souls, Rivaz of the claw, and classic standard lands as follow :

parse irencrag OK
parse rivaz OK
parse cavern OK
parse plaza OK
prototype checkManaAvailability OK
call checkManaAvailability OK
implement rivaz OK
implement cavern for dragons OK
implement plaza for legend OK
implement rivaz restriction OK
implement cavern for second NO
implement plaza for second OK

write painlands OK
write fastland OK
write slowland OK
write triomes OK
write manlands OK
implement painlands NO
implement fastland OK
implement slowland OK
implement triomes OK
implement manlands OK