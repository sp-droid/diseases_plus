# Diseases+, an EU5 disease system expansion

A mod to improve on the realism and satisfaction of EU5's disease system.

## Main changes

* System overhaul: *removed America's great pestilence event, added partial immunity mechanic*. Diseases will naturally, through their intrinsic characteristics, ravage *any* territory with prior exposure, without an arbitrary predisposition for any region. 
* Changes to core mechanics in mortality, resistance decay, propagation and endemic spots. Disease spread is no longer arbitrarily bound to be near their original outbreak location.
* New clearer colours for the different diseases
* Naming: Great pestilence replaced with Enteric fever, Bubonic plague with Plague
* Other changes

## Diseases table

**Nomenclature:**

* Naive population: Population with no immunity
* r0: How many people does 1 infect on average, in naive populations, a measure of transmisivity
* HIT: Herd Immunity Threshold. Above this number there is no transmission. It loses meaning when the disease is transmitted through very mobile vectors (fleas, mosquitos) or when there are chronic carriers
* IFR\_naive: Infection Fatality Rate (for naive populations. Think Americas, or Europe first contacts with the bubonic plague in centuries)
* p\_i\_eff: Partial immunity reduction in disease mortality from IFR\_naive. I wrote in parenthesis the average resulting IFR. At 100, immunity from previous generations is enough to completely eliminate mortality, at 0, it doesn't do anything. Notice how measles and smallpox do not suffer significant reductions.
* m: Yearly immunity loss modifier. Goes from 0 (100% lost in a year) to 1 (no loss).
* interval: Province spread interval in days. How quickly it propagates geographically

| Name          | Color | IFR\_naive % | p\_i\_eff % | r0            | HIT %     | m    | interval |
| :------------ | :---- | :----------- | :---------- | :------------ | :-------- | :--- | :------- |
| Influenza     | 🟦     | \[1-3\]      | 97.5 (0.05) | \[1.2-2.5\]   | \[16-66\] | 0.8  | 5        |
| Measles       | 🟥     | \[20-30\]    | 80 (5)      | \[12-18\]     | \[92-94\] | 1.0  | 11       |
| Enteric fever | 🟨     | \[10-30\]    | 95 (1)      | \[2-5\]       | 100\*\*   | 0.95 | 12       |
| Typhus        | 🟪     | \[20-40\]    | 93.3 (2)    | \[1.5-3\]     | \[33-66\] | 0.98 | 30       |
| Smallpox      | 🟫     | \[20-30\]    | 40 (15)     | \[3.5-6\]     | \[71-83\] | 1.0  | 12       |
| Plague        | ⬛     | \[30-50\]*   | 75 (10)*    | \[1.5-1.9\]\* | 100\*\*   | 0.99 | 24       |
| Malaria       | 🟩     | \[10-20\]    | 86.7 (2)    | 0             | 100\*\*   | 0.8  | \-       |

\*: For the pneumonic variation of the disease, a +95% severity is applied, which means IFR_naive becomes \[57-95\] or \[35-58\] with partial immunity. This event happens when the disease presence (checked once every month) is:

- Greater than 75% for rural settings
- Greater than 50% for towns
- Greater than 25% for cities

\*\*: Chronic carriers and mobile vectors make herd immunity not efficient.

**Formulas:**

* immunity\_(year i+1) / immunity\_(year i) = m / (1+k \* %pop\_growth)
* HIT = 1 - 1/r0

## Other

**Justifications:**

* In line with some research on the Cocoliztli epidemics which attributed the most likely cause to a strain of Salmonella bacteria, the "Great pestilence" disease itself will be called Enteric fever (paratyphoid or typhoid fever caused by Salmonella. Though not all Salmonella strains cause it.), but leaving the situation name the same. The future intent would be to group deaths from all European diseases that happen in the Americas, not just one. Smallpox, measles... also had a great impact. Sources: [https://www.nature.com/articles/s41559-017-0446-6](https://www.nature.com/articles/s41559-017-0446-6) [https://journals.openedition.org/nda/14270](https://journals.openedition.org/nda/14270)
* Bubonic plague only refers to one stage of the plague disease. Plague includes the pneumonic variant as well.
* Would love to add more diseases with distinct profiles, like slower diseases such as leprosy or diseases coming from America like syphilis (which is also rather slow), but currently the game crashes when adding new ones.
* Made disease colours more distinct, in line with their characteristics: winter blues (influenza), greens (malaria), reds (measles), blacks (bubonic plague), light browns (smallpox), purples (typhus) and yellows (salmonella).
* In the base game, mortality is unlinked from the local resistance of the disease. In other words, every infected person, after class modifiers, has the same chance to die. This is unrealistic for many reasons:
   * Even if a person hasn't been infected with the disease directly, it's proven it receives antibodies and immunological information from the mother while in the placenta, as seen with COVID. Therefore, even if they can still get infected, you would expect better immune responses. **The more resistance in a population, the lesser the mortality should be, but only long term.** Source: [https://link.springer.com/article/10.1186/s12879-025-11225-6](https://link.springer.com/article/10.1186/s12879-025-11225-6)
   * **The base mortality number should be much higher**. Smallpox in the Americas killed between 40 and 50% of the infected, far above the 20-30% Europe was seeing. The game currently randomises a value between 5 and 30%, which is fair, each strain has different characteristics, but that matters more when the person has some defences. America showed the strain didn't matter much against naive immune systems. **The base mortality shouldn't be the only value being randomised, but its interaction with local resistance**.
   * Pathogens' main evolutionary goal is to replicate and persist. Against naive populations, the pathogen can optimise transmission fully, so variants have little pressure not to kill more slowly. In immune populations they require more sophisticated evasion mechanisms, even at some cost. For example the influenza virus has a harder time living outside of the host, or even replicating as quickly as it would, because it has made sacrifices to be able to live in partially immune hosts. Introduce it to a large naive population and suddenly this pressure is gone, new strains are no longer bounded and can optimise themselves. Strains can't be simulated in this game, so again, more reason to **change mortality according to initial local resistance**. Source: [https://pmc.ncbi.nlm.nih.gov/articles/PMC7118948/](https://pmc.ncbi.nlm.nih.gov/articles/PMC7118948/)
   * As with pathogens, humans evolve to fight them, making sacrifices in the process. For example it was found that the expression of the gene related to auto-immune diseases (Crohn's) rose up in European populations during the worst epidemics of the Middle Ages, because even though it raised inflammation long-term, it kept the immune system alert and readier to fight. **Populations should get secondary resistance, for example from fighting smallpox, they get slightly better at fighting flu.** Source: [https://doi.org/10.1038/s41586-022-05349-x](https://doi.org/10.1038/s41586-022-05349-x)
* Herd immunity is not very obvious in the game. When resistance is very high, disease should not be able to spread at all, because every infection path is blocked by immune people, specially on certain diseases. Some others do spread regardless, such as influenza, but of course we can simulate this with a higher resistance decay and spawn rates.
* **Resistance should grow less with highly lethal variants**. If you kill 100% of your hosts, regardless of how many are infected, there can't be any growth in resistance.
* In reality, the bubonic plague became endemic to Europe in rural rodent populations (during Justinian's plague, it didn't go as far north and inland as the Black Death did. Most dense cities were all in the coast, there were not as many rats...). Malaria was endemic to several parts in Europe. **Add a system for automatic creation and deletion of endemic disease spots.**
* The most frustrating aspect of this whole system, to me at least, is how little we can do. I propose a system of **event-driven inventions that allow countries to learn from failures**. This means your country will learn valuable knowledge from suffering mainly the Black Death, which will give you access to new policies, new buildings and even court practices, with much less impact than the initially available ones.
* **You will be able to stop every disease from the start by closing down your country inside-out**, with massive repercussions to your economy and estates, and even a slight chance of failing regardless. But will you want to do it? **I will set up some AI countries to go this route**. Your population will be intact, but you won't learn anything from the plague. With new dynamic endemic spots around Europe, recurrent outbreaks will be common, and countries without immunity should suffer the same fate in the end. **Disease inventions can be researched, but they are slow and expensive** (in the first few ages, where traditions are not the healthiest), they will set you back. Diseases will be much more frequent and fast moving, your country will need to close down every time. But the path is there.
* Bubonic plague specific decisions will be reduced and kept to a minimum. Decisions will be available all the time.
* To simulate a particularly virulent strain of the same type of microbe we can reset the resistance of the whole world and remove the partial immunity flags. For example for the Spanish flu, even though it was influenza, nobody had immunity because of its unique mutations offering around 2.5% mortality, much higher than the less of 0.1% of seasonal flu.
* Change of names: disease resistance to immunity.
* Minimum immunity loss will depend on pop growth and a factor k based on the ability to pass immunological information past generations. Depends on disease and it can be <=1