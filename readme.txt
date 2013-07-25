PFRPG-NPCGen
============

Pathfinder RPG - Random NPC Generator


To Use The Generator:
First compile the source code.  The generator is written in C++, using whatever compiler you prefer simply compile the code found in the "Generator" file.
Once the code is compiled run the program in terminal, or command prompt if using Windows.  You will be prompted to for a party size and level.  Simply enter how many characters you want and what level you want them.  The generator will randomly create an a party of that many characters at that level.
Characters will be created with one of the following classes:
Aristocrat, Caster, Expert, Scout, Warrior, Hireling.  Casters are the Core Rulebook's Adepts, although since the generator does not produce a spell list the statblock is applicable for most purely casting characters the GM chooses to create.  Scouts are an averaged "multiclass" hybrid of Expert and Warrior, with their skill list borrowed from Rangers.  Hirelings are built using the Commoner class, since even among NPC's the role of water-boy is all they're useful for.
The generator produces a full character (sans spell list) complete with skills, feats, and equipment.  All skill, save, and attack bonuses from feats are already accounted for in the statblock, no need to add these in.
In addition to all character abilities the generator will also produce character equipment.  It randomly assigns equipment to the character appropriate to his class and abilities.  It also gives his equipment level appropriate bonuses and he comes ready with a suitable number of healing potions, also of an appropriate level.  Character equipment value scales with level.

How It Works:
For each character the generator rolls ability scores, 3d6 assigned in order.  It chooses a class based on the highest ability score, asssuming this is greater than 12.  If no score is greater than 12 the character is relegated the role of Hireling.  After selecting class is selects a race at random, afterwards so each race has an equal chance of producing each class.  Racial skill, ability score, and save bonuses are applied.  It then calculates ability score level increases.  Then saving throws and attack bonuses are calculated.  Now it selects skills, at random based on the class skill list (completely randomly for Experts).  Then feats, as it does so is assigns whatever bonus the feat grants.  Finally equipment, giving equipment a magic enhancement bonus based on character level if appropriate.
