### hannah ###

7 c before Dan clarified in chat, this docstring read like jargon soup
  * Will add to GLOSSARY OF ABBREVIATIONS in the chgptmodels docstring as jargon is logged as a problem. The code assumes some familiarity with the algorithm it implements, so reminders of important definitions should be fine.
_18 c http//docs.python.org/2.7/library/math.html is also a valid address_
315 c not sure info about future code should go in docstring, as it's not relevant to current use
  * Agree; removed sentence
_321 m(mini) typo (ot->to)_
322 c does this need to be in the docstring if it's commented later?
  * No; docstring hadn't been updated after I changed minbic() from being a giant function to doling out the model tests. Removed.
331 c source/target is the standard notation in the machine learning community
  * Keeping predictor/predictand to reflect nomenclature in Menne/Williams 2009
334 m string 'missing\_val' or -9999 (whatever number they pass?)
  * Documented that it needs to be a float or an int.
_339 c maybe just give a psuedo example: ('model\_name', model analysis function)_
348 m random line break
  * PEP-8 suggests "Note that most importantly, the """ that ends a multiline docstring should be on a line by itself, and preferably preceded by a blank line". No change.
_352 m grammar: were not was_
_353 m break them out for clarity while skimming_
362 c ##?
  * I tend to use "#" on my first pass through documenting code while I write, and switch to "##" on subsequent iterations. I'll hold off on standardizing to a single format since the structure of this code will change very dramatically based on other input from this code review (at some point in the future)
365 c why isn't models a list?
  * Changed to be a list to match documentation
376 c what does this line do?
  * The code calling minbic() is built in such a way that it should **never** pass x, y with unequal lengths to minbic(). If for some reason this invariant is violated, it means something went terribly wrong before minbic() so I want the program to stop before it goes into the next phase and wastes the user's time.
382 m use "\t".join(['QYTP', 'QVAL',...]) or use width or align in a .format statement "{{0:width}, {1:width} etc".format(['QYTP',..], width)
  * All of the debug printing will be changed in the near-future on a feature iteration of chgptmodels. Leaving alone for now, just for having easy debugging against the Fortran code
388 m what does Values actually store? this is vague
  * Yes; as per Nick's comments below, I will re-write minbic() in the future so that it's less procedural and more object-oriented. On that feature iteration I'll remove Values; for now it just restores intermediate computations.
406 m TPR, PHA?
  * See glossary at top docstring
411 c written more like a tutorial then a doc
  * Leaving comments so that I can quickly remind myself of what various segments do while I build the algorithm. Logged as an Issue in Google Code.
414 c is the sorted function in the code or external?
  * standard Python sorted()
422-436 c maybe use a logging library?
  * Logged as "to-do" in Issue tracker. Leaving for now, for debugging purposes against Fortran code.
424 c why +2?
  * To match **exactly** the Fortran changepoint numeration
441 c why? is KTHTPR0 simple needed?
  * Yes; it doesn't consider the slope on either side of the breakpoint, whereas KTHTPR1 does. The difference between KTHTPR0 and KTHTPR1, then, is that KTHTPR0 assumes the slope is insignificant before and after the changepoint (flat-to-flat)
475 m be somewhat consistent about how you doc dicts
  * RChanged to :Param xyz: w/ description, as in previous docstrings
478 c here?
  * end of sentence started on previous line
485 c there's an actual name for this (can't remember if it's p at alpha = .5 or something) but use the stats terminology
  * Yes, stats terminology is alpha=0.05 or 95% significance. I chnaged it (and subsequent references) to "The critical value for the test statistic at alpha=0.05."
485 c  one tail or two tail?
  * Test is one-tailed by nature; user should defer to menne williams 2009 for specfic details.
505 m  comment on why it's a constant, and maybe use CAPS to signify?
  * Not really a 'constant'; it's a default value, so I added the comment that "Test assumes no step change"
508 c why 2?
  * Degrees of freedom, which is equal to the number of parameters fit. The model we're testing is a simple linear regression which has two parameters - slope and y-intercept. The BIC (computed by Bayes) penalizes based on the parameters fit to help counter-act overfitting of a model. This is documented in bayes() (in this file, excluded in our review).
508-513 c use the string functions
  * A general comment since this issue comes up a lot - All of these outputs are for debugging purposes only and are designed to facilitate exact comparison agains the Fortran code. These will all be replaced by logging functionality, which as mentioned earlier, is on the feature to-do list. I'll italicize future referencse to this issue.
518 c any reason why mu and alpha are a double list of the same value? and why you need test\_stat and criT\_val if they're 0
  * Consistency; want to return the same basic data with the same structure (in this case a dictionary with certain keys and values) for every model test function.
537 c link to wiki entry on kendall-theill?
  * Added in glossary
_539 m typo breajpoint->breakpoint_
582 c is it okay that the break point is folded into the left?
  * Yes; this is how the test is implemented in the original Fortran code and is done on purpose.
604 c what's t?
  * legacy value from Fortran code. Removed.
_614 m apply retro to all the print statements, but label what you're printing "head: ", etc.. so people who don't know how you structured it know what they're seeing_
_654 m type to->two_
679 c why are you copying y?
  * Replaced all\_data with y (only one reference)
692 c why copy all\_valid?
  * Replaced valid\_all with all\_valid\_data (only one reference)
704 why don't you just do nslp=len(slopes) after the loops finish
  * done


---


### ocefpaf ###

1054 (q) why no default for missing\_val
  * Added default value -9999 and updated docstring
1039 (m): print more info with the number (Actually this applies to all your print statements.)
  * Logging to-do list
968 (q) why no default for missing\_val
  * Added default value -9999 and updated docstring
_874: (q) here you do define a default for missing\_val..._
533 (q): Does it make sense to have kthtpr0, kthtpr1, kthtpr2, kthtpr3 and kthtpr4 wrapped in one func with kw?
  * We talked about this on Monday in our post-Review meeting. My code will need a big re-write to be more object-oriented and less complex, Fortrany. I'll log this as a major defect in my code and resolve it when I re-write the module.
_392 (q): Is it worth having a get/set property here or some sort of validation for the values?_
  * Above
378 (q): Raise something here to help the user of the assert is enough?
  * See Hannah's question above; something truly terrible has to happen for this assert to fail, so I think it is enough.
19 (c): import math instead of from math.
  * PEP 8 doesn't care what method; I prefer using `from module import fcn`


---


### Nick ###

308 m variable names: suggest "breakpoint" and "missing".
  * changed `bp_index`->`breakpoint`. `missing_val` is a relic from early one, when I used `missing` as a boolean placeholder for when there was missing data. I prefer `missing_val`
310 m "(minbic\_models, above)" doesn't refer to anything.
  * removed reference
340 m don't pass names as well as functions: I suggest just functions.
  * The big problem I face is that I need to know the names of the functions and have a list of them so I can know their order later on. I could've had two parameters in the minbic() signature - model\_names, model\_fcns (or something similar), but it seemed reasonable to zip them up and unzip them in the function (which I do on line 374). Since I'll be re-structuring this code in a major fashion, I'll change it then.
347 m unclear name "cmodel"
  * changed `cmodel`->`model_name` through line 417. Beyond that, retaining convention to match Fortran source (for help in debugging)
349 m unclear name "iqtype"
  * I _really_ want to change this, but I need to keep it to match the Fortran source while I debug; the source has `iqtype`, `inqtype`, `oqtype`, and a few other similar variables and I need this reference to keep myself straight.
353 q what is a z-score?
  * Added a short explanation in the docstring stating it is "the amplitude normalized by the error from the model fit at the breakpoint
358 m blank line? (or is this part of our coding standard?)
  * PEP 8
366-373 m horrible horrible names.  Why?
  * This block is superfluous beyond debugging purposes. Retained for easy comparison to the Fortran source. When I implement logging (on the feature list), I'll re-do this with meaningful names and output.
373 m don't like model\_fcns name.
  * changed `model_order, model_fcns`->`model_order, _`
381-383 m why these two variables?
  * The output of the changepoint tests is formatted like table; this is the header row for that table. I re-did the comment above it to indicate this.
390 m blank line (see above)
  * PEP 8
396 m prefer {}
  * changed `dict()`->`{}`
375 m model\_fcns variables not used.
  * Fixed above.
400,401 m horrible names.
  * changed `cmodel`->`model_name`, `test_fcn`->`model_test`
401 m calling convention.  vals doesn't need to be returned
  * removed return of `vals` in all the model testing function
401 m calling convention: put missing\_val after bp\_index, for consistency with minbic()
  * Done. This has the side-effect of requiring `missing_vals` to be a non-optional field; it can't have a default value since I don't have a default `vals` object to use, and default arguments have to come after positional arguments in the Python method signature. Not a big deal, but this will have to be re-evaluated when I re-structure the code here.
405 m we might not have done any TPR tests.
(I have a much longer comment about that)
407-408 m "I'll come back to that" when? In later development or further down this current code?
  * I totally changed that comment to read "Identify the best-fitting model computed above by finding which one has the lowest BIC value"
415-416 m we assume we've run models which compute these? Could extend the Values class and then have a method which computes it if not already computed.
  * As it's written now the code sort of has to assume that it was computed during the model test. I'll add a note for my list of things to consider when I re-structure the code so that this isn't a huge assumption anymore.
415-416 m orphans: move down to 438ish.
  * done. Moved to 431 in latest version of code, right above `slopes = [kthl_left.slope, kthl_right.slope]`.
413 q what about draws? 412 means you don't have a stable sort.  Suggest
> > [(model, changepoint\_dict[model](model.md)['bic']) for (model,(underscore)) in models]
> > then since sorted() is stable you are at least deterministic
  * done
424-430 q why a dict rather than a Values?
  * I meant Values to only be used for the changepoint model tests. Another thing to add to the list of considerations when chgptmodels is re-written.
499 m extra space
  * PEP 8
497 m move down to after 499
  * Was this a typo? my line 497 is `cmodel`... not sure what is meant here.
507 m horrible names
  * Made the following changes: `b_tot`->`bic`, `b_err`->`bic_error`, `b_pen`->`bic_dof_penalty`
504 m remove? move to 511,523?
  * Added comment indicating `amp_est = 0.0` by assumption in the statistical test in this method
538 m s/student/Student/
  * per Wiki, "Student's t-test"
582 m The fact that bp\_index counts from 0 should be clarified in the doc strings.
  * changed all `bp_index`->`breakpoint` to match minbic(); Added docstring clarification that "Must be in range(len(x))"
594 M astonished if this is the right way around: stepping down is positive?!
  * It's deliberate. See USHCN\_v52d/src\_codes/lib/pha\_src/chgptmodels.v6b.f:504. Convention is... silly, but as long as the convention is maintained it shouldn't matter (and it doesn't in the end). The bigger issue is keeping the order of the series paired together straight; I force the invariant that int(id1)<int(id2). If I get this subtraction order backwards it will be very obvious in the final output, so should be easy to see and fix.
147-158 m suspect you can use a faster selection algorithm for this.
  * Yeah, there is a faster one in the Fortran code but it's difficult to implement. For good\_x and good\_y sorting isn't bad because n is ~10<sup>3. It's slopes that's the problem - n is ~10</sup>6 there. I'm using the median function I e-mailed the listserv about right now to improve this.
191-192 q log10? really? Surely natural logs, or base-2?! (you can
tell I don't know much about stats) It's just a scaling factor, but
still.
  * Yes, subroutine bayes in chgptmodels.v6b.f uses log10() and the whitepaper cites the formula BIC(p) = -2log(L)+log(n)p.
618 m so mu is medians?  I was expecting it to be means.
  * The original PHA doesn't stick to a single convention so I try to emulate the most-often used one here. With x the independent variable, mu is the coefficient of the factor x<sup>0 and alpha is the coefficient of x</sup>1. In the whitepaper, they use beta instead of alpha. This sucks and I'll add to my feature list for the re-write to use y-intercept and slope instead of the greek letters - only makes things confusing.
687-777 M this algorithm doesn't make any sense!  It doesn't fit a
constant slope, it fits a slope which would be constant if the x
values were equally spaced.
  * This is what the Fortran code specified (see chgptmodels.v6b.f:2199-2200). It's wrong, and Claude confirmed it. We want to take pairs of valid (x, y) data (that is, neither x nor y equals the missing value). I've logged this major defect and will retain it for debugging purposes; the final code will have this corrected.
703,708,719 m get rid of nslp, use len(slopes) or a direct computation.
  * done
707 m can only happen when j = i, so why not use range(i+1, ...) in the previous line.
  * This check is related to the bug that I've documented here. You're right - the sanity check isn't necessary (it's to avoid dividing by 0 in the computation of the slope) since we shouldn't ever match a pair with each other. But there is a bug in the code where Claude is computed far too many slopes. Notice that the bounds on the i, j in computing slopes in the first segment are different from those in the right segment. Claude confirmed with me that this is a major bug, and I've described the fix in the inline comments in the code. Retain for now just for debugging purposes.
699,700 m I don't believe these ranges are necessary.
710 and 721 m use j-i.
  * They're not - they're relics of the incorrect method for computing the valid predictor dataset. This is logged as another defect and I'll correct it when I remove major bugs that are relics of the Fortran source. Same with the j-i.
723-726 m delete.
  * done
733,734 m delete.
  * done
792-803 m horrid names
  * yeah. I thought the comment improved it. Thinking back, this entire method will need to be re-written; it can probably be streamlined and simplified heavily. The problem is there are a lot of major bugs in it and fixing any of them drastically changes my result against the Fortran code. I want to prioritize finishing the Python implementation of the algorithm, then I can go back and make the Fortran corrections and subsequent corrections here, which will let me totally re-write this method.
815 m use k <= bp\_index
  * done
819,829 m again, we don't need this array.
  * changed to not save the computed residuals
820,821 m use +=
  * changed
840 q aren't we still assuming this? I thought we're allowing a step but not a slope change.
  * yes.. we're only passing back one slope, which is the slope of the line fit to the entire segment. The y-intercept should change.
util.py 31 M either copy or don't!  This function doesn't take a copy,
so it's destructive of its 'data' argument, and should say so in its
docstring.
  * I changed the docstring to indicate this and specifically state that it produced a copy of data.
util.py 63 m there's no reason to use >> 1 instead of // 2.
  * I changed this; I was preserving the original code but obviously "// 2" accomplishes the floor division much more clearly than the bit shift does.
util.py 68 m ewww.  I'm sure there's a better way to do this.
general: it is not necessarily a bug to use a lower median or an upper
median in place of a true median, as long as you know what you're
doing.
  * This is true and the in pratice, taking an upper/lower/true median dpoesn't change the changepoint analysis in a significant way. In fact, the Fortran code father of kthtpr1() computes the upper median in all cases, so I changed the median method here to replicate that function. It'll mess up kth\_line() though, which computes a true median.
general M Dependencies of the form "model a must run before model b"
are fragile.  Get rid of them like this:
[a big bit of example code](snipped.md)
  * we'll talk about this in detail on Wednesday morning.


---


### David ###


21 m (new) import operator instead of import thing from operator.  Not only in general, but particular for this case.
  * I changed this in chgptmodels, splitmerge, and util.
733 m imed is assigned here, but never used. (def'd and not used), it's overwritten in 742.
  * Removed the imed reference; it's not needed because I rolled my own median() method instead of sorting the slopes in place and finding the middle value via index.
737 m "imet" typo.
  * Fixed typo
688 c (and throughout).  I kept confusing "kth" to mean item number k in a sequence.  despite the fact that there's a glossary item for KTHJ.
  * Added KTH to Glossary
  1. 82 M I feel this should be a class instance.
    * Nick raised this point too. We'll have to talk about it on Wednesday morning. Might be relegated to changing when I re-write and re-structure the code.
1097 m assert len(x) == len(y) and throughout.  (new: hmm, it's asserted in minbic.)
    * Yes, I assert it in minbic(). Nothing changes to x or y and they're not sliced between minbic() and when they're passed to test functions, so I don't think I need to sprinkle these assertions everywhere.
1054 c missing\_val not used.  recommend calling it _missing\_val with a trailing underscore.  (this should be in coding standard too)
    * Another flaw associated with the way I'm letting the model tests cascade. Added to consideration when I re-structure the code.
1116 c seems off to divide by 1.0 (I guess it's some count).
    * Nope, it's superfluous and a carry-over from the Fortran code. I got rid of it._


---


### Daniel ###

DANIEL

322-324 (m) they're actually described in docstrings for each method
  * Documentation has totally changed at this point; no longer refers to anything.
348/350 (m) "best fitting changepoint model"
  * done
351 (m) "order of 'models' passed to this function"
  * done
365-373 (c) add TPR, SLR to glossar
  * added
378 (c) This can raise AssertionError - should be documented with a :Raises: entry in the minbic() docstring
  * added
413 (c) Needs comment saying "Sort by computed BIC value, ascending order"
  * Changed comment to read "Identify the best-fitting model computed above by finding which one has the lowest BIC value. Conveys the same meaning behind the process occurring.
_(c) Superfluous; only used for debugging against Fortran output_
437 (c) How are we "double-checking" them? Comment isn't clear
  * Changed to just indicate that we're returning the details of the best-fitting model.
441 (c) rather than store tpr\_offset, can compute it from kthl\_left, kthl\_right
  * Changed to compute this, and elaborated on the comment above it explaining why we compute the offset (changepoint amplitude) in this fashion.
454 (c) All the following methods use the same signature. Is there any way to write an interface for them, like with Java classes? Then I could check during minbic() that the model test has the right inputs and outputs.
  * Gets into the details of how to re-structure the code. Shelved for Wednesday meeting.
457 (c) External link to a doc on Kendall-Theil method
  * Added in glossary.
477-479 (m) By definition of the test, the mu and alpha tuples should have the same value in each slot (the mu and alpha for the regression fit to all the data). Might as well document this explicitly
  * Changed the documentation. All future logs about eplicit documentation will be italicized because I am dealing with them directly.
_481 (m) By definition of the test, seg\_lens will be [len(all the data), 0]. Document epxlicitly.
483-484 (m) Both these values are defined as 0.0 because this model is the null hypothesis fit. Document explicitly.
492 (c) "Kendall-Theil method" rather than "kth"_
508-512 (c) Debug output here really should be refactored into its own method
  * Logging issue; logged.
_534 (c) change to "no slope change between segments"_
_556 (m) Explicitly state that mu is the median value found in each of the left, right data segments
557 (m) Explicitly state that alph is the slope of the regression fit to each of hte left, right data segments_
_608-613 (c) Debug output here really should be refactored into its own method_
640 (c) add to glossary - "breakpoint" = "changepoint". That or for consistency, make all breakpoint references into changepoint references
  * added to glossary.
_660 (c) Same as above; be more explicit in what values mu, alpha, and other results will have._
_(c) valid data := [for d in data if d != missing\_val](d.md)_
737 (c) Debug print statement could use comment indicating it as such
  * Logging
744-747 (c) Computing median in range\_left, valid\_left can be accomplished with my median() function
  * But it doesn't matter. ignore.
840 (m) comment is wrong - the different assumption in KTHSLR1 is that the **amplitude change** is 0. Both kthslr1 and kthtpr1 assume that there is no change in slope.
  * Changed