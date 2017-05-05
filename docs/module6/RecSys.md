Recommender Systems
========================================================
css: ../../assets/style/uw.css
author: Justin Donaldson
date: May-04-2017
autosize: true

Applied Machine Learning 410
---------------------------------
(AKA: If you like that, your gonna love this)

Recommender Systems Arrive
======
Anybody remember this contest? (2009)
![netflix prixe](img/netflix-prize.png)

Recommender Systems Arrive
======
![netflix prixe](img/netflix-prize.png)
***
- One of the first "big money" prizes for data science
- Contestants from all over the world
- No holds barred, any technique was considered
- Winner gets a million bucks!

Old and Blockbusted
======
<a title="By Tracy the astonishing (The video store on Flickr) [CC BY-SA 2.0 (http://creativecommons.org/licenses/by-sa/2.0)], via Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File%3AVideo_shop.jpg"><img width="512" alt="Video shop" src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/bc/Video_shop.jpg/512px-Video_shop.jpg"/></a>
***
- Previously, each copy of a movie took up space... somewhere.
- Impossible to stock *every* movie that *everyone* wants in a physical store
- Movies/Content catered towards *satisficing* broader demographics.
  - Sacrifice smaller fringe titles, stock more crowd pleasers

Overview
===
type : sub-section
- Content-based recommendation
- User-based recommendation
- Item-based recommendation
- Hybrids and New Techniques


Content-based recommendation
====
What constitutes *content*? 

![moana](img/moana.jpg)
***
- Simple measures/categories (themes, genres, abstract qualities)
- E.g. Moana is a [family] [musical] featuring [cg animation]
- May be automatically extracted
- Relies on a *profile* of the individual content, and *profile* of user preference

IMDB part deux
====
type : small-code

```r
dat = read.csv("../module3/movie_metadata.csv")
str(dat)
```

```
'data.frame':	5043 obs. of  28 variables:
 $ color                    : chr  "Color" "Color" "Color" "Color" ...
 $ director_name            : chr  "James Cameron" "Gore Verbinski" "Sam Mendes" "Christopher Nolan" ...
 $ num_critic_for_reviews   : int  723 302 602 813 NA 462 392 324 635 375 ...
 $ duration                 : int  178 169 148 164 NA 132 156 100 141 153 ...
 $ director_facebook_likes  : int  0 563 0 22000 131 475 0 15 0 282 ...
 $ actor_3_facebook_likes   : int  855 1000 161 23000 NA 530 4000 284 19000 10000 ...
 $ actor_2_name             : chr  "Joel David Moore" "Orlando Bloom" "Rory Kinnear" "Christian Bale" ...
 $ actor_1_facebook_likes   : int  1000 40000 11000 27000 131 640 24000 799 26000 25000 ...
 $ gross                    : int  760505847 309404152 200074175 448130642 NA 73058679 336530303 200807262 458991599 301956980 ...
 $ genres                   : chr  "Action|Adventure|Fantasy|Sci-Fi" "Action|Adventure|Fantasy" "Action|Adventure|Thriller" "Action|Thriller" ...
 $ actor_1_name             : chr  "CCH Pounder" "Johnny Depp" "Christoph Waltz" "Tom Hardy" ...
 $ movie_title              : chr  "Avatar " "Pirates of the Caribbean: At World's End " "Spectre " "The Dark Knight Rises " ...
 $ num_voted_users          : int  886204 471220 275868 1144337 8 212204 383056 294810 462669 321795 ...
 $ cast_total_facebook_likes: int  4834 48350 11700 106759 143 1873 46055 2036 92000 58753 ...
 $ actor_3_name             : chr  "Wes Studi" "Jack Davenport" "Stephanie Sigman" "Joseph Gordon-Levitt" ...
 $ facenumber_in_poster     : int  0 0 1 0 0 1 0 1 4 3 ...
 $ plot_keywords            : chr  "avatar|future|marine|native|paraplegic" "goddess|marriage ceremony|marriage proposal|pirate|singapore" "bomb|espionage|sequel|spy|terrorist" "deception|imprisonment|lawlessness|police officer|terrorist plot" ...
 $ movie_imdb_link          : chr  "http://www.imdb.com/title/tt0499549/?ref_=fn_tt_tt_1" "http://www.imdb.com/title/tt0449088/?ref_=fn_tt_tt_1" "http://www.imdb.com/title/tt2379713/?ref_=fn_tt_tt_1" "http://www.imdb.com/title/tt1345836/?ref_=fn_tt_tt_1" ...
 $ num_user_for_reviews     : int  3054 1238 994 2701 NA 738 1902 387 1117 973 ...
 $ language                 : chr  "English" "English" "English" "English" ...
 $ country                  : chr  "USA" "USA" "UK" "USA" ...
 $ content_rating           : chr  "PG-13" "PG-13" "PG-13" "PG-13" ...
 $ budget                   : num  2.37e+08 3.00e+08 2.45e+08 2.50e+08 NA ...
 $ title_year               : int  2009 2007 2015 2012 NA 2012 2007 2010 2015 2009 ...
 $ actor_2_facebook_likes   : int  936 5000 393 23000 12 632 11000 553 21000 11000 ...
 $ imdb_score               : num  7.9 7.1 6.8 8.5 7.1 6.6 6.2 7.8 7.5 7.5 ...
 $ aspect_ratio             : num  1.78 2.35 2.35 2.35 NA 2.35 2.35 1.85 2.35 2.35 ...
 $ movie_facebook_likes     : int  33000 0 85000 164000 0 24000 0 29000 118000 10000 ...
```

IMDB part deux
====
type : small-code 

```r
library(stringr)
library(coop)
dat = dat[dat$plot_keywords != '', ]
dat = dat[1:1000,]
keywords = str_split(dat$plot_keywords, "\\s*\\|\\s*")
keywords = lapply(keywords, str_trim)
all_keywords = sort(unique(unlist(keywords)))
keywords = sapply(keywords, function(x) {
  y = rep(0, length(all_keywords)); 
  names(y)<- all_keywords; 
  y[unlist(x)] = 1; 
  y
})
colnames(keywords)<- str_trim(dat$movie_title)

t(keywords[40:43,1:5])
```

```
                                         acorn act of kindness
Avatar                                       0               0
Pirates of the Caribbean: At World's End     0               0
Spectre                                      0               0
The Dark Knight Rises                        0               0
John Carter                                  0               0
                                         action figure action hero
Avatar                                               0           0
Pirates of the Caribbean: At World's End             0           0
Spectre                                              0           0
The Dark Knight Rises                                0           0
John Carter                                          0           0
```

IMDB part deux
====

```r
sims = cosine(keywords)
diag(sims) = 0
as.matrix(apply(sims,2,function(x) names(x)[which.max(x)]))[1:100,]
```

```
                                                        Avatar 
                                        "Terminator Salvation" 
                      Pirates of the Caribbean: At World's End 
                 "Pirates of the Caribbean: On Stranger Tides" 
                                                       Spectre 
                                               "Casino Royale" 
                                         The Dark Knight Rises 
                                             "The Devil's Own" 
                                                   John Carter 
                                              "Men in Black 3" 
                                                  Spider-Man 3 
                                      "The Amazing Spider-Man" 
                                                       Tangled 
                                  "The Huntsman: Winter's War" 
                                       Avengers: Age of Ultron 
                                  "Captain America: Civil War" 
                        Harry Potter and the Half-Blood Prince 
                                                 "The Holiday" 
                            Batman v Superman: Dawn of Justice 
                                     "Avengers: Age of Ultron" 
                                              Superman Returns 
                                          "The Golden Compass" 
                                             Quantum of Solace 
                 "Pirates of the Caribbean: On Stranger Tides" 
                    Pirates of the Caribbean: Dead Man's Chest 
                                         "Godzilla Resurgence" 
                                               The Lone Ranger 
                             "Transformers: Age of Extinction" 
                                                  Man of Steel 
                                     "Avengers: Age of Ultron" 
                      The Chronicles of Narnia: Prince Caspian 
                                       "Jack the Giant Slayer" 
                                                  The Avengers 
                                                "The Avengers" 
                   Pirates of the Caribbean: On Stranger Tides 
                    "Pirates of the Caribbean: At World's End" 
                                                Men in Black 3 
                                             "Men in Black II" 
                     The Hobbit: The Battle of the Five Armies 
           "The Lord of the Rings: The Fellowship of the Ring" 
                                        The Amazing Spider-Man 
                                                  "Spider-Man" 
                                                    Robin Hood 
                                             "Children of Men" 
                           The Hobbit: The Desolation of Smaug 
                   "The Hobbit: The Battle of the Five Armies" 
                                            The Golden Compass 
                                        "Crazy, Stupid, Love." 
                                                     King Kong 
                                    "How to Train Your Dragon" 
                                                       Titanic 
                      "Harry Potter and the Half-Blood Prince" 
                                    Captain America: Civil War 
                                     "Avengers: Age of Ultron" 
                                                    Battleship 
                                                     "Titanic" 
                                                Jurassic World 
                                           "Jurassic Park III" 
                                                       Skyfall 
                                                      "Avatar" 
                                                  Spider-Man 2 
                                                   "Dragonfly" 
                                                    Iron Man 3 
                                                     "Spectre" 
                                           Alice in Wonderland 
                                 "Snow White and the Huntsman" 
                                         X-Men: The Last Stand 
                                           "X-Men: Apocalypse" 
                                           Monsters University 
                                         "Godzilla Resurgence" 
                           Transformers: Revenge of the Fallen 
                              "Transformers: Dark of the Moon" 
                               Transformers: Age of Extinction 
                                             "The Lone Ranger" 
                                     Oz the Great and Powerful 
                                                   "Bewitched" 
                                      The Amazing Spider-Man 2 
                                      "The Amazing Spider-Man" 
                                                  TRON: Legacy 
                                   "The Chronicles of Riddick" 
                                                        Cars 2 
                                                     "Spectre" 
                                                 Green Lantern 
                                                  "The Lovers" 
                                                   Toy Story 3 
                                                "TRON: Legacy" 
                                          Terminator Salvation 
                          "Terminator 3: Rise of the Machines" 
                                                     Furious 7 
                                                     "Spectre" 
                                                   World War Z 
                                                    "Carriers" 
                                    X-Men: Days of Future Past 
                                                "Tomorrowland" 
                                       Star Trek Into Darkness 
                 "Pirates of the Caribbean: On Stranger Tides" 
                                         Jack the Giant Slayer 
                         "Prince of Persia: The Sands of Time" 
                                              The Great Gatsby 
                                            "The Green Hornet" 
                           Prince of Persia: The Sands of Time 
                                       "Jack the Giant Slayer" 
                                                   Pacific Rim 
                       "Sky Captain and the World of Tomorrow" 
                                Transformers: Dark of the Moon 
                         "Transformers: Revenge of the Fallen" 
            Indiana Jones and the Kingdom of the Crystal Skull 
                                                      "Wanted" 
                                             The Good Dinosaur 
                                              "Jurassic World" 
                                                         Brave 
                                                 "John Carter" 
                                              Star Trek Beyond 
                                         "Godzilla Resurgence" 
                                                        WALL·E 
                                                 "Pacific Rim" 
                                                   Rush Hour 3 
                                                 "Ratatouille" 
                                                          2012 
                                               "Evan Almighty" 
                                             A Christmas Carol 
                                           "The Polar Express" 
                                             Jupiter Ascending 
                                              "Fantastic Four" 
                                          The Legend of Tarzan 
                                        "The Legend of Tarzan" 
The Chronicles of Narnia: The Lion, the Witch and the Wardrobe 
                                                      "Frozen" 
                                             X-Men: Apocalypse 
                                                       "X-Men" 
                                               The Dark Knight 
                                                 "Constantine" 
                                                            Up 
                                                    "Spy Game" 
                                           Monsters vs. Aliens 
                                                 "John Carter" 
                                                      Iron Man 
                                              "Wild Wild West" 
                                                          Hugo 
                                             "The Lone Ranger" 
                                                Wild Wild West 
                                                    "Iron Man" 
                         The Mummy: Tomb of the Dragon Emperor 
                   "The Hobbit: The Battle of the Five Armies" 
                                                 Suicide Squad 
                                     "Avengers: Age of Ultron" 
                                                 Evan Almighty 
                                                        "Noah" 
                                              Edge of Tomorrow 
                                                 "John Carter" 
                                                    Waterworld 
                                                      "Avatar" 
                                   G.I. Joe: The Rise of Cobra 
                                             "The Lone Ranger" 
                                                    Inside Out 
                                        "The Bourne Ultimatum" 
                                               The Jungle Book 
                                                "Two Brothers" 
                                                    Iron Man 2 
                                           "Quantum of Solace" 
                                   Snow White and the Huntsman 
                                         "Alice in Wonderland" 
                                                    Maleficent 
                    "The Chronicles of Narnia: Prince Caspian" 
                                Dawn of the Planet of the Apes 
                                         "Godzilla Resurgence" 
                                                    The Lovers 
                                               "Green Lantern" 
                                                      47 Ronin 
                                                  "Battleship" 
                           Captain America: The Winter Soldier 
                                                      "Cars 2" 
                                           Shrek Forever After 
                                                       "Brave" 
                                                  Tomorrowland 
                                       "Mr. Peabody & Sherman" 
                                                    Big Hero 6 
                                     "Avengers: Age of Ultron" 
                                                Wreck-It Ralph 
                    "The Chronicles of Narnia: Prince Caspian" 
                                             The Polar Express 
                                           "A Christmas Carol" 
                                  Independence Day: Resurgence 
                                                 "John Carter" 
                                      How to Train Your Dragon 
                                  "How to Train Your Dragon 2" 
                            Terminator 3: Rise of the Machines 
                                        "Terminator Salvation" 
                                       Guardians of the Galaxy 
                                                  "Armageddon" 
                                                  Interstellar 
                                               "Suicide Squad" 
                                                     Inception 
                                                "Total Recall" 
                                           Godzilla Resurgence 
                                         "Godzilla Resurgence" 
                             The Hobbit: An Unexpected Journey 
                   "The Hobbit: The Battle of the Five Armies" 
                                      The Fast and the Furious 
                                    "The Fast and the Furious" 
```

#```{r child="CollabFilter.Rpres"}

# ```


