#ACM Europe Summer School 2017 - Data cleaning lab

##Process
All merging and filtering was performed following the provided guidelines. 
To the "open" guidelines requiring us to think more about it ourselves, the following operations were performed

- When clustering street names, all found clusters (after uppercasing and whitespace trimming) were merged except for one (AVE ST JOHNS / ST JOHNS AVE), since they all seemed either like clear typos or like alternative writings for streets in the same county. This merging grouped 941 cells.
- When clustering car color values, the decision was taken to keep as many simple values as possible, i.e. with the lowest proportion of "subjective" colors such as "dark/light/bright" adjectives. The groups are color codes with 5 characters (or less if the full color name is shorter than that).
Of course this solution is not perfect, since the color codes are sometimes very short and can  get very ambiguous; the most typical case here is "GRE", which can mean both "Grey" or "Green" or "BL" for "Blue" or "Black". These cases weren't resolved.

##Deliverable tasks

Some tasks were expected specifically for the deliverable:

- **Fix violation times**: for the *violation_time* field, times were filtered using the regexp `/((1[0-2]|0[1-9])([0-5][0-9])[A|P])/`, returning a non-empty list of patterns for every correct AM time. By providing this field's facet with a rule `value.match(/((1[0-2]|0[1-9])([0-5][0-9])[A|P])/) != null`; then, we can include only rows having a `true` value.
We notice that a lot of times have a (wrong) value of "0 AM" or "0 PM", which should not be possible. For the sake of the example, we therefore change the regexp to  `/((1[0-2]|0[0-9])([0-5][0-9])[A|P])/`. This yields seven wrong values. 

- **Fix registration states**: for the *registration_state* value, we also check in the same way, using the regexp `/^(?:(A[KLRZ]|C[AOT]|D[CE]|FL|GA|HI|I[ADLN]|K[SY]|LA|M[ADEINOST]|N[CDEHJMVY]|O[HKR]|P[AR]|RI|S[CD]|T[NX]|UT|V[AIT]|W[AIVY]))$/`, which gives us `value.match(/^(?:(A[KLRZ]|C[AOT]|D[CE]|FL|GA|HI|I[ADLN]|K[SY]|LA|M[ADEINOST]|N[CDEHJMVY]|O[HKR]|P[AR]|RI|S[CD]|T[NX]|UT|V[AIT]|W[AIVY]))$/) != null` to match on the field facet. This removed the special "99" and empty values, along with errors such as 'GV'.

- **Fix vehicle maker values**: for the *vehicle_make* value, values were already quite well clustered, but some clustering was made mostly manually. There are still some ambiguity, for example for "MERC" valid for both "Mercury" and "Mercedes".

- **Find other errors**: some other mistakes and suggestions were found, such as problem-inducing fields. Here is a list of such remarks:
    * The *vehicle_year* field has wrong values (e.g. > 2017) but at this point in our filtering, ni violations are found to be wrong on this level.
    * As mentioned earlier, same goes for the time values (with wrong AM/PM times)
    * In several fields, special values are used for missing information (usually "99" or "999")... while still letting blank values sometimes. This should be improved.

