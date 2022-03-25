INFO 3010 - Assignment 5
================

## by Lingzi Hong

### Instructions

1.  This is an R Markdown format used for publishing markdown documents
    to GitHub. When you click the **Knit** button, all R code chunks are
    run and a markdown file (.md) suitable for publishing to GitHub is
    generated.
2.  Please download the snowstorm.json from canvas. Fill in the code
    chunks for following question and submit this R markdown file to the
    assignment on Canvas. Make sure when you save that you have run all
    cells, so the outputs displace between the cells.
3.  make sure to replace the directoryID in the filename with your
    student ID.

### Q1. (5pts) Load and parse the “snowstorm_sample2.json” file to a dataframe.

``` r
library('rjson')
library('jsonlite')
```

    ## 
    ## Attaching package: 'jsonlite'

    ## The following objects are masked from 'package:rjson':
    ## 
    ##     fromJSON, toJSON

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.6     v dplyr   1.0.7
    ## v tidyr   1.2.0     v stringr 1.4.0
    ## v readr   2.1.2     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter()      masks stats::filter()
    ## x purrr::flatten()     masks jsonlite::flatten()
    ## x jsonlite::fromJSON() masks rjson::fromJSON()
    ## x dplyr::lag()         masks stats::lag()
    ## x jsonlite::toJSON()   masks rjson::toJSON()

``` r
library(streamR)
```

    ## Loading required package: RCurl

    ## 
    ## Attaching package: 'RCurl'

    ## The following object is masked from 'package:tidyr':
    ## 
    ##     complete

    ## Loading required package: ndjson

    ## 
    ## Attaching package: 'ndjson'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     flatten

    ## The following objects are masked from 'package:jsonlite':
    ## 
    ##     flatten, stream_in, validate

``` r
library (tm)
```

    ## Loading required package: NLP

    ## 
    ## Attaching package: 'NLP'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     annotate

``` r
tweet_df <- parseTweets(tweets='snowstorm_sample2.json')
```

    ## 1058 tweets have been parsed.

### Q2. (5pts) Take the text of tweet, compute the corpus and check the first 10 lines of the corpus.

``` r
textdf <- tweet_df$text
myCorpus <- Corpus(VectorSource(textdf)) #computing corpus
inspect(myCorpus[1:10])
```

    ## <<SimpleCorpus>>
    ## Metadata:  corpus specific: 1, document level (indexed): 0
    ## Content:  documents: 10
    ## 
    ##  [1] Whew. My liver.                                                                                                                       
    ##  [2] â\200œ@kdubbbbb_: @AJasmineFlower talkin shit ðŸ\230­â\200\235 ðŸ\230­ðŸ\230­ðŸ\230­ real bad ðŸ\230©                                                         
    ##  [3] but i still hate drew storen                                                                                                          
    ##  [4] Where the #Redskins fans at tonight?                                                                                                  
    ##  [5] I feel like oomf is always on twitter and doesn't have a life                                                                         
    ##  [6] 1. #TheWalkingDeadMarathon\n2. #scottysireisadisease\n3. Avery Bradley\n4. #TheOriginalsBack\n5. #5SOSGOODGIRLS\n\n2014/10/6 18:53 CDT
    ##  [7] I need some new fresh people to talk to                                                                                               
    ##  [8] 6. Hollis Thompson\n7. #RAWBrooklyn\n8. Storen\n9. Welcome to DC\n10. What TF\n\n2014/10/6 18:53 CDT #trndnl http://t.co/lcaeh6tWOl   
    ##  [9] Nats already did it #Redskinstime.                                                                                                    
    ## [10] Happy birthday Renren @Umpa_Lumpia http://t.co/0AbYzf8wrQ

### Q3. (5pts) Use a stop words list to remove the stop words from corpus, then check the first 10 lines of the corpus.

``` r
myStopwords <- stopwords('english')
myCorpus <- tm_map(myCorpus,removeWords,myStopwords)
```

    ## Warning in tm_map.SimpleCorpus(myCorpus, removeWords, myStopwords):
    ## transformation drops documents

``` r
inspect(myCorpus[1:10])
```

    ## <<SimpleCorpus>>
    ## Metadata:  corpus specific: 1, document level (indexed): 0
    ## Content:  documents: 10
    ## 
    ##  [1] Whew. My liver.                                                                                                                       
    ##  [2] â\200œ@kdubbbbb_: @AJasmineFlower talkin shit ðŸ\230­â\200\235 ðŸ\230­ðŸ\230­ðŸ\230­ real bad ðŸ\230©                                                         
    ##  [3]   still hate drew storen                                                                                                              
    ##  [4] Where  #Redskins fans  tonight?                                                                                                       
    ##  [5] I feel like oomf  always  twitter     life                                                                                            
    ##  [6] 1. #TheWalkingDeadMarathon\n2. #scottysireisadisease\n3. Avery Bradley\n4. #TheOriginalsBack\n5. #5SOSGOODGIRLS\n\n2014/10/6 18:53 CDT
    ##  [7] I need  new fresh people  talk                                                                                                        
    ##  [8] 6. Hollis Thompson\n7. #RAWBrooklyn\n8. Storen\n9. Welcome  DC\n10. What TF\n\n2014/10/6 18:53 CDT #trndnl http://t.co/lcaeh6tWOl     
    ##  [9] Nats already   #Redskinstime.                                                                                                         
    ## [10] Happy birthday Renren @Umpa_Lumpia http://t.co/0AbYzf8wrQ

### Q4. (5pts) Remove punctuations in the corpus, then check the first 10 lines of the corpus.

``` r
myCorpus <- tm_map(myCorpus,removePunctuation)
```

    ## Warning in tm_map.SimpleCorpus(myCorpus, removePunctuation): transformation
    ## drops documents

``` r
inspect(myCorpus[1:10]) #first 10 lines
```

    ## <<SimpleCorpus>>
    ## Metadata:  corpus specific: 1, document level (indexed): 0
    ## Content:  documents: 10
    ## 
    ##  [1] Whew My liver                                                                                                             
    ##  [2] â\200œkdubbbbb AJasmineFlower talkin shit ðŸ\230­â\200\235 ðŸ\230­ðŸ\230­ðŸ\230­ real bad ðŸ\230©                                                 
    ##  [3]   still hate drew storen                                                                                                  
    ##  [4] Where  Redskins fans  tonight                                                                                             
    ##  [5] I feel like oomf  always  twitter     life                                                                                
    ##  [6] 1 TheWalkingDeadMarathon\n2 scottysireisadisease\n3 Avery Bradley\n4 TheOriginalsBack\n5 5SOSGOODGIRLS\n\n2014106 1853 CDT
    ##  [7] I need  new fresh people  talk                                                                                            
    ##  [8] 6 Hollis Thompson\n7 RAWBrooklyn\n8 Storen\n9 Welcome  DC\n10 What TF\n\n2014106 1853 CDT trndnl httptcolcaeh6tWOl        
    ##  [9] Nats already   Redskinstime                                                                                               
    ## [10] Happy birthday Renren UmpaLumpia httptco0AbYzf8wrQ

### Q5. (5pts) Convert all characters to lower case, then check the first 10 lines of the corpus.

``` r
myCorpus <- tm_map(myCorpus,tolower) #all characters to lowercase
```

    ## Warning in tm_map.SimpleCorpus(myCorpus, tolower): transformation drops
    ## documents

``` r
inspect(myCorpus[1:10]) #first 10 lines
```

    ## <<SimpleCorpus>>
    ## Metadata:  corpus specific: 1, document level (indexed): 0
    ## Content:  documents: 10
    ## 
    ##  [1] whew my liver                                                                                                             
    ##  [2] â\200œkdubbbbb ajasmineflower talkin shit ðÿ\230­â\200\235 ðÿ\230­ðÿ\230­ðÿ\230­ real bad ðÿ\230©                                                 
    ##  [3]   still hate drew storen                                                                                                  
    ##  [4] where  redskins fans  tonight                                                                                             
    ##  [5] i feel like oomf  always  twitter     life                                                                                
    ##  [6] 1 thewalkingdeadmarathon\n2 scottysireisadisease\n3 avery bradley\n4 theoriginalsback\n5 5sosgoodgirls\n\n2014106 1853 cdt
    ##  [7] i need  new fresh people  talk                                                                                            
    ##  [8] 6 hollis thompson\n7 rawbrooklyn\n8 storen\n9 welcome  dc\n10 what tf\n\n2014106 1853 cdt trndnl httptcolcaeh6twol        
    ##  [9] nats already   redskinstime                                                                                               
    ## [10] happy birthday renren umpalumpia httptco0abyzf8wrq

### Q6. (5pts) Generate the document term matrix and check the 10th to 20th rows of the matrix.

``` r
myDtm <- TermDocumentMatrix(myCorpus,control=list(minWordLength =1))
inspect(myDtm[266:270,10:20])
```

    ## <<TermDocumentMatrix (terms: 5, documents: 11)>>
    ## Non-/sparse entries: 0/55
    ## Sparsity           : 100%
    ## Maximal term length: 6
    ## Weighting          : term frequency (tf)
    ## Sample             :
    ##         Docs
    ## Terms    10 11 12 13 14 15 16 17 18 19 20
    ##   bus     0  0  0  0  0  0  0  0  0  0  0
    ##   driver  0  0  0  0  0  0  0  0  0  0  0
    ##   kinda   0  0  0  0  0  0  0  0  0  0  0
    ##   moving  0  0  0  0  0  0  0  0  0  0  0
    ##   swift   0  0  0  0  0  0  0  0  0  0  0

### Q7. (10pts) Get the total number of frequency for each word.

``` r
findMostFreqTerms(myDtm)
```

    ## $`1`
    ## liver  whew 
    ##     1     1 
    ## 
    ## $`2`
    ## ajasmineflower            bad           real           shit         talkin 
    ##              1              1              1              1              1 
    ##      “kdubbbbb 
    ##              1 
    ## 
    ## $`3`
    ##   drew   hate  still storen 
    ##      1      1      1      1 
    ## 
    ## $`4`
    ##     fans redskins  tonight    where 
    ##        1        1        1        1 
    ## 
    ## $`5`
    ##  always    feel    life    like    oomf twitter 
    ##       1       1       1       1       1       1 
    ## 
    ## $`6`
    ##          1853       2014106 5sosgoodgirls         avery       bradley 
    ##             1             1             1             1             1 
    ##           cdt 
    ##             1 
    ## 
    ## $`7`
    ##  fresh   need    new people   talk 
    ##      1      1      1      1      1 
    ## 
    ## $`8`
    ##              1853           2014106               cdt            hollis 
    ##                 1                 1                 1                 1 
    ## httptcolcaeh6twol       rawbrooklyn 
    ##                 1                 1 
    ## 
    ## $`9`
    ##      already         nats redskinstime 
    ##            1            1            1 
    ## 
    ## $`10`
    ##          birthday             happy httptco0abyzf8wrq            renren 
    ##                 1                 1                 1                 1 
    ##        umpalumpia 
    ##                 1 
    ## 
    ## $`11`
    ##                          copesaywhaaa                                  hate 
    ##                                     1                                     1 
    ## <f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+0082><U+270B> 
    ##                                     1 
    ## 
    ## $`12`
    ##      believe         butt         httr skinsnation”         will          win 
    ##            1            1            1            1            1            1 
    ## 
    ## $`13`
    ##    confidence        little selfconscious          wish 
    ##             1             1             1             1 
    ## 
    ## $`14`
    ##  important    ratchet     sports television 
    ##          1          1          1          1 
    ## 
    ## $`15`
    ## aarondmiller2         cause         claim        crises        crisis 
    ##             1             1             1             1             1 
    ##         great 
    ##             1 
    ## 
    ## $`16`
    ##  back bring gotta  nats  shit   win 
    ##     1     1     1     1     1     1 
    ## 
    ## $`17`
    ## bishopulmer       hello 
    ##           1           1 
    ## 
    ## $`18`
    ## going throw 
    ##     1     1 
    ## 
    ## $`19`
    ##              feel httptcok4zbgg5isp      ibackthenats              like 
    ##                 1                 1                 1                 1 
    ##            making         nationals 
    ##                 1                 1 
    ## 
    ## $`20`
    ##        could         girl     thickest      twitter virgoperidot        white 
    ##            1            1            1            1            1            1 
    ## 
    ## $`21`
    ##     gotcha aviroberts      going     hasn’t       hope       know 
    ##          2          1          1          1          1          1 
    ## 
    ## $`22`
    ##             awesome httpstco5nupojypss”     “badlipreadings 
    ##                   1                   1                   1 
    ## 
    ## $`23`
    ##                                       good 
    ##                                          1 
    ##                                 joeyowey77 
    ##                                          1 
    ##                                       luck 
    ##                                          1 
    ## <f0><ff><U+0092><U+0099><U+26F3><U+FE0F><f0><ff><U+0098><U+009A> 
    ##                                          1 
    ## 
    ## $`24`
    ##                                 everyone 
    ##                                        1 
    ##                                     joke 
    ##                                        1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                        1 
    ## 
    ## $`25`
    ##          avi bbraxton1297        sweet 
    ##            1            1            1 
    ## 
    ## $`26`
    ##     anarchy      boston       drive        ever   karenfink melanierizk 
    ##           1           1           1           1           1           1 
    ## 
    ## $`27`
    ## nationals       now   survive 
    ##         1         1         1 
    ## 
    ## $`28`
    ##   life social  sucks 
    ##      1      1      1 
    ## 
    ## $`29`
    ##      come redpandas     tammy 
    ##         1         1         1 
    ## 
    ## $`30`
    ## httptcoa5akum9su0”           ignoring               like               mans 
    ##                  1                  1                  1                  1 
    ##               stop                why 
    ##                  1                  1 
    ## 
    ## $`31`
    ##      chief     earned gtgtgtgtgt       keef 
    ##          1          1          1          1 
    ## 
    ## $`32`
    ##               asking                dming                  lol 
    ##                    1                    1                    1 
    ##               random                 stop <f0><ff><U+0098><U+0082> 
    ##                    1                    1                    1 
    ## 
    ## $`33`
    ##   alive beltway    nats     one orioles  series 
    ##       1       1       1       1       1       1 
    ## 
    ## $`34`
    ##    goddamubernice httptcotgr0rsxhea         pondering 
    ##                 1                 1                 1 
    ## 
    ## $`35`
    ##     bed earlier heading   tired tonight    wait 
    ##       1       1       1       1       1       1 
    ## 
    ## $`36`
    ## feel like  why 
    ##    1    1    1 
    ## 
    ## $`37`
    ##  even girls  guys happy  know  love 
    ##     1     1     1     1     1     1 
    ## 
    ## $`38`
    ## woop down game httr 
    ##    2    1    1    1 
    ## 
    ## $`39`
    ## anything     give      ill     want 
    ##        1        1        1        1 
    ## 
    ## $`40`
    ##             amp brianjameswalsh            even           happy            hope 
    ##               1               1               1               1               1 
    ##            nats 
    ##               1 
    ## 
    ## $`41`
    ##            okay           sleep <f0><ff><U+0098><U+00B4> 
    ##               1               1               1 
    ## 
    ## $`42`
    ##   lol   sad thats   tru truth 
    ##     1     1     1     1     1 
    ## 
    ## $`43`
    ##         fuck       haters         httr kirkcousins8          man          the 
    ##            1            1            1            1            1            1 
    ## 
    ## $`44`
    ##          big       clutch dougfister58         good          man         spot 
    ##            1            1            1            1            1            1 
    ## 
    ## $`45`
    ## amandapleaasee     apologizes           nope 
    ##              1              1              1 
    ## 
    ## $`46`
    ##       hip      just       now queenbrie 
    ##         1         1         1         1 
    ## 
    ## $`47`
    ##     1st  683205  became  hollis mention  people 
    ##       1       1       1       1       1       1 
    ## 
    ## $`48`
    ##  coming   didnt earlier    even    ever    hear 
    ##       1       1       1       1       1       1 
    ## 
    ## $`49`
    ##     jai devoirs   finir  flemme     mal     mes 
    ##       2       1       1       1       1       1 
    ## 
    ## $`50`
    ##    169    213    222 hollis   made    rts 
    ##      1      1      1      1      1      1 
    ## 
    ## $`51`
    ## build   can  drew  hope  lets  nats 
    ##     1     1     1     1     1     1 
    ## 
    ## $`52`
    ##        accounts             amp georgetownhoyas          helped          hollis 
    ##               2               1               1               1               1 
    ##   ryenarussillo 
    ##               1 
    ## 
    ## $`53`
    ##           finally httptconhg9enkkkz               i95           resting 
    ##                 1                 1                 1                 1 
    ##   shoulderarmhand          watching 
    ##                 1                 1 
    ## 
    ## $`54`
    ##                  bus               driver                kinda 
    ##                    1                    1                    1 
    ##               moving                swift <f0><ff><U+0098><U+0082> 
    ##                    1                    1                    1 
    ## 
    ## $`55`
    ##            hollis httptcoc8vtqhkk7m            impact         published 
    ##                 1                 1                 1                 1 
    ##               rts            sixers 
    ##                 1                 1 
    ## 
    ## $`56`
    ##  twitter  android   client   hollis   iphone thompson 
    ##        3        1        1        1        1        1 
    ## 
    ## $`57`
    ##    eyes    make sparkle     you 
    ##       1       1       1       1 
    ## 
    ## $`58`
    ##         crowd      barclays           can       chicago disappointing 
    ##             2             1             1             1             1 
    ##          last 
    ##             1 
    ## 
    ## $`59`
    ##    dies   every    like minutes   phone     why 
    ##       1       1       1       1       1       1 
    ## 
    ## $`60`
    ##                                                   goals 
    ##                                                       1 
    ##                                                 someone 
    ##                                                       1 
    ##                                                    want 
    ##                                                       1 
    ## <f0><ff><U+0091><U+00AB><f0><ff><U+0092><U+008D><f0><ff><U+0092><U+0098> 
    ##                                                       1 
    ## 
    ## $`61`
    ## around  comes    day  every   feel   like 
    ##      1      1      1      1      1      1 
    ## 
    ## $`62`
    ## early sleep tired  want 
    ##     1     1     1     1 
    ## 
    ## $`63`
    ## lahhhollywood        mother       omarion 
    ##             1             1             1 
    ## 
    ## $`64`
    ##                                                    dumb 
    ##                                                       1 
    ##                                                     now 
    ##                                                       1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+00B4><f0><ff><U+0092><U+0085> 
    ##                                                       1 
    ## 
    ## $`65`
    ## nats 
    ##    1 
    ## 
    ## $`66`
    ##    chris      fag     hate   mcghan subtweet 
    ##        1        1        1        1        1 
    ## 
    ## $`67`
    ##             canope            cmonman httpstco2zlhai99e1                sur 
    ##                  1                  1                  1                  1 
    ##         washington 
    ##                  1 
    ## 
    ## $`68`
    ##          amazon          hunter iheartmastodons             irs            pays 
    ##               1               1               1               1               1 
    ##           pence 
    ##               1 
    ## 
    ## $`69`
    ##  actually     after    better finishing       mix  mxmeraki 
    ##         1         1         1         1         1         1 
    ## 
    ## $`70`
    ##          business        consultant              corp              cort 
    ##                 1                 1                 1                 1 
    ## httptcowlpfsyfwdu               job 
    ##                 1                 1 
    ## 
    ## $`71`
    ##               hiphop                 love <f0><ff><U+0098><U+009C> 
    ##                    1                    1                    1 
    ## 
    ## $`72`
    ##         will          lot mrdcsportssr         need     official         this 
    ##            2            1            1            1            1            1 
    ## 
    ## $`73`
    ##            beat            best           fight           still            that 
    ##               1               1               1               1               1 
    ## tonyperkinsfox5 
    ##               1 
    ## 
    ## $`74`
    ##      now seahawks 
    ##        1        1 
    ## 
    ## $`75`
    ##  chrisgolden danmericacnn      traitor 
    ##            1            1            1 
    ## 
    ## $`76`
    ##   back dollla   igns   look    see 
    ##      1      1      1      1      1 
    ## 
    ## $`77`
    ##     crib lilgerb9 
    ##        1        1 
    ## 
    ## $`78`
    ##    brother       club        dad explaining        now    penguin 
    ##          1          1          1          1          1          1 
    ## 
    ## $`79`
    ## one 
    ##   1 
    ## 
    ## $`80`
    ##    bacardi       call       doug     fister       good mlbnetwork 
    ##          1          1          1          1          1          1 
    ## 
    ## $`81`
    ##       watch allaboutjas       cause        dead       first       gonna 
    ##           2           1           1           1           1           1 
    ## 
    ## $`82`
    ##        1st    appears       fs14 janaboruta    mention        now 
    ##          1          1          1          1          1          1 
    ## 
    ## $`83`
    ##  show   the   tho voice 
    ##     1     1     1     1 
    ## 
    ## $`84`
    ##  dats jordo  neck nigga  said   yur 
    ##     1     1     1     1     1     1 
    ## 
    ## $`85`
    ##           accurate             bodies           entirely              girls 
    ##                  1                  1                  1                  1 
    ## httptcotyi50o6bc3”            judging 
    ##                  1                  1 
    ## 
    ## $`86`
    ##      around       drive     happily   karenfink melanierizk         nyc 
    ##           1           1           1           1           1           1 
    ## 
    ## $`87`
    ## daysiaaa 
    ##        1 
    ## 
    ## $`88`
    ## ashlaayraeee          got         shot 
    ##            1            1            1 
    ## 
    ## $`89`
    ## ibelieve     lets     nats 
    ##        1        1        1 
    ## 
    ## $`90`
    ## kschorback       stop 
    ##          1          1 
    ## 
    ## $`91`
    ## much shit  tbh 
    ##    1    1    1 
    ## 
    ## $`92`
    ##    confidence           gio jameswagnerwp           lot      natitude 
    ##             1             1             1             1             1 
    ##          nats 
    ##             1 
    ## 
    ## $`93`
    ##       itaintover             nlds thatsmynationals 
    ##                1                1                1 
    ## 
    ## $`94`
    ## 2morrow   alive   dcsaa    game  giants     gio 
    ##       1       1       1       1       1       1 
    ## 
    ## $`95`
    ## biggestbabyshower         safety1st              tyvm 
    ##                 1                 1                 1 
    ## 
    ## $`96`
    ## important     sleep      word   yaelpnt 
    ##         1         1         1         1 
    ## 
    ## $`97`
    ##      nats       win nationals 
    ##         3         3         1 
    ## 
    ## $`98`
    ##  completely   different frustrating        kind         mnf         now 
    ##           1           1           1           1           1           1 
    ## 
    ## $`99`
    ## guwopfatzz  halaheeem      never 
    ##          1          1          1 
    ## 
    ## $`100`
    ## espn yeah 
    ##    1    1 
    ## 
    ## $`101`
    ## actually   doctor    happy     said 
    ##        1        1        1        1 
    ## 
    ## $`102`
    ## bitches    like     act     get  niggas treated 
    ##       2       2       1       1       1       1 
    ## 
    ## $`103`
    ##      hate literally       pic  pictures   praying   trappin 
    ##         1         1         1         1         1         1 
    ## 
    ## $`104`
    ##           hensons httptcocsshhdxejk              part             still 
    ##                 1                 1                 1                 1 
    ## 
    ## $`105`
    ##                            bigbangtheory 
    ##                                        1 
    ## <f0><ff><U+0098><U+0083><f0><ff><U+0091><U+008D> 
    ##                                        1 
    ## 
    ## $`106`
    ## curtisatkinson           dead            man 
    ##              1              1              1 
    ## 
    ## $`107`
    ## blackbuckethats            know            nice           thats 
    ##               1               1               1               1 
    ## 
    ## $`108`
    ## appreciate        bro       cool        ima   kamekami     really 
    ##          1          1          1          1          1          1 
    ## 
    ## $`109`
    ## gothamtime       time       what 
    ##          1          1          1 
    ## 
    ## $`110`
    ##   quarantining         africa allinwithchris         answer           best 
    ##              2              1              1              1              1 
    ##    chrislhayes 
    ##              1 
    ## 
    ## $`111`
    ## eunyangnbc    naughty       need        now       oops   redskins 
    ##          1          1          1          1          1          1 
    ## 
    ## $`112`
    ## nats 
    ##    4 
    ## 
    ## $`113`
    ## lance210      mcm 
    ##        1        1 
    ## 
    ## $`114`
    ##        differently               game             here’s httptcoaqqvs7obgd” 
    ##                  1                  1                  1                  1 
    ##               sees                she 
    ##                  1                  1 
    ## 
    ## $`115`
    ##      alire     begins   benjamin       club       ends everything 
    ##          1          1          1          1          1          1 
    ## 
    ## $`116`
    ##  calvert      got     hall mountain toughest     unis 
    ##        1        1        1        1        1        1 
    ## 
    ## $`117`
    ##                         are                       comes 
    ##                           1                           1 
    ##                         one tonight<f0><ff><U+0098><U+008D> 
    ##                           1                           1 
    ## 
    ## $`118`
    ##         awwiebabyy            dressed           edmyakua idkwhoelsewasthere 
    ##                  1                  1                  1                  1 
    ##               lmao           maryroni 
    ##                  1                  1 
    ## 
    ## $`119`
    ## patrickmj  sheepeeh       yes 
    ##         1         1         1 
    ## 
    ## $`120`
    ## say 
    ##   1 
    ## 
    ## $`121`
    ## yaaaaassss      finna      honey      lahhh       turn 
    ##          2          1          1          1          1 
    ## 
    ## $`122`
    ##               follow                  hip                  hop 
    ##                    1                    1                    1 
    ##              jinx202                 real <f0><ff><U+0091><U+0088> 
    ##                    1                    1                    1 
    ## 
    ## $`123`
    ## httptco8sh6zecjc4           yusssss 
    ##                 1                 1 
    ## 
    ## $`124`
    ##       bruhh hoopaholick 
    ##           1           1 
    ## 
    ## $`125`
    ##       ask     basis     daily      dogg heybswizz     snoop 
    ##         1         1         1         1         1         1 
    ## 
    ## $`126`
    ## anything     hell migraine     take 
    ##        1        1        1        1 
    ## 
    ## $`127`
    ## couldve     lrt orange…   sworn 
    ##       1       1       1       1 
    ## 
    ## $`128`
    ##      bbl homework     time 
    ##        1        1        1 
    ## 
    ## $`129`
    ##          always             but             can katherinerhoad9        mentions 
    ##               1               1               1               1               1 
    ##          public 
    ##               1 
    ## 
    ## $`130`
    ##   all girls  ugly 
    ##     1     1     1 
    ## 
    ## $`131`
    ##  get  raw time 
    ##    1    1    1 
    ## 
    ## $`132`
    ##            analysis              better              dilfer httpstcoh81qliseis” 
    ##                   1                   1                   1                   1 
    ##           timboj126               trent 
    ##                   1                   1 
    ## 
    ## $`133`
    ## httptcojpuy2kvut9 <f0><ff><U+0094><U+00A5> 
    ##                 1                 1 
    ## 
    ## $`134`
    ## back chop game love  one play 
    ##    1    1    1    1    1    1 
    ## 
    ## $`135`
    ## boyfriend<f0><ff><U+0092><U+008D><f0><ff><U+0092><U+0091> 
    ##                                                 1 
    ##                                              miss 
    ##                                                 1 
    ## 
    ## $`136`
    ##     found francisco nationals  natitude       san       the 
    ##         1         1         1         1         1         1 
    ## 
    ## $`137`
    ##        just allaboutjas      answer         but       crazy         hip 
    ##           2           1           1           1           1           1 
    ## 
    ## $`138`
    ##    monday       raw       wwe highlight      last     night 
    ##         2         2         2         1         1         1 
    ## 
    ## $`139`
    ##      ago baseball  college   column enjoying    equal 
    ##        1        1        1        1        1        1 
    ## 
    ## $`140`
    ##                                                      gotham” 
    ##                                                            1 
    ##                                                   “thexfiles 
    ##                                                            1 
    ## <f0><ff><U+0099><U+009C><f0><ff><U+0099><U+009C><f0><ff><U+0099><U+009C> 
    ##                                                            1 
    ## 
    ## $`141`
    ## aisharobinson           ate        chynaf      hera7155      visiting 
    ##             1             1             1             1             1 
    ## 
    ## $`142`
    ##    now   room tumblr 
    ##      1      1      1 
    ## 
    ## $`143`
    ##   about   check wizards 
    ##       1       1       1 
    ## 
    ## $`144`
    ##       always       connor connorfranta         damn         dont    following 
    ##            1            1            1            1            1            1 
    ## 
    ## $`145`
    ##        article   baltimoresun          horse            how       industry 
    ##              1              1              1              1              1 
    ## infrastructure 
    ##              1 
    ## 
    ## $`146`
    ## amandaradan04         holla          just       kidding          rara 
    ##             1             1             1             1             1 
    ##         tryna 
    ##             1 
    ## 
    ## $`147`
    ##   days   fs14   made    rts states  topic 
    ##      1      1      1      1      1      1 
    ## 
    ## $`148`
    ##        great        alive         baby         boys dougfister58         game 
    ##            2            1            1            1            1            1 
    ## 
    ## $`149`
    ##    twitter    android       fs14     iphone i<ee><ff>s   top3apps 
    ##          2          1          1          1          1          1 
    ## 
    ## $`150`
    ##                                queenbrie 
    ##                                        1 
    ## <f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092> 
    ##                                        1 
    ## 
    ## $`151`
    ##     1st   38644  became    fs14 mention  people 
    ##       1       1       1       1       1       1 
    ## 
    ## $`152`
    ##               amp            bolans              crab          foodporn 
    ##                 1                 1                 1                 1 
    ##             gouda httptcobg9drysowo 
    ##                 1                 1 
    ## 
    ## $`153`
    ##                nigga                talib                 this 
    ##                    1                    1                    1 
    ##                 wild <f0><ff><U+0098><U+0082> 
    ##                    1                    1 
    ## 
    ## $`154`
    ##     gush pharrell 
    ##        1        1 
    ## 
    ## $`155`
    ##  him miss 
    ##    1    1 
    ## 
    ## $`156`
    ##       books csnredskins         eat        game       great        httr 
    ##           1           1           1           1           1           1 
    ## 
    ## $`157`
    ##         love dougfister58   drewstoren         hell    nationals 
    ##            2            1            1            1            1 
    ## 
    ## $`158`
    ##        advice          take wackyjacky125          will 
    ##             1             1             1             1 
    ## 
    ## $`159`
    ##  gun just  son 
    ##    1    1    1 
    ## 
    ## $`160`
    ##      much       big      care      hate     heart sometimes 
    ##         2         1         1         1         1         1 
    ## 
    ## $`161`
    ##    ive beanie missed  never  since  today 
    ##      2      1      1      1      1      1 
    ## 
    ## $`162`
    ##    baby   eezus excited    love  school  sister 
    ##       1       1       1       1       1       1 
    ## 
    ## $`163`
    ##                                    scandalabc 
    ##                                             1 
    ## <f0><ff><U+0097><U+00BB><f0><ff><U+0091><U+00B0><f0><ff><U+0093><U+00B9> 
    ##                                             1 
    ## 
    ## $`164`
    ##       100layercake              board      clarabeara731 httptcoblyoqosngu” 
    ##                  1                  1                  1                  1 
    ##              ideas                our 
    ##                  1                  1 
    ## 
    ## $`165`
    ##            bodies             could             found              four 
    ##                 1                 1                 1                 1 
    ## httptcocyboyvv23h         roadworks 
    ##                 1                 1 
    ## 
    ## $`166`
    ##  creepy decides    door     not    open    room 
    ##       1       1       1       1       1       1 
    ## 
    ## $`167`
    ##      attack       bring cardiaccats         fan       heart        home 
    ##           1           1           1           1           1           1 
    ## 
    ## $`168`
    ##  httptcoi1px8ewivw                plm       plmefficient principallifestyle 
    ##                  1                  1                  1                  1 
    ## 
    ## $`169`
    ##     1st appears hiwalls mention     now  states 
    ##       1       1       1       1       1       1 
    ## 
    ## $`170`
    ##       aint       hard       idgt    operate      sleep yoimblaine 
    ##          1          1          1          1          1          1 
    ## 
    ## $`171`
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0091><U+008F> 
    ##                                        1 
    ## 
    ## $`172`
    ## httptcouf3gftujqw        installing          mathfail            update 
    ##                 1                 1                 1                 1 
    ##           windows 
    ##                 1 
    ## 
    ## $`173`
    ##       almost electrocuted          got         just          omg        phone 
    ##            1            1            1            1            1            1 
    ## 
    ## $`174`
    ##         bunch        couple    fimustauri           had           jim 
    ##             1             1             1             1             1 
    ## joecienkowski 
    ##             1 
    ## 
    ## $`175`
    ## birdman<f0><ff><U+0098><U+00AD>                geeeked                 tooooo 
    ##                      1                      1                      1 
    ## 
    ## $`176`
    ##            awesome             hudson       ibackthenats idonotbackthebirds 
    ##                  1                  1                  1                  1 
    ##               legs               lots 
    ##                  1                  1 
    ## 
    ## $`177`
    ##               37m             bryan              face               fld 
    ##                 1                 1                 1                 1 
    ## httptcoxnvmnj9oms          maryland 
    ##                 1                 1 
    ## 
    ## $`178`
    ##       and    gonats      hope    mynats nationals  natitude 
    ##         1         1         1         1         1         1 
    ## 
    ## $`179`
    ##         beheading              fans            giants httptcogzzdsaz2kj 
    ##                 1                 1                 1                 1 
    ##              loss          mourning 
    ##                 1                 1 
    ## 
    ## $`180`
    ##      guitar        much        need nikkiwhitee         now    practice 
    ##           1           1           1           1           1           1 
    ## 
    ## $`181`
    ## nbcthevoice        time 
    ##           1           1 
    ## 
    ## $`182`
    ##             blunt              hits httptcogxmfk3h67w           longest 
    ##                 1                 1                 1                 1 
    ##               one          syllable 
    ##                 1                 1 
    ## 
    ## $`183`
    ##   gonats    least natitude     nlds    sweep  wasvssf 
    ##        1        1        1        1        1        1 
    ## 
    ## $`184`
    ##         bet      except kimsherayko        nats       start   zimmerman 
    ##           1           1           1           1           1           1 
    ## 
    ## $`185`
    ## her<f0><ff><U+0098><U+008B><f0><ff><U+0098><U+008B><f0><ff><U+0098><U+0098> 
    ##                                                               1 
    ##                                                          kissin 
    ##                                                               1 
    ##                                                            like 
    ##                                                               1 
    ## 
    ## $`186`
    ##                                     difficult 
    ##                                             1 
    ##                             httptcotljp1jakvv 
    ##                                             1 
    ##                                stuathproblems 
    ##                                             1 
    ##                                          test 
    ##                                             1 
    ##                                           the 
    ##                                             1 
    ## truth<f0><ff><U+0098><U+0092><f0><ff><U+0091><U+008F> 
    ##                                             1 
    ## 
    ## $`187`
    ##  ibuprofen megster143   shoutout      today 
    ##          1          1          1          1 
    ## 
    ## $`188`
    ##     the     why   women answers because     get 
    ##       2       2       2       1       1       1 
    ## 
    ## $`189`
    ## chance   hear   rock    see    the   town 
    ##      1      1      1      1      1      1 
    ## 
    ## $`190`
    ##       bigcitymoms biggestbabyshower               day             party 
    ##                 1                 1                 1                 1 
    ##         safety1st           waiting 
    ##                 1                 1 
    ## 
    ## $`191`
    ##        fedexfield httptcozzcbernatr              just             photo 
    ##                 1                 1                 1                 1 
    ##            posted 
    ##                 1 
    ## 
    ## $`192`
    ##               erkyya                 niya <f0><ff><U+0098><U+0085> 
    ##                    1                    1                    1 
    ## 
    ## $`193`
    ## enasstyyyy       good        one 
    ##          1          1          1 
    ## 
    ## $`194`
    ## funny   hip   hop  love   now right 
    ##     1     1     1     1     1     1 
    ## 
    ## $`195`
    ##                                                        smart 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                            1 
    ## 
    ## $`196`
    ##             final              game              horn httptco2h9c2cnyg3 
    ##                 1                 1                 1                 1 
    ##         nationals              nats 
    ##                 1                 1 
    ## 
    ## $`197`
    ##    comcast       game        net  preseason     sports washington 
    ##          1          1          1          1          1          1 
    ## 
    ## $`198`
    ##  baby   can daddy  fizz 
    ##     1     1     1     1 
    ## 
    ## $`199`
    ##               boy httptcolarx9lplxd          krybae86   thefaygowarrior 
    ##                 1                 1                 1                 1 
    ## 
    ## $`200`
    ## excited  mayday    next  parade  seeing    week 
    ##       1       1       1       1       1       1 
    ## 
    ## $`201`
    ##       aint      bring  certified everywhere      gotta  resumeeee 
    ##          1          1          1          1          1          1 
    ## 
    ## $`202`
    ##   hit shake  some steak 
    ##     1     1     1     1 
    ## 
    ## $`203`
    ## brat even love  mom shes 
    ##    1    1    1    1    1 
    ## 
    ## $`204`
    ## penguin 
    ##       1 
    ## 
    ## $`205`
    ##  shooting       4th  addition afternoon       amp   another 
    ##         2         1         1         1         1         1 
    ## 
    ## $`206`
    ##      fucking         just         like       movies myyownworldd        perry 
    ##            1            1            1            1            1            1 
    ## 
    ## $`207`
    ## gonats<U+26BE><U+FE0F>                wahoo <f0><ff><U+0099><U+009C> 
    ##                    1                    1                    1 
    ## 
    ## $`208`
    ##                                   lurkin 
    ##                                        1 
    ##                                    nigga 
    ##                                        1 
    ##                                     this 
    ##                                        1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                        1 
    ## 
    ## $`209`
    ##                                                                   gossipgirliee” 
    ##                                                                                1 
    ##                                                                             hate 
    ##                                                                                1 
    ##                                                                              wow 
    ##                                                                                1 
    ##                                                                          yarinya 
    ##                                                                                1 
    ##                                                                              you 
    ##                                                                                1 
    ## <f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092> 
    ##                                                                                1 
    ## 
    ## $`210`
    ## bullsnation     playing     tonight     wizards         yea 
    ##           1           1           1           1           1 
    ## 
    ## $`211`
    ##      annaleebelle httptcoizuz8pqrxj        radiantinc 
    ##                 1                 1                 1 
    ## 
    ## $`212`
    ##         back mindymoretti     natitude       theres 
    ##            1            1            1            1 
    ## 
    ## $`213`
    ## lhhhollywood    whatevahh 
    ##            1            1 
    ## 
    ## $`214`
    ## bruh  its long 
    ##    1    1    1 
    ## 
    ## $`215`
    ##     ass    beat    need     omg tiearra 
    ##       1       1       1       1       1 
    ## 
    ## $`216`
    ##            gonats httptcoo4bzml0i58 
    ##                 1                 1 
    ## 
    ## $`217`
    ##  about shower   take 
    ##      1      1      1 
    ## 
    ## $`218`
    ##   audreymcclellan       bigcitymoms biggestbabyshower         following 
    ##                 1                 1                 1                 1 
    ##         safety1st 
    ##                 1 
    ## 
    ## $`219`
    ##    lmao serious t1v1018   think 
    ##       2       1       1       1 
    ## 
    ## $`220`
    ##          twitter          android           client           iphone 
    ##                3                1                1                1 
    ## theoriginalsback         top3apps 
    ##                1                1 
    ## 
    ## $`221`
    ## 1951598     1st  became mention  people    seen 
    ##       1       1       1       1       1       1 
    ## 
    ## $`222`
    ##     154     260     323    made minutes     rts 
    ##       1       1       1       1       1       1 
    ## 
    ## $`223`
    ##   fans giants  going   home    why 
    ##      1      1      1      1      1 
    ## 
    ## $`224`
    ##      amp   hiphop     love     time <U+23F0><U+23F0><U+23F0><U+23F0> 
    ##        1        1        1        1        1 
    ## 
    ## $`225`
    ##           hiwalls httptcoxwcsvi679i            impact         published 
    ##                 1                 1                 1                 1 
    ##               rts               the 
    ##                 1                 1 
    ## 
    ## $`226`
    ##    depends       haha litterally       ones      range    riuuuuh 
    ##          1          1          1          1          1          1 
    ## 
    ## $`227`
    ## believe    damn fucking     god regress   still 
    ##       1       1       1       1       1       1 
    ## 
    ## $`228`
    ##         amp    anything       back”         can        know remembering 
    ##           1           1           1           1           1           1 
    ## 
    ## $`229`
    ##         day desperately       every   literally     retweet   something 
    ##           1           1           1           1           1           1 
    ## 
    ## $`230`
    ##                      days         httptcoqrrgwrc7pw                lightskins 
    ##                         1                         1                         1 
    ## pants<f0><ff><U+0098><U+0082> 
    ##                         1 
    ## 
    ## $`231`
    ## birdman<f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD> 
    ##                                                    1 
    ##                                               geeked 
    ##                                                    1 
    ##                                                hazel 
    ##                                                    1 
    ##                                                toooo 
    ##                                                    1 
    ## 
    ## $`232`
    ##      call   dancing    ending happening       not     phone 
    ##         1         1         1         1         1         1 
    ## 
    ## $`233`
    ## awesome     job    nats 
    ##       1       1       1 
    ## 
    ## $`234`
    ## gossipgirliee          pele therealuchman             ” 
    ##             1             1             1             1 
    ## 
    ## $`235`
    ##          back bigbangtheory           its         later          time 
    ##             1             1             1             1             1 
    ## 
    ## $`236`
    ##               alert   httptcolcaeh6twol   httptcozlnjc6nmze                more 
    ##                   1                   1                   1                   1 
    ## theoriginalsseason2               trend 
    ##                   1                   1 
    ## 
    ## $`237`
    ##            love dangerusswilson        football            gift           great 
    ##               3               1               1               1               1 
    ##       interview 
    ##               1 
    ## 
    ## $`238`
    ##  food  nice  said share  sort sumos 
    ##     1     1     1     1     1     1 
    ## 
    ## $`239`
    ##    date someone   wanna     why 
    ##       1       1       1       1 
    ## 
    ## $`240`
    ## drought     lor real<U+2049><U+FE0F>    this     too 
    ##       1       1       1       1       1 
    ## 
    ## $`241`
    ##             alert httptcoatsw6rguiw httptcolcaeh6twol              more 
    ##                 1                 1                 1                 1 
    ##    thankyouaustin             trend 
    ##                 1                 1 
    ## 
    ## $`242`
    ##        get     whered youcourtup 
    ##          1          1          1 
    ## 
    ## $`243`
    ##        job       balt  baltimore       full     health healthcare 
    ##          2          1          1          1          1          1 
    ## 
    ## $`244`
    ##   ate first  just salad  time years 
    ##     1     1     1     1     1     1 
    ## 
    ## $`245`
    ##             alert httptco4ibqcx9wrn httptcolcaeh6twol              more 
    ##                 1                 1                 1                 1 
    ##     qlovingronnie             trend 
    ##                 1                 1 
    ## 
    ## $`246`
    ##  gawwwwwd squaremir     swear 
    ##         1         1         1 
    ## 
    ## $`247`
    ##             alert httptco3zoi3ndjsm httptcolcaeh6twol              more 
    ##                 1                 1                 1                 1 
    ##             trend            trends 
    ##                 1                 1 
    ## 
    ## $`248`
    ##             alert httptcoeysjtp0dft httptcolcaeh6twol              more 
    ##                 1                 1                 1                 1 
    ##             trend            trends 
    ##                 1                 1 
    ## 
    ## $`249`
    ## account    back    damn     fix   gotta    knew 
    ##       1       1       1       1       1       1 
    ## 
    ## $`250`
    ##                                      another 
    ##                                            1 
    ##                                        bring 
    ##                                            1 
    ##                                         cake 
    ##                                            1 
    ##                                       delise 
    ##                                            1 
    ##                                        piece 
    ##                                            1 
    ## tomoorrow<f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+008B> 
    ##                                            1 
    ## 
    ## $`251`
    ##         greenbelt      businessmgmt         companies         detective 
    ##                 2                 1                 1                 1 
    ## httptcoliscp16mku               job 
    ##                 1                 1 
    ## 
    ## $`252`
    ##   alcatel corporate     event    making  metropcs     money 
    ##         1         1         1         1         1         1 
    ## 
    ## $`253`
    ##            head…           medusa             want <f0><ff><U+0091><U+00BF><U+271A> 
    ##                1                1                1                1 
    ## 
    ## $`254`
    ##     care   jersey     nats tomorrow  wearing 
    ##        1        1        1        1        1 
    ## 
    ## $`255`
    ## friends     get   gotta    just   might     pay 
    ##       1       1       1       1       1       1 
    ## 
    ## $`256`
    ##  officialaztec perfectlymixed            reg         tellem 
    ##              1              1              1              1 
    ## 
    ## $`257`
    ## redskins seahawks 
    ##        1        1 
    ## 
    ## $`258`
    ##                      fizz yummy<f0><ff><U+0091><U+0085> 
    ##                         1                         1 
    ## 
    ## $`259`
    ##                                 god                              little 
    ##                                   1                                   1 
    ##                                poor                          savmillerr 
    ##                                   1                                   1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+00A4> 
    ##                                   1 
    ## 
    ## $`260`
    ## day<f0><ff><U+0098><U+00AD><U+263A><U+FE0F>                lily        lilygracekay                made 
    ##                   1                   1                   1                   1 
    ## 
    ## $`261`
    ##                                                                        boooooooy 
    ##                                                                                1 
    ##                                                                             just 
    ##                                                                                1 
    ##                                                                             lost 
    ##                                                                                1 
    ##                                                                            words 
    ##                                                                                1 
    ##                                 <f0><ff><U+0098><U+0084><f0><ff><U+008F><U+0088> 
    ##                                                                                1 
    ## <f0><ff><U+0098><U+008F><f0><ff><U+0098><U+0098><f0><ff><U+0098><U+0089><f0><ff><U+0098><U+008D> 
    ##                                                                                1 
    ## 
    ## $`262`
    ##          football httptcoxyukpxovuo            monday             night 
    ##                 1                 1                 1                 1 
    ##           outchea 
    ##                 1 
    ## 
    ## $`263`
    ##   ebjunkies         guy interesting        lose       makes      storen 
    ##           1           1           1           1           1           1 
    ## 
    ## $`264`
    ## single 
    ##      1 
    ## 
    ## $`265`
    ##  gotham    home penguin 
    ##       1       1       1 
    ## 
    ## $`266`
    ##      coach   football johnubacon     making     nearly      outta 
    ##          1          1          1          1          1          1 
    ## 
    ## $`267`
    ##   drugs nothing  really    talk    want    weed 
    ##       1       1       1       1       1       1 
    ## 
    ## $`268`
    ##          al3x6069 httptcoa14anjp5cu         jimbobruh              left 
    ##                 1                 1                 1                 1 
    ##               lol            niggas 
    ##                 1                 1 
    ## 
    ## $`269`
    ##    love   don’t reasons   right 
    ##       2       1       1       1 
    ## 
    ## $`270`
    ##             alive     beltwayseries              hope httptcosorsyaaiex 
    ##                 1                 1                 1                 1 
    ##          natitude              nats 
    ##                 1                 1 
    ## 
    ## $`271`
    ##                   erkyya know<f0><ff><U+0098><U+009C> 
    ##                        1                        1 
    ## 
    ## $`272`
    ##                                                                                                                                and 
    ##                                                                                                                                  1 
    ##                                                                                                                             hiphop 
    ##                                                                                                                                  1 
    ##                                                                                                                          hollywood 
    ##                                                                                                                                  1 
    ##                                                                                                                               love 
    ##                                                                                                                                  1 
    ## <f0><ff><U+0086><U+0097><f0><ff><U+0091><U+00AF><f0><ff><U+0091><U+00AF><f0><ff><U+0091><U+00AF><f0><ff><U+0091><U+00AC><f0><ff><U+0091><U+00AC><f0><ff><U+0091><U+00AC><f0><ff><U+0098><U+0082> 
    ##                                                                                                                                  1 
    ## 
    ## $`273`
    ##                                                  hungry 
    ##                                                       1 
    ## <f0><ff><U+0098><U+00B6><f0><ff><U+0098><U+0094><f0><ff><U+0091><U+009E> 
    ##                                                       1 
    ## 
    ## $`274`
    ##       game    happens       lets  nationals       next postseason 
    ##          1          1          1          1          1          1 
    ## 
    ## $`275`
    ##      amp broccoli   cheesy  chicken   dinner     rice 
    ##        1        1        1        1        1        1 
    ## 
    ## $`276`
    ##            company                ftw mondanightfootball           redskins 
    ##                  1                  1                  1                  1 
    ##           seahawks            sitting 
    ##                  1                  1 
    ## 
    ## $`277`
    ##        2004 bharper3407       bryce         hey   kmillar15        lets 
    ##           1           1           1           1           1           1 
    ## 
    ## $`278`
    ## early   gon   its night 
    ##     1     1     1     1 
    ## 
    ## $`279`
    ##        day       home       httr skinsbring 
    ##          1          1          1          1 
    ## 
    ## $`280`
    ## break  fall 
    ##     1     1 
    ## 
    ## $`281`
    ##    comes  finally kamekami      lol      mad  patient 
    ##        1        1        1        1        1        1 
    ## 
    ## $`282`
    ##   breakinankles12               got httptcosi8hhbqz4x              like 
    ##                 1                 1                 1                 1 
    ##         statement 
    ##                 1 
    ## 
    ## $`283`
    ##    2006     80s    drug footage    good     its 
    ##       1       1       1       1       1       1 
    ## 
    ## $`284`
    ## brights   gotta    know   light   thing 
    ##       1       1       1       1       1 
    ## 
    ## $`285`
    ##                    beyond                      brit                      done 
    ##                         1                         1                         1 
    ##                       lit paper<f0><ff><U+0098><U+0091>                  research 
    ##                         1                         1                         1 
    ## 
    ## $`286`
    ##         2817 eddieroyalwr         love      seattle        skins 
    ##            1            1            1            1            1 
    ## 
    ## $`287`
    ## httptcovhbessmm2n 
    ##                 1 
    ## 
    ## $`288`
    ## ballyessir       fizz    omarion     playin 
    ##          1          1          1          1 
    ## 
    ## $`289`
    ##                                     everybody 
    ##                                             1 
    ##                                           got 
    ##                                             1 
    ##                                           now 
    ##                                             1 
    ## <f0><ff><U+0098><U+00B7><f0><ff><U+0098><U+00B7><f0><ff><U+0098><U+00B7> 
    ##                                             1 
    ## 
    ## $`290`
    ##           hazel   lahhhollywood         sneaker             tho          wedges 
    ##               1               1               1               1               1 
    ## <f0><ff><U+0098><U+00B3> 
    ##               1 
    ## 
    ## $`291`
    ##                                                                                                                                                                                                            omarion 
    ##                                                                                                                                                                                                                  1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+0092><f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+0098><f0><ff><U+0092><U+0095><f0><ff><U+0092><U+008B><f0><ff><U+0099><U+0088> 
    ##                                                                                                                                                                                                                  1 
    ## 
    ## $`292`
    ##                     got httpstcocxdrf1o3eu”bruh                question 
    ##                       1                       1                       1 
    ##                     yoo           “bforbrittany          “lgotaquestion 
    ##                       1                       1                       1 
    ## 
    ## $`293`
    ##            lets        seahawks <f0><ff><U+0098><U+00AD> 
    ##               1               1               1 
    ## 
    ## $`294`
    ##               gym httptcotwrgpxuqmy 
    ##                 1                 1 
    ## 
    ## $`295`
    ##           bae     charissat foxsportslive 
    ##             1             1             1 
    ## 
    ## $`296`
    ##                                           its 
    ##                                             1 
    ##                                        rachet 
    ##                                             1 
    ##                                          time 
    ##                                             1 
    ## <f0><ff><U+0098><U+00B9><f0><ff><U+0098><U+00B9><f0><ff><U+0098><U+00B9> 
    ##                                             1 
    ## 
    ## $`297`
    ##               xnoomzx <f0><ff><U+0098><U+0082><U+2764><U+FE0F> 
    ##                     1                     1 
    ## 
    ## $`298`
    ##            actual              copy           finally             first 
    ##                 1                 1                 1                 1 
    ##               got httptco5pvmwuccc7 
    ##                 1                 1 
    ## 
    ## $`299`
    ##              atleast              friends                 good 
    ##                    1                    1                    1 
    ##                 know                  lol <f0><ff><U+0098><U+0082> 
    ##                    1                    1                    1 
    ## 
    ## $`300`
    ## baby nats  win yeah 
    ##    1    1    1    1 
    ## 
    ## $`301`
    ##    amp   hair  nails shower <U+2714> 
    ##      1      1      1      1      1 
    ## 
    ## $`302`
    ##                                                        ratchet 
    ##                                                              1 
    ## tv<f0><ff><U+0098><U+0081><f0><ff><U+0098><U+0081><f0><ff><U+0098><U+0081> 
    ##                                                              1 
    ## 
    ## $`303`
    ## awwiebabyy        bar    costume   edmyakua       ever     hookah 
    ##          1          1          1          1          1          1 
    ## 
    ## $`304`
    ## bulls  gear   got   ive  like looks 
    ##     1     1     1     1     1     1 
    ## 
    ## $`305`
    ##  bitch  chose  house  leave listen    now 
    ##      1      1      1      1      1      1 
    ## 
    ## $`306`
    ## broadcast     bulls   getting       why 
    ##         1         1         1         1 
    ## 
    ## $`307`
    ## running  shower   water 
    ##       1       1       1 
    ## 
    ## $`308`
    ##          600         been    followers          get         like melodictouch 
    ##            1            1            1            1            1            1 
    ## 
    ## $`309`
    ##                                      1st 
    ##                                        1 
    ##                               conference 
    ##                                        1 
    ##                                     pgfh 
    ##                                        1 
    ## <f0><ff><U+0098><U+009C><f0><ff><U+0091><U+008F> 
    ##                                        1 
    ## 
    ## $`310`
    ##      better      brewed        cool         day gallonsized        iced 
    ##           1           1           1           1           1           1 
    ## 
    ## $`311`
    ##      ale  awesome    brown      nut organics     peak 
    ##        1        1        1        1        1        1 
    ## 
    ## $`312`
    ## don’t  lose money ohhoe  sure wanna 
    ##     1     1     1     1     1     1 
    ## 
    ## $`313`
    ##       avec        ben        les        lt3 manondvstr     manque 
    ##          1          1          1          1          1          1 
    ## 
    ## $`314`
    ## fuckkk    not  ready 
    ##      1      1      1 
    ## 
    ## $`315`
    ##         damn     freez610  mutheadsite tradepostmut         xbox 
    ##            1            1            1            1            1 
    ## 
    ## $`316`
    ##    apes blowing    miss physics 
    ##       1       1       1       1 
    ## 
    ## $`317`
    ##              chip              good httptcohfbak92rcc               ill 
    ##                 1                 1                 1                 1 
    ##          shoulder               the 
    ##                 1                 1 
    ## 
    ## $`318`
    ##      games        now        row wills4lwen        win 
    ##          1          1          1          1          1 
    ## 
    ## $`319`
    ## change 
    ##      1 
    ## 
    ## $`320`
    ##   blue    ivy   like   look lowkey 
    ##      1      1      1      1      1 
    ## 
    ## $`321`
    ##         god       jessi jessirose14        read       thank 
    ##           1           1           1           1           1 
    ## 
    ## $`322`
    ##    boo   love monica    nah   sike 
    ##      1      1      1      1      1 
    ## 
    ## $`323`
    ##                    httptcoafzevjhbwg <f0><ff><U+0092><U+008B><U+271C><U+FE0F><f0><ff><U+0092><U+00AF> 
    ##                                    1                                    1 
    ## 
    ## $`324`
    ##    httptcodcdh51glhd                 like              trvllll 
    ##                    1                    1                    1 
    ## <f0><ff><U+0091><U+0080> 
    ##                    1 
    ## 
    ## $`325`
    ##    alright      apply    believe everything       feel        ill 
    ##          1          1          1          1          1          1 
    ## 
    ## $`326`
    ##       fuckkkkkkkk              hard httptcoxifkgfjyjr             today 
    ##                 1                 1                 1                 1 
    ## 
    ## $`327`
    ##       bigcitymoms biggestbabyshower              good              luck 
    ##                 1                 1                 1                 1 
    ##         safety1st 
    ##                 1 
    ## 
    ## $`328`
    ##              alex          birthday          earrings httptcol3vkzso0d4 
    ##                 1                 1                 1                 1 
    ##       monogrammed               new 
    ##                 1                 1 
    ## 
    ## $`329`
    ## gone 
    ##    1 
    ## 
    ## $`330`
    ##      worst      being borderline     cancer      curse     justin 
    ##          2          1          1          1          1          1 
    ## 
    ## $`331`
    ## lhhhollywood 
    ##            1 
    ## 
    ## $`332`
    ##            gang            mess thatmanmansaray 
    ##               1               1               1 
    ## 
    ## $`333`
    ##     ago    days dumbass     got    like     lol 
    ##       1       1       1       1       1       1 
    ## 
    ## $`334`
    ##  attitude    brunch   matches modernday perfectly      psls 
    ##         1         1         1         1         1         1 
    ## 
    ## $`335`
    ##    cant cuffing    find     get    hand     kno 
    ##       1       1       1       1       1       1 
    ## 
    ## $`336`
    ##   nice really   sean taylor 
    ##      1      1      1      1 
    ## 
    ## $`337`
    ##                                          feedup 
    ##                                               1 
    ##                                           jimmy 
    ##                                               1 
    ##                                          niggas 
    ##                                               1 
    ##                                          posted 
    ##                                               1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082>waiting 
    ##                                               1 
    ## 
    ## $`338`
    ##           laugh           night <f0><ff><U+0098><U+00B9> 
    ##               1               1               1 
    ## 
    ## $`339`
    ## lhhhollywood         must          one       rating       really         show 
    ##            1            1            1            1            1            1 
    ## 
    ## $`340`
    ## anyone   else    for   game   hype    the 
    ##      1      1      1      1      1      1 
    ## 
    ## $`341`
    ##      good     gotta      keep nationals      nats       win 
    ##         1         1         1         1         1         1 
    ## 
    ## $`342`
    ##             can            last            like littlenaijagirl            mean 
    ##               1               1               1               1               1 
    ##    nigerianinme 
    ##               1 
    ## 
    ## $`343`
    ##       another         bryce brycegoesboom           day game4herewego 
    ##             1             1             1             1             1 
    ##         great 
    ##             1 
    ## 
    ## $`344`
    ##                  day                  lol                  one 
    ##                    1                    1                    1 
    ## <f0><ff><U+0098><U+008D> 
    ##                    1 
    ## 
    ## $`345`
    ##     alive      just      nats sloanesw7    stayin       won 
    ##         1         1         1         1         1         1 
    ## 
    ## $`346`
    ##       angry        come        here         raw rawbrooklyn        ring 
    ##           1           1           1           1           1           1 
    ## 
    ## $`347`
    ##              beah            folger              gone httptcoxchdhlckpy 
    ##                 1                 1                 1                 1 
    ##           ishmeal           library 
    ##                 1                 1 
    ## 
    ## $`348`
    ##  chic  fila   now right <U+2764><U+FE0F> 
    ##     1     1     1     1     1 
    ## 
    ## $`349`
    ## espnnfl     nah 
    ##       1       1 
    ## 
    ## $`350`
    ##   going    need     see weekend   whats 
    ##       1       1       1       1       1 
    ## 
    ## $`351`
    ## begining      new 
    ##        1        1 
    ## 
    ## $`352`
    ## angry  seth 
    ##     1     1 
    ## 
    ## $`353`
    ##           assisted             chrisp          firstever          headstand 
    ##                  1                  1                  1                  1 
    ## httpstcoevbnhbe7m3         makeitwork 
    ##                  1                  1 
    ## 
    ## $`354`
    ##                                                                     aint 
    ##                                                                        1 
    ##                                                                    money 
    ##                                                                        1 
    ##                                                                  talking 
    ##                                                                        1 
    ##                                                                    topic 
    ##                                                                        1 
    ## <f0><ff><U+0098><U+008F><f0><ff><U+0092><U+00B0><f0><ff><U+0092><U+00B0><U+270B><f0><ff><U+0091><U+009C> 
    ##                                                                        1 
    ## 
    ## $`355`
    ##               boy           hottest httptcov4uisapliy             white 
    ##                 1                 1                 1                 1 
    ## 
    ## $`356`
    ##            faggot httptcot5r76b2p9k              look              pink 
    ##                 1                 1                 1                 1 
    ##               smh           wearing 
    ##                 1                 1 
    ## 
    ## $`357`
    ##                tired <f0><ff><U+0098><U+0082> 
    ##                    1                    1 
    ## 
    ## $`358`
    ##                                                           got 
    ##                                                             2 
    ##                                                        snacks 
    ##                                                             1 
    ##                                    us<f0><ff><U+0098><U+0088> 
    ##                                                             1 
    ##                                                 “xlovingdessy 
    ##                                                             1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D>” 
    ##                                                             1 
    ## 
    ## $`359`
    ## 2stubbornlymine 
    ##               1 
    ## 
    ## $`360`
    ##                     cracked                        fizz 
    ##                           1                           1 
    ##                      gawwwd smiiiiiiiiiiiiiiiiiiilecoot 
    ##                           1                           1 
    ##                        when 
    ##                           1 
    ## 
    ## $`361`
    ##    late minutes  monday morning     was 
    ##       1       1       1       1       1 
    ## 
    ## $`362`
    ##     called eliminated       glad       know       must       nats 
    ##          1          1          1          1          1          1 
    ## 
    ## $`363`
    ##                                                         fine 
    ##                                                            1 
    ##                                                         fizz 
    ##                                                            1 
    ## <f0><ff><U+0098><U+00AB><f0><ff><U+0098><U+00AB><f0><ff><U+0098><U+00AB><f0><ff><U+0098><U+00AB> 
    ##                                                            1 
    ## 
    ## $`364`
    ##   ago   big cents  fell   gas   now 
    ##     1     1     1     1     1     1 
    ## 
    ## $`365`
    ##  floating hilarious  lmaooooo 
    ##         1         1         1 
    ## 
    ## $`366`
    ##   anyone facetime     want 
    ##        1        1        1 
    ## 
    ## $`367`
    ##  make shine wanna 
    ##     1     1     1 
    ## 
    ## $`368`
    ##    got  moose niggas saying    why  years 
    ##      1      1      1      1      1      1 
    ## 
    ## $`369`
    ##                                                             bout 
    ##                                                                1 
    ##                                                             lets 
    ##                                                                1 
    ##                                                            skins 
    ##                                                                1 
    ## work<f0><ff><U+008F><U+0088><f0><ff><U+008F><U+0088><f0><ff><U+008F><U+0088> 
    ##                                                                1 
    ## 
    ## $`370`
    ##     hmm    many   movie tonight   watch 
    ##       1       1       1       1       1 
    ## 
    ## $`371`
    ##     baby natitude 
    ##        1        1 
    ## 
    ## $`372`
    ##         can        drew lukerussert     nothing      proves      sample 
    ##           1           1           1           1           1           1 
    ## 
    ## $`373`
    ##           redskins              fedex              field httpstcove4etgmqm6 
    ##                  2                  1                  1                  1 
    ##           landover                mnf 
    ##                  1                  1 
    ## 
    ## $`374`
    ##                                                         fizz 
    ##                                                            1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D> 
    ##                                                            1 
    ## 
    ## $`375`
    ## now<e2><U+00BF>    sucks      who 
    ##        1        1        1 
    ## 
    ## $`376`
    ## httptcow0lkhrt03g          midterms 
    ##                 1                 1 
    ## 
    ## $`377`
    ##          bilerico         celebrate               day             great 
    ##                 1                 1                 1                 1 
    ## httptcopobhp5j81p            jerame 
    ##                 1                 1 
    ## 
    ## $`378`
    ##  ascentricly      eyebrow         game         mine rachelmorton        ryans 
    ##            1            1            1            1            1            1 
    ## 
    ## $`379`
    ##   bout   full   lawn    mow snakes   time 
    ##      1      1      1      1      1      1 
    ## 
    ## $`380`
    ##    alive decisive     game     nats  putting   series 
    ##        1        1        1        1        1        1 
    ## 
    ## $`381`
    ##         god 35bazillion  fimustauri        just        pick    rational 
    ##           2           1           1           1           1           1 
    ## 
    ## $`382`
    ##                                    booty 
    ##                                        1 
    ##                                   bubble 
    ##                                        1 
    ##                                    gotta 
    ##                                        1 
    ##                                   shawty 
    ##                                        1 
    ## <f0><ff><U+0091><U+0080><f0><ff><U+008D><U+0091> 
    ##                                        1 
    ## 
    ## $`383`
    ##              club             fedex             field httptco6xbtodahll 
    ##                 1                 1                 1                 1 
    ##       hyattsville             level 
    ##                 1                 1 
    ## 
    ## $`384`
    ##   ass   bae   chy goofy   hey  know 
    ##     1     1     1     1     1     1 
    ## 
    ## $`385`
    ##                                                 “officialaztec 
    ##                                                              2 
    ## faderegg2k14”fadeajay2k14”fadechubgod2k14”fadechubbyniggas2k14 
    ##                                                              1 
    ##                                                       “ajayyyy 
    ##                                                              1 
    ## 
    ## $`386`
    ## different      name       new     story      year 
    ##         1         1         1         1         1 
    ## 
    ## $`387`
    ##    best  friend hanging    miss 
    ##       1       1       1       1 
    ## 
    ## $`388`
    ##   redskins announcers    chicken       lets        say   tonights 
    ##          3          1          1          1          1          1 
    ## 
    ## $`389`
    ## masnnationals        thanks 
    ##             1             1 
    ## 
    ## $`390`
    ##     assume       dont  extremely irritating 
    ##          1          1          1          1 
    ## 
    ## $`391`
    ##              gift httptcongm7thpnlt               kia            motors 
    ##                 1                 1                 1                 1 
    ##               one            owning 
    ##                 1                 1 
    ## 
    ## $`392`
    ##         fade myyownworldd        still          yet 
    ##            1            1            1            1 
    ## 
    ## $`393`
    ##     gym     nah    open playing 
    ##       1       1       1       1 
    ## 
    ## $`394`
    ##  ibelieve      make nationals     night  redskins      turn 
    ##         1         1         1         1         1         1 
    ## 
    ## $`395`
    ##               36m              face httptco1jpyxq1pqg          lockette 
    ##                 1                 1                 1                 1 
    ##          maryland     mondaynightfb 
    ##                 1                 1 
    ## 
    ## $`396`
    ##                                                                                                                           gorman8r 
    ##                                                                                                                                  1 
    ## <f0><ff><U+0098><U+0086><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                                                                                                  1 
    ## 
    ## $`397`
    ##      day tomorrow     work 
    ##        1        1        1 
    ## 
    ## $`398`
    ##           happens httptcoihvmsohqhq httptcoizoak29qtx          ibelieve 
    ##                 1                 1                 1                 1 
    ##              nats            pandas 
    ##                 1                 1 
    ## 
    ## $`399`
    ##       lmaooo mrdcsportssr      perfect        video 
    ##            1            1            1            1 
    ## 
    ## $`400`
    ##                                                            resentment 
    ##                                                                     1 
    ## shit<f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+008D><f0><ff><U+009E><U+00A7><f0><ff><U+009E><U+00B6> 
    ##                                                                     1 
    ## 
    ## $`401`
    ## baptizing   welcome 
    ##         1         1 
    ## 
    ## $`402`
    ##             drone          episodes          homeland httptcohatfj3kbum 
    ##                 1                 1                 1                 1 
    ##       perisphere”            queen” 
    ##                 1                 1 
    ## 
    ## $`403`
    ##                                          and 
    ##                                            1 
    ##                                       feelin 
    ##                                            1 
    ##                                         have 
    ##                                            1 
    ##                                        jumps 
    ##                                            1 
    ##                                       niggas 
    ##                                            1 
    ## oomf<f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008F> 
    ##                                            1 
    ## 
    ## $`404`
    ##         amp       ariel  especially        lmao         mom nishaaaaaaa 
    ##           1           1           1           1           1           1 
    ## 
    ## $`405`
    ##              fedx           feeeeel             field httptcorytlplt56i 
    ##                 1                 1                 1                 1 
    ##              lets          redskins 
    ##                 1                 1 
    ## 
    ## $`406`
    ##    are    bad before    end   fans   game 
    ##      1      1      1      1      1      1 
    ## 
    ## $`407`
    ##         gotta lahhhollywood          like       omarion     something 
    ##             1             1             1             1             1 
    ## 
    ## $`408`
    ##                                                          ass 
    ##                                                            1 
    ##                                                      smashed 
    ##                                                            1 
    ##                                                         they 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                            1 
    ## 
    ## $`409`
    ## black  haus 
    ##     1     1 
    ## 
    ## $`410`
    ##        broke        broom       giants ibackthenats     ibelieve         just 
    ##            1            1            1            1            1            1 
    ## 
    ## $`411`
    ## attitudes      make   peoples      some      ugly 
    ##         1         1         1         1         1 
    ## 
    ## $`412`
    ##               amphip                  hop                 love 
    ##                    1                    1                    1 
    ## <f0><ff><U+0098><U+0081> 
    ##                    1 
    ## 
    ## $`413`
    ##                                     back 
    ##                                        1 
    ##                               officially 
    ##                                        1 
    ##                                originals 
    ##                                        1 
    ## <f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+00A9> 
    ##                                        1 
    ## <f0><ff><U+0099><U+009C><f0><ff><U+0099><U+009C> 
    ##                                        1 
    ## 
    ## $`414`
    ##         dvnjr      entitled          feel          hmmm menshealthmag 
    ##             1             1             1             1             1 
    ##         soooo 
    ##             1 
    ## 
    ## $`415`
    ## attractive        bit       just      kinda        lil 
    ##          1          1          1          1          1 
    ## 
    ## $`416`
    ##              alive               game                get httptcoixecjlcweb” 
    ##                  1                  1                  1                  1 
    ##               huge       ibackthenats 
    ##                  1                  1 
    ## 
    ## $`417`
    ##        alright darnjesssssica        factory           good            hah 
    ##              1              1              1              1              1 
    ##            hat 
    ##              1 
    ## 
    ## $`418`
    ##               day httptcow03qrtmz27            pidgey pok<e3><U+00A9>mon 
    ##                 1                 1                 1                 1 
    ## 
    ## $`419`
    ## bonus   got today  work   wow 
    ##     1     1     1     1     1 
    ## 
    ## $`420`
    ##  aint  baby   cha fools  head   mad 
    ##     1     1     1     1     1     1 
    ## 
    ## $`421`
    ##  head  song stuck worst 
    ##     1     1     1     1 
    ## 
    ## $`422`
    ##            belize httptcobrnjjojtyl             pedro             place 
    ##                 1                 1                 1                 1 
    ##               san        takemeback 
    ##                 1                 1 
    ## 
    ## $`423`
    ##          everyday              fall         highlines httptcoanu4bgwb4c 
    ##                 1                 1                 1                 1 
    ##          nofilter          snowshoe 
    ##                 1                 1 
    ## 
    ## $`424`
    ##                         emojis <f0><ff><U+0093><U+00A5><f0><ff><U+0093><U+00B2> 
    ##                              1                              1 
    ## 
    ## $`425`
    ##  celtics     game   guards  jumpers   losing muchlong 
    ##        1        1        1        1        1        1 
    ## 
    ## $`426`
    ## completely        day       love        one   stressed    without 
    ##          1          1          1          1          1          1 
    ## 
    ## $`427`
    ##       amp beautiful       boy      fizz      just    lmaooo 
    ##         1         1         1         1         1         1 
    ## 
    ## $`428`
    ## pointlessblog 
    ##             1 
    ## 
    ## $`429`
    ##        eunyangnbc httptcovyexyuptsm 
    ##                 1                 1 
    ## 
    ## $`430`
    ## ashlaayraeee    condition     critical         good          lls         said 
    ##            1            1            1            1            1            1 
    ## 
    ## $`431`
    ##              april              fedex              field                got 
    ##                  1                  1                  1                  1 
    ##              great httpstcojveaws22mj 
    ##                  1                  1 
    ## 
    ## $`432`
    ##  bryan daniel   ever    get    god  gonna 
    ##      1      1      1      1      1      1 
    ## 
    ## $`433`
    ##   boy  dont   een  hang likee  look 
    ##     1     1     1     1     1     1 
    ## 
    ## $`434`
    ## erkyya   lost  phone 
    ##      1      1      1 
    ## 
    ## $`435`
    ##    else feeling   never  nobody   swear    will 
    ##       1       1       1       1       1       1 
    ## 
    ## $`436`
    ##            freakin httpstcoqfcegw6eq9               joes            palooza 
    ##                  1                  1                  1                  1 
    ##            pumpkin             trader 
    ##                  1                  1 
    ## 
    ## $`437`
    ## httptcotdofy6gmru              just         northside             photo 
    ##                 1                 1                 1                 1 
    ##            posted            social 
    ##                 1                 1 
    ## 
    ## $`438`
    ##  back hurts 
    ##     1     1 
    ## 
    ## $`439`
    ## braveeee    youre 
    ##        1        1 
    ## 
    ## $`440`
    ##        awwiebabyy          edmyakua httptcosscshgtfn2              lmao 
    ##                 1                 1                 1                 1 
    ##          maryroni              miss 
    ##                 1                 1 
    ## 
    ## $`441`
    ##         bout        break         httr interception         kirk       record 
    ##            1            1            1            1            1            1 
    ## 
    ## $`442`
    ## httptcoxq3i5ahmrs             obola 
    ##                 1                 1 
    ## 
    ## $`443`
    ##               12s             dream             fedex              from 
    ##                 1                 1                 1                 1 
    ##           gohawks httptco5thk0tbzca 
    ##                 1                 1 
    ## 
    ## $`444`
    ##                               brothers bruuuuhhhhhhhhhhhh<f0><ff><U+0098><U+0082> 
    ##                                      1                                      1 
    ##                      httptcoaosubonyxy                                  pants 
    ##                                      1                                      1 
    ##                                  tight 
    ##                                      1 
    ## 
    ## $`445`
    ##    back    come timeeee <U+2764><U+FE0F><U+26BE><U+FE0F><U+26BE><U+FE0F> 
    ##       1       1       1       1 
    ## 
    ## $`446`
    ##     b2rmusic   b2rwaynepa         lets         matt mattmcandrew  nbcthevoice 
    ##            1            1            1            1            1            1 
    ## 
    ## $`447`
    ##   around     cool     guys      hey     late lindczar 
    ##        1        1        1        1        1        1 
    ## 
    ## $`448`
    ## bitch<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                             1 
    ##                                    qweenjiggy 
    ##                                             1 
    ##                  song<f0><ff><U+0092><U+0080> 
    ##                                             1 
    ## 
    ## $`449`
    ## friends     got    weak 
    ##       1       1       1 
    ## 
    ## $`450`
    ## getting   inked  winter 
    ##       1       1       1 
    ## 
    ## $`451`
    ##                                                                                                   african 
    ##                                                                                                         1 
    ##                                                                                            bakedchuchinii 
    ##                                                                                                         1 
    ##                                                                                                       hot 
    ##                                                                                                         1 
    ##                                                                                        httptcomcw664fp2p” 
    ##                                                                                                         1 
    ## nigga<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0092><U+0080> 
    ##                                                                                                         1 
    ##                                                                                            “drakethetypee 
    ##                                                                                                         1 
    ## 
    ## $`452`
    ##          also brucejohnson9     candidate      growthdc      interest 
    ##             1             1             1             1             1 
    ##     markleedc 
    ##             1 
    ## 
    ## $`453`
    ## feelings     like   niggas    sound   startn 
    ##        1        1        1        1        1 
    ## 
    ## $`454`
    ##     hope natitude     nats  remains 
    ##        1        1        1        1 
    ## 
    ## $`455`
    ##          boy  consolation         hell          hey lhhhollywood         paid 
    ##            1            1            1            1            1            1 
    ## 
    ## $`456`
    ##         gaia          hey intersection monicawilcox         much         name 
    ##            1            1            1            1            1            1 
    ## 
    ## $`457`
    ## ugly 
    ##    1 
    ## 
    ## $`458`
    ##         bulls     extremely         hyped motherfucking        nation 
    ##             1             1             1             1             1 
    ## 
    ## $`459`
    ##       hell missmichy1        one        put       show 
    ##          1          1          1          1          1 
    ## 
    ## $`460`
    ##                                         disney 
    ##                                              1 
    ##                                      halloween 
    ##                                              1 
    ## movies<f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D> 
    ##                                              1 
    ## 
    ## $`461`
    ## left lets nats  win 
    ##    1    1    1    1 
    ## 
    ## $`462`
    ## dirtyyydiii         eye      forget     swollen 
    ##           1           1           1           1 
    ## 
    ## $`463`
    ##        can savmillerr       tell 
    ##          1          1          1 
    ## 
    ## $`464`
    ##       avi daddymiaa       lol       one 
    ##         1         1         1         1 
    ## 
    ## $`465`
    ##      bit honestly    jacob   kevuhn    might   outfit 
    ##        1        1        1        1        1        1 
    ## 
    ## $`466`
    ##      blown thoroughly 
    ##          1          1 
    ## 
    ## $`467`
    ## bionicbombshell          braids            lame  lizzslockeroom  musicisfr33dom 
    ##               1               1               1               1               1 
    ##             why 
    ##               1 
    ## 
    ## $`468`
    ##  aramesharash           big        enough          home middleeastguy 
    ##             1             1             1             1             1 
    ##      nuisance 
    ##             1 
    ## 
    ## $`469`
    ## effective    lovers    really     today  virginia 
    ##         1         1         1         1         1 
    ## 
    ## $`470`
    ## forgetsaboutyou         genesis            girl       instagram             mcm 
    ##               1               1               1               1               1 
    ##            post 
    ##               1 
    ## 
    ## $`471`
    ## marciemarr        yea 
    ##          1          1 
    ## 
    ## $`472`
    ##      curlyw       great   hitheball   nationals    natitude stayfocused 
    ##           1           1           1           1           1           1 
    ## 
    ## $`473`
    ## koledragoo    perfect        wow 
    ##          1          1          1 
    ## 
    ## $`474`
    ##                                                           httptcohij1a7goqt 
    ##                                                                           1 
    ## <f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD> 
    ##                                                                           1 
    ## 
    ## $`475`
    ## everyone     hows 
    ##        1        1 
    ## 
    ## $`476`
    ##   anubis    house marathon   season teennick watching 
    ##        1        1        1        1        1        1 
    ## 
    ## $`477`
    ## benbernanke      called    chairman    couldn’t         due         fed 
    ##           1           1           1           1           1           1 
    ## 
    ## $`478`
    ##        foto       poner         una etiquetarme    facebook        girl 
    ##           2           2           2           1           1           1 
    ## 
    ## $`479`
    ##                  freezing house<f0><ff><U+0098><U+0085>                 literally 
    ##                         1                         1                         1 
    ## 
    ## $`480`
    ##          cbelldba            chilis          grrlgeek httptcogca45cfog0 
    ##                 1                 1                 1                 1 
    ##              many               omg 
    ##                 1                 1 
    ## 
    ## $`481`
    ## early going sleep 
    ##     1     1     1 
    ## 
    ## $`482`
    ## theres    way 
    ##      1      1 
    ## 
    ## $`483`
    ##              asaprozsahegyi welcome<f0><ff><U+0098><U+009A> 
    ##                           1                           1 
    ## 
    ## $`484`
    ##  annoying followers      just       one  realized 
    ##         1         1         1         1         1 
    ## 
    ## $`485`
    ##      amp     bees   coffee enjoying    fresh  friends 
    ##        1        1        1        1        1        1 
    ## 
    ## $`486`
    ##                 feel                 just                 know 
    ##                    1                    1                    1 
    ##                wanna <f0><ff><U+0098><U+009C> 
    ##                    1                    1 
    ## 
    ## $`487`
    ##       anyway        bring         home ibackthenats     ibelieve          may 
    ##            1            1            1            1            1            1 
    ## 
    ## $`488`
    ## alive   for  nats   now  were 
    ##     1     1     1     1     1 
    ## 
    ## $`489`
    ##    1500     500     bmw   bonus dollars     get 
    ##       1       1       1       1       1       1 
    ## 
    ## $`490`
    ## actually     into     jerk      lol   turned 
    ##        1        1        1        1        1 
    ## 
    ## $`491`
    ##        already            but         tested thatbrownguyoo            you 
    ##              1              1              1              1              1 
    ## 
    ## $`492`
    ##      beating myyownworldd         part         play   savvyzeeee      trained 
    ##            1            1            1            1            1            1 
    ## 
    ## $`493`
    ##          gotham        watching <f0><ff><U+0093><U+00BA> 
    ##               1               1               1 
    ## 
    ## $`494`
    ##              fedex              field httpstco7sjtlzsfwb            jerseys 
    ##                  1                  1                  1                  1 
    ##               look                new 
    ##                  1                  1 
    ## 
    ## $`495`
    ##   i’m tired 
    ##     1     1 
    ## 
    ## $`496`
    ## hightop1231        move         omg 
    ##           1           1           1 
    ## 
    ## $`497`
    ##                                                         aint 
    ##                                                            1 
    ##                                                        babby 
    ##                                                            1 
    ##                                                    kaymonell 
    ##                                                            1 
    ##                                                         lied 
    ##                                                            1 
    ##                                                        never 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                            1 
    ## 
    ## $`498`
    ##                  amp         bridalmarket              excited 
    ##                    1                    1                    1 
    ## tasselandtastemakers 
    ##                    1 
    ## 
    ## $`499`
    ##     evening     filling      judges   mealonez1 nbcthevoice        rule 
    ##           1           1           1           1           1           1 
    ## 
    ## $`500`
    ##        get   ibelieve       nlds       play postseason      there 
    ##          1          1          1          1          1          1 
    ## 
    ## $`501`
    ##      ball       how   jewelry      lhhh       lol souljaboy 
    ##         1         1         1         1         1         1 
    ## 
    ## $`502`
    ## actually football    slays 
    ##        1        1        1 
    ## 
    ## $`503`
    ##        last     seasons      better        like        much nbcthevoice 
    ##           2           2           1           1           1           1 
    ## 
    ## $`504`
    ## httptcosqcfa02zqi”          “cutebeds <f0><ff><U+0098><U+00BB> 
    ##                  1                  1                  1 
    ## 
    ## $`505`
    ##          baltimore     cardinaltavern httpstcorssjfllwgj 
    ##                  1                  1                  1 
    ## 
    ## $`506`
    ##                                                   chill 
    ##                                                       1 
    ##                                                   funny 
    ##                                                       1 
    ##                                                  really 
    ##                                                       1 
    ## <f0><ff><U+0090><U+00AA><f0><ff><U+0090><U+0089><f0><ff><U+0098><U+009A> 
    ##                                                       1 
    ## 
    ## $`507`
    ##     gaianism         hone       ideals   interested monicawilcox        novel 
    ##            1            1            1            1            1            1 
    ## 
    ## $`508`
    ##  always closing    hgil 
    ##       1       1       1 
    ## 
    ## $`509`
    ##         smaller aacountyschools     achievement             avg      discipline 
    ##               2               1               1               1               1 
    ##            gaps 
    ##               1 
    ## 
    ## $`510`
    ##                  actually                     makes sense<f0><ff><U+0098><U+0082> 
    ##                         1                         1                         1 
    ##                      true                wills4lwen 
    ##                         1                         1 
    ## 
    ## $`511`
    ## httptcom90noie5kv 
    ##                 1 
    ## 
    ## $`512`
    ##             anything              capable <f0><ff><U+0098><U+0089> 
    ##                    1                    1                    1 
    ## 
    ## $`513`
    ## eayly   its 
    ##     1     1 
    ## 
    ## $`514`
    ##     choice”       girls        wear       weave “staypurpin 
    ##           1           1           1           1           1 
    ## 
    ## $`515`
    ## ibelievethatwewillwin                  nats 
    ##                     1                     1 
    ## 
    ## $`516`
    ##            banana        drewmagary httptcosjp8c9zgyw              mayo 
    ##                 1                 1                 1                 1 
    ##              nats          sandwich 
    ##                 1                 1 
    ## 
    ## $`517`
    ##  shit tired 
    ##     1     1 
    ## 
    ## $`518`
    ##          bet johnjharwood mikeviqueira     navista7 pamelaschuur          see 
    ##            1            1            1            1            1            1 
    ## 
    ## $`519`
    ##   guess   hated thought     two     you 
    ##       1       1       1       1       1 
    ## 
    ## $`520`
    ## awwiebabyy     campus   edmyakua        get kariclaure       miss 
    ##          1          1          1          1          1          1 
    ## 
    ## $`521`
    ## abbygill1  yaaaasss 
    ##         1         1 
    ## 
    ## $`522`
    ##     ago charger contact    died    hard   hours 
    ##       1       1       1       1       1       1 
    ## 
    ## $`523`
    ##                                     making 
    ##                                          1 
    ##                                      tacos 
    ##                                          1 
    ## tonight<f0><ff><U+0099><U+009C><f0><ff><U+0092><U+00A3> 
    ##                                          1 
    ## 
    ## $`524`
    ## already     its october  sheesh 
    ##       1       1       1       1 
    ## 
    ## $`525`
    ## gemmaannestyles            love           queen 
    ##               1               1               1 
    ## 
    ## $`526`
    ## text 
    ##    1 
    ## 
    ## $`527`
    ## everywhere    looking       tell 
    ##          1          1          1 
    ## 
    ## $`528`
    ## amyambierotic         cindy     cindydees       message          sent 
    ##             1             1             1             1             1 
    ## 
    ## $`529`
    ##         bruh itsreiillyyy     jaybeerz         lord         trap 
    ##            1            1            1            1            1 
    ## 
    ## $`530`
    ##    also    cant focused   great  harper    nats 
    ##       1       1       1       1       1       1 
    ## 
    ## $`531`
    ## annieatanner15           girl           gods          great           loud 
    ##              1              1              1              1              1 
    ##           must 
    ##              1 
    ## 
    ## $`532`
    ##              baes           display          gorgeous             house 
    ##                 1                 1                 1                 1 
    ## httptcozhtn358dxb              shes 
    ##                 1                 1 
    ## 
    ## $`533`
    ##                                               longlivekatria 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0085><f0><ff><U+0091><U+0080> 
    ##                                                            1 
    ## 
    ## $`534`
    ##       also      candy     follow      going       hard hcftoronto 
    ##          1          1          1          1          1          1 
    ## 
    ## $`535`
    ## birthday<f0><ff><U+0098><U+009A><f0><ff><U+009E><U+009A><f0><ff><U+009E><U+0088> 
    ##                                                                    1 
    ##                                                                happy 
    ##                                                                    1 
    ##                                                             sammlite 
    ##                                                                    1 
    ## 
    ## $`536`
    ##             bakers httpstcofcebz2tn7u             mellow         mellowadmo 
    ##                  1                  1                  1                  1 
    ##           mushroom              pizza 
    ##                  1                  1 
    ## 
    ## $`537`
    ##              bane           colored            fuckin httptcoyaicgust1z 
    ##                 1                 1                 1                 1 
    ##             lynch          marshawn 
    ##                 1                 1 
    ## 
    ## $`538`
    ##        cohencosby           freedom               gym            health 
    ##                 1                 1                 1                 1 
    ## httptcorxsmk5rcpf         knowledge 
    ##                 1                 1 
    ## 
    ## $`539`
    ##                                          allen 
    ##                                              1 
    ## braids<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                              1 
    ##                                            got 
    ##                                              1 
    ##                                        iverson 
    ##                                              1 
    ##                                            rg3 
    ##                                              1 
    ## 
    ## $`540`
    ##   annoyed   getting    lowkey   lowkey”      word “majinjii 
    ##         1         1         1         1         1         1 
    ## 
    ## $`541`
    ## httptcoyhvvrqgymw               mcm 
    ##                 1                 1 
    ## 
    ## $`542`
    ##  loyal   aint  claim niggas really  these 
    ##      2      1      1      1      1      1 
    ## 
    ## $`543`
    ##          fizz          fuck           got        jordan         kinda 
    ##             1             1             1             1             1 
    ## lahhhollywood 
    ##             1 
    ## 
    ## $`544`
    ##                      boyfriend                           evil 
    ##                              1                              1 
    ##                          swear <f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+00A9> 
    ##                              1                              1 
    ## 
    ## $`545`
    ## fleek<f0><ff><U+0098><U+0085>                     games                       got 
    ##                         1                         1                         1 
    ##                  hairline                      hoes         httptcoe4bo20r6ap 
    ##                         1                         1                         1 
    ## 
    ## $`546`
    ## annoying    apple      cry  fucking    keeps     next 
    ##        1        1        1        1        1        1 
    ## 
    ## $`547`
    ##     fanboy       obey       real     scarce soarscares        you 
    ##          1          1          1          1          1          1 
    ## 
    ## $`548`
    ## football     httr     lets    ready redskins 
    ##        1        1        1        1        1 
    ## 
    ## $`549`
    ##      ann     cmon      not      now    seton tombomb7 
    ##        1        1        1        1        1        1 
    ## 
    ## $`550`
    ## allibaabyy       fake    friends       glad       life        lol 
    ##          1          1          1          1          1          1 
    ## 
    ## $`551`
    ##         agree brucejohnson9       carried      growthdc     markleedc 
    ##             1             1             1             1             1 
    ##          news 
    ##             1 
    ## 
    ## $`552`
    ##       anytime     available         daddy freakngeek706 
    ##             1             1             1             1 
    ## 
    ## $`553`
    ##       anything          avoid           best disappointment         expect 
    ##              1              1              1              1              1 
    ##            the 
    ##              1 
    ## 
    ## $`554`
    ## <f0><ff><U+0099><U+008B><U+263A><U+FE0F> 
    ##                     1 
    ## 
    ## $`555`
    ## embora  muito triste    vai victor 
    ##      1      1      1      1      1 
    ## 
    ## $`556`
    ##                                                                        daddymiaa 
    ##                                                                                1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0094><f0><ff><U+0098><U+009D> 
    ##                                                                                1 
    ## 
    ## $`557`
    ##             never              band              even          favorite 
    ##                 2                 1                 1                 1 
    ##            girl’s httptcoad1c44kz4k 
    ##                 1                 1 
    ## 
    ## $`558`
    ##      brother     clueless          dad rramirezryan 
    ##            1            1            1            1 
    ## 
    ## $`559`
    ##             got             lol        traycare <f0><ff><U+0098><U+00A6> 
    ##               1               1               1               1 
    ## 
    ## $`560`
    ##           already              back        fedexfield         forfeited 
    ##                 1                 1                 1                 1 
    ##           heading httptcolxkr8mhskb 
    ##                 1                 1 
    ## 
    ## $`561`
    ##    bad   feet   glad   home hungry   hurt 
    ##      1      1      1      1      1      1 
    ## 
    ## $`562`
    ##                                                                  littlenaijagirl 
    ##                                                                                1 
    ##                                                                     nigerianinme 
    ##                                                                                1 
    ## <f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092> 
    ##                                                                                1 
    ## 
    ## $`563`
    ##       favorite           show       thevoice           time watchingblinds 
    ##              1              1              1              1              1 
    ## 
    ## $`564`
    ##     baby    brand cadillac    drove      new theclash 
    ##        1        1        1        1        1        1 
    ## 
    ## $`565`
    ##     contact  faaaacckkk farronafter   initiated         smh     telling 
    ##           1           1           1           1           1           1 
    ## 
    ## $`566`
    ##                  ham    httptcolzgm72ggvd                quick 
    ##                    1                    1                    1 
    ##                  rey                right <f0><ff><U+0098><U+009E> 
    ##                    1                    1                    1 
    ## 
    ## $`567`
    ##   online   school shopping    sooth   stress 
    ##        1        1        1        1        1 
    ## 
    ## $`568`
    ## httptco16pryjdkhw           wizards 
    ##                 1                 1 
    ## 
    ## $`569`
    ##    definitely      favorite           one      pictures ximenaaaaaisa 
    ##             1             1             1             1             1 
    ## 
    ## $`570`
    ## somo <U+2764><U+FE0F> 
    ##    1    1 
    ## 
    ## $`571`
    ## awwiebabyy     crying   edmyakua  literally   maryroni        now 
    ##          1          1          1          1          1          1 
    ## 
    ## $`572`
    ## omarionlawwwd     thickness          this 
    ##             1             1             1 
    ## 
    ## $`573`
    ## anyone enough famous   good    idk   like 
    ##      1      1      1      1      1      1 
    ## 
    ## $`574`
    ##        game         got        next shanzwalshy 
    ##           1           1           1           1 
    ## 
    ## $`575`
    ##             10000        attractive              cute             girls 
    ##                 1                 1                 1                 1 
    ## httptcodh68cdobd3      lacemysneaks 
    ##                 1                 1 
    ## 
    ## $`576`
    ##            1st        appears iamronniebanks        mention            now 
    ##              1              1              1              1              1 
    ##  qlovingronnie 
    ##              1 
    ## 
    ## $`577`
    ##      dress    excited        get homecoming      soooo    tomorow 
    ##          1          1          1          1          1          1 
    ## 
    ## $`578`
    ## httptcokac4mspe8j               lt3           missing               now 
    ##                 1                 1                 1                 1 
    ##             right              twin 
    ##                 1                 1 
    ## 
    ## $`579`
    ##     good taquetoh 
    ##        1        1 
    ## 
    ## $`580`
    ##    back massage     now     pay   right someone 
    ##       1       1       1       1       1       1 
    ## 
    ## $`581`
    ##               around           chargerday                 town 
    ##                    1                    1                    1 
    ## <f0><ff><U+009A><U+0099> 
    ##                    1 
    ## 
    ## $`582`
    ## ibackthenats     ibelieve        night      quietly         will 
    ##            1            1            1            1            1 
    ## 
    ## $`583`
    ##                 asap               frosty                 need 
    ##                    1                    1                    1 
    ## <f0><ff><U+0098><U+0082> 
    ##                    1 
    ## 
    ## $`584`
    ## everybody       few       for     girls       its    school 
    ##         1         1         1         1         1         1 
    ## 
    ## $`585`
    ## literally    people     stand 
    ##         1         1         1 
    ## 
    ## $`586`
    ## everyone     fuck 
    ##        1        1 
    ## 
    ## $`587`
    ##         david         favor           get          life   maccarone96 
    ##             1             1             1             1             1 
    ## michel4beaute 
    ##             1 
    ## 
    ## $`588`
    ## dance gavin  like  talk 
    ##     2     1     1     1 
    ## 
    ## $`589`
    ##     baby  bitches forehead   fuckin    glued    hairs 
    ##        1        1        1        1        1        1 
    ## 
    ## $`590`
    ##                                              alrantisym 
    ##                                                       1 
    ## <f8><U+00A7><f8><U+00A8><f9><U+0088><f8><U+00B1><f9><U+0086><f8><U+00AA><f8><U+00B3> 
    ##                                                       1 
    ## <f8><U+00A7><f8><U+00AD><f8><U+00AA><f8><U+00B1><f8><U+00A7><f9><U+0085><f9><U+009A> 
    ##                                                       1 
    ## <f8><U+00A7><f9><U+0084><f8><U+00AE><f8><U+00AF><f9><U+009A><f9><U+0088><f9><U+009A> 
    ##                                                       1 
    ##                    <f8><U+00A8><f9><U+009A><f9><U+0083> 
    ##                                                       1 
    ##        <f8><U+00AC><f9><U+0086><f8><U+00A7><f8><U+00A8> 
    ##                                                       1 
    ## 
    ## $`591`
    ##                  804                  but         marctrain001 
    ##                    1                    1                    1 
    ## <f0><ff><U+009E><U+0089> 
    ##                    1 
    ## 
    ## $`592`
    ##   back broski   home  least little 
    ##      1      1      1      1      1 
    ## 
    ## $`593`
    ##     game    alive baseball     best     ever      get 
    ##        3        1        1        1        1        1 
    ## 
    ## $`594`
    ## drink   use 
    ##     1     1 
    ## 
    ## $`595`
    ##             2nd            best            burn            cant          corner 
    ##               1               1               1               1               1 
    ## deseanjackson11 
    ##               1 
    ## 
    ## $`596`
    ##                               forever                                listen 
    ##                                     1                                     1 
    ##                               weekend                                weeknd 
    ##                                     1                                     1 
    ##                           “rosyceline <f0><ff><U+0098><U+008F><f0><ff><U+009E><U+00A7><U+2764><U+FE0F>” 
    ##                                     1                                     1 
    ## 
    ## $`597`
    ##  always   tired    back  depend forward     one 
    ##       2       2       1       1       1       1 
    ## 
    ## $`598`
    ##                     get                    name                   never 
    ##                       1                       1                       1 
    ##                   nigga                  tatted tho<f0><ff><U+0098><U+0092> 
    ##                       1                       1                       1 
    ## 
    ## $`599`
    ##  actually       can      guys instagram  pictures      post 
    ##         1         1         1         1         1         1 
    ## 
    ## $`600`
    ## emotional  jaybeerz lightskin    niggah 
    ##         1         1         1         1 
    ## 
    ## $`601`
    ## another  credit deserve    give granted  harder 
    ##       1       1       1       1       1       1 
    ## 
    ## $`602`
    ##             games               get            hockey httptcofptqyebrxk 
    ##                 1                 1                 1                 1 
    ##              made             night 
    ##                 1                 1 
    ## 
    ## $`603`
    ##          alive dcmarkwhitaker fightinhydrant           four           game 
    ##              1              1              1              1              1 
    ##           haha 
    ##              1 
    ## 
    ## $`604`
    ##   border     darn delaware     fall     good    happy 
    ##        1        1        1        1        1        1 
    ## 
    ## $`605`
    ## cute  hes  lol 
    ##    1    1    1 
    ## 
    ## $`606`
    ##                                     know 
    ##                                        1 
    ## <f0><ff><U+0098><U+008B><f0><ff><U+009C><U+008D> 
    ##                                        1 
    ## 
    ## $`607`
    ##   breweryommegang          drinking httptcobxmstesnoe          ommegang 
    ##                 1                 1                 1                 1 
    ##     publictrustdc             witte 
    ##                 1                 1 
    ## 
    ## $`608`
    ##                                                         babe 
    ##                                                            1 
    ##                                            httptcod3s9h6qano 
    ##                                                            1 
    ##                                                          mcm 
    ##                                                            1 
    ## <f0><ff><U+0099><U+009A><f0><ff><U+0099><U+009A><f0><ff><U+0098><U+008D> 
    ##                                                            1 
    ## 
    ## $`609`
    ## indiaeaton    inikkee    reaveta      right       time 
    ##          1          1          1          1          1 
    ## 
    ## $`610`
    ## homeopathicdana        hormesis             say       sceptiguy         support 
    ##               1               1               1               1               1 
    ##      tetenterre 
    ##               1 
    ## 
    ## $`611`
    ## hungry<f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092> 
    ##                                                                  1 
    ##                                                               make 
    ##                                                                  1 
    ##                                                               toni 
    ##                                                                  1 
    ##                                                              tryna 
    ##                                                                  1 
    ## 
    ## $`612`
    ##   always gorgeous  jasmine    lahhh     like     look 
    ##        1        1        1        1        1        1 
    ## 
    ## $`613`
    ## httptco6cys1nhha3 
    ##                 1 
    ## 
    ## $`614`
    ##    amp  bathe boudda   call  night 
    ##      1      1      1      1      1 
    ## 
    ## $`615`
    ## abused    dog justin  owner rather    smh 
    ##      1      1      1      1      1      1 
    ## 
    ## $`616`
    ##       can      deep       get      once something     think 
    ##         1         1         1         1         1         1 
    ## 
    ## $`617`
    ##                            bae              httptcosxtwx53ry1 
    ##                              1                              1 
    ## <f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00BB> 
    ##                              1 
    ## 
    ## $`618`
    ## makewards     shirt 
    ##         1         1 
    ## 
    ## $`619`
    ##   both bottom    eat    fun    idk   like 
    ##      1      1      1      1      1      1 
    ## 
    ## $`620`
    ##                                          forever 
    ##                                                1 
    ##                                             lied 
    ##                                                1 
    ## on<f0><ff><U+0098><U+00A1><f0><ff><U+0098><U+00A1><f0><ff><U+0098><U+00A3>” 
    ##                                                1 
    ##                                              tho 
    ##                                                1 
    ##                                        “lilswahh 
    ##                                                1 
    ## 
    ## $`621`
    ##       get hatpurley       hbd      hope      lets       mom 
    ##         1         1         1         1         1         1 
    ## 
    ## $`622`
    ##        alright        doggies indirectionstx 
    ##              1              1              1 
    ## 
    ## $`623`
    ##       aishadahira             group httptcoqjsnvyh3ph              like 
    ##                 1                 1                 1                 1 
    ##           someone              when 
    ##                 1                 1 
    ## 
    ## $`624`
    ##   got guess pizza 
    ##     1     1     1 
    ## 
    ## $`625`
    ##          can         hand        heavy         hold        looks mattsteele23 
    ##            1            1            1            1            1            1 
    ## 
    ## $`626`
    ##                                 idk                                lmao 
    ##                                   1                                   1 
    ## <f0><ff><U+0098><U+0088><f0><ff><U+0092><U+00B0> 
    ##                                   1 
    ## 
    ## $`627`
    ##        caltor ethanamitasty         qodba 
    ##             1             1             1 
    ## 
    ## $`628`
    ##    breasts everything       just       rosa 
    ##          1          1          1          1 
    ## 
    ## $`629`
    ##          finished          grateful httptcoi9yynzxt0s           iceland 
    ##                 1                 1                 1                 1 
    ##           photos…          pictures 
    ##                 1                 1 
    ## 
    ## $`630`
    ## days<f0><ff><U+0098><U+00AB><f0><ff><U+0092><U+008D><f0><ff><U+0092><U+0091> 
    ##                                                           1 
    ##                                                      havent 
    ##                                                           1 
    ##                                                         him 
    ##                                                           1 
    ##                                                        seen 
    ##                                                           1 
    ## 
    ## $`631`
    ##           academy       forestville httptcoxe4etbadex          military 
    ##                 1                 1                 1                 1 
    ##          suitland 
    ##                 1 
    ## 
    ## $`632`
    ##  constantly  everything       great        make overanalyze       worry 
    ##           1           1           1           1           1           1 
    ## 
    ## $`633`
    ##           299           315           324          made       minutes 
    ##             1             1             1             1             1 
    ## qlovingronnie 
    ##             1 
    ## 
    ## $`634`
    ## burglar  expect   fully   homer  kristy    like 
    ##       1       1       1       1       1       1 
    ## 
    ## $`635`
    ##               133 httptco3bok3lcnge    iamronniebanks            impact 
    ##                 1                 1                 1                 1 
    ##         published     qlovingronnie 
    ##                 1                 1 
    ## 
    ## $`636`
    ##         council        debate14 electgeorge2014           never            pass 
    ##               1               1               1               1               1 
    ##            says 
    ##               1 
    ## 
    ## $`637`
    ##       twitter       android          ipad        iphone qlovingronnie 
    ##             3             1             1             1             1 
    ##      top3apps 
    ##             1 
    ## 
    ## $`638`
    ##           1st        408627        became       mention        people 
    ##             1             1             1             1             1 
    ## qlovingronnie 
    ##             1 
    ## 
    ## $`639`
    ##               33m           baldwin              doug              face 
    ##                 1                 1                 1                 1 
    ## httptcoaojjh1vdib          maryland 
    ##                 1                 1 
    ## 
    ## $`640`
    ##   change leeeeeet minnnnnd 
    ##        1        1        1 
    ## 
    ## $`641`
    ##     better    feeling      helps        hub sportsmans       stub 
    ##          1          1          1          1          1          1 
    ## 
    ## $`642`
    ##    got   knew   lien  moose niggas saying 
    ##      1      1      1      1      1      1 
    ## 
    ## $`643`
    ##          can         chat        depth        email monicawilcox        novel 
    ##            2            1            1            1            1            1 
    ## 
    ## $`644`
    ##          bar  blackfinndc    celebrate ibackthenats         just         like 
    ##            1            1            1            1            1            1 
    ## 
    ## $`645`
    ##    contact       game        ill       info        one sabraham16 
    ##          1          1          1          1          1          1 
    ## 
    ## $`646`
    ## america bennett   bless     god   botch   cool” 
    ##       2       2       2       2       1       1 
    ## 
    ## $`647`
    ## bored 
    ##     2 
    ## 
    ## $`648`
    ##         brick       geezaws nicolassizzle     otsutsukl 
    ##             1             1             1             1 
    ## 
    ## $`649`
    ## nigga claim 
    ##     2     1 
    ## 
    ## $`650`
    ## riuuuuh 
    ##       1 
    ## 
    ## $`651`
    ##     bout    cares holycash lmaooooo    looks  talking 
    ##        1        1        1        1        1        1 
    ## 
    ## $`652`
    ##       pants         can christopher         fit     hunting         top 
    ##           2           1           1           1           1           1 
    ## 
    ## $`653`
    ##  duper  super wankin 
    ##      1      1      1 
    ## 
    ## $`654`
    ##            1st        appears        mention            now         states 
    ##              1              1              1              1              1 
    ## thankyouaustin 
    ##              1 
    ## 
    ## $`655`
    ##   gets   lawd tumblr 
    ##      1      1      1 
    ## 
    ## $`656`
    ##               job              cath            health httptcoycokq3r7q6 
    ##                 2                 1                 1                 1 
    ##              jobs               lab 
    ##                 1                 1 
    ## 
    ## $`657`
    ##  arms booty   now right veins 
    ##     1     1     1     1     1 
    ## 
    ## $`658`
    ##                                    comes 
    ##                                        1 
    ##                                     here 
    ##                                        1 
    ##                                     pics 
    ##                                        1 
    ##                                    puppy 
    ##                                        1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0099><U+0088> 
    ##                                        1 
    ## 
    ## $`659`
    ## brucejohnson9       catania        except      growthdc          gt30 
    ##             1             1             1             1             1 
    ##     markleedc 
    ##             1 
    ## 
    ## $`660`
    ## lhhhollywood         like        mafia        movaa         nnaa       shawdy 
    ##            1            1            1            1            1            1 
    ## 
    ## $`661`
    ## 9thwonder       idc      lmao       mom      plus       sit 
    ##         1         1         1         1         1         1 
    ## 
    ## $`662`
    ##              home httptcoblyp0d9ljt              much               now 
    ##                 1                 1                 1                 1 
    ##            pretty             right 
    ##                 1                 1 
    ## 
    ## $`663`
    ##   care   just really    say   wish 
    ##      2      1      1      1      1 
    ## 
    ## $`664`
    ##    hickey       mom      neck       saw       say “xxgeeexx 
    ##         1         1         1         1         1         1 
    ## 
    ## $`665`
    ##       get honeymoon       ill       lml       lol   married 
    ##         1         1         1         1         1         1 
    ## 
    ## $`666`
    ## favorite      get    gifts   greedy     time  winters 
    ##        1        1        1        1        1        1 
    ## 
    ## $`667`
    ##   910  gets   idk  weak whats 
    ##     1     1     1     1     1 
    ## 
    ## $`668`
    ## doziee99      eze     love     mama 
    ##        1        1        1        1 
    ## 
    ## $`669`
    ##         either           fine   monicawilcox            one tracietnichols 
    ##              1              1              1              1              1 
    ## 
    ## $`670`
    ##          big       course         dick         know lhhhollywood          ray 
    ##            1            1            1            1            1            1 
    ## 
    ## $`671`
    ##            corner             early httptcoud593dwycq              late 
    ##                 1                 1                 1                 1 
    ##          mornings             night 
    ##                 1                 1 
    ## 
    ## $`672`
    ##        adams          bad          her lhhhollywood     morticia       mother 
    ##            1            1            1            1            1            1 
    ## 
    ## $`673`
    ## heem  txt 
    ##    1    1 
    ## 
    ## $`674`
    ##                 feelings                     like                   matter 
    ##                        1                        1                        1 
    ##                     much                   mutual shit<f0><ff><U+0098><U+0092> 
    ##                        1                        1                        1 
    ## 
    ## $`675`
    ##       bout       frfr       fuck        get homecoming        say 
    ##          1          1          1          1          1          1 
    ## 
    ## $`676`
    ##   jackson     match   sherman something      that   tonight 
    ##         1         1         1         1         1         1 
    ## 
    ## $`677`
    ##             100am httptco5oznh3q18p             sucks              that 
    ##                 1                 1                 1                 1 
    ## 
    ## $`678`
    ##              fuck              give httptcoojl7lzxmkv              life 
    ##                 1                 1                 1                 1 
    ##            really               wiz 
    ##                 1                 1 
    ## 
    ## $`679`
    ## httptcot4gndocncv       yungeggroll 
    ##                 1                 1 
    ## 
    ## $`680`
    ##            football   httptco9rhkfjxjde   httptcoe0z6gmbrqn              monday 
    ##                   1                   1                   1                   1 
    ## mondaynightfootball                 nfl 
    ##                   1                   1 
    ## 
    ## $`681`
    ## accelerated       among         amp      cancer    conflict        east 
    ##           1           1           1           1           1           1 
    ## 
    ## $`682`
    ##            a1day1               bad           bitches httptcown3egc3oa6 
    ##                 1                 1                 1                 1 
    ##              like            spirit 
    ##                 1                 1 
    ## 
    ## $`683`
    ##    aka anyone   care   food    get   join 
    ##      1      1      1      1      1      1 
    ## 
    ## $`684`
    ##     baker   running      surf treadmill    twiggy  watching 
    ##         1         1         1         1         1         1 
    ## 
    ## $`685`
    ## along child civil crazy  even   get 
    ##     1     1     1     1     1     1 
    ## 
    ## $`686`
    ## httptcoxhqul7anhe 
    ##                 1 
    ## 
    ## $`687`
    ##           ima soufsidevonny          talk      tomorrow 
    ##             1             1             1             1 
    ## 
    ## $`688`
    ## enemies  fuckin  niggas   these 
    ##       1       1       1       1 
    ## 
    ## $`689`
    ##   many photos 
    ##      1      1 
    ## 
    ## $`690`
    ## boutaaa  friend   kersh   stamp yeaaaaa 
    ##       1       1       1       1       1 
    ## 
    ## $`691`
    ## built  lhhh   lls  momz  what   yes 
    ##     1     1     1     1     1     1 
    ## 
    ## $`692`
    ## ready 
    ##     1 
    ## 
    ## $`693`
    ##       bigcitymoms biggestbabyshower              wish 
    ##                 1                 1                 1 
    ## 
    ## $`694`
    ##                 menace                   dont on<f0><ff><U+0099><U+009C> 
    ##                      2                      1                      1 
    ##                society 
    ##                      1 
    ## 
    ## $`695`
    ##                360        classkiller          classpass httpstcoj5kixnxq86 
    ##                  1                  1                  1                  1 
    ##             sculpt           sculptdc 
    ##                  1                  1 
    ## 
    ## $`696`
    ## disgust     you 
    ##       1       1 
    ## 
    ## $`697`
    ##   innings   bigdata    giants nationals outscored      runs 
    ##         2         1         1         1         1         1 
    ## 
    ## $`698`
    ##  alike   just  looks mother  nikki 
    ##      1      1      1      1      1 
    ## 
    ## $`699`
    ##      and 19881990     blue   campus   dinner    fives 
    ##        2        1        1        1        1        1 
    ## 
    ## $`700`
    ##                                                     teamryan 
    ##                                                            1 
    ##                                                   thevoiceth 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0081><f0><ff><U+0098><U+0081><f0><ff><U+0098><U+0081> 
    ##                                                            1 
    ## 
    ## $`701`
    ##  pick  spot sweet third  yess 
    ##     1     1     1     1     1 
    ## 
    ## $`702`
    ## 98rocknitb       good    heading       mike     should       time 
    ##          1          1          1          1          1          1 
    ## 
    ## $`703`
    ##              azz defensehopefully       impressive              ish 
    ##                1                1                1                1 
    ##             kick             lets 
    ##                1                1 
    ## 
    ## $`704`
    ##         isnt      netflix       season supernatural          why 
    ##            1            1            1            1            1 
    ## 
    ## $`705`
    ## httptco5eggq7ok8n 
    ##                 1 
    ## 
    ## $`706`
    ## awwiebabyy   edmyakua kariclaure        yas 
    ##          1          1          1          1 
    ## 
    ## $`707`
    ##         dumb       people toricaitlinn 
    ##            1            1            1 
    ## 
    ## $`708`
    ##       dreams         girl itsreiillyyy     jaybeerz        white         yeet 
    ##            1            1            1            1            1            1 
    ## 
    ## $`709`
    ##          damascus          cellular        connection httptcooapslrkp6z 
    ##                 2                 1                 1                 1 
    ##               job              jobs 
    ##                 1                 1 
    ## 
    ## $`710`
    ## ebnoxi0us    gotchu 
    ##         1         1 
    ## 
    ## $`711`
    ##      got      hey natitude      one 
    ##        1        1        1        1 
    ## 
    ## $`712`
    ##           chicken           dcbarno       dcsportsbog            forget 
    ##                 1                 1                 1                 1 
    ## httptco5gfg4ciinh         nationals 
    ##                 1                 1 
    ## 
    ## $`713`
    ##      bag      get    gotta hospital   packed     soon 
    ##        1        1        1        1        1        1 
    ## 
    ## $`714`
    ## actually    didnt  dislike   forget    forso    funny 
    ##        1        1        1        1        1        1 
    ## 
    ## $`715`
    ##  almost  aspect changed   crazy   every    feel 
    ##       1       1       1       1       1       1 
    ## 
    ## $`716`
    ##    batman    coming    gotham halloween     smile    tweeps 
    ##         1         1         1         1         1         1 
    ## 
    ## $`717`
    ##      cause       city       dont       know scandalous      trust 
    ##          1          1          1          1          1          1 
    ## 
    ## $`718`
    ##            does dominiquearamos            even          famous             get 
    ##               1               1               1               1               1 
    ##          number 
    ##               1 
    ## 
    ## $`719`
    ##           ahora carlosramirezl3           corte             dio            eeuu 
    ##               1               1               1               1               1 
    ##             gay 
    ##               1 
    ## 
    ## $`720`
    ##  bumping     card    chris      fun gamelets      got 
    ##        1        1        1        1        1        1 
    ## 
    ## $`721`
    ##     ate  cheese evening    girl   gross    kind 
    ##       1       1       1       1       1       1 
    ## 
    ## $`722`
    ## collegegameday            dak 
    ##              1              1 
    ## 
    ## $`723`
    ##       better         know       mother zeaddddddddd 
    ##            1            1            1            1 
    ## 
    ## $`724`
    ##                 good                 nats                those 
    ##                    1                    1                    1 
    ##                  wow     <U+2764><U+26BE> <f0><ff><U+0092><U+0083> 
    ##                    1                    1                    1 
    ## 
    ## $`725`
    ## <f0><ff><U+0098><U+00B4><f0><ff><U+0098><U+00B4> 
    ##                              1 
    ## 
    ## $`726`
    ## lifeofdess 
    ##          1 
    ## 
    ## $`727`
    ## fine<U+2764><U+FE0F> maugst8 squints 
    ##       1       1       1 
    ## 
    ## $`728`
    ##     1st   78178  became mention  people    seen 
    ##       1       1       1       1       1       1 
    ## 
    ## $`729`
    ##           days           made            rts         states thankyouaustin 
    ##              1              1              1              1              1 
    ##          topic 
    ##              1 
    ## 
    ## $`730`
    ##        twitter        android         client         iphone thankyouaustin 
    ##              3              1              1              1              1 
    ##       top3apps 
    ##              1 
    ## 
    ## $`731`
    ## httptcooiixekde6g           message            repair              this 
    ##                 1                 1                 1                 1 
    ##              time 
    ##                 1 
    ## 
    ## $`732`
    ##      sum1     think       cus      else insulting   insults 
    ##         2         2         1         1         1         1 
    ## 
    ## $`733`
    ##  aint freak   gas ready  road  side 
    ##     1     1     1     1     1     1 
    ## 
    ## $`734`
    ## <f0><ff><U+0098><U+0082>    httptco4fz7ryhrnb            mooreofme 
    ##                    4                    1                    1 
    ##             somebody            unfollows             wayneljr 
    ##                    1                    1                    1 
    ## 
    ## $`735`
    ##      because doseresponse      drawing     evidence         free         hand 
    ##            1            1            1            1            1            1 
    ## 
    ## $`736`
    ##                                                dream 
    ##                                                    1 
    ##                                                  its 
    ##                                                    1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0099><U+0088>cullentaylor 
    ##                                                    1 
    ## 
    ## $`737`
    ##       fuck irrelevant   starting    wizards 
    ##          1          1          1          1 
    ## 
    ## $`738`
    ##                     katie                      love sarah<f0><ff><U+0092><U+009A> 
    ##                         1                         1                         1 
    ## 
    ## $`739`
    ## lahhhollywood          love         nikki 
    ##             1             1             1 
    ## 
    ## $`740`
    ##   630am     amp another     day     got     job 
    ##       1       1       1       1       1       1 
    ## 
    ## $`741`
    ##  allison     died remember     teen     wolf 
    ##        1        1        1        1        1 
    ## 
    ## $`742`
    ## bitches    aint     and   basic     but    club 
    ##       2       1       1       1       1       1 
    ## 
    ## $`743`
    ## aint  got type 
    ##    1    1    1 
    ## 
    ## $`744`
    ##       back     deargo   dribbles fimustauri   happyill       send 
    ##          1          1          1          1          1          1 
    ## 
    ## $`745`
    ##  getting      god     help question    ready     root 
    ##        1        1        1        1        1        1 
    ## 
    ## $`746`
    ## elevators   existed    people    phones     smart    wonder 
    ##         1         1         1         1         1         1 
    ## 
    ## $`747`
    ##     chinese     feeling lightheaded    nauseous        soup       still 
    ##           1           1           1           1           1           1 
    ## 
    ## $`748`
    ##   fucking     idgaf      lets preseason   wizards 
    ##         1         1         1         1         1 
    ## 
    ## $`749`
    ##  babyy better    can   much  nikki    soo 
    ##      1      1      1      1      1      1 
    ## 
    ## $`750`
    ##                aww chickfilaisthebest  happylatebirthday          raykane13 
    ##                  1                  1                  1                  1 
    ##              yummy 
    ##                  1 
    ## 
    ## $`751`
    ##      attempt        draft        dream        going pennsocialdc        phone 
    ##            1            1            1            1            1            1 
    ## 
    ## $`752`
    ## everyone      you 
    ##        1        1 
    ## 
    ## $`753`
    ##                        cold                        hope 
    ##                           1                           1 
    ##                     outside tonight<f0><ff><U+0098><U+0092> 
    ##                           1                           1 
    ## 
    ## $`754`
    ## long  not  now 
    ##    1    1    1 
    ## 
    ## $`755`
    ##        mind    creating  meditation mindfulness     perfect      rather 
    ##           2           1           1           1           1           1 
    ## 
    ## $`756`
    ##       para cu<e3><U+00A1>ndo       esos  fortaleza    notiuno   preparar 
    ##          2          1          1          1          1          1 
    ## 
    ## $`757`
    ## fun<f0><ff><U+0092><U+0081><f0><ff><U+0098><U+009C> 
    ##                                           1 
    ##                                       gonna 
    ##                                           1 
    ##                                        this 
    ##                                           1 
    ##                                     weekend 
    ##                                           1 
    ## 
    ## $`758`
    ##                                                     birthday 
    ##                                                            1 
    ##                                                        happy 
    ##                                                            1 
    ##                                                 lovekisses18 
    ##                                                            1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+009E><U+0089><f0><ff><U+009E><U+0081> 
    ##                                                            1 
    ## 
    ## $`759`
    ##   actually   baseball    believe      hoops    matters mikememoli 
    ##          1          1          1          1          1          1 
    ## 
    ## $`760`
    ## bridges burning    long     mad     ppl     run 
    ##       1       1       1       1       1       1 
    ## 
    ## $`761`
    ##   boythatsmirah           phone       sometimes           texts           threw 
    ##               1               1               1               1               1 
    ## <f0><ff><U+0098><U+00A9> 
    ##               1 
    ## 
    ## $`762`
    ## buckhantz   excited       for     steve 
    ##         1         1         1         1 
    ## 
    ## $`763`
    ##     cause clamyetti      fire      fuck   jwalt96   moltres 
    ##         1         1         1         1         1         1 
    ## 
    ## $`764`
    ##     its  monday    need     now weekend 
    ##       1       1       1       1       1 
    ## 
    ## $`765`
    ##      baker motivation       surf  treadmill     twiggy   watching 
    ##          1          1          1          1          1          1 
    ## 
    ## $`766`
    ##                                     bipolar 
    ##                                           1 
    ##                                         hes 
    ##                                           1 
    ## lls<f0><ff><U+0098><U+0082><f0><ff><U+0099><U+0088> 
    ##                                           1 
    ##                                        sooo 
    ##                                           1 
    ## 
    ## $`767`
    ##           chicken              fear httptcodnrf8ryirq           urchkin 
    ##                 1                 1                 1                 1 
    ##         willstick 
    ##                 1 
    ## 
    ## $`768`
    ## beautiful<f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+00A9> 
    ##                                                      1 
    ##                                                    boy 
    ##                                                      1 
    ##                                                    fav 
    ##                                                      1 
    ##                                                    omg 
    ##                                                      1 
    ##                                                    one 
    ##                                                      1 
    ##                                                singing 
    ##                                                      1 
    ## 
    ## $`769`
    ##                  get                payed             tomorrow 
    ##                    1                    1                    1 
    ## <f0><ff><U+0092><U+0081> 
    ##                    1 
    ## 
    ## $`770`
    ##     congressional              gets             golf… httptco6h0q5bahvh 
    ##                 2                 1                 1                 1 
    ##             never               old 
    ##                 1                 1 
    ## 
    ## $`771`
    ##         franfisto      ibackthenats nothingbutoctober               san 
    ##                 1                 1                 1                 1 
    ##             treat 
    ##                 1 
    ## 
    ## $`772`
    ##     game    alive baseball     best     ever      get 
    ##        2        1        1        1        1        1 
    ## 
    ## $`773`
    ##   get  just wanna 
    ##     1     1     1 
    ## 
    ## $`774`
    ##  barbarianlogic          gotham     interesting mckeevemichelle 
    ##               1               1               1               1 
    ## 
    ## $`775`
    ##   always   chicks    money moroccan     seem 
    ##        1        1        1        1        1 
    ## 
    ## $`776`
    ## attention      dont     dress       get    trashy 
    ##         1         1         1         1         1 
    ## 
    ## $`777`
    ## clothes   coven    want 
    ##       1       1       1 
    ## 
    ## $`778`
    ##           dat          hell          need          talk          text 
    ##             1             1             1             1             1 
    ## yothatslexxxo 
    ##             1 
    ## 
    ## $`779`
    ## boardcast   chenier      dude      hell      phil      team 
    ##         1         1         1         1         1         1 
    ## 
    ## $`780`
    ##         lt3        okay veravulture 
    ##           1           1           1 
    ## 
    ## $`781`
    ##              emma          faulkner            folger httptcohwqt38y2hb 
    ##                 1                 1                 1                 1 
    ##           library     litlifeislife 
    ##                 1                 1 
    ## 
    ## $`782`
    ## fucking goooooo    lets 
    ##       1       1       1 
    ## 
    ## $`783`
    ##             think              zeta             alpha httptcogf68vpronb 
    ##                 2                 2                 1                 1 
    ##              pink          supports 
    ##                 1                 1 
    ## 
    ## $`784`
    ## lowkeyyyyy   majinjii 
    ##          1          1 
    ## 
    ## $`785`
    ##    clothes       espn fatshaming      fully        fun      great 
    ##          1          1          1          1          1          1 
    ## 
    ## $`786`
    ## makaveliix        win       yall 
    ##          1          1          1 
    ## 
    ## $`787`
    ##      espn inspiring      love       mnf      nice     panel 
    ##         1         1         1         1         1         1 
    ## 
    ## $`788`
    ##                                    tired 
    ##                                        1 
    ## <f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092> 
    ##                                        1 
    ## 
    ## $`789`
    ##      doe     dont     even      get   sneeze squeezin 
    ##        1        1        1        1        1        1 
    ## 
    ## $`790`
    ##       alike eiramarreis       great  hahahahaaa       minds       think 
    ##           1           1           1           1           1           1 
    ## 
    ## $`791`
    ##           average          european httptco67ybfvrngp       interesting 
    ##                 1                 1                 1                 1 
    ##         languages     michelamasson 
    ##                 1                 1 
    ## 
    ## $`792`
    ## nothing    talk    wish   wrong 
    ##       1       1       1       1 
    ## 
    ## $`793`
    ##             can forgetsaboutyou            mean             til            wait 
    ##               1               1               1               1               1 
    ##             wcw 
    ##               1 
    ## 
    ## $`794`
    ##     welovegiirlzz               amp httptcouwqs3d4dmr               pic 
    ##                 2                 1                 1                 1 
    ##             quote           samisul 
    ##                 1                 1 
    ## 
    ## $`795`
    ##        fedexfield          football httptcob5dtq0ywjl            monday 
    ##                 1                 1                 1                 1 
    ##              nats             night 
    ##                 1                 1 
    ## 
    ## $`796`
    ##    birthday         boy coryescobar         lol       today         wyd 
    ##           1           1           1           1           1           1 
    ## 
    ## $`797`
    ##      100 attitude     fast     from     that     went 
    ##        1        1        1        1        1        1 
    ## 
    ## $`798`
    ##   big bills cause  face faces   fix 
    ##     1     1     1     1     1     1 
    ## 
    ## $`799`
    ##             alert httptco0yx2lhoflq httptcolcaeh6twol              more 
    ##                 1                 1                 1                 1 
    ##             trend            trends 
    ##                 1                 1 
    ## 
    ## $`800`
    ##             alert             curly httptcolcaeh6twol httptcovtw4odemak 
    ##                 1                 1                 1                 1 
    ##              more             trend 
    ##                 1                 1 
    ## 
    ## $`801`
    ##   goat justin rather    smh 
    ##      1      1      1      1 
    ## 
    ## $`802`
    ##             alert            gerald httptcodvdk5fhfvj httptcolcaeh6twol 
    ##                 1                 1                 1                 1 
    ##              more             trend 
    ##                 1                 1 
    ## 
    ## $`803`
    ##             alert              game httptcoc73ahgdwqv httptcolcaeh6twol 
    ##                 1                 1                 1                 1 
    ##              more             trend 
    ##                 1                 1 
    ## 
    ## $`804`
    ##  broadcast      bulls    comcast gunitradio      shown  sportsnet 
    ##          1          1          1          1          1          1 
    ## 
    ## $`805`
    ## bitches    love     all     bad    beat  better 
    ##       2       2       1       1       1       1 
    ## 
    ## $`806`
    ##             alert httptcolcaeh6twol httptcoud5b63svme              more 
    ##                 1                 1                 1                 1 
    ##    thankyouaustin             trend 
    ##                 1                 1 
    ## 
    ## $`807`
    ##     building     everyone        front        leave savageleek16         seen 
    ##            1            1            1            1            1            1 
    ## 
    ## $`808`
    ## feel hell like 
    ##    1    1    1 
    ## 
    ## $`809`
    ##  joinin     lil sisters    stay 
    ##       1       1       1       1 
    ## 
    ## $`810`
    ## bitch   day  dude   leg today 
    ##     1     1     1     1     1 
    ## 
    ## $`811`
    ##   despite       gas impending      love      much 
    ##         1         1         1         1         1 
    ## 
    ## $`812`
    ##     eenytsed embarrassing         just       loving        never         show 
    ##            1            1            1            1            1            1 
    ## 
    ## $`813`
    ## concerned       fix    future   instead     meant     never 
    ##         1         1         1         1         1         1 
    ## 
    ## $`814`
    ## ayoookaee      heyy 
    ##         1         1 
    ## 
    ## $`815`
    ## cute  lol shes 
    ##    1    1    1 
    ## 
    ## $`816`
    ##         peer ekeeleymoore         gfry   healthcare   kzcallihan         like 
    ##            2            1            1            1            1            1 
    ## 
    ## $`817`
    ## bigbangtheory       haircut    horrendous        pennys 
    ##             1             1             1             1 
    ## 
    ## $`818`
    ##       day       ive      just wonderful 
    ##         1         1         1         1 
    ## 
    ## $`819`
    ## bae<f0><ff><U+0092><U+0094>       httptcommykdniowe                    miss 
    ##                       1                       1                       1 
    ## 
    ## $`820`
    ##   bus  fuck nigga  they 
    ##     1     1     1     1 
    ## 
    ## $`821`
    ##    lose  pounds product  sample    send   tyrna 
    ##       1       1       1       1       1       1 
    ## 
    ## $`822`
    ## assholes    cards    green     need   puerto   ricans 
    ##        1        1        1        1        1        1 
    ## 
    ## $`823`
    ##    alone annoying   enough  getting   guilty     hope 
    ##        1        1        1        1        1        1 
    ## 
    ## $`824`
    ##     drive elizabeth       ems       gas       ill       lol 
    ##         1         1         1         1         1         1 
    ## 
    ## $`825`
    ## comes  game  here 
    ##     1     1     1 
    ## 
    ## $`826`
    ##    buddy   desean england”     good      hes      new 
    ##        1        1        1        1        1        1 
    ## 
    ## $`827`
    ##        girl         ive        just laurentayyy     monster       never 
    ##           1           1           1           1           1           1 
    ## 
    ## $`828`
    ## harpershomeboyz 
    ##               1 
    ## 
    ## $`829`
    ##          heck bythekil0watt          cute        taylor 
    ##             2             1             1             1 
    ## 
    ## $`830`
    ##                                           bad 
    ##                                             1 
    ##                                     irritated 
    ##                                             1 
    ##                                          real 
    ##                                             1 
    ## <f0><ff><U+0098><U+00AB><f0><ff><U+0098><U+00AB><f0><ff><U+0091><U+00BF> 
    ##                                             1 
    ## 
    ## $`831`
    ##          chills          giving             guy            this           voice 
    ##               1               1               1               1               1 
    ## <f0><ff><U+0098><U+00A9> 
    ##               1 
    ## 
    ## $`832`
    ## bulletsforever            can          court           home         jersey 
    ##              1              1              1              1              1 
    ##           logo 
    ##              1 
    ## 
    ## $`833`
    ##                          blows                       graphing 
    ##                              1                              1 
    ##                           math <f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+00A1> 
    ##                              1                              1 
    ## 
    ## $`834`
    ##  bsweeneydc       howie metsbullpen   reference        rose      tweets 
    ##           1           1           1           1           1           1 
    ## 
    ## $`835`
    ##     aint arranged   family    lahhh    least marriage 
    ##        1        1        1        1        1        1 
    ## 
    ## $`836`
    ##   amazing      been centuries     flaws listening     night 
    ##         1         1         1         1         1         1 
    ## 
    ## $`837`
    ##                                         girl 
    ##                                            1 
    ## girl<f0><ff><U+0092><U+0095><f0><ff><U+0092><U+0095> 
    ##                                            1 
    ##                            httptcojqqlc0tr1j 
    ##                                            1 
    ##                                          mcm 
    ##                                            1 
    ## 
    ## $`838`
    ## belleboogiee          lol 
    ##            1            1 
    ## 
    ## $`839`
    ##                                     5sos 
    ##                                        1 
    ##                                   record 
    ##                                        1 
    ##                                superhero 
    ##                                        1 
    ##                                      you 
    ##                                        1 
    ## <f0><ff><U+0098><U+008F><f0><ff><U+0098><U+008F> 
    ##                                        1 
    ## 
    ## $`840`
    ##        can crystalcpr foundation   jtthekid        pls      thank 
    ##          1          1          1          1          1          1 
    ## 
    ## $`841`
    ##            cant          chilis           drive             get <f0><ff><U+0098><U+00A9> 
    ##               1               1               1               1               1 
    ## 
    ## $`842`
    ##   wit  just quiet  ride 
    ##     2     1     1     1 
    ## 
    ## $`843`
    ##    fall     day falling   looks    love   model 
    ##       2       1       1       1       1       1 
    ## 
    ## $`844`
    ##       anything           else       everyone fredsavagefan1        getting 
    ##              1              1              1              1              1 
    ##           gosh 
    ##              1 
    ## 
    ## $`845`
    ## hahahahahah   roromusso 
    ##           1           1 
    ## 
    ## $`846`
    ##        taeebandzz     birdfreestyle httptcokdlcrrfgds            listen 
    ##                 2                 1                 1                 1 
    ##             music               new 
    ##                 1                 1 
    ## 
    ## $`847`
    ##                           talk <f0><ff><U+009A><U+00A8><f0><ff><U+009A><U+00A8> 
    ##                              1                              1 
    ## 
    ## $`848`
    ##                                        sammlite 
    ##                                               1 
    ## welcome<f0><ff><U+0098><U+009A><f0><ff><U+0092><U+0081> 
    ##                                               1 
    ## 
    ## $`849`
    ## bythekil0watt          heck          said         twice           wow 
    ##             1             1             1             1             1 
    ## 
    ## $`850`
    ##              doctors                 moms              staying 
    ##                    1                    1                    1 
    ##             tomorrow              tonight <f0><ff><U+0098><U+0092> 
    ##                    1                    1                    1 
    ## 
    ## $`851`
    ##        almonds pareesabuerger 
    ##              1              1 
    ## 
    ## $`852`
    ##   audreymcclellan biggestbabyshower             great         safety1st 
    ##                 1                 1                 1                 1 
    ##               see 
    ##                 1 
    ## 
    ## $`853`
    ##          backside            beauty           djzeeti   lolo2irrelevant 
    ##                 1                 1                 1                 1 
    ##            lovely mentionperfection 
    ##                 1                 1 
    ## 
    ## $`854`
    ## everybody   getting    nerves <U+203C><U+FE0F> 
    ##         1         1         1         1 
    ## 
    ## $`855`
    ## everything        lot   maturity 
    ##          1          1          1 
    ## 
    ## $`856`
    ##             album              bout httptcoczfkx0x2tk            khaled 
    ##                 1                 1                 1                 1 
    ##               man              next 
    ##                 1                 1 
    ## 
    ## $`857`
    ##                                   naedyy 
    ##                                        1 
    ##                                    thank 
    ##                                        1 
    ##                                    youuu 
    ##                                        1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+0098> 
    ##                                        1 
    ## <f0><ff><U+0098><U+009A><f0><ff><U+0098><U+009A> 
    ##                                        1 
    ## 
    ## $`858`
    ##        better eatmoreesskay          game           get          much 
    ##             1             1             1             1             1 
    ##       package 
    ##             1 
    ## 
    ## $`859`
    ##    gunsight        hood   karenfink       makes melanierizk    ornament 
    ##           1           1           1           1           1           1 
    ## 
    ## $`860`
    ##  aint  ayee cubes   got  like  milo 
    ##     1     1     1     1     1     1 
    ## 
    ## $`861`
    ##     sorry     cough  everyone    school   serious something 
    ##         2         1         1         1         1         1 
    ## 
    ## $`862`
    ##  dont panic 
    ##     1     1 
    ## 
    ## $`863`
    ##                                                                                                             abstractsoui 
    ##                                                                                                                        1 
    ##                                                                                                                 clitoria 
    ##                                                                                                                        1 
    ##                                                                                                                 daughter 
    ##                                                                                                                        1 
    ##                                                                                                                     name 
    ##                                                                                                                        1 
    ##                                                                                                                     will 
    ##                                                                                                                        1 
    ## <f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                                                                                        1 
    ## 
    ## $`864`
    ##  media social 2cents   feel   like people 
    ##      2      2      1      1      1      1 
    ## 
    ## $`865`
    ##     beautiful georgiaspeach         hello 
    ##             1             1             1 
    ## 
    ## $`866`
    ## everything      means    nothing      thats 
    ##          1          1          1          1 
    ## 
    ## $`867`
    ## named numeric(0)
    ## 
    ## $`868`
    ## audreyslade        four fsbaltimore fsfoodtruck     fstaste         fun 
    ##           1           1           1           1           1           1 
    ## 
    ## $`869`
    ##     easy esssbeee      fun     gets     hard   really 
    ##        1        1        1        1        1        1 
    ## 
    ## $`870`
    ##        ass       deal everywhere       fake   happened       lhhh 
    ##          1          1          1          1          1          1 
    ## 
    ## $`871`
    ##    believe    brought     gotham        ive jinxedjo15       just 
    ##          1          1          1          1          1          1 
    ## 
    ## $`872`
    ## additional       even      great        may  sacrifice  schuh2014 
    ##          1          1          1          1          1          1 
    ## 
    ## $`873`
    ##                                                       caused 
    ##                                                            1 
    ##                                                      eatting 
    ##                                                            1 
    ##                                                        ebola 
    ##                                                            1 
    ##                                                       gluten 
    ##                                                            1 
    ##                                                        guess 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                            1 
    ## 
    ## $`874`
    ## examen    con  hasta  hogar    mas    mis 
    ##      2      1      1      1      1      1 
    ## 
    ## $`875`
    ##                                    wilin 
    ##                                        1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                        1 
    ## 
    ## $`876`
    ##       need        see starssssss  telescope        you 
    ##          1          1          1          1          1 
    ## 
    ## $`877`
    ##                                                                       factor 
    ##                                                                            1 
    ##                                                                          the 
    ##                                                                            1 
    ##                                                                   ukintheusa 
    ##                                                                            1 
    ##                                                                     watching 
    ##                                                                            1 
    ## <f0><ff><U+0099><U+008F><f0><ff><U+0091><U+008F><f0><ff><U+0099><U+009C><f0><ff><U+0098><U+00B1><U+2764><U+FE0F> 
    ##                                                                            1 
    ## 
    ## $`878`
    ##                                       anngie 
    ##                                            1 
    ## bruh<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                            1 
    ##                                        going 
    ##                                            1 
    ##                                         hell 
    ##                                            1 
    ## 
    ## $`879`
    ## volume  alarm   dumb    ios ringer that’s 
    ##      2      1      1      1      1      1 
    ## 
    ## $`880`
    ## talk <U+2693><U+FE0F> 
    ##    1    1 
    ## 
    ## $`881`
    ##            ayoookaee <f0><ff><U+0098><U+0094> 
    ##                    1                    1 
    ## 
    ## $`882`
    ## foodiechats    tweeting 
    ##           1           1 
    ## 
    ## $`883`
    ## another blowout    game    late   night 
    ##       2       1       1       1       1 
    ## 
    ## $`884`
    ##    lol    may  shock t3rr3z 
    ##      1      1      1      1 
    ## 
    ## $`885`
    ##  good looks nikki 
    ##     1     1     1 
    ## 
    ## $`886`
    ##          cali         drums         first jordanstewart         kevin 
    ##             1             1             1             1             1 
    ##        ogezra 
    ##             1 
    ## 
    ## $`887`
    ##     dad    care  family friends     god    lost 
    ##       2       1       1       1       1       1 
    ## 
    ## $`888`
    ##   better  dollars hayfield     lose      not   paying 
    ##        1        1        1        1        1        1 
    ## 
    ## $`889`
    ##  getting      god involved     kane    orton   please 
    ##        1        1        1        1        1        1 
    ## 
    ## $`890`
    ##                dude           hilarious httpstcoedc9moespa”                late 
    ##                   1                   1                   1                   1 
    ##                 lol              popeye 
    ##                   1                   1 
    ## 
    ## $`891`
    ##     bill  getting     ipod     just      lls messages 
    ##        1        1        1        1        1        1 
    ## 
    ## $`892`
    ##    like    days feeling    fuck    just   ready 
    ##       2       1       1       1       1       1 
    ## 
    ## $`893`
    ##                                         about 
    ##                                             1 
    ##                                          care 
    ##                                             1 
    ## nigga<f0><ff><U+0092><U+008D><f0><ff><U+0098><U+008D> 
    ##                                             1 
    ##                                           one 
    ##                                             1 
    ##                                          only 
    ##                                             1 
    ## 
    ## $`894`
    ##    came  clutch jermari 
    ##       1       1       1 
    ## 
    ## $`895`
    ##     air almost…  autumn caramel    cool  fiddle 
    ##       1       1       1       1       1       1 
    ## 
    ## $`896`
    ##                                                                                     <U+2049><U+FE0F> 
    ##                                                                                                    1 
    ##                                                                             <f0><ff><U+0090><U+0089> 
    ##                                                                                                    1 
    ##                                                                             <f0><ff><U+0090><U+0092> 
    ##                                                                                                    1 
    ##                                                                             <f0><ff><U+0090><U+0098> 
    ##                                                                                                    1 
    ##                                                                             <f0><ff><U+0091><U+00B8> 
    ##                                                                                                    1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D> 
    ##                                                                                                    1 
    ## 
    ## $`897`
    ##  httr  just watch 
    ##     1     1     1 
    ## 
    ## $`898`
    ## adrenaline    awkward       body  centuries      couch       hype 
    ##          1          1          1          1          1          1 
    ## 
    ## $`899`
    ##      head ooooooooh    waking 
    ##         1         1         1 
    ## 
    ## $`900`
    ##               ais annapolisjunction         developer          dynamics 
    ##                 1                 1                 1                 1 
    ##           general httptco5tbzcpfnta 
    ##                 1                 1 
    ## 
    ## $`901`
    ##     are chapped    lips 
    ##       1       1       1 
    ## 
    ## $`902`
    ##               battle               friday             sections 
    ##                    1                    1                    1 
    ##              student <f0><ff><U+0099><U+009C> 
    ##                    1                    1 
    ## 
    ## $`903`
    ##  uglier   every looking quarter samsung 
    ##       2       1       1       1       1 
    ## 
    ## $`904`
    ##  better deserve    dime   lahhh   mally   nikki 
    ##       1       1       1       1       1       1 
    ## 
    ## $`905`
    ## witcha   eyes  hands    see 
    ##      2      1      1      1 
    ## 
    ## $`906`
    ## downnnngrade<U+203C><U+FE0F><U+203C><U+FE0F><U+203C><U+FE0F><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+00AD> 
    ##                                                  1 
    ##                                           flasshya 
    ##                                                  1 
    ## 
    ## $`907`
    ##             bowie           advisor            beauty         cosmetics 
    ##                 3                 1                 1                 1 
    ## httptcopw2pxne3zy               job 
    ##                 1                 1 
    ## 
    ## $`908`
    ## jahciknick 
    ##          1 
    ## 
    ## $`909`
    ##          say         aint        bitch          den         gone lhhhollywood 
    ##            2            1            1            1            1            1 
    ## 
    ## $`910`
    ## crime<f0><ff><U+0091><U+00AD><f0><ff><U+0092><U+0097><f0><ff><U+0098><U+00BB> 
    ##                                                       1 
    ##                                       httptcoxna6msxsf6 
    ##                                                       1 
    ##                                                     mcm 
    ##                                                       1 
    ##                                                 partner 
    ##                                                       1 
    ##                                                  twinny 
    ##                                                       1 
    ## 
    ## $`911`
    ##                                                                                boots 
    ##                                                                                    1 
    ##                                                                               combat 
    ##                                                                                    1 
    ## guys<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                                                    1 
    ##                                                                              looking 
    ##                                                                                    1 
    ##                                                                                  ppl 
    ##                                                                                    1 
    ##                                                                                 shit 
    ##                                                                                    1 
    ## 
    ## $`912`
    ## wizards   bulls    lets 
    ##       2       1       1 
    ## 
    ## $`913`
    ##       get      hype       idk sometimes 
    ##         1         1         1         1 
    ## 
    ## $`914`
    ##   lol nip78 
    ##     1     1 
    ## 
    ## $`915`
    ##           birthday         fourchette              happy httpstcoewgdjmffz8 
    ##                  1                  1                  1                  1 
    ##            nyemadi         washington 
    ##                  1                  1 
    ## 
    ## $`916`
    ## biggestbabyshower              come          congrats              many 
    ##                 1                 1                 1                 1 
    ##         safety1st             years 
    ##                 1                 1 
    ## 
    ## $`917`
    ##        act especially      ivaxa     single      wanna 
    ##          1          1          1          1          1 
    ## 
    ## $`918`
    ##      ham   justin   rather sandwich      smh 
    ##        1        1        1        1        1 
    ## 
    ## $`919`
    ##   her  like  lmao  look   mom oreo” 
    ##     1     1     1     1     1     1 
    ## 
    ## $`920`
    ## beyonc<e3><U+00A9>        dead        like        sing       sound       thats 
    ##           1           1           1           1           1           1 
    ## 
    ## $`921`
    ##           autumnhoes <f0><ff><U+0099><U+0087> 
    ##                    1                    1 
    ## 
    ## $`922`
    ##   bulls wizards 
    ##       1       1 
    ## 
    ## $`923`
    ## httptconryqdhkelz              need               now          ohmygoff 
    ##                 1                 1                 1                 1 
    ##          redskins               win 
    ##                 1                 1 
    ## 
    ## $`924`
    ##                                       but 
    ##                                         1 
    ##                                 messaging 
    ##                                         1 
    ##                                     still 
    ##                                         1 
    ##                                      time 
    ##                                         1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><U+271C><U+FE0F> 
    ##                                         1 
    ## 
    ## $`925`
    ##     distributes           going lindseyschnabel            look             not 
    ##               1               1               1               1               1 
    ##            sure 
    ##               1 
    ## 
    ## $`926`
    ## anywhere<f0><ff><U+0092><U+008D><f0><ff><U+0092><U+0091> 
    ##                                                1 
    ##                                             cant 
    ##                                                1 
    ##                                              him 
    ##                                                1 
    ##                                              see 
    ##                                                1 
    ## 
    ## $`927`
    ##                           idek <f0><ff><U+009E><U+00AD><f0><ff><U+009E><U+00AD> 
    ##                              1                              1 
    ## 
    ## $`928`
    ##            attractive                  cute               djzeeti 
    ##                     1                     1                     1 
    ## mentionsomeonepopular            mixedtingg                   pic 
    ##                     1                     1                     1 
    ## 
    ## $`929`
    ##         afternoon              food           friends              girl 
    ##                 1                 1                 1                 1 
    ##         goodtimes httptcouztcrtc3jb 
    ##                 1                 1 
    ## 
    ## $`930`
    ##     name clitoria daughter   entire     just    laugh 
    ##        2        1        1        1        1        1 
    ## 
    ## $`931`
    ##   beach  desean     hve jackson   known  lalong 
    ##       1       1       1       1       1       1 
    ## 
    ## $`932`
    ##                                   months 
    ##                                        1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0092><U+008D> 
    ##                                        1 
    ## 
    ## $`933`
    ##   black  closet    love     see wearing 
    ##       1       1       1       1       1 
    ## 
    ## $`934`
    ##         can comfortable        like      mother       never     talking 
    ##           1           1           1           1           1           1 
    ## 
    ## $`935`
    ##  around     dat    less     moe smoking <U+26FD><U+26FD> 
    ##       1       1       1       1       1       1 
    ## 
    ## $`936`
    ##          5sos 5sosgoodgirls  adorableness       consist       feeling 
    ##             1             1             1             1             1 
    ##         gonna 
    ##             1 
    ## 
    ## $`937`
    ##  call phone 
    ##     1     1 
    ## 
    ## $`938`
    ##    seattle      upset washington       will 
    ##          1          1          1          1 
    ## 
    ## $`939`
    ## nats  win 
    ##    1    1 
    ## 
    ## $`940`
    ##         aww caitblanche 
    ##           1           1 
    ## 
    ## $`941`
    ##                                                          lrt 
    ##                                                            1 
    ##                                                   raiabadass 
    ##                                                            1 
    ##                                                      tuuucky 
    ##                                                            1 
    ##                                                         yall 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                            1 
    ## 
    ## $`942`
    ## tf<f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0091> 
    ##                                          1 
    ## 
    ## $`943`
    ##     fanatic haleyfrasca    mcnugget         say   surprised 
    ##           1           1           1           1           1 
    ## 
    ## $`944`
    ## named numeric(0)
    ## 
    ## $`945`
    ##      dad    found  freaked listened     lmao    oomfs 
    ##        1        1        1        1        1        1 
    ## 
    ## $`946`
    ## doe<e2><U+00BF> thenamesolive 
    ##             1             1 
    ## 
    ## $`947`
    ##                bar httpstcofab1kzanjc              pilar         washington 
    ##                  1                  1                  1                  1 
    ## 
    ## $`948`
    ##              bout         everybody httptcodyllh7nm5q            niggas 
    ##                 1                 1                 1                 1 
    ##            talkin          <U+271A> 
    ##                 1                 1 
    ## 
    ## $`949`
    ## calculatorre          one         will 
    ##            1            1            1 
    ## 
    ## $`950`
    ##           2005         albums    davejglaser glaserhallfull          going 
    ##              1              1              1              1              1 
    ##          least 
    ##              1 
    ## 
    ## $`951`
    ##            2600 arbeiaromanfort             bce            diff hadrianswallqst 
    ##               1               1               1               1               1 
    ##       headgears 
    ##               1 
    ## 
    ## $`952`
    ##  lol sike 
    ##    1    1 
    ## 
    ## $`953`
    ##     drivers   karenfink        mean melanierizk    virginia   zoyaroses 
    ##           1           1           1           1           1           1 
    ## 
    ## $`954`
    ## butler  gonna  jimmy stupid   year 
    ##      1      1      1      1      1 
    ## 
    ## $`955`
    ## 5sosgoodgirls          hear          live          omfg        passes 
    ##             1             1             1             1             1 
    ##          poor 
    ##             1 
    ## 
    ## $`956`
    ##     carlyepagano carlyetotacobell 
    ##                1                1 
    ## 
    ## $`957`
    ## carried<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+009C> 
    ##                                                                                       1 
    ##                                                                                 excited 
    ##                                                                                       1 
    ##                                                                                     get 
    ##                                                                                       1 
    ##                                                                                   hazel 
    ##                                                                                       1 
    ##                                                                                     see 
    ##                                                                                       1 
    ## 
    ## $`958`
    ##               yassss                bitch             erikaaxo 
    ##                    2                    1                    1 
    ## <f0><ff><U+0098><U+0098> 
    ##                    1 
    ## 
    ## $`959`
    ## cmon  man 
    ##    1    1 
    ## 
    ## $`960`
    ##             die           fruit             now           right           would 
    ##               1               1               1               1               1 
    ## <f0><ff><U+0098><U+00AB> 
    ##               1 
    ## 
    ## $`961`
    ##         get      little        love      nerves      sister “aniyal8ter 
    ##           1           1           1           1           1           1 
    ## 
    ## $`962`
    ## berg cant with yung 
    ##    1    1    1    1 
    ## 
    ## $`963`
    ##  really thought trahara <U+2757><U+FE0F> 
    ##       1       1       1       1 
    ## 
    ## $`964`
    ##                     are asf<f0><ff><U+0098><U+0092>                   extra 
    ##                       1                       1                       1 
    ##                  lamphh                   these 
    ##                       1                       1 
    ## 
    ## $`965`
    ##  alive gonats stayin 
    ##      1      1      1 
    ## 
    ## $`966`
    ##     raw   couch   going  happen sitting    wait 
    ##       2       1       1       1       1       1 
    ## 
    ## $`967`
    ## derrick    rose 
    ##       1       1 
    ## 
    ## $`968`
    ##    lake    eels germany   great   gross growing 
    ##       2       1       1       1       1       1 
    ## 
    ## $`969`
    ##  connor564 everything      ruins       that    wowwwww 
    ##          1          1          1          1          1 
    ## 
    ## $`970`
    ##                                     mans 
    ##                                        1 
    ##                 <f0><ff><U+0092><U+00A0> 
    ##                                        1 
    ## <f0><ff><U+0098><U+0088><f0><ff><U+0092><U+009E> 
    ##                                        1 
    ## 
    ## $`971`
    ##      bum dhberman      hes 
    ##        1        1        1 
    ## 
    ## $`972`
    ##           come           httr           just         looked       redskins 
    ##              1              1              1              1              1 
    ## redskinsnation 
    ##              1 
    ## 
    ## $`973`
    ##     easily  extremely frustrated        get        low       math 
    ##          1          1          1          1          1          1 
    ## 
    ## $`974`
    ##     cant  outlets saturday      the     till     wait 
    ##        1        1        1        1        1        1 
    ## 
    ## $`975`
    ##          escuchar            animar              como          deseando 
    ##                 2                 1                 1                 1 
    ## encanta<e2><U+00A1>estoy             lunes 
    ##                 1                 1 
    ## 
    ## $`976`
    ##    can  marry matter  plant    sex  smoke 
    ##      1      1      1      1      1      1 
    ## 
    ## $`977`
    ## header<f0><ff><U+0098><U+008B>                      wants 
    ##                          1                          1 
    ## 
    ## $`978`
    ## ebnoxi0us 
    ##         1 
    ## 
    ## $`979`
    ##                               hazel                                omfg 
    ##                                   1                                   1 
    ##                                shit                                ugly 
    ##                                   1                                   1 
    ## <f0><ff><U+0092><U+00A9><f0><ff><U+0092><U+0080> 
    ##                                   1 
    ## 
    ## $`980`
    ##      changed jimmytooreal        twice 
    ##            1            1            1 
    ## 
    ## $`981`
    ##    httptcovze7a77oxb <f0><ff><U+0092><U+009A> 
    ##                    1                    1 
    ## 
    ## $`982`
    ##     alone beautiful      ever    female      said      seen 
    ##         1         1         1         1         1         1 
    ## 
    ## $`983`
    ##                        attractive                              cute 
    ##                                 1                                 1 
    ##                           djzeeti mentionpeoplethatyouloveontwitter 
    ##                                 1                                 1 
    ##                               pic                       teamvasquez 
    ##                                 1                                 1 
    ## 
    ## $`984`
    ##    anytime       days       dear        lol       mine missmichy1 
    ##          1          1          1          1          1          1 
    ## 
    ## $`985`
    ## jwalt96     see 
    ##       1       1 
    ## 
    ## $`986`
    ##         always annieatanner15          belts          birth        control 
    ##              1              1              1              1              1 
    ##          found 
    ##              1 
    ## 
    ## $`987`
    ## around   baby   even   face  gonna    its 
    ##      1      1      1      1      1      1 
    ## 
    ## $`988`
    ## baseball    field     home    least    major   matter 
    ##        1        1        1        1        1        1 
    ## 
    ## $`989`
    ##     all balance     out  trynna 
    ##       1       1       1       1 
    ## 
    ## $`990`
    ## bulletsforever           phil          steve 
    ##              1              1              1 
    ## 
    ## $`991`
    ##                                                                    danielsharman 
    ##                                                                                1 
    ##                                                                             holy 
    ##                                                                                1 
    ##                                                                        originals 
    ##                                                                                1 
    ##                                                                             shit 
    ##                                                                                1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D> 
    ##                                                                                1 
    ## 
    ## $`992`
    ##     cant contacts      mnf      see     wish     wore 
    ##        1        1        1        1        1        1 
    ## 
    ## $`993`
    ##      dabulls         funk         neil         rock       seered stacey21king 
    ##            1            1            1            1            1            1 
    ## 
    ## $`994`
    ## apartment   country      free     guess       hey    horses 
    ##         1         1         1         1         1         1 
    ## 
    ## $`995`
    ##      get     shit straight virginia      yoo 
    ##        1        1        1        1        1 
    ## 
    ## $`996`
    ##            burnie            church             gbumc              glen 
    ##                 1                 1                 1                 1 
    ## httptcofhlzsqkc2s         methodist 
    ##                 1                 1 
    ## 
    ## $`997`
    ##          chauffeur httptcodc50j81gqz”             meeeee        “leeahprint 
    ##                  1                  1                  1                  1 
    ##   <U+2764><U+FE0F> 
    ##                  1 
    ## 
    ## $`998`
    ##    hate    keep     ppl talking 
    ##       1       1       1       1 
    ## 
    ## $`999`
    ##                          cant                         drive 
    ##                             1                             1 
    ##                           til                         truck 
    ##                             1                             1 
    ## wednesday<f0><ff><U+0098><U+0092> 
    ##                             1 
    ## 
    ## $`1000`
    ##                 girla<f0><ff><U+0098><U+0092> 
    ##                                             1 
    ## homecoming<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+00A9> 
    ##                                             1 
    ##                                           lol 
    ##                                             1 
    ##                                       tonight 
    ##                                             1 
    ## 
    ## $`1001`
    ##          ambiente autoriz<e3><U+00B3>           auxilio constituci<e3><U+00B3>n 
    ##                 1                 1                 1                 1 
    ## httptcoesiqu2qcx0           maracay 
    ##                 1                 1 
    ## 
    ## $`1002`
    ##              casa httptcoysvkzjugpm            kurrus     mollythedoxie 
    ##                 1                 1                 1                 1 
    ##           snoozer 
    ##                 1 
    ## 
    ## $`1003`
    ##            bethany               blue              grill httpstco8y0odzubgz 
    ##                  1                  1                  1                  1 
    ##               httr           leesburg 
    ##                  1                  1 
    ## 
    ## $`1004`
    ##       bigcitymoms biggestbabyshower              come      thedealmatch 
    ##                 1                 1                 1                 1 
    ##          virginia              west 
    ##                 1                 1 
    ## 
    ## $`1005`
    ##     <U+2049><U+FE0F> <f0><ff><U+0090><U+0089> <f0><ff><U+0090><U+0092> 
    ##                    1                    1                    1 
    ## <f0><ff><U+0090><U+0098> <f0><ff><U+0091><U+00B8> 
    ##                    1                    1 
    ## 
    ## $`1006`
    ##    can   dude   even   mids  smell smokin 
    ##      1      1      1      1      1      1 
    ## 
    ## $`1007`
    ##    fister nationals    please    resign 
    ##         1         1         1         1 
    ## 
    ## $`1008`
    ## hours 
    ##     1 
    ## 
    ## $`1009`
    ##   aint    bad  bitch change   even   life 
    ##      1      1      1      1      1      1 
    ## 
    ## $`1010`
    ##          database     administrator          clinical         frederick 
    ##                 2                 1                 1                 1 
    ## httptco4qfflqv0x3               job 
    ##                 1                 1 
    ## 
    ## $`1011`
    ## airiel2    know    nice 
    ##       1       1       1 
    ## 
    ## $`1012`
    ##         hazel lahhhollywood          ugly 
    ##             1             1             1 
    ## 
    ## $`1013`
    ##            curley           flooded httptcogazqlpxbvq            iphone 
    ##                 1                 1                 1                 1 
    ##            opened           request 
    ##                 1                 1 
    ## 
    ## $`1014`
    ## narcisisticego          never        stopped 
    ##              1              1              1 
    ## 
    ## $`1015`
    ##        able        gets       he’ll joelhousman       phone        pics 
    ##           1           1           1           1           1           1 
    ## 
    ## $`1016`
    ## irritated 
    ##         1 
    ## 
    ## $`1017`
    ## arbeiaromanfort hadrianswallqst             how lovearchaeology             old 
    ##               1               1               1               1               1 
    ##           stone 
    ##               1 
    ## 
    ## $`1018`
    ##           def          game kingkennethii           lol      redskins 
    ##             1             1             1             1             1 
    ##      watching 
    ##             1 
    ## 
    ## $`1019`
    ##    back derrick    rose 
    ##       1       1       1 
    ## 
    ## $`1020`
    ##           facts    uniquewoodie <f0><ff><U+0092><U+00AF> 
    ##               1               1               1 
    ## 
    ## $`1021`
    ##      classic        dance          end       lmaooo mrdcsportssr        video 
    ##            1            1            1            1            1            1 
    ## 
    ## $`1022`
    ## flasshya      new 
    ##        1        1 
    ## 
    ## $`1023`
    ## cheese justin  piece rather    smh 
    ##      1      1      1      1      1 
    ## 
    ## $`1024`
    ##    get hahaha   hair   long people shower 
    ##      1      1      1      1      1      1 
    ## 
    ## $`1025`
    ##            attractive               djzeeti mentionsomeonepopular 
    ##                     1                     1                     1 
    ##         mikeytwilight                   pic            teamlauren 
    ##                     1                     1                     1 
    ## 
    ## $`1026`
    ## ass”crying       berg     hazele   lmaooooo       nose        see 
    ##          1          1          1          1          1          1 
    ## 
    ## $`1027`
    ##           faves             get heartheartheart             lol            mike 
    ##               1               1               1               1               1 
    ##        retweets 
    ##               1 
    ## 
    ## $`1028`
    ## blame 
    ##     1 
    ## 
    ## $`1029`
    ##        debate14           early electgeorge2014         funding   georgearlotto 
    ##               1               1               1               1               1 
    ##             get 
    ##               1 
    ## 
    ## $`1030`
    ##  httr  time upset 
    ##     1     1     1 
    ## 
    ## $`1031`
    ##  crucible      drop glpodcast       got    random 
    ##         1         1         1         1         1 
    ## 
    ## $`1032`
    ##       early     evening        goes hotlinejosh       lastl   otherwise 
    ##           1           1           1           1           1           1 
    ## 
    ## $`1033`
    ##     derrick jessirose14        rose 
    ##           1           1           1 
    ## 
    ## $`1034`
    ##        feeess         payed serendippityy 
    ##             1             1             1 
    ## 
    ## $`1035`
    ##   karenfink melanierizk      people        take       taxis   zoyaroses 
    ##           1           1           1           1           1           1 
    ## 
    ## $`1036`
    ##   back  bball   bruh    pre season  siced 
    ##      1      1      1      1      1      1 
    ## 
    ## $`1037`
    ## followers       lob  somebody 
    ##         1         1         1 
    ## 
    ## $`1038`
    ## weirdo 
    ##      1 
    ## 
    ## $`1039`
    ##             chase httptcouh2sttrddp              like              look 
    ##                 1                 1                 1                 1 
    ##              mean          pictures 
    ##                 1                 1 
    ## 
    ## $`1040`
    ## fuck  you 
    ##    1    1 
    ## 
    ## $`1041`
    ## buddys  lolol   wild 
    ##      1      1      1 
    ## 
    ## $`1042`
    ##           stadium             empty              fedx             field 
    ##                 2                 1                 1                 1 
    ##          football httptcootbvrapyg3 
    ##                 1                 1 
    ## 
    ## $`1043`
    ##    scary seahawks 
    ##        1        1 
    ## 
    ## $`1044`
    ## hotlinejosh        nats        park    thursday 
    ##           1           1           1           1 
    ## 
    ## $`1045`
    ##  else wants   who 
    ##     1     1     1 
    ## 
    ## $`1046`
    ##               art            framed               got httptcocgnx1df7rm 
    ##                 1                 1                 1                 1 
    ##            mexico          postcard 
    ##                 1                 1 
    ## 
    ## $`1047`
    ##         and colindickey      debcha      jzimms        love     ostrich 
    ##           1           1           1           1           1           1 
    ## 
    ## $`1048`
    ## wtf 
    ##   1 
    ## 
    ## $`1049`
    ##                         fuckkk                           juss 
    ##                              1                              1 
    ##                   lhhhollywood                        trynaaa 
    ##                              1                              1 
    ## <f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD> 
    ##                              1 
    ## 
    ## $`1050`
    ## kayeekulit      thank 
    ##          1          1 
    ## 
    ## $`1051`
    ##   bae   fun wanna 
    ##     1     1     1 
    ## 
    ## $`1052`
    ##    bitch      boy   diming  gotdamn practice 
    ##        1        1        1        1        1 
    ## 
    ## $`1053`
    ## berniebaby94         good       missed        night      tonight 
    ##            1            1            1            1            1 
    ## 
    ## $`1054`
    ## here 
    ##    1 
    ## 
    ## $`1055`
    ## breminardi21          can          get         mess         till     together 
    ##            1            1            1            1            1            1 
    ## 
    ## $`1056`
    ##  gawd   get  hate  kane orton 
    ##     1     1     1     1     1 
    ## 
    ## $`1057`
    ##                            beauty                              body 
    ##                                 1                                 1 
    ##                           djzeeti                         iamtaixox 
    ##                                 1                                 1 
    ## mentionpeoplethatyouloveontwitter                               pic 
    ##                                 1                                 1 
    ## 
    ## $`1058`
    ##              hair           harpers httptcodzv7vzibtg 
    ##                 1                 1                 1

### Q8. (10pts) Get the ten most frequent words based on Q7.

``` r
findMostFreqTerms(myDtm,n=10)
```

    ## $`1`
    ## liver  whew 
    ##     1     1 
    ## 
    ## $`2`
    ##                                ajasmineflower 
    ##                                             1 
    ##                                           bad 
    ##                                             1 
    ##                                          real 
    ##                                             1 
    ##                                          shit 
    ##                                             1 
    ##                                        talkin 
    ##                                             1 
    ##                                     “kdubbbbb 
    ##                                             1 
    ##                      <f0><ff><U+0098><U+00A9> 
    ##                                             1 
    ##                     <f0><ff><U+0098><U+00AD>” 
    ##                                             1 
    ## <f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD> 
    ##                                             1 
    ## 
    ## $`3`
    ##   drew   hate  still storen 
    ##      1      1      1      1 
    ## 
    ## $`4`
    ##     fans redskins  tonight    where 
    ##        1        1        1        1 
    ## 
    ## $`5`
    ##  always    feel    life    like    oomf twitter 
    ##       1       1       1       1       1       1 
    ## 
    ## $`6`
    ##                   1853                2014106          5sosgoodgirls 
    ##                      1                      1                      1 
    ##                  avery                bradley                    cdt 
    ##                      1                      1                      1 
    ##   scottysireisadisease       theoriginalsback thewalkingdeadmarathon 
    ##                      1                      1                      1 
    ## 
    ## $`7`
    ##  fresh   need    new people   talk 
    ##      1      1      1      1      1 
    ## 
    ## $`8`
    ##              1853           2014106               cdt            hollis 
    ##                 1                 1                 1                 1 
    ## httptcolcaeh6twol       rawbrooklyn            storen          thompson 
    ##                 1                 1                 1                 1 
    ##            trndnl           welcome 
    ##                 1                 1 
    ## 
    ## $`9`
    ##      already         nats redskinstime 
    ##            1            1            1 
    ## 
    ## $`10`
    ##          birthday             happy httptco0abyzf8wrq            renren 
    ##                 1                 1                 1                 1 
    ##        umpalumpia 
    ##                 1 
    ## 
    ## $`11`
    ##                          copesaywhaaa                                  hate 
    ##                                     1                                     1 
    ## <f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+0082><U+270B> 
    ##                                     1 
    ## 
    ## $`12`
    ##      believe         butt         httr skinsnation”         will          win 
    ##            1            1            1            1            1            1 
    ##         yall “jamirwilson 
    ##            1            1 
    ## 
    ## $`13`
    ##    confidence        little selfconscious          wish 
    ##             1             1             1             1 
    ## 
    ## $`14`
    ##  important    ratchet     sports television 
    ##          1          1          1          1 
    ## 
    ## $`15`
    ## aarondmiller2         cause         claim        crises        crisis 
    ##             1             1             1             1             1 
    ##         great          mean           npr     president        seemed 
    ##             1             1             1             1             1 
    ## 
    ## $`16`
    ##  back bring gotta  nats  shit   win 
    ##     1     1     1     1     1     1 
    ## 
    ## $`17`
    ## bishopulmer       hello 
    ##           1           1 
    ## 
    ## $`18`
    ## going throw 
    ##     1     1 
    ## 
    ## $`19`
    ##              feel httptcok4zbgg5isp      ibackthenats              like 
    ##                 1                 1                 1                 1 
    ##            making         nationals          natitude             thank 
    ##                 1                 1                 1                 1 
    ##               win 
    ##                 1 
    ## 
    ## $`20`
    ##        could         girl     thickest      twitter virgoperidot        white 
    ##            1            1            1            1            1            1 
    ## 
    ## $`21`
    ##     gotcha aviroberts      going     hasn’t       hope       know       long 
    ##          2          1          1          1          1          1          1 
    ##       miss      since        spx 
    ##          1          1          1 
    ## 
    ## $`22`
    ##             awesome httpstco5nupojypss”     “badlipreadings 
    ##                   1                   1                   1 
    ## 
    ## $`23`
    ##                                       good 
    ##                                          1 
    ##                                 joeyowey77 
    ##                                          1 
    ##                                       luck 
    ##                                          1 
    ## <f0><ff><U+0092><U+0099><U+26F3><U+FE0F><f0><ff><U+0098><U+009A> 
    ##                                          1 
    ## 
    ## $`24`
    ##                                 everyone 
    ##                                        1 
    ##                                     joke 
    ##                                        1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                        1 
    ## 
    ## $`25`
    ##          avi bbraxton1297        sweet 
    ##            1            1            1 
    ## 
    ## $`26`
    ##     anarchy      boston       drive        ever   karenfink melanierizk 
    ##           1           1           1           1           1           1 
    ##       never       think        well 
    ##           1           1           1 
    ## 
    ## $`27`
    ## nationals       now   survive 
    ##         1         1         1 
    ## 
    ## $`28`
    ##   life social  sucks 
    ##      1      1      1 
    ## 
    ## $`29`
    ##      come redpandas     tammy 
    ##         1         1         1 
    ## 
    ## $`30`
    ## httptcoa5akum9su0”           ignoring               like               mans 
    ##                  1                  1                  1                  1 
    ##               stop                why     “amandaradan04 
    ##                  1                  1                  1 
    ## 
    ## $`31`
    ##      chief     earned gtgtgtgtgt       keef 
    ##          1          1          1          1 
    ## 
    ## $`32`
    ##               asking                dming                  lol 
    ##                    1                    1                    1 
    ##               random                 stop <f0><ff><U+0098><U+0082> 
    ##                    1                    1                    1 
    ## 
    ## $`33`
    ##   alive beltway    nats     one orioles  series   still     the     two   world 
    ##       1       1       1       1       1       1       1       1       1       1 
    ## 
    ## $`34`
    ##    goddamubernice httptcotgr0rsxhea         pondering 
    ##                 1                 1                 1 
    ## 
    ## $`35`
    ##             bed         earlier         heading           tired         tonight 
    ##               1               1               1               1               1 
    ##            wait <f0><ff><U+0098><U+00B4> 
    ##               1               1 
    ## 
    ## $`36`
    ## feel like  why 
    ##    1    1    1 
    ## 
    ## $`37`
    ##   even  girls   guys  happy   know   love  makes seeing <U+263A><U+FE0F> 
    ##      1      1      1      1      1      1      1      1      1 
    ## 
    ## $`38`
    ## woop down game httr 
    ##    2    1    1    1 
    ## 
    ## $`39`
    ## anything     give      ill     want 
    ##        1        1        1        1 
    ## 
    ## $`40`
    ##             amp brianjameswalsh            even           happy            hope 
    ##               1               1               1               1               1 
    ##            nats           never          pulled             see      semiclutch 
    ##               1               1               1               1               1 
    ## 
    ## $`41`
    ##            okay           sleep <f0><ff><U+0098><U+00B4> 
    ##               1               1               1 
    ## 
    ## $`42`
    ##   lol   sad thats   tru truth 
    ##     1     1     1     1     1 
    ## 
    ## $`43`
    ##         fuck       haters         httr kirkcousins8          man          the 
    ##            1            1            1            1            1            1 
    ##         time       turnup 
    ##            1            1 
    ## 
    ## $`44`
    ##          big       clutch dougfister58         good          man         spot 
    ##            1            1            1            1            1            1 
    ##         year 
    ##            1 
    ## 
    ## $`45`
    ## amandapleaasee     apologizes           nope 
    ##              1              1              1 
    ## 
    ## $`46`
    ##       hip      just       now queenbrie 
    ##         1         1         1         1 
    ## 
    ## $`47`
    ##      1st   683205   became   hollis  mention   people     seen    since 
    ##        1        1        1        1        1        1        1        1 
    ## thompson    topic 
    ##        1        1 
    ## 
    ## $`48`
    ##                                   coming 
    ##                                        1 
    ##                                    didnt 
    ##                                        1 
    ##                                  earlier 
    ##                                        1 
    ##                                     even 
    ##                                        1 
    ##                                     ever 
    ##                                        1 
    ##                                     hear 
    ##                                        1 
    ## mouth<f0><ff><U+0098><U+00A2><f0><ff><U+0098><U+009E> 
    ##                                        1 
    ##                                     said 
    ##                                        1 
    ##                                     shit 
    ##                                        1 
    ##                                    think 
    ##                                        1 
    ## 
    ## $`49`
    ##      jai  devoirs    finir   flemme      mal      mes t<e3><U+00AA>te     vasy 
    ##        2        1        1        1        1        1        1        1 
    ## 
    ## $`50`
    ##      169      213      222   hollis     made      rts   states thompson 
    ##        1        1        1        1        1        1        1        1 
    ##    topic trending 
    ##        1        1 
    ## 
    ## $`51`
    ## build   can  drew  hope  lets  nats 
    ##     1     1     1     1     1     1 
    ## 
    ## $`52`
    ##        accounts             amp georgetownhoyas          helped          hollis 
    ##               2               1               1               1               1 
    ##   ryenarussillo          sixers           these        thompson           topic 
    ##               1               1               1               1               1 
    ## 
    ## $`53`
    ##           finally httptconhg9enkkkz               i95           resting 
    ##                 1                 1                 1                 1 
    ##   shoulderarmhand          watching 
    ##                 1                 1 
    ## 
    ## $`54`
    ##                  bus               driver                kinda 
    ##                    1                    1                    1 
    ##               moving                swift <f0><ff><U+0098><U+0082> 
    ##                    1                    1                    1 
    ## 
    ## $`55`
    ##            hollis httptcoc8vtqhkk7m            impact         published 
    ##                 1                 1                 1                 1 
    ##               rts            sixers               the          thompson 
    ##                 1                 1                 1                 1 
    ##             trend            trndnl 
    ##                 1                 1 
    ## 
    ## $`56`
    ##  twitter  android   client   hollis   iphone thompson top3apps      web 
    ##        3        1        1        1        1        1        1        1 
    ## 
    ## $`57`
    ##    eyes    make sparkle     you 
    ##       1       1       1       1 
    ## 
    ## $`58`
    ##         crowd      barclays           can       chicago disappointing 
    ##             2             1             1             1             1 
    ##          last          lets          make           new           raw 
    ##             1             1             1             1             1 
    ## 
    ## $`59`
    ##    dies   every    like minutes   phone     why 
    ##       1       1       1       1       1       1 
    ## 
    ## $`60`
    ##                                                   goals 
    ##                                                       1 
    ##                                                 someone 
    ##                                                       1 
    ##                                                    want 
    ##                                                       1 
    ## <f0><ff><U+0091><U+00AB><f0><ff><U+0092><U+008D><f0><ff><U+0092><U+0098> 
    ##                                                       1 
    ## 
    ## $`61`
    ##                                                                                                    around 
    ##                                                                                                         1 
    ##                                                                                                     comes 
    ##                                                                                                         1 
    ##                                                                                                       day 
    ##                                                                                                         1 
    ##                                                                                                     every 
    ##                                                                                                         1 
    ##                                                                                                      feel 
    ##                                                                                                         1 
    ##                                                                                                      like 
    ##                                                                                                         1 
    ##                                                                                                   tuesday 
    ##                                                                                                         1 
    ## <f0><ff><U+0092><U+00B0><f0><ff><U+0092><U+00B0><f0><ff><U+0092><U+00B0><f0><ff><U+0092><U+00B0><f0><ff><U+0092><U+00B0><f0><ff><U+0092><U+00B0><f0><ff><U+0092><U+00B0> 
    ##                                                                                                         1 
    ## 
    ## $`62`
    ## early sleep tired  want 
    ##     1     1     1     1 
    ## 
    ## $`63`
    ## lahhhollywood        mother       omarion 
    ##             1             1             1 
    ## 
    ## $`64`
    ##                                                    dumb 
    ##                                                       1 
    ##                                                     now 
    ##                                                       1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+00B4><f0><ff><U+0092><U+0085> 
    ##                                                       1 
    ## 
    ## $`65`
    ## nats 
    ##    1 
    ## 
    ## $`66`
    ##    chris      fag     hate   mcghan subtweet 
    ##        1        1        1        1        1 
    ## 
    ## $`67`
    ##             canope            cmonman httpstco2zlhai99e1                sur 
    ##                  1                  1                  1                  1 
    ##         washington 
    ##                  1 
    ## 
    ## $`68`
    ##          amazon          hunter iheartmastodons             irs            pays 
    ##               1               1               1               1               1 
    ##           pence     selfreports           taxes 
    ##               1               1               1 
    ## 
    ## $`69`
    ##  actually     after    better finishing       mix  mxmeraki      song    sounds 
    ##         1         1         1         1         1         1         1         1 
    ##   thought       way 
    ##         1         1 
    ## 
    ## $`70`
    ##          business        consultant              corp              cort 
    ##                 1                 1                 1                 1 
    ## httptcowlpfsyfwdu               job              jobs            rental 
    ##                 1                 1                 1                 1 
    ##             sales          services 
    ##                 1                 1 
    ## 
    ## $`71`
    ##               hiphop                 love <f0><ff><U+0098><U+009C> 
    ##                    1                    1                    1 
    ## 
    ## $`72`
    ##         will          lot mrdcsportssr         need     official         this 
    ##            2            1            1            1            1            1 
    ##        video          win      wizards  wizardsxtra 
    ##            1            1            1            1 
    ## 
    ## $`73`
    ##            beat            best           fight           still            that 
    ##               1               1               1               1               1 
    ## tonyperkinsfox5 
    ##               1 
    ## 
    ## $`74`
    ##      now seahawks 
    ##        1        1 
    ## 
    ## $`75`
    ##  chrisgolden danmericacnn      traitor 
    ##            1            1            1 
    ## 
    ## $`76`
    ##   back dollla   igns   look    see 
    ##      1      1      1      1      1 
    ## 
    ## $`77`
    ##     crib lilgerb9 
    ##        1        1 
    ## 
    ## $`78`
    ##      brother         club          dad   explaining          now      penguin 
    ##            1            1            1            1            1            1 
    ##      picture rramirezryan       showed        stuck 
    ##            1            1            1            1 
    ## 
    ## $`79`
    ## one 
    ##   1 
    ## 
    ## $`80`
    ##    bacardi       call       doug     fister       good mlbnetwork   natitude 
    ##          1          1          1          1          1          1          1 
    ##       nlds     pretty  untamable 
    ##          1          1          1 
    ## 
    ## $`81`
    ##       watch allaboutjas       cause        dead       first       gonna 
    ##           2           1           1           1           1           1 
    ##         ill      scicin     walking 
    ##           1           1           1 
    ## 
    ## $`82`
    ##        1st    appears       fs14 janaboruta    mention        now     states 
    ##          1          1          1          1          1          1          1 
    ##      topic   trending     trndnl 
    ##          1          1          1 
    ## 
    ## $`83`
    ##  show   the   tho voice 
    ##     1     1     1     1 
    ## 
    ## $`84`
    ##                                                         dats 
    ##                                                            1 
    ##                                                        jordo 
    ##                                                            1 
    ##                                                         neck 
    ##                                                            1 
    ##                                                        nigga 
    ##                                                            1 
    ##                                                         said 
    ##                                                            1 
    ##                                                          yur 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0082> 
    ##                                                            1 
    ## 
    ## $`85`
    ##           accurate             bodies           entirely              girls 
    ##                  1                  1                  1                  1 
    ## httptcotyi50o6bc3”            judging               stop        “googlepics 
    ##                  1                  1                  1                  1 
    ## 
    ## $`86`
    ##      around       drive     happily   karenfink melanierizk         nyc 
    ##           1           1           1           1           1           1 
    ##      people        will 
    ##           1           1 
    ## 
    ## $`87`
    ## daysiaaa 
    ##        1 
    ## 
    ## $`88`
    ## ashlaayraeee          got         shot 
    ##            1            1            1 
    ## 
    ## $`89`
    ## ibelieve     lets     nats 
    ##        1        1        1 
    ## 
    ## $`90`
    ## kschorback       stop 
    ##          1          1 
    ## 
    ## $`91`
    ## much shit  tbh 
    ##    1    1    1 
    ## 
    ## $`92`
    ##      confidence             gio   jameswagnerwp             lot        natitude 
    ##               1               1               1               1               1 
    ##            nats natsaintdoneyet      postsports 
    ##               1               1               1 
    ## 
    ## $`93`
    ##       itaintover             nlds thatsmynationals 
    ##                1                1                1 
    ## 
    ## $`94`
    ## 2morrow   alive   dcsaa    game  giants     gio    lets    nats staying victory 
    ##       1       1       1       1       1       1       1       1       1       1 
    ## 
    ## $`95`
    ## biggestbabyshower         safety1st              tyvm 
    ##                 1                 1                 1 
    ## 
    ## $`96`
    ## important     sleep      word   yaelpnt 
    ##         1         1         1         1 
    ## 
    ## $`97`
    ##      nats       win nationals 
    ##         3         3         1 
    ## 
    ## $`98`
    ##  completely   different frustrating        kind         mnf         now 
    ##           1           1           1           1           1           1 
    ## 
    ## $`99`
    ## guwopfatzz  halaheeem      never 
    ##          1          1          1 
    ## 
    ## $`100`
    ## espn yeah 
    ##    1    1 
    ## 
    ## $`101`
    ## actually   doctor    happy     said 
    ##        1        1        1        1 
    ## 
    ## $`102`
    ## bitches    like     act     get  niggas treated 
    ##       2       2       1       1       1       1 
    ## 
    ## $`103`
    ##      hate literally       pic  pictures   praying   trappin 
    ##         1         1         1         1         1         1 
    ## 
    ## $`104`
    ##           hensons httptcocsshhdxejk              part             still 
    ##                 1                 1                 1                 1 
    ## 
    ## $`105`
    ##                            bigbangtheory 
    ##                                        1 
    ## <f0><ff><U+0098><U+0083><f0><ff><U+0091><U+008D> 
    ##                                        1 
    ## 
    ## $`106`
    ## curtisatkinson           dead            man 
    ##              1              1              1 
    ## 
    ## $`107`
    ## blackbuckethats            know            nice           thats 
    ##               1               1               1               1 
    ## 
    ## $`108`
    ## appreciate        bro       cool        ima   kamekami     really     thanks 
    ##          1          1          1          1          1          1          1 
    ## 
    ## $`109`
    ## gothamtime       time       what 
    ##          1          1          1 
    ## 
    ## $`110`
    ##   quarantining         africa allinwithchris         answer           best 
    ##              2              1              1              1              1 
    ##    chrislhayes     devichechi       entrance           exit          msnbc 
    ##              1              1              1              1              1 
    ## 
    ## $`111`
    ## eunyangnbc    naughty       need        now       oops   redskins       said 
    ##          1          1          1          1          1          1          1 
    ##        win       word 
    ##          1          1 
    ## 
    ## $`112`
    ## nats 
    ##    4 
    ## 
    ## $`113`
    ## lance210      mcm 
    ##        1        1 
    ## 
    ## $`114`
    ##        differently               game             here’s httptcoaqqvs7obgd” 
    ##                  1                  1                  1                  1 
    ##               sees                she               sigh     “menshealthmag 
    ##                  1                  1                  1                  1 
    ## 
    ## $`115`
    ##             alire            begins          benjamin              club 
    ##                 1                 1                 1                 1 
    ##              ends        everything          faulkner              home 
    ##                 1                 1                 1                 1 
    ## httptcoqaeubpmus1          kentucky 
    ##                 1                 1 
    ## 
    ## $`116`
    ##             calvert                 got                hall            mountain 
    ##                   1                   1                   1                   1 
    ##            toughest                unis                view <f0><ff><U+009C><ff>\231 
    ##                   1                   1                   1                   1 
    ## 
    ## $`117`
    ##                         are                       comes 
    ##                           1                           1 
    ##                         one tonight<f0><ff><U+0098><U+008D> 
    ##                           1                           1 
    ## 
    ## $`118`
    ##         awwiebabyy            dressed           edmyakua idkwhoelsewasthere 
    ##                  1                  1                  1                  1 
    ##               lmao           maryroni           michelle              obama 
    ##                  1                  1                  1                  1 
    ##           remember               time 
    ##                  1                  1 
    ## 
    ## $`119`
    ## patrickmj  sheepeeh       yes 
    ##         1         1         1 
    ## 
    ## $`120`
    ## say 
    ##   1 
    ## 
    ## $`121`
    ## yaaaaassss      finna      honey      lahhh       turn 
    ##          2          1          1          1          1 
    ## 
    ## $`122`
    ##               follow                  hip                  hop 
    ##                    1                    1                    1 
    ##              jinx202                 real <f0><ff><U+0091><U+0088> 
    ##                    1                    1                    1 
    ## 
    ## $`123`
    ## httptco8sh6zecjc4           yusssss 
    ##                 1                 1 
    ## 
    ## $`124`
    ##       bruhh hoopaholick 
    ##           1           1 
    ## 
    ## $`125`
    ##       ask     basis     daily      dogg heybswizz     snoop 
    ##         1         1         1         1         1         1 
    ## 
    ## $`126`
    ## anything     hell migraine     take 
    ##        1        1        1        1 
    ## 
    ## $`127`
    ## couldve     lrt orange…   sworn 
    ##       1       1       1       1 
    ## 
    ## $`128`
    ##      bbl homework     time 
    ##        1        1        1 
    ## 
    ## $`129`
    ##          always             but             can katherinerhoad9        mentions 
    ##               1               1               1               1               1 
    ##          public            sure     technically          twitch         twitter 
    ##               1               1               1               1               1 
    ## 
    ## $`130`
    ##   all girls  ugly 
    ##     1     1     1 
    ## 
    ## $`131`
    ##  get  raw time 
    ##    1    1    1 
    ## 
    ## $`132`
    ##            analysis              better              dilfer httpstcoh81qliseis” 
    ##                   1                   1                   1                   1 
    ##           timboj126               trent          “nfltalkrt 
    ##                   1                   1                   1 
    ## 
    ## $`133`
    ## httptcojpuy2kvut9 <f0><ff><U+0094><U+00A5> 
    ##                 1                 1 
    ## 
    ## $`134`
    ##  back  chop  game  love   one  play would 
    ##     1     1     1     1     1     1     1 
    ## 
    ## $`135`
    ## boyfriend<f0><ff><U+0092><U+008D><f0><ff><U+0092><U+0091> 
    ##                                                 1 
    ##                                              miss 
    ##                                                 1 
    ## 
    ## $`136`
    ##     found francisco nationals  natitude       san       the 
    ##         1         1         1         1         1         1 
    ## 
    ## $`137`
    ##        just allaboutjas      answer         but       crazy         hip 
    ##           2           1           1           1           1           1 
    ##        knew         lol        make    question 
    ##           1           1           1           1 
    ## 
    ## $`138`
    ##    monday       raw       wwe highlight      last     night       now     right 
    ##         2         2         2         1         1         1         1         1 
    ##     start 
    ##         1 
    ## 
    ## $`139`
    ##      ago baseball  college   column enjoying    equal headline   hockey 
    ##        1        1        1        1        1        1        1        1 
    ## jcmeloni    paper 
    ##        1        1 
    ## 
    ## $`140`
    ##                                                      gotham” 
    ##                                                            1 
    ##                                                   “thexfiles 
    ##                                                            1 
    ## <f0><ff><U+0099><U+009C><f0><ff><U+0099><U+009C><f0><ff><U+0099><U+009C> 
    ##                                                            1 
    ## 
    ## $`141`
    ## aisharobinson           ate        chynaf      hera7155      visiting 
    ##             1             1             1             1             1 
    ## 
    ## $`142`
    ##    now   room tumblr 
    ##      1      1      1 
    ## 
    ## $`143`
    ##   about   check wizards 
    ##       1       1       1 
    ## 
    ## $`144`
    ##                                              always 
    ##                                                   1 
    ##                                              connor 
    ##                                                   1 
    ##                                        connorfranta 
    ##                                                   1 
    ##                                                damn 
    ##                                                   1 
    ##                                                dont 
    ##                                                   1 
    ##                                           following 
    ##                                                   1 
    ##                                                 god 
    ##                                                   1 
    ##                                                miss 
    ##                                                   1 
    ## sprees<f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD> 
    ##                                                   1 
    ##                                          understand 
    ##                                                   1 
    ## 
    ## $`145`
    ##        article   baltimoresun          horse            how       industry 
    ##              1              1              1              1              1 
    ## infrastructure           much         racing        schools          still 
    ##              1              1              1              1              1 
    ## 
    ## $`146`
    ##                            amandaradan04 
    ##                                        1 
    ##                                    holla 
    ##                                        1 
    ##                                     just 
    ##                                        1 
    ##                                  kidding 
    ##                                        1 
    ##                                     rara 
    ##                                        1 
    ##                                    tryna 
    ##                                        1 
    ## <f0><ff><U+0086><U+0098><f0><ff><U+0086><U+0098> 
    ##                                        1 
    ## 
    ## $`147`
    ##     days     fs14     made      rts   states    topic trending   trndnl 
    ##        1        1        1        1        1        1        1        1 
    ##   tweets   united 
    ##        1        1 
    ## 
    ## $`148`
    ##             great             alive              baby              boys 
    ##                 2                 1                 1                 1 
    ##      dougfister58              game      ibackthenats         nationals 
    ##                 1                 1                 1                 1 
    ## nothingbutoctober          pitching 
    ##                 1                 1 
    ## 
    ## $`149`
    ##    twitter    android       fs14     iphone i<ee><ff>s   top3apps   tweetbot 
    ##          2          1          1          1          1          1          1 
    ## 
    ## $`150`
    ##                                queenbrie 
    ##                                        1 
    ## <f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092> 
    ##                                        1 
    ## 
    ## $`151`
    ##      1st    38644   became     fs14  mention   people     seen    since 
    ##        1        1        1        1        1        1        1        1 
    ##    topic trending 
    ##        1        1 
    ## 
    ## $`152`
    ##               amp            bolans              crab          foodporn 
    ##                 1                 1                 1                 1 
    ##             gouda httptcobg9drysowo             irish            pepper 
    ##                 1                 1                 1                 1 
    ##               pub               red 
    ##                 1                 1 
    ## 
    ## $`153`
    ##                nigga                talib                 this 
    ##                    1                    1                    1 
    ##                 wild <f0><ff><U+0098><U+0082> 
    ##                    1                    1 
    ## 
    ## $`154`
    ##     gush pharrell 
    ##        1        1 
    ## 
    ## $`155`
    ##  him miss 
    ##    1    1 
    ## 
    ## $`156`
    ##       books csnredskins         eat        game       great        httr 
    ##           1           1           1           1           1           1 
    ## httredskins         job        nats         now 
    ##           1           1           1           1 
    ## 
    ## $`157`
    ##         love dougfister58   drewstoren         hell    nationals 
    ##            2            1            1            1            1 
    ## 
    ## $`158`
    ##        advice          take wackyjacky125          will 
    ##             1             1             1             1 
    ## 
    ## $`159`
    ##  gun just  son 
    ##    1    1    1 
    ## 
    ## $`160`
    ##            much             big            care            hate           heart 
    ##               2               1               1               1               1 
    ##       sometimes <f0><ff><U+0098><U+00A9> 
    ##               1               1 
    ## 
    ## $`161`
    ##    ive beanie missed  never  since  today 
    ##      2      1      1      1      1      1 
    ## 
    ## $`162`
    ##    baby   eezus excited    love  school  sister 
    ##       1       1       1       1       1       1 
    ## 
    ## $`163`
    ##                                    scandalabc 
    ##                                             1 
    ## <f0><ff><U+0097><U+00BB><f0><ff><U+0091><U+00B0><f0><ff><U+0093><U+00B9> 
    ##                                             1 
    ## 
    ## $`164`
    ##       100layercake              board      clarabeara731 httptcoblyoqosngu” 
    ##                  1                  1                  1                  1 
    ##              ideas                our          pinterest              plans 
    ##                  1                  1                  1                  1 
    ##         realsimple            wedding 
    ##                  1                  1 
    ## 
    ## $`165`
    ##            bodies             could             found              four 
    ##                 1                 1                 1                 1 
    ## httptcocyboyvv23h         roadworks           trinity           vikings 
    ##                 1                 1                 1                 1 
    ## 
    ## $`166`
    ##  creepy decides    door     not    open    room 
    ##       1       1       1       1       1       1 
    ## 
    ## $`167`
    ##              attack               bring         cardiaccats                 fan 
    ##                   1                   1                   1                   1 
    ##               heart                home             managed                 not 
    ##                   1                   1                   1                   1 
    ## official40something                paid 
    ##                   1                   1 
    ## 
    ## $`168`
    ##  httptcoi1px8ewivw                plm       plmefficient principallifestyle 
    ##                  1                  1                  1                  1 
    ## 
    ## $`169`
    ##              1st          appears          hiwalls          mention 
    ##                1                1                1                1 
    ##              now           states theoriginalsback            topic 
    ##                1                1                1                1 
    ##         trending           trndnl 
    ##                1                1 
    ## 
    ## $`170`
    ##       aint       hard       idgt    operate      sleep yoimblaine 
    ##          1          1          1          1          1          1 
    ## 
    ## $`171`
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0091><U+008F> 
    ##                                        1 
    ## 
    ## $`172`
    ## httptcouf3gftujqw        installing          mathfail            update 
    ##                 1                 1                 1                 1 
    ##           windows 
    ##                 1 
    ## 
    ## $`173`
    ##       almost electrocuted          got         just          omg        phone 
    ##            1            1            1            1            1            1 
    ##     plugging          saw        spark         well 
    ##            1            1            1            1 
    ## 
    ## $`174`
    ##         bunch        couple    fimustauri           had           jim 
    ##             1             1             1             1             1 
    ## joecienkowski   lizfebruary          long          ones    persistent 
    ##             1             1             1             1             1 
    ## 
    ## $`175`
    ## birdman<f0><ff><U+0098><U+00AD>                geeeked                 tooooo 
    ##                      1                      1                      1 
    ## 
    ## $`176`
    ##            awesome             hudson       ibackthenats idonotbackthebirds 
    ##                  1                  1                  1                  1 
    ##               legs               lots  nothingbutoctober           pressure 
    ##                  1                  1                  1                  1 
    ##            shifted              stuff 
    ##                  1                  1 
    ## 
    ## $`177`
    ##               37m             bryan              face               fld 
    ##                 1                 1                 1                 1 
    ## httptcoxnvmnj9oms          maryland           richard          seahawks 
    ##                 1                 1                 1                 1 
    ##      seattletimes           sherman 
    ##                 1                 1 
    ## 
    ## $`178`
    ##       and    gonats      hope    mynats nationals  natitude     there 
    ##         1         1         1         1         1         1         1 
    ## 
    ## $`179`
    ##         beheading              fans            giants httptcogzzdsaz2kj 
    ##                 1                 1                 1                 1 
    ##              loss          mourning          natitude            pandas 
    ##                 1                 1                 1                 1 
    ##              sick 
    ##                 1 
    ## 
    ## $`180`
    ##      guitar        much        need nikkiwhitee         now    practice 
    ##           1           1           1           1           1           1 
    ##       right   suzydehor     wedding 
    ##           1           1           1 
    ## 
    ## $`181`
    ## nbcthevoice        time 
    ##           1           1 
    ## 
    ## $`182`
    ##             blunt              hits httptcogxmfk3h67w           longest 
    ##                 1                 1                 1                 1 
    ##               one          syllable              word 
    ##                 1                 1                 1 
    ## 
    ## $`183`
    ##   gonats    least natitude     nlds    sweep  wasvssf     well 
    ##        1        1        1        1        1        1        1 
    ## 
    ## $`184`
    ##         bet      except kimsherayko        nats       start   zimmerman 
    ##           1           1           1           1           1           1 
    ## 
    ## $`185`
    ## her<f0><ff><U+0098><U+008B><f0><ff><U+0098><U+008B><f0><ff><U+0098><U+0098> 
    ##                                                               1 
    ##                                                          kissin 
    ##                                                               1 
    ##                                                            like 
    ##                                                               1 
    ## 
    ## $`186`
    ##                                     difficult 
    ##                                             1 
    ##                             httptcotljp1jakvv 
    ##                                             1 
    ##                                stuathproblems 
    ##                                             1 
    ##                                          test 
    ##                                             1 
    ##                                           the 
    ##                                             1 
    ## truth<f0><ff><U+0098><U+0092><f0><ff><U+0091><U+008F> 
    ##                                             1 
    ##                                        worlds 
    ##                                             1 
    ## 
    ## $`187`
    ##  ibuprofen megster143   shoutout      today 
    ##          1          1          1          1 
    ## 
    ## $`188`
    ##     the     why   women answers because     get    long    need     omg    one” 
    ##       2       2       2       1       1       1       1       1       1       1 
    ## 
    ## $`189`
    ## chance   hear   rock    see    the   town 
    ##      1      1      1      1      1      1 
    ## 
    ## $`190`
    ##       bigcitymoms biggestbabyshower               day             party 
    ##                 1                 1                 1                 1 
    ##         safety1st           waiting 
    ##                 1                 1 
    ## 
    ## $`191`
    ##        fedexfield httptcozzcbernatr              just             photo 
    ##                 1                 1                 1                 1 
    ##            posted 
    ##                 1 
    ## 
    ## $`192`
    ##               erkyya                 niya <f0><ff><U+0098><U+0085> 
    ##                    1                    1                    1 
    ## 
    ## $`193`
    ## enasstyyyy       good        one 
    ##          1          1          1 
    ## 
    ## $`194`
    ## funny   hip   hop  love   now right  shit 
    ##     1     1     1     1     1     1     1 
    ## 
    ## $`195`
    ##                                                        smart 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                            1 
    ## 
    ## $`196`
    ##             final              game              horn httptco2h9c2cnyg3 
    ##                 1                 1                 1                 1 
    ##         nationals              nats             night          sfgiants 
    ##                 1                 1                 1                 1 
    ##         submarine             thats 
    ##                 1                 1 
    ## 
    ## $`197`
    ##    comcast       game        net  preseason     sports washington    wizards 
    ##          1          1          1          1          1          1          1 
    ## 
    ## $`198`
    ##  baby   can daddy  fizz 
    ##     1     1     1     1 
    ## 
    ## $`199`
    ##               boy httptcolarx9lplxd          krybae86   thefaygowarrior 
    ##                 1                 1                 1                 1 
    ## 
    ## $`200`
    ##              excited               mayday                 next 
    ##                    1                    1                    1 
    ##               parade               seeing                 week 
    ##                    1                    1                    1 
    ## <f0><ff><U+0098><U+0081> 
    ##                    1 
    ## 
    ## $`201`
    ##       aint      bring  certified everywhere      gotta  resumeeee 
    ##          1          1          1          1          1          1 
    ## 
    ## $`202`
    ##   hit shake  some steak 
    ##     1     1     1     1 
    ## 
    ## $`203`
    ## brat even love  mom shes 
    ##    1    1    1    1    1 
    ## 
    ## $`204`
    ## penguin 
    ##       1 
    ## 
    ## $`205`
    ##  shooting       4th  addition afternoon       amp   another     busch      dead 
    ##         2         1         1         1         1         1         1         1 
    ##   healstl      hear 
    ##         1         1 
    ## 
    ## $`206`
    ##      fucking         just         like       movies myyownworldd        perry 
    ##            1            1            1            1            1            1 
    ##        tyler       whoops 
    ##            1            1 
    ## 
    ## $`207`
    ## gonats<U+26BE><U+FE0F>                wahoo <f0><ff><U+0099><U+009C> 
    ##                    1                    1                    1 
    ## 
    ## $`208`
    ##                                   lurkin 
    ##                                        1 
    ##                                    nigga 
    ##                                        1 
    ##                                     this 
    ##                                        1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                        1 
    ## 
    ## $`209`
    ##                                                                   gossipgirliee” 
    ##                                                                                1 
    ##                                                                             hate 
    ##                                                                                1 
    ##                                                                              wow 
    ##                                                                                1 
    ##                                                                          yarinya 
    ##                                                                                1 
    ##                                                                              you 
    ##                                                                                1 
    ## <f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092> 
    ##                                                                                1 
    ## 
    ## $`210`
    ## bullsnation     playing     tonight     wizards         yea 
    ##           1           1           1           1           1 
    ## 
    ## $`211`
    ##      annaleebelle httptcoizuz8pqrxj        radiantinc 
    ##                 1                 1                 1 
    ## 
    ## $`212`
    ##         back mindymoretti     natitude       theres 
    ##            1            1            1            1 
    ## 
    ## $`213`
    ## lhhhollywood    whatevahh 
    ##            1            1 
    ## 
    ## $`214`
    ## bruh  its long 
    ##    1    1    1 
    ## 
    ## $`215`
    ##     ass    beat    need     omg tiearra 
    ##       1       1       1       1       1 
    ## 
    ## $`216`
    ##            gonats httptcoo4bzml0i58 
    ##                 1                 1 
    ## 
    ## $`217`
    ##  about shower   take 
    ##      1      1      1 
    ## 
    ## $`218`
    ##   audreymcclellan       bigcitymoms biggestbabyshower         following 
    ##                 1                 1                 1                 1 
    ##         safety1st 
    ##                 1 
    ## 
    ## $`219`
    ##    lmao serious t1v1018   think 
    ##       2       1       1       1 
    ## 
    ## $`220`
    ##          twitter          android           client           iphone 
    ##                3                1                1                1 
    ## theoriginalsback         top3apps              web 
    ##                1                1                1 
    ## 
    ## $`221`
    ##          1951598              1st           became          mention 
    ##                1                1                1                1 
    ##           people             seen            since theoriginalsback 
    ##                1                1                1                1 
    ##            topic         trending 
    ##                1                1 
    ## 
    ## $`222`
    ##              154              260              323             made 
    ##                1                1                1                1 
    ##          minutes              rts           states theoriginalsback 
    ##                1                1                1                1 
    ##            topic         trending 
    ##                1                1 
    ## 
    ## $`223`
    ##   fans giants  going   home    why 
    ##      1      1      1      1      1 
    ## 
    ## $`224`
    ##      amp   hiphop     love     time <U+23F0><U+23F0><U+23F0><U+23F0> 
    ##        1        1        1        1        1 
    ## 
    ## $`225`
    ##           hiwalls httptcoxwcsvi679i            impact         published 
    ##                 1                 1                 1                 1 
    ##               rts               the  theoriginalsback             trend 
    ##                 1                 1                 1                 1 
    ##            trndnl             tweet 
    ##                 1                 1 
    ## 
    ## $`226`
    ##    depends       haha litterally       ones      range    riuuuuh 
    ##          1          1          1          1          1          1 
    ## 
    ## $`227`
    ## believe    damn fucking     god regress   still     way 
    ##       1       1       1       1       1       1       1 
    ## 
    ## $`228`
    ##            amp       anything          back”            can           know 
    ##              1              1              1              1              1 
    ##    remembering        someone          sucks “bankofamerica 
    ##              1              1              1              1 
    ## 
    ## $`229`
    ##         day desperately       every   literally     retweet   something 
    ##           1           1           1           1           1           1 
    ##         tor      tweets        want 
    ##           1           1           1 
    ## 
    ## $`230`
    ##                      days         httptcoqrrgwrc7pw                lightskins 
    ##                         1                         1                         1 
    ## pants<f0><ff><U+0098><U+0082> 
    ##                         1 
    ## 
    ## $`231`
    ## birdman<f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD> 
    ##                                                    1 
    ##                                               geeked 
    ##                                                    1 
    ##                                                hazel 
    ##                                                    1 
    ##                                                toooo 
    ##                                                    1 
    ## 
    ## $`232`
    ##      call   dancing    ending happening       not     phone    police     stars 
    ##         1         1         1         1         1         1         1         1 
    ##      sure       the 
    ##         1         1 
    ## 
    ## $`233`
    ## awesome     job    nats 
    ##       1       1       1 
    ## 
    ## $`234`
    ## gossipgirliee          pele therealuchman             ” 
    ##             1             1             1             1 
    ## 
    ## $`235`
    ##          back bigbangtheory           its         later          time 
    ##             1             1             1             1             1 
    ## 
    ## $`236`
    ##               alert   httptcolcaeh6twol   httptcozlnjc6nmze                more 
    ##                   1                   1                   1                   1 
    ## theoriginalsseason2               trend              trends              trndnl 
    ##                   1                   1                   1                   1 
    ## 
    ## $`237`
    ##            love dangerusswilson        football            gift           great 
    ##               3               1               1               1               1 
    ##       interview         learned             mnf          people    russelwilson 
    ##               1               1               1               1               1 
    ## 
    ## $`238`
    ##      food      nice      said     share      sort     sumos zaynmalik 
    ##         1         1         1         1         1         1         1 
    ## 
    ## $`239`
    ##    date someone   wanna     why 
    ##       1       1       1       1 
    ## 
    ## $`240`
    ## drought     lor real<U+2049><U+FE0F>    this     too 
    ##       1       1       1       1       1 
    ## 
    ## $`241`
    ##             alert httptcoatsw6rguiw httptcolcaeh6twol              more 
    ##                 1                 1                 1                 1 
    ##    thankyouaustin             trend            trends            trndnl 
    ##                 1                 1                 1                 1 
    ## 
    ## $`242`
    ##        get     whered youcourtup 
    ##          1          1          1 
    ## 
    ## $`243`
    ##               job              balt         baltimore              full 
    ##                 2                 1                 1                 1 
    ##            health        healthcare httptcohpfpga811k              near 
    ##                 1                 1                 1                 1 
    ##      occupational         pediatric 
    ##                 1                 1 
    ## 
    ## $`244`
    ##                                      ate 
    ##                                        1 
    ##                                    first 
    ##                                        1 
    ##                                     just 
    ##                                        1 
    ##                                    salad 
    ##                                        1 
    ##                                     time 
    ##                                        1 
    ##                                    years 
    ##                                        1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0091><U+008F> 
    ##                                        1 
    ## 
    ## $`245`
    ##             alert httptco4ibqcx9wrn httptcolcaeh6twol              more 
    ##                 1                 1                 1                 1 
    ##     qlovingronnie             trend            trends            trndnl 
    ##                 1                 1                 1                 1 
    ## 
    ## $`246`
    ##  gawwwwwd squaremir     swear 
    ##         1         1         1 
    ## 
    ## $`247`
    ##             alert httptco3zoi3ndjsm httptcolcaeh6twol              more 
    ##                 1                 1                 1                 1 
    ##             trend            trends            trndnl           welcome 
    ##                 1                 1                 1                 1 
    ## 
    ## $`248`
    ##             alert httptcoeysjtp0dft httptcolcaeh6twol              more 
    ##                 1                 1                 1                 1 
    ##             trend            trends            trndnl              what 
    ##                 1                 1                 1                 1 
    ## 
    ## $`249`
    ## account    back    damn     fix   gotta    knew     log     lol     now    unit 
    ##       1       1       1       1       1       1       1       1       1       1 
    ## 
    ## $`250`
    ##                                      another 
    ##                                            1 
    ##                                        bring 
    ##                                            1 
    ##                                         cake 
    ##                                            1 
    ##                                       delise 
    ##                                            1 
    ##                                        piece 
    ##                                            1 
    ## tomoorrow<f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+008B> 
    ##                                            1 
    ##                                         want 
    ##                                            1 
    ## 
    ## $`251`
    ##         greenbelt      businessmgmt         companies         detective 
    ##                 2                 1                 1                 1 
    ## httptcoliscp16mku               job              loss        prevention 
    ##                 1                 1                 1                 1 
    ##               the               tjx 
    ##                 1                 1 
    ## 
    ## $`252`
    ##              alcatel            corporate                event 
    ##                    1                    1                    1 
    ##               making             metropcs                money 
    ##                    1                    1                    1 
    ##                  out <f0><ff><U+0098><U+008F> 
    ##                    1                    1 
    ## 
    ## $`253`
    ##            head…           medusa             want <f0><ff><U+0091><U+00BF><U+271A> 
    ##                1                1                1                1 
    ## 
    ## $`254`
    ##     care   jersey     nats tomorrow  wearing 
    ##        1        1        1        1        1 
    ## 
    ## $`255`
    ##              friends                  get                gotta 
    ##                    1                    1                    1 
    ##                 just                might                  pay 
    ##                    1                    1                    1 
    ##               people           rialicious                  tho 
    ##                    1                    1                    1 
    ## <f0><ff><U+0098><U+0095> 
    ##                    1 
    ## 
    ## $`256`
    ##  officialaztec perfectlymixed            reg         tellem 
    ##              1              1              1              1 
    ## 
    ## $`257`
    ## redskins seahawks 
    ##        1        1 
    ## 
    ## $`258`
    ##                      fizz yummy<f0><ff><U+0091><U+0085> 
    ##                         1                         1 
    ## 
    ## $`259`
    ##                                 god                              little 
    ##                                   1                                   1 
    ##                                poor                          savmillerr 
    ##                                   1                                   1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+00A4> 
    ##                                   1 
    ## 
    ## $`260`
    ## day<f0><ff><U+0098><U+00AD><U+263A><U+FE0F>                lily        lilygracekay                made 
    ##                   1                   1                   1                   1 
    ## 
    ## $`261`
    ##                                                                        boooooooy 
    ##                                                                                1 
    ##                                                                             just 
    ##                                                                                1 
    ##                                                                             lost 
    ##                                                                                1 
    ##                                                                            words 
    ##                                                                                1 
    ##                                 <f0><ff><U+0098><U+0084><f0><ff><U+008F><U+0088> 
    ##                                                                                1 
    ## <f0><ff><U+0098><U+008F><f0><ff><U+0098><U+0098><f0><ff><U+0098><U+0089><f0><ff><U+0098><U+008D> 
    ##                                                                                1 
    ## 
    ## $`262`
    ##          football httptcoxyukpxovuo            monday             night 
    ##                 1                 1                 1                 1 
    ##           outchea 
    ##                 1 
    ## 
    ## $`263`
    ##   ebjunkies         guy interesting        lose       makes      storen 
    ##           1           1           1           1           1           1 
    ##         win 
    ##           1 
    ## 
    ## $`264`
    ## single 
    ##      1 
    ## 
    ## $`265`
    ##  gotham    home penguin 
    ##       1       1       1 
    ## 
    ## $`266`
    ##      coach   football johnubacon     making     nearly      outta       path 
    ##          1          1          1          1          1          1          1 
    ##       penn    pricing    reality 
    ##          1          1          1 
    ## 
    ## $`267`
    ##   drugs nothing  really    talk    want    weed 
    ##       1       1       1       1       1       1 
    ## 
    ## $`268`
    ##          al3x6069 httptcoa14anjp5cu         jimbobruh              left 
    ##                 1                 1                 1                 1 
    ##               lol            niggas             today 
    ##                 1                 1                 1 
    ## 
    ## $`269`
    ##    love   don’t reasons   right 
    ##       2       1       1       1 
    ## 
    ## $`270`
    ##             alive     beltwayseries              hope httptcosorsyaaiex 
    ##                 1                 1                 1                 1 
    ##          natitude              nats              nlds           sfvswas 
    ##                 1                 1                 1                 1 
    ##             still               win 
    ##                 1                 1 
    ## 
    ## $`271`
    ##                   erkyya know<f0><ff><U+0098><U+009C> 
    ##                        1                        1 
    ## 
    ## $`272`
    ##                                                                                                                                and 
    ##                                                                                                                                  1 
    ##                                                                                                                             hiphop 
    ##                                                                                                                                  1 
    ##                                                                                                                          hollywood 
    ##                                                                                                                                  1 
    ##                                                                                                                               love 
    ##                                                                                                                                  1 
    ## <f0><ff><U+0086><U+0097><f0><ff><U+0091><U+00AF><f0><ff><U+0091><U+00AF><f0><ff><U+0091><U+00AF><f0><ff><U+0091><U+00AC><f0><ff><U+0091><U+00AC><f0><ff><U+0091><U+00AC><f0><ff><U+0098><U+0082> 
    ##                                                                                                                                  1 
    ## 
    ## $`273`
    ##                                                  hungry 
    ##                                                       1 
    ## <f0><ff><U+0098><U+00B6><f0><ff><U+0098><U+0094><f0><ff><U+0091><U+009E> 
    ##                                                       1 
    ## 
    ## $`274`
    ##       game    happens       lets  nationals       next postseason        see 
    ##          1          1          1          1          1          1          1 
    ## 
    ## $`275`
    ##                            amp                       broccoli 
    ##                              1                              1 
    ##                         cheesy                        chicken 
    ##                              1                              1 
    ##                         dinner                           rice 
    ##                              1                              1 
    ## <f0><ff><U+008D><U+00B2><f0><ff><U+008D><U+00B4> 
    ##                              1 
    ## 
    ## $`276`
    ##            company                ftw mondanightfootball           redskins 
    ##                  1                  1                  1                  1 
    ##           seahawks            sitting              suite           watching 
    ##                  1                  1                  1                  1 
    ## 
    ## $`277`
    ##        2004 bharper3407       bryce         hey   kmillar15        lets 
    ##           1           1           1           1           1           1 
    ##    remember      series        told     winning 
    ##           1           1           1           1 
    ## 
    ## $`278`
    ## early   gon   its night 
    ##     1     1     1     1 
    ## 
    ## $`279`
    ##        day       home       httr skinsbring 
    ##          1          1          1          1 
    ## 
    ## $`280`
    ## break  fall 
    ##     1     1 
    ## 
    ## $`281`
    ##    comes  finally kamekami      lol      mad  patient  quality     sent 
    ##        1        1        1        1        1        1        1        1 
    ##   sucked 
    ##        1 
    ## 
    ## $`282`
    ##   breakinankles12               got httptcosi8hhbqz4x              like 
    ##                 1                 1                 1                 1 
    ##         statement 
    ##                 1 
    ## 
    ## $`283`
    ##       2006        80s       drug    footage       good        its    mikesty 
    ##          1          1          1          1          1          1          1 
    ##        new     pretty rereleased 
    ##          1          1          1 
    ## 
    ## $`284`
    ## brights   gotta    know   light   thing 
    ##       1       1       1       1       1 
    ## 
    ## $`285`
    ##                    beyond                      brit                      done 
    ##                         1                         1                         1 
    ##                       lit paper<f0><ff><U+0098><U+0091>                  research 
    ##                         1                         1                         1 
    ## 
    ## $`286`
    ##         2817 eddieroyalwr         love      seattle        skins 
    ##            1            1            1            1            1 
    ## 
    ## $`287`
    ## httptcovhbessmm2n 
    ##                 1 
    ## 
    ## $`288`
    ## ballyessir       fizz    omarion     playin 
    ##          1          1          1          1 
    ## 
    ## $`289`
    ##                                     everybody 
    ##                                             1 
    ##                                           got 
    ##                                             1 
    ##                                           now 
    ##                                             1 
    ## <f0><ff><U+0098><U+00B7><f0><ff><U+0098><U+00B7><f0><ff><U+0098><U+00B7> 
    ##                                             1 
    ## 
    ## $`290`
    ##           hazel   lahhhollywood         sneaker             tho          wedges 
    ##               1               1               1               1               1 
    ## <f0><ff><U+0098><U+00B3> 
    ##               1 
    ## 
    ## $`291`
    ##                                                                                                                                                                                                            omarion 
    ##                                                                                                                                                                                                                  1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+0092><f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+0098><f0><ff><U+0092><U+0095><f0><ff><U+0092><U+008B><f0><ff><U+0099><U+0088> 
    ##                                                                                                                                                                                                                  1 
    ## 
    ## $`292`
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               got 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                           httpstcocxdrf1o3eu”bruh 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          question 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               yoo 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     “bforbrittany 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    “lgotaquestion 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082>”<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ## 
    ## $`293`
    ##            lets        seahawks <f0><ff><U+0098><U+00AD> 
    ##               1               1               1 
    ## 
    ## $`294`
    ##               gym httptcotwrgpxuqmy 
    ##                 1                 1 
    ## 
    ## $`295`
    ##           bae     charissat foxsportslive 
    ##             1             1             1 
    ## 
    ## $`296`
    ##                                           its 
    ##                                             1 
    ##                                        rachet 
    ##                                             1 
    ##                                          time 
    ##                                             1 
    ## <f0><ff><U+0098><U+00B9><f0><ff><U+0098><U+00B9><f0><ff><U+0098><U+00B9> 
    ##                                             1 
    ## 
    ## $`297`
    ##               xnoomzx <f0><ff><U+0098><U+0082><U+2764><U+FE0F> 
    ##                     1                     1 
    ## 
    ## $`298`
    ##            actual              copy           finally             first 
    ##                 1                 1                 1                 1 
    ##               got httptco5pvmwuccc7               mcm              more 
    ##                 1                 1                 1                 1 
    ##             novak               one 
    ##                 1                 1 
    ## 
    ## $`299`
    ##              atleast              friends                 good 
    ##                    1                    1                    1 
    ##                 know                  lol <f0><ff><U+0098><U+0082> 
    ##                    1                    1                    1 
    ## 
    ## $`300`
    ## baby nats  win yeah 
    ##    1    1    1    1 
    ## 
    ## $`301`
    ##    amp   hair  nails shower <U+2714> 
    ##      1      1      1      1      1 
    ## 
    ## $`302`
    ##                                                        ratchet 
    ##                                                              1 
    ## tv<f0><ff><U+0098><U+0081><f0><ff><U+0098><U+0081><f0><ff><U+0098><U+0081> 
    ##                                                              1 
    ## 
    ## $`303`
    ## awwiebabyy        bar    costume   edmyakua       ever     hookah        ive 
    ##          1          1          1          1          1          1          1 
    ## kariclaure   michelle       most 
    ##          1          1          1 
    ## 
    ## $`304`
    ##  bulls   gear    got    ive   like  looks    new  order season 
    ##      1      1      1      1      1      1      1      1      1 
    ## 
    ## $`305`
    ##      bitch      chose      house      leave     listen        now permission 
    ##          1          1          1          1          1          1          1 
    ##    without 
    ##          1 
    ## 
    ## $`306`
    ## broadcast     bulls   getting       why 
    ##         1         1         1         1 
    ## 
    ## $`307`
    ## running  shower   water 
    ##       1       1       1 
    ## 
    ## $`308`
    ##          600         been    followers          get         like melodictouch 
    ##            1            1            1            1            1            1 
    ##  nowstruggle         real       trying         year 
    ##            1            1            1            1 
    ## 
    ## $`309`
    ##                                      1st 
    ##                                        1 
    ##                               conference 
    ##                                        1 
    ##                                     pgfh 
    ##                                        1 
    ## <f0><ff><U+0098><U+009C><f0><ff><U+0091><U+008F> 
    ##                                        1 
    ## 
    ## $`310`
    ##      better      brewed        cool         day gallonsized        iced 
    ##           1           1           1           1           1           1 
    ##      kettle      making  refreshing         tea 
    ##           1           1           1           1 
    ## 
    ## $`311`
    ##      ale  awesome    brown      nut organics     peak      wow 
    ##        1        1        1        1        1        1        1 
    ## 
    ## $`312`
    ## don’t  lose money ohhoe  sure wanna   you 
    ##     1     1     1     1     1     1     1 
    ## 
    ## $`313`
    ##       avec        ben        les        lt3 manondvstr     manque spectacles 
    ##          1          1          1          1          1          1          1 
    ## 
    ## $`314`
    ## fuckkk    not  ready 
    ##      1      1      1 
    ## 
    ## $`315`
    ##         damn     freez610  mutheadsite tradepostmut         xbox 
    ##            1            1            1            1            1 
    ## 
    ## $`316`
    ##    apes blowing    miss physics 
    ##       1       1       1       1 
    ## 
    ## $`317`
    ##              chip              good httptcohfbak92rcc               ill 
    ##                 1                 1                 1                 1 
    ##          shoulder               the             thing           weekend 
    ##                 1                 1                 1                 1 
    ## 
    ## $`318`
    ##      games        now        row wills4lwen        win 
    ##          1          1          1          1          1 
    ## 
    ## $`319`
    ## change 
    ##      1 
    ## 
    ## $`320`
    ##   blue    ivy   like   look lowkey 
    ##      1      1      1      1      1 
    ## 
    ## $`321`
    ##         god       jessi jessirose14        read       thank 
    ##           1           1           1           1           1 
    ## 
    ## $`322`
    ##    boo   love monica    nah   sike 
    ##      1      1      1      1      1 
    ## 
    ## $`323`
    ##                    httptcoafzevjhbwg <f0><ff><U+0092><U+008B><U+271C><U+FE0F><f0><ff><U+0092><U+00AF> 
    ##                                    1                                    1 
    ## 
    ## $`324`
    ##    httptcodcdh51glhd                 like              trvllll 
    ##                    1                    1                    1 
    ## <f0><ff><U+0091><U+0080> 
    ##                    1 
    ## 
    ## $`325`
    ##                     alright                       apply 
    ##                           1                           1 
    ##                     believe                  everything 
    ##                           1                           1 
    ##                        feel                         ill 
    ##                           1                           1 
    ## inside<f0><ff><U+0099><U+0088>”                         now 
    ##                           1                           1 
    ##                      really                        told 
    ##                           1                           1 
    ## 
    ## $`326`
    ##       fuckkkkkkkk              hard httptcoxifkgfjyjr             today 
    ##                 1                 1                 1                 1 
    ## 
    ## $`327`
    ##       bigcitymoms biggestbabyshower              good              luck 
    ##                 1                 1                 1                 1 
    ##         safety1st 
    ##                 1 
    ## 
    ## $`328`
    ##                 alex             birthday             earrings 
    ##                    1                    1                    1 
    ##    httptcol3vkzso0d4          monogrammed                  new 
    ##                    1                    1                    1 
    ## <f0><ff><U+0098><U+008D> 
    ##                    1 
    ## 
    ## $`329`
    ## gone 
    ##    1 
    ## 
    ## $`330`
    ##      worst      being borderline     cancer      curse     justin 
    ##          2          1          1          1          1          1 
    ## 
    ## $`331`
    ## lhhhollywood 
    ##            1 
    ## 
    ## $`332`
    ##            gang            mess thatmanmansaray 
    ##               1               1               1 
    ## 
    ## $`333`
    ##        ago       days    dumbass        got       like        lol     looked 
    ##          1          1          1          1          1          1          1 
    ##  realizing responding       text 
    ##          1          1          1 
    ## 
    ## $`334`
    ##  attitude    brunch   matches modernday perfectly      psls     scary     tammy 
    ##         1         1         1         1         1         1         1         1 
    ##     think      were 
    ##         1         1 
    ## 
    ## $`335`
    ##    cant cuffing    find     get    hand     kno     lls     man   nigga  niggas 
    ##       1       1       1       1       1       1       1       1       1       1 
    ## 
    ## $`336`
    ##   nice really   sean taylor 
    ##      1      1      1      1 
    ## 
    ## $`337`
    ##                                          feedup 
    ##                                               1 
    ##                                           jimmy 
    ##                                               1 
    ##                                          niggas 
    ##                                               1 
    ##                                          posted 
    ##                                               1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082>waiting 
    ##                                               1 
    ## 
    ## $`338`
    ##           laugh           night <f0><ff><U+0098><U+00B9> 
    ##               1               1               1 
    ## 
    ## $`339`
    ## lhhhollywood         must          one       rating       really         show 
    ##            1            1            1            1            1            1 
    ##      talking 
    ##            1 
    ## 
    ## $`340`
    ##    anyone      else       for      game      hype       the tonighttt 
    ##         1         1         1         1         1         1         1 
    ## 
    ## $`341`
    ##      good     gotta      keep nationals      nats       win 
    ##         1         1         1         1         1         1 
    ## 
    ## $`342`
    ##                  can                 last                 like 
    ##                    1                    1                    1 
    ##      littlenaijagirl                 mean         nigerianinme 
    ##                    1                    1                    1 
    ##                share                slice <f0><ff><U+008D><U+0095> 
    ##                    1                    1                    1 
    ## 
    ## $`343`
    ##           another             bryce     brycegoesboom               day 
    ##                 1                 1                 1                 1 
    ##     game4herewego             great              home httptcoxehqphj0du 
    ##                 1                 1                 1                 1 
    ##              live          natitude 
    ##                 1                 1 
    ## 
    ## $`344`
    ##                  day                  lol                  one 
    ##                    1                    1                    1 
    ## <f0><ff><U+0098><U+008D> 
    ##                    1 
    ## 
    ## $`345`
    ##     alive      just      nats sloanesw7    stayin       won 
    ##         1         1         1         1         1         1 
    ## 
    ## $`346`
    ##       angry        come        here         raw rawbrooklyn        ring 
    ##           1           1           1           1           1           1 
    ##     rolling        seth         wwe wwebrooklyn 
    ##           1           1           1           1 
    ## 
    ## $`347`
    ##              beah            folger              gone httptcoxchdhlckpy 
    ##                 1                 1                 1                 1 
    ##           ishmeal           library     litlifeislife              long 
    ##                 1                 1                 1                 1 
    ## penfaulknergala14       shakespeare 
    ##                 1                 1 
    ## 
    ## $`348`
    ##  chic  fila   now right <U+2764><U+FE0F> 
    ##     1     1     1     1     1 
    ## 
    ## $`349`
    ## espnnfl     nah 
    ##       1       1 
    ## 
    ## $`350`
    ##   going    need     see weekend   whats 
    ##       1       1       1       1       1 
    ## 
    ## $`351`
    ## begining      new 
    ##        1        1 
    ## 
    ## $`352`
    ## angry  seth 
    ##     1     1 
    ## 
    ## $`353`
    ##           assisted             chrisp          firstever          headstand 
    ##                  1                  1                  1                  1 
    ## httpstcoevbnhbe7m3         makeitwork             thanks             tripod 
    ##                  1                  1                  1                  1 
    ##      vidafitnessdc         washington 
    ##                  1                  1 
    ## 
    ## $`354`
    ##                                                                     aint 
    ##                                                                        1 
    ##                                                                    money 
    ##                                                                        1 
    ##                                                                  talking 
    ##                                                                        1 
    ##                                                                    topic 
    ##                                                                        1 
    ## <f0><ff><U+0098><U+008F><f0><ff><U+0092><U+00B0><f0><ff><U+0092><U+00B0><U+270B><f0><ff><U+0091><U+009C> 
    ##                                                                        1 
    ## 
    ## $`355`
    ##               boy           hottest httptcov4uisapliy             white 
    ##                 1                 1                 1                 1 
    ## 
    ## $`356`
    ##            faggot httptcot5r76b2p9k              look              pink 
    ##                 1                 1                 1                 1 
    ##               smh           wearing 
    ##                 1                 1 
    ## 
    ## $`357`
    ##                tired <f0><ff><U+0098><U+0082> 
    ##                    1                    1 
    ## 
    ## $`358`
    ##                                                           got 
    ##                                                             2 
    ##                                                        snacks 
    ##                                                             1 
    ##                                    us<f0><ff><U+0098><U+0088> 
    ##                                                             1 
    ##                                                 “xlovingdessy 
    ##                                                             1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D>” 
    ##                                                             1 
    ## 
    ## $`359`
    ## 2stubbornlymine 
    ##               1 
    ## 
    ## $`360`
    ##                     cracked                        fizz 
    ##                           1                           1 
    ##                      gawwwd smiiiiiiiiiiiiiiiiiiilecoot 
    ##                           1                           1 
    ##                        when 
    ##                           1 
    ## 
    ## $`361`
    ##    late minutes  monday morning     was 
    ##       1       1       1       1       1 
    ## 
    ## $`362`
    ##     called eliminated       glad       know       must       nats      today 
    ##          1          1          1          1          1          1          1 
    ##        win 
    ##          1 
    ## 
    ## $`363`
    ##                                                         fine 
    ##                                                            1 
    ##                                                         fizz 
    ##                                                            1 
    ## <f0><ff><U+0098><U+00AB><f0><ff><U+0098><U+00AB><f0><ff><U+0098><U+00AB><f0><ff><U+0098><U+00AB> 
    ##                                                            1 
    ## 
    ## $`364`
    ##    ago    big  cents   fell    gas    now   past prices  story   this 
    ##      1      1      1      1      1      1      1      1      1      1 
    ## 
    ## $`365`
    ##  floating hilarious  lmaooooo 
    ##         1         1         1 
    ## 
    ## $`366`
    ##   anyone facetime     want 
    ##        1        1        1 
    ## 
    ## $`367`
    ##  make shine wanna 
    ##     1     1     1 
    ## 
    ## $`368`
    ##             got           moose          niggas          saying             why 
    ##               1               1               1               1               1 
    ##           years <f0><ff><U+0098><U+00B3> 
    ##               1               1 
    ## 
    ## $`369`
    ##                                                             bout 
    ##                                                                1 
    ##                                                             lets 
    ##                                                                1 
    ##                                                            skins 
    ##                                                                1 
    ## work<f0><ff><U+008F><U+0088><f0><ff><U+008F><U+0088><f0><ff><U+008F><U+0088> 
    ##                                                                1 
    ## 
    ## $`370`
    ##     hmm    many   movie tonight   watch 
    ##       1       1       1       1       1 
    ## 
    ## $`371`
    ##     baby natitude 
    ##        1        1 
    ## 
    ## $`372`
    ##         can        drew lukerussert     nothing      proves      sample 
    ##           1           1           1           1           1           1 
    ##        size 
    ##           1 
    ## 
    ## $`373`
    ##           redskins              fedex              field httpstcove4etgmqm6 
    ##                  2                  1                  1                  1 
    ##           landover                mnf           seahawks 
    ##                  1                  1                  1 
    ## 
    ## $`374`
    ##                                                         fizz 
    ##                                                            1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D> 
    ##                                                            1 
    ## 
    ## $`375`
    ## now<e2><U+00BF>    sucks      who 
    ##        1        1        1 
    ## 
    ## $`376`
    ## httptcow0lkhrt03g          midterms 
    ##                 1                 1 
    ## 
    ## $`377`
    ##          bilerico         celebrate               day             great 
    ##                 1                 1                 1                 1 
    ## httptcopobhp5j81p            jerame            lovely          marriage 
    ##                 1                 1                 1                 1 
    ##           overdue 
    ##                 1 
    ## 
    ## $`378`
    ##  ascentricly      eyebrow         game         mine rachelmorton        ryans 
    ##            1            1            1            1            1            1 
    ##         same    shayedavi     stronger 
    ##            1            1            1 
    ## 
    ## $`379`
    ##   bout   full   lawn    mow snakes   time   yard 
    ##      1      1      1      1      1      1      1 
    ## 
    ## $`380`
    ##    alive decisive     game     nats  putting   series  staying together 
    ##        1        1        1        1        1        1        1        1 
    ##      yes 
    ##        1 
    ## 
    ## $`381`
    ##         god 35bazillion  fimustauri        just        pick    rational 
    ##           2           1           1           1           1           1 
    ##         say       words 
    ##           1           1 
    ## 
    ## $`382`
    ##                                    booty 
    ##                                        1 
    ##                                   bubble 
    ##                                        1 
    ##                                    gotta 
    ##                                        1 
    ##                                   shawty 
    ##                                        1 
    ## <f0><ff><U+0091><U+0080><f0><ff><U+008D><U+0091> 
    ##                                        1 
    ## 
    ## $`383`
    ##              club             fedex             field httptco6xbtodahll 
    ##                 1                 1                 1                 1 
    ##       hyattsville             level 
    ##                 1                 1 
    ## 
    ## $`384`
    ##                                      ass 
    ##                                        1 
    ##                                      bae 
    ##                                        1 
    ##                                      chy 
    ##                                        1 
    ##                                    goofy 
    ##                                        1 
    ##                                      hey 
    ##                                        1 
    ##                                     know 
    ##                                        1 
    ##                              niggggggggg 
    ##                                        1 
    ## <f0><ff><U+0091><U+008B><f0><ff><U+0098><U+0082> 
    ##                                        1 
    ## <f0><ff><U+009C><U+00B4><f0><ff><U+008D><U+0092> 
    ##                                        1 
    ## 
    ## $`385`
    ##                                                 “officialaztec 
    ##                                                              2 
    ## faderegg2k14”fadeajay2k14”fadechubgod2k14”fadechubbyniggas2k14 
    ##                                                              1 
    ##                                                       “ajayyyy 
    ##                                                              1 
    ## 
    ## $`386`
    ## different      name       new     story      year 
    ##         1         1         1         1         1 
    ## 
    ## $`387`
    ##    best  friend hanging    miss 
    ##       1       1       1       1 
    ## 
    ## $`388`
    ##   redskins announcers    chicken       lets        say   tonights       will 
    ##          3          1          1          1          1          1          1 
    ##     wonder 
    ##          1 
    ## 
    ## $`389`
    ## masnnationals        thanks 
    ##             1             1 
    ## 
    ## $`390`
    ##     assume       dont  extremely irritating 
    ##          1          1          1          1 
    ## 
    ## $`391`
    ##              gift httptcongm7thpnlt               kia            motors 
    ##                 1                 1                 1                 1 
    ##               one            owning              sent             thank 
    ##                 1                 1                 1                 1 
    ##             three          vehicles 
    ##                 1                 1 
    ## 
    ## $`392`
    ##         fade myyownworldd        still          yet 
    ##            1            1            1            1 
    ## 
    ## $`393`
    ##     gym     nah    open playing 
    ##       1       1       1       1 
    ## 
    ## $`394`
    ##  ibelieve      make nationals     night  redskins      turn      your 
    ##         1         1         1         1         1         1         1 
    ## 
    ## $`395`
    ##               36m              face httptco1jpyxq1pqg          lockette 
    ##                 1                 1                 1                 1 
    ##          maryland     mondaynightfb               nfl           ricardo 
    ##                 1                 1                 1                 1 
    ##          seahawks      seattletimes 
    ##                 1                 1 
    ## 
    ## $`396`
    ##                                                                                                                           gorman8r 
    ##                                                                                                                                  1 
    ## <f0><ff><U+0098><U+0086><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                                                                                                  1 
    ## 
    ## $`397`
    ##      day tomorrow     work 
    ##        1        1        1 
    ## 
    ## $`398`
    ##           happens httptcoihvmsohqhq httptcoizoak29qtx          ibelieve 
    ##                 1                 1                 1                 1 
    ##              nats            pandas              play              this 
    ##                 1                 1                 1                 1 
    ## 
    ## $`399`
    ##       lmaooo mrdcsportssr      perfect        video 
    ##            1            1            1            1 
    ## 
    ## $`400`
    ##                                                            resentment 
    ##                                                                     1 
    ## shit<f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+008D><f0><ff><U+009E><U+00A7><f0><ff><U+009E><U+00B6> 
    ##                                                                     1 
    ## 
    ## $`401`
    ## baptizing   welcome 
    ##         1         1 
    ## 
    ## $`402`
    ##             drone          episodes          homeland httptcohatfj3kbum 
    ##                 1                 1                 1                 1 
    ##       perisphere”            queen”             recap            season 
    ##                 1                 1                 1                 1 
    ##          spyfest…            twisty 
    ##                 1                 1 
    ## 
    ## $`403`
    ##                                          and 
    ##                                            1 
    ##                                       feelin 
    ##                                            1 
    ##                                         have 
    ##                                            1 
    ##                                        jumps 
    ##                                            1 
    ##                                       niggas 
    ##                                            1 
    ## oomf<f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008F> 
    ##                                            1 
    ##                                          smh 
    ##                                            1 
    ##                                         some 
    ##                                            1 
    ##                                        tweet 
    ##                                            1 
    ##                                         type 
    ##                                            1 
    ## 
    ## $`404`
    ##         amp       ariel  especially        lmao         mom nishaaaaaaa 
    ##           1           1           1           1           1           1 
    ##   witnessed         yet 
    ##           1           1 
    ## 
    ## $`405`
    ##              fedx           feeeeel             field httptcorytlplt56i 
    ##                 1                 1                 1                 1 
    ##              lets          redskins          seahawks           stadium 
    ##                 1                 1                 1                 1 
    ## 
    ## $`406`
    ##               are               bad            before               end 
    ##                 1                 1                 1                 1 
    ##              fans              game httptcoeaquurrfht            insane 
    ##                 1                 1                 1                 1 
    ##              left           lengths 
    ##                 1                 1 
    ## 
    ## $`407`
    ##         gotta lahhhollywood          like       omarion     something 
    ##             1             1             1             1             1 
    ## 
    ## $`408`
    ##                                                          ass 
    ##                                                            1 
    ##                                                      smashed 
    ##                                                            1 
    ##                                                         they 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                            1 
    ## 
    ## $`409`
    ## black  haus 
    ##     1     1 
    ## 
    ## $`410`
    ##             broke             broom            giants      ibackthenats 
    ##                 1                 1                 1                 1 
    ##          ibelieve              just      jwerthsbeard         nationals 
    ##                 1                 1                 1                 1 
    ##          natitude nothingbutoctober 
    ##                 1                 1 
    ## 
    ## $`411`
    ## attitudes      make   peoples      some      ugly 
    ##         1         1         1         1         1 
    ## 
    ## $`412`
    ##               amphip                  hop                 love 
    ##                    1                    1                    1 
    ## <f0><ff><U+0098><U+0081> 
    ##                    1 
    ## 
    ## $`413`
    ##                                     back 
    ##                                        1 
    ##                               officially 
    ##                                        1 
    ##                                originals 
    ##                                        1 
    ## <f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+00A9> 
    ##                                        1 
    ## <f0><ff><U+0099><U+009C><f0><ff><U+0099><U+009C> 
    ##                                        1 
    ## 
    ## $`414`
    ##         dvnjr      entitled          feel          hmmm menshealthmag 
    ##             1             1             1             1             1 
    ##         soooo         women 
    ##             1             1 
    ## 
    ## $`415`
    ## attractive        bit       just      kinda        lil 
    ##          1          1          1          1          1 
    ## 
    ## $`416`
    ##              alive               game                get httptcoixecjlcweb” 
    ##                  1                  1                  1                  1 
    ##               huge       ibackthenats           natitude               nats 
    ##                  1                  1                  1                  1 
    ##               play            staying 
    ##                  1                  1 
    ## 
    ## $`417`
    ##        alright darnjesssssica        factory           good            hah 
    ##              1              1              1              1              1 
    ##            hat         really          wanna 
    ##              1              1              1 
    ## 
    ## $`418`
    ##               day httptcow03qrtmz27            pidgey pok<e3><U+00A9>mon 
    ##                 1                 1                 1                 1 
    ## 
    ## $`419`
    ## bonus   got today  work   wow 
    ##     1     1     1     1     1 
    ## 
    ## $`420`
    ##  aint  baby   cha fools  head   mad  play 
    ##     1     1     1     1     1     1     1 
    ## 
    ## $`421`
    ##  head  song stuck worst 
    ##     1     1     1     1 
    ## 
    ## $`422`
    ##            belize httptcobrnjjojtyl             pedro             place 
    ##                 1                 1                 1                 1 
    ##               san        takemeback           worries 
    ##                 1                 1                 1 
    ## 
    ## $`423`
    ##          everyday              fall         highlines httptcoanu4bgwb4c 
    ##                 1                 1                 1                 1 
    ##          nofilter          snowshoe          virginia              west 
    ##                 1                 1                 1                 1 
    ##             where              work 
    ##                 1                 1 
    ## 
    ## $`424`
    ##                         emojis <f0><ff><U+0093><U+00A5><f0><ff><U+0093><U+00B2> 
    ##                              1                              1 
    ## 
    ## $`425`
    ##  celtics     game   guards  jumpers   losing muchlong      nba  painful 
    ##        1        1        1        1        1        1        1        1 
    ##   season shooting 
    ##        1        1 
    ## 
    ## $`426`
    ## completely        day       love        one   stressed    without 
    ##          1          1          1          1          1          1 
    ## 
    ## $`427`
    ##       amp beautiful       boy      fizz      just    lmaooo       odd   omarion 
    ##         1         1         1         1         1         1         1         1 
    ##       one    soulja 
    ##         1         1 
    ## 
    ## $`428`
    ## pointlessblog 
    ##             1 
    ## 
    ## $`429`
    ##        eunyangnbc httptcovyexyuptsm 
    ##                 1                 1 
    ## 
    ## $`430`
    ## ashlaayraeee    condition     critical         good          lls         said 
    ##            1            1            1            1            1            1 
    ## 
    ## $`431`
    ##              april              fedex              field                got 
    ##                  1                  1                  1                  1 
    ##              great httpstcojveaws22mj  httptcof3lizlkxlz           landover 
    ##                  1                  1                  1                  1 
    ##           redskins              seats 
    ##                  1                  1 
    ## 
    ## $`432`
    ##    bryan   daniel     ever      get      god    gonna greatest      pop 
    ##        1        1        1        1        1        1        1        1 
    ##  returns     wait 
    ##        1        1 
    ## 
    ## $`433`
    ##     boy    dont     een    hang   likee    look niggass    show  soulja    them 
    ##       1       1       1       1       1       1       1       1       1       1 
    ## 
    ## $`434`
    ## erkyya   lost  phone 
    ##      1      1      1 
    ## 
    ## $`435`
    ##    else feeling   never  nobody   swear    will 
    ##       1       1       1       1       1       1 
    ## 
    ## $`436`
    ##            freakin httpstcoqfcegw6eq9               joes            palooza 
    ##                  1                  1                  1                  1 
    ##            pumpkin             trader         washington 
    ##                  1                  1                  1 
    ## 
    ## $`437`
    ## httptcotdofy6gmru              just         northside             photo 
    ##                 1                 1                 1                 1 
    ##            posted            social 
    ##                 1                 1 
    ## 
    ## $`438`
    ##  back hurts 
    ##     1     1 
    ## 
    ## $`439`
    ## braveeee    youre 
    ##        1        1 
    ## 
    ## $`440`
    ##        awwiebabyy          edmyakua httptcosscshgtfn2              lmao 
    ##                 1                 1                 1                 1 
    ##          maryroni              miss              much         secretary 
    ##                 1                 1                 1                 1 
    ##              sexy 
    ##                 1 
    ## 
    ## $`441`
    ##         bout        break         httr interception         kirk       record 
    ##            1            1            1            1            1            1 
    ##   warmupcolt 
    ##            1 
    ## 
    ## $`442`
    ## httptcoxq3i5ahmrs             obola 
    ##                 1                 1 
    ## 
    ## $`443`
    ##               12s             dream             fedex              from 
    ##                 1                 1                 1                 1 
    ##           gohawks httptco5thk0tbzca               mnf          seahawks 
    ##                 1                 1                 1                 1 
    ##             seats           seattle 
    ##                 1                 1 
    ## 
    ## $`444`
    ##                               brothers bruuuuhhhhhhhhhhhh<f0><ff><U+0098><U+0082> 
    ##                                      1                                      1 
    ##                      httptcoaosubonyxy                                  pants 
    ##                                      1                                      1 
    ##                                  tight 
    ##                                      1 
    ## 
    ## $`445`
    ##    back    come timeeee <U+2764><U+FE0F><U+26BE><U+FE0F><U+26BE><U+FE0F> 
    ##       1       1       1       1 
    ## 
    ## $`446`
    ##       b2rmusic     b2rwaynepa           lets           matt   mattmcandrew 
    ##              1              1              1              1              1 
    ##    nbcthevoice        tonight watchingblinds 
    ##              1              1              1 
    ## 
    ## $`447`
    ##      around        cool        guys         hey        late    lindczar 
    ##           1           1           1           1           1           1 
    ##   paleopunk paperskin22         qwq   streaming 
    ##           1           1           1           1 
    ## 
    ## $`448`
    ## bitch<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                             1 
    ##                                    qweenjiggy 
    ##                                             1 
    ##                  song<f0><ff><U+0092><U+0080> 
    ##                                             1 
    ## 
    ## $`449`
    ## friends     got    weak 
    ##       1       1       1 
    ## 
    ## $`450`
    ## getting   inked  winter 
    ##       1       1       1 
    ## 
    ## $`451`
    ##                                                                                                                                                                                                            african 
    ##                                                                                                                                                                                                                  1 
    ##                                                                                                                                                                                                     bakedchuchinii 
    ##                                                                                                                                                                                                                  1 
    ##                                                                                                                                                                                                                hot 
    ##                                                                                                                                                                                                                  1 
    ##                                                                                                                                                                                                 httptcomcw664fp2p” 
    ##                                                                                                                                                                                                                  1 
    ##                                                                                      nigga<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0092><U+0080> 
    ##                                                                                                                                                                                                                  1 
    ##                                                                                                                                                                                                     “drakethetypee 
    ##                                                                                                                                                                                                                  1 
    ## <f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD> 
    ##                                                                                                                                                                                                                  1 
    ## 
    ## $`452`
    ##          also brucejohnson9     candidate      growthdc      interest 
    ##             1             1             1             1             1 
    ##     markleedc          mean         polls      publicly      released 
    ##             1             1             1             1             1 
    ## 
    ## $`453`
    ## feelings     like   niggas    sound   startn 
    ##        1        1        1        1        1 
    ## 
    ## $`454`
    ##     hope natitude     nats  remains 
    ##        1        1        1        1 
    ## 
    ## $`455`
    ##          boy  consolation         hell          hey lhhhollywood         paid 
    ##            1            1            1            1            1            1 
    ##        prize       soulja        three         want 
    ##            1            1            1            1 
    ## 
    ## $`456`
    ##         gaia          hey intersection monicawilcox         much         name 
    ##            1            1            1            1            1            1 
    ##       novels     religion   technology       thanks 
    ##            1            1            1            1 
    ## 
    ## $`457`
    ## ugly 
    ##    1 
    ## 
    ## $`458`
    ##         bulls     extremely         hyped motherfucking        nation 
    ##             1             1             1             1             1 
    ## 
    ## $`459`
    ##       hell missmichy1        one        put       show 
    ##          1          1          1          1          1 
    ## 
    ## $`460`
    ##                                         disney 
    ##                                              1 
    ##                                      halloween 
    ##                                              1 
    ## movies<f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D> 
    ##                                              1 
    ## 
    ## $`461`
    ## left lets nats  win 
    ##    1    1    1    1 
    ## 
    ## $`462`
    ## dirtyyydiii         eye      forget     swollen 
    ##           1           1           1           1 
    ## 
    ## $`463`
    ##        can savmillerr       tell 
    ##          1          1          1 
    ## 
    ## $`464`
    ##       avi daddymiaa       lol       one 
    ##         1         1         1         1 
    ## 
    ## $`465`
    ##           bit      honestly         jacob        kevuhn         might 
    ##             1             1             1             1             1 
    ##        outfit         point teamfucksalot         think 
    ##             1             1             1             1 
    ## 
    ## $`466`
    ##      blown thoroughly 
    ##          1          1 
    ## 
    ## $`467`
    ## bionicbombshell          braids            lame  lizzslockeroom  musicisfr33dom 
    ##               1               1               1               1               1 
    ##             why 
    ##               1 
    ## 
    ## $`468`
    ##   aramesharash            big         enough           home  middleeastguy 
    ##              1              1              1              1              1 
    ##       nuisance olivernorthfnc        present           stay            yes 
    ##              1              1              1              1              1 
    ## 
    ## $`469`
    ## effective    lovers    really     today  virginia 
    ##         1         1         1         1         1 
    ## 
    ## $`470`
    ## forgetsaboutyou         genesis            girl       instagram             mcm 
    ##               1               1               1               1               1 
    ##            post 
    ##               1 
    ## 
    ## $`471`
    ## marciemarr        yea 
    ##          1          1 
    ## 
    ## $`472`
    ##      curlyw       great   hitheball   nationals    natitude stayfocused 
    ##           1           1           1           1           1           1 
    ##         win 
    ##           1 
    ## 
    ## $`473`
    ## koledragoo    perfect        wow 
    ##          1          1          1 
    ## 
    ## $`474`
    ##                                                           httptcohij1a7goqt 
    ##                                                                           1 
    ## <f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD> 
    ##                                                                           1 
    ## 
    ## $`475`
    ## everyone     hows 
    ##        1        1 
    ## 
    ## $`476`
    ##   anubis    house marathon   season teennick watching 
    ##        1        1        1        1        1        1 
    ## 
    ## $`477`
    ## benbernanke      called    chairman    couldn’t         due         fed 
    ##           1           1           1           1           1           1 
    ##        frmr         get     lending      market 
    ##           1           1           1           1 
    ## 
    ## $`478`
    ##        foto       poner         una etiquetarme    facebook        girl 
    ##           2           2           2           1           1           1 
    ##         lol        plis     portada     pudemos 
    ##           1           1           1           1 
    ## 
    ## $`479`
    ##                  freezing house<f0><ff><U+0098><U+0085>                 literally 
    ##                         1                         1                         1 
    ## 
    ## $`480`
    ##          cbelldba            chilis          grrlgeek httptcogca45cfog0 
    ##                 1                 1                 1                 1 
    ##              many               omg            recipe         sqlspouse 
    ##                 1                 1                 1                 1 
    ## 
    ## $`481`
    ## early going sleep 
    ##     1     1     1 
    ## 
    ## $`482`
    ## theres    way 
    ##      1      1 
    ## 
    ## $`483`
    ##              asaprozsahegyi welcome<f0><ff><U+0098><U+009A> 
    ##                           1                           1 
    ## 
    ## $`484`
    ##  annoying followers      just       one  realized 
    ##         1         1         1         1         1 
    ## 
    ## $`485`
    ##      amp     bees   coffee enjoying    fresh  friends      gal   ginger 
    ##        1        1        1        1        1        1        1        1 
    ##   grated     hits 
    ##        1        1 
    ## 
    ## $`486`
    ##                 feel                 just                 know 
    ##                    1                    1                    1 
    ##                wanna <f0><ff><U+0098><U+009C> 
    ##                    1                    1 
    ## 
    ## $`487`
    ##       anyway        bring         home ibackthenats     ibelieve          may 
    ##            1            1            1            1            1            1 
    ##     natitude      natsin5         team         well 
    ##            1            1            1            1 
    ## 
    ## $`488`
    ## alive   for  nats   now  were 
    ##     1     1     1     1     1 
    ## 
    ## $`489`
    ##     1500      500      bmw    bonus  dollars      get lifetime     till 
    ##        1        1        1        1        1        1        1        1 
    ##   vilife 
    ##        1 
    ## 
    ## $`490`
    ## actually     into     jerk      lol   turned 
    ##        1        1        1        1        1 
    ## 
    ## $`491`
    ##        already            but         tested thatbrownguyoo            you 
    ##              1              1              1              1              1 
    ## 
    ## $`492`
    ##      beating myyownworldd         part         play   savvyzeeee      trained 
    ##            1            1            1            1            1            1 
    ##         wife 
    ##            1 
    ## 
    ## $`493`
    ##          gotham        watching <f0><ff><U+0093><U+00BA> 
    ##               1               1               1 
    ## 
    ## $`494`
    ##              fedex              field httpstco7sjtlzsfwb            jerseys 
    ##                  1                  1                  1                  1 
    ##               look                new           redskins                sea 
    ##                  1                  1                  1                  1 
    ##           seahawks              shiny 
    ##                  1                  1 
    ## 
    ## $`495`
    ##   i’m tired 
    ##     1     1 
    ## 
    ## $`496`
    ## hightop1231        move         omg 
    ##           1           1           1 
    ## 
    ## $`497`
    ##                                                         aint 
    ##                                                            1 
    ##                                                        babby 
    ##                                                            1 
    ##                                                    kaymonell 
    ##                                                            1 
    ##                                                         lied 
    ##                                                            1 
    ##                                                        never 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                            1 
    ## 
    ## $`498`
    ##                  amp         bridalmarket              excited 
    ##                    1                    1                    1 
    ## tasselandtastemakers 
    ##                    1 
    ## 
    ## $`499`
    ##     evening     filling      judges   mealonez1 nbcthevoice        rule 
    ##           1           1           1           1           1           1 
    ##    teamadam      thanks        want         way 
    ##           1           1           1           1 
    ## 
    ## $`500`
    ##        get   ibelieve       nlds       play postseason      there   tomorrow 
    ##          1          1          1          1          1          1          1 
    ## 
    ## $`501`
    ##      ball       how   jewelry      lhhh       lol souljaboy 
    ##         1         1         1         1         1         1 
    ## 
    ## $`502`
    ## actually football    slays 
    ##        1        1        1 
    ## 
    ## $`503`
    ##        last     seasons      better        like        much nbcthevoice 
    ##           2           2           1           1           1           1 
    ##         one      season     singers       there 
    ##           1           1           1           1 
    ## 
    ## $`504`
    ## httptcosqcfa02zqi”          “cutebeds <f0><ff><U+0098><U+00BB> 
    ##                  1                  1                  1 
    ## 
    ## $`505`
    ##          baltimore     cardinaltavern httpstcorssjfllwgj 
    ##                  1                  1                  1 
    ## 
    ## $`506`
    ##                                                   chill 
    ##                                                       1 
    ##                                                   funny 
    ##                                                       1 
    ##                                                  really 
    ##                                                       1 
    ## <f0><ff><U+0090><U+00AA><f0><ff><U+0090><U+0089><f0><ff><U+0098><U+009A> 
    ##                                                       1 
    ## 
    ## $`507`
    ##       gaianism           hone         ideals     interested   monicawilcox 
    ##              1              1              1              1              1 
    ##          novel         really tracietnichols           true           want 
    ##              1              1              1              1              1 
    ## 
    ## $`508`
    ##  always closing    hgil 
    ##       1       1       1 
    ## 
    ## $`509`
    ##         smaller aacountyschools     achievement             avg      discipline 
    ##               2               1               1               1               1 
    ##            gaps           lower        national           rates            says 
    ##               1               1               1               1               1 
    ## 
    ## $`510`
    ##                  actually                     makes sense<f0><ff><U+0098><U+0082> 
    ##                         1                         1                         1 
    ##                      true                wills4lwen 
    ##                         1                         1 
    ## 
    ## $`511`
    ## httptcom90noie5kv 
    ##                 1 
    ## 
    ## $`512`
    ##             anything              capable <f0><ff><U+0098><U+0089> 
    ##                    1                    1                    1 
    ## 
    ## $`513`
    ## eayly   its 
    ##     1     1 
    ## 
    ## $`514`
    ##     choice”       girls        wear       weave “staypurpin 
    ##           1           1           1           1           1 
    ## 
    ## $`515`
    ## ibelievethatwewillwin                  nats 
    ##                     1                     1 
    ## 
    ## $`516`
    ##            banana        drewmagary httptcosjp8c9zgyw              mayo 
    ##                 1                 1                 1                 1 
    ##              nats          sandwich               the               won 
    ##                 1                 1                 1                 1 
    ## 
    ## $`517`
    ##  shit tired 
    ##     1     1 
    ## 
    ## $`518`
    ##          bet johnjharwood mikeviqueira     navista7 pamelaschuur          see 
    ##            1            1            1            1            1            1 
    ##     tomorrow          yes 
    ##            1            1 
    ## 
    ## $`519`
    ##   guess   hated thought     two     you 
    ##       1       1       1       1       1 
    ## 
    ## $`520`
    ## awwiebabyy     campus   edmyakua        get kariclaure       miss       this 
    ##          1          1          1          1          1          1          1 
    ##        too    weekend        wtf 
    ##          1          1          1 
    ## 
    ## $`521`
    ## abbygill1  yaaaasss 
    ##         1         1 
    ## 
    ## $`522`
    ##     ago charger contact    died    hard   hours     mia   phone   sorry    went 
    ##       1       1       1       1       1       1       1       1       1       1 
    ## 
    ## $`523`
    ##                                     making 
    ##                                          1 
    ##                                      tacos 
    ##                                          1 
    ## tonight<f0><ff><U+0099><U+009C><f0><ff><U+0092><U+00A3> 
    ##                                          1 
    ## 
    ## $`524`
    ## already     its october  sheesh 
    ##       1       1       1       1 
    ## 
    ## $`525`
    ## gemmaannestyles            love           queen 
    ##               1               1               1 
    ## 
    ## $`526`
    ## text 
    ##    1 
    ## 
    ## $`527`
    ## everywhere    looking       tell 
    ##          1          1          1 
    ## 
    ## $`528`
    ## amyambierotic         cindy     cindydees       message          sent 
    ##             1             1             1             1             1 
    ## 
    ## $`529`
    ##         bruh itsreiillyyy     jaybeerz         lord         trap 
    ##            1            1            1            1            1 
    ## 
    ## $`530`
    ##      also      cant   focused     great    harper      nats negatives  pitching 
    ##         1         1         1         1         1         1         1         1 
    ##    rendon    series 
    ##         1         1 
    ## 
    ## $`531`
    ## annieatanner15           girl           gods          great           loud 
    ##              1              1              1              1              1 
    ##           must          proud          share        stories           then 
    ##              1              1              1              1              1 
    ## 
    ## $`532`
    ##                                     baes 
    ##                                        1 
    ##                                  display 
    ##                                        1 
    ##                                 gorgeous 
    ##                                        1 
    ##                                    house 
    ##                                        1 
    ##                        httptcozhtn358dxb 
    ##                                        1 
    ##                                     shes 
    ##                                        1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D> 
    ##                                        1 
    ## 
    ## $`533`
    ##                                               longlivekatria 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0085><f0><ff><U+0091><U+0080> 
    ##                                                            1 
    ## 
    ## $`534`
    ##       also      candy     follow      going       hard hcftoronto       hugs 
    ##          1          1          1          1          1          1          1 
    ##    madonna        nov      peace 
    ##          1          1          1 
    ## 
    ## $`535`
    ## birthday<f0><ff><U+0098><U+009A><f0><ff><U+009E><U+009A><f0><ff><U+009E><U+0088> 
    ##                                                                    1 
    ##                                                                happy 
    ##                                                                    1 
    ##                                                             sammlite 
    ##                                                                    1 
    ## 
    ## $`536`
    ##             bakers httpstcofcebz2tn7u             mellow         mellowadmo 
    ##                  1                  1                  1                  1 
    ##           mushroom              pizza         washington 
    ##                  1                  1                  1 
    ## 
    ## $`537`
    ##              bane           colored            fuckin httptcoyaicgust1z 
    ##                 1                 1                 1                 1 
    ##             lynch          marshawn              mask             rasta 
    ##                 1                 1                 1                 1 
    ##         thesp0rts           warmups 
    ##                 1                 1 
    ## 
    ## $`538`
    ##        cohencosby           freedom               gym            health 
    ##                 1                 1                 1                 1 
    ## httptcorxsmk5rcpf         knowledge      letstalkreal    letstalkrealdc 
    ##                 1                 1                 1                 1 
    ##               ltr        soundtrack 
    ##                 1                 1 
    ## 
    ## $`539`
    ##                                          allen 
    ##                                              1 
    ## braids<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                              1 
    ##                                            got 
    ##                                              1 
    ##                                        iverson 
    ##                                              1 
    ##                                            rg3 
    ##                                              1 
    ## 
    ## $`540`
    ##   annoyed   getting    lowkey   lowkey”      word “majinjii 
    ##         1         1         1         1         1         1 
    ## 
    ## $`541`
    ## httptcoyhvvrqgymw               mcm 
    ##                 1                 1 
    ## 
    ## $`542`
    ##  loyal   aint  claim niggas really  these 
    ##      2      1      1      1      1      1 
    ## 
    ## $`543`
    ##                                          fizz 
    ##                                             1 
    ##                                          fuck 
    ##                                             1 
    ##                                           got 
    ##                                             1 
    ##                                        jordan 
    ##                                             1 
    ##                                         kinda 
    ##                                             1 
    ##                                 lahhhollywood 
    ##                                             1 
    ##                                          ones 
    ##                                             1 
    ##                                           the 
    ##                                             1 
    ## <f0><ff><U+0098><U+00B3><f0><ff><U+0098><U+00B3><f0><ff><U+0098><U+00B3> 
    ##                                             1 
    ## 
    ## $`544`
    ##                      boyfriend                           evil 
    ##                              1                              1 
    ##                          swear <f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+00A9> 
    ##                              1                              1 
    ## 
    ## $`545`
    ## fleek<f0><ff><U+0098><U+0085>                     games                       got 
    ##                         1                         1                         1 
    ##                  hairline                      hoes         httptcoe4bo20r6ap 
    ##                         1                         1                         1 
    ##             mixbreedlexie                     phone                     quick 
    ##                         1                         1                         1 
    ##                      real 
    ##                         1 
    ## 
    ## $`546`
    ##  annoying     apple       cry   fucking     keeps      next  spacebar squeaking 
    ##         1         1         1         1         1         1         1         1 
    ##     store      time 
    ##         1         1 
    ## 
    ## $`547`
    ##     fanboy       obey       real     scarce soarscares        you 
    ##          1          1          1          1          1          1 
    ## 
    ## $`548`
    ## football     httr     lets    ready redskins 
    ##        1        1        1        1        1 
    ## 
    ## $`549`
    ##      ann     cmon      not      now    seton tombomb7 
    ##        1        1        1        1        1        1 
    ## 
    ## $`550`
    ## allibaabyy       fake    friends       glad       life        lol        man 
    ##          1          1          1          1          1          1          1 
    ##    selfish 
    ##          1 
    ## 
    ## $`551`
    ##         agree brucejohnson9       carried      growthdc     markleedc 
    ##             1             1             1             1             1 
    ##          news nonaffiliated organizations        others         polls 
    ##             1             1             1             1             1 
    ## 
    ## $`552`
    ##       anytime     available         daddy freakngeek706 
    ##             1             1             1             1 
    ## 
    ## $`553`
    ##       anything          avoid           best disappointment         expect 
    ##              1              1              1              1              1 
    ##            the            way 
    ##              1              1 
    ## 
    ## $`554`
    ## <f0><ff><U+0099><U+008B><U+263A><U+FE0F> 
    ##                     1 
    ## 
    ## $`555`
    ## embora  muito triste    vai victor 
    ##      1      1      1      1      1 
    ## 
    ## $`556`
    ##                                                                        daddymiaa 
    ##                                                                                1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0094><f0><ff><U+0098><U+009D> 
    ##                                                                                1 
    ## 
    ## $`557`
    ##             never              band              even          favorite 
    ##                 2                 1                 1                 1 
    ##            girl’s httptcoad1c44kz4k              love            minute 
    ##                 1                 1                 1                 1 
    ##              she…             think 
    ##                 1                 1 
    ## 
    ## $`558`
    ##      brother     clueless          dad rramirezryan 
    ##            1            1            1            1 
    ## 
    ## $`559`
    ##             got             lol        traycare <f0><ff><U+0098><U+00A6> 
    ##               1               1               1               1 
    ## 
    ## $`560`
    ##           already              back        fedexfield         forfeited 
    ##                 1                 1                 1                 1 
    ##           heading httptcolxkr8mhskb            locker              room 
    ##                 1                 1                 1                 1 
    ## 
    ## $`561`
    ##                               bad                              feet 
    ##                                 1                                 1 
    ##                              glad                              home 
    ##                                 1                                 1 
    ##                            hungry                              hurt 
    ##                                 1                                 1 
    ##                              mood now<f0><ff><U+0098><U+00A1><f0><ff><U+0098><U+00A4> 
    ##                                 1                                 1 
    ##      shit<f0><ff><U+0098><U+00A0>                          standing 
    ##                                 1                                 1 
    ## 
    ## $`562`
    ##                                                                  littlenaijagirl 
    ##                                                                                1 
    ##                                                                     nigerianinme 
    ##                                                                                1 
    ## <f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092> 
    ##                                                                                1 
    ## 
    ## $`563`
    ##       favorite           show       thevoice           time watchingblinds 
    ##              1              1              1              1              1 
    ## 
    ## $`564`
    ##     baby    brand cadillac    drove      new theclash 
    ##        1        1        1        1        1        1 
    ## 
    ## $`565`
    ##          contact       faaaacckkk      farronafter        initiated 
    ##                1                1                1                1 
    ##              smh          telling wfarronelizabeth 
    ##                1                1                1 
    ## 
    ## $`566`
    ##                  ham    httptcolzgm72ggvd                quick 
    ##                    1                    1                    1 
    ##                  rey                right <f0><ff><U+0098><U+009E> 
    ##                    1                    1                    1 
    ## 
    ## $`567`
    ##   online   school shopping    sooth   stress 
    ##        1        1        1        1        1 
    ## 
    ## $`568`
    ## httptco16pryjdkhw           wizards 
    ##                 1                 1 
    ## 
    ## $`569`
    ##    definitely      favorite           one      pictures ximenaaaaaisa 
    ##             1             1             1             1             1 
    ## 
    ## $`570`
    ## somo <U+2764><U+FE0F> 
    ##    1    1 
    ## 
    ## $`571`
    ## awwiebabyy     crying   edmyakua  literally   maryroni        now        omg 
    ##          1          1          1          1          1          1          1 
    ##     please      right 
    ##          1          1 
    ## 
    ## $`572`
    ## omarionlawwwd     thickness          this 
    ##             1             1             1 
    ## 
    ## $`573`
    ##       anyone       enough       famous         good          idk         like 
    ##            1            1            1            1            1            1 
    ## melodictouch    mrbarreda    redicrite         shit 
    ##            1            1            1            1 
    ## 
    ## $`574`
    ##        game         got        next shanzwalshy 
    ##           1           1           1           1 
    ## 
    ## $`575`
    ##             10000        attractive              cute             girls 
    ##                 1                 1                 1                 1 
    ## httptcodh68cdobd3      lacemysneaks              uggs 
    ##                 1                 1                 1 
    ## 
    ## $`576`
    ##            1st        appears iamronniebanks        mention            now 
    ##              1              1              1              1              1 
    ##  qlovingronnie         states          topic       trending         trndnl 
    ##              1              1              1              1              1 
    ## 
    ## $`577`
    ##      dress    excited        get homecoming      soooo    tomorow       will 
    ##          1          1          1          1          1          1          1 
    ## 
    ## $`578`
    ## httptcokac4mspe8j               lt3           missing               now 
    ##                 1                 1                 1                 1 
    ##             right              twin 
    ##                 1                 1 
    ## 
    ## $`579`
    ##     good taquetoh 
    ##        1        1 
    ## 
    ## $`580`
    ##                 back              massage                  now 
    ##                    1                    1                    1 
    ##                  pay                right              someone 
    ##                    1                    1                    1 
    ##                 will <f0><ff><U+0092><U+0086> 
    ##                    1                    1 
    ## 
    ## $`581`
    ##               around           chargerday                 town 
    ##                    1                    1                    1 
    ## <f0><ff><U+009A><U+0099> 
    ##                    1 
    ## 
    ## $`582`
    ## ibackthenats     ibelieve        night      quietly         will 
    ##            1            1            1            1            1 
    ## 
    ## $`583`
    ##                 asap               frosty                 need 
    ##                    1                    1                    1 
    ## <f0><ff><U+0098><U+0082> 
    ##                    1 
    ## 
    ## $`584`
    ## everybody       few       for     girls       its    school      smhh     thats 
    ##         1         1         1         1         1         1         1         1 
    ## 
    ## $`585`
    ## literally    people     stand 
    ##         1         1         1 
    ## 
    ## $`586`
    ## everyone     fuck 
    ##        1        1 
    ## 
    ## $`587`
    ##         david         favor           get          life   maccarone96 
    ##             1             1             1             1             1 
    ## michel4beaute           pls           thx 
    ##             1             1             1 
    ## 
    ## $`588`
    ## dance gavin  like  talk 
    ##     2     1     1     1 
    ## 
    ## $`589`
    ##                                     baby 
    ##                                        1 
    ##                                  bitches 
    ##                                        1 
    ##                                 forehead 
    ##                                        1 
    ##                                   fuckin 
    ##                                        1 
    ##                                    glued 
    ##                                        1 
    ##                                    hairs 
    ##                                        1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0092><U+0080> 
    ##                                        1 
    ## 
    ## $`590`
    ##                                              alrantisym 
    ##                                                       1 
    ## <f8><U+00A7><f8><U+00A8><f9><U+0088><f8><U+00B1><f9><U+0086><f8><U+00AA><f8><U+00B3> 
    ##                                                       1 
    ## <f8><U+00A7><f8><U+00AD><f8><U+00AA><f8><U+00B1><f8><U+00A7><f9><U+0085><f9><U+009A> 
    ##                                                       1 
    ## <f8><U+00A7><f9><U+0084><f8><U+00AE><f8><U+00AF><f9><U+009A><f9><U+0088><f9><U+009A> 
    ##                                                       1 
    ##                    <f8><U+00A8><f9><U+009A><f9><U+0083> 
    ##                                                       1 
    ##        <f8><U+00AC><f9><U+0086><f8><U+00A7><f8><U+00A8> 
    ##                                                       1 
    ## <f9><U+0084><f9><U+0085><f8><U+00B9><f8><U+00A7><f9><U+0084><f9><U+009A> 
    ##                                                       1 
    ## <f9><U+0088><f8><U+00A7><f8><U+00AD><f8><U+00B4><f9><U+0086><f8><U+00A7> 
    ##                                                       1 
    ## 
    ## $`591`
    ##                  804                  but         marctrain001 
    ##                    1                    1                    1 
    ## <f0><ff><U+009E><U+0089> 
    ##                    1 
    ## 
    ## $`592`
    ##   back broski   home  least little 
    ##      1      1      1      1      1 
    ## 
    ## $`593`
    ##               game              alive           baseball               best 
    ##                  3                  1                  1                  1 
    ##               ever                get httptcoaetkwz91ma”               huge 
    ##                  1                  1                  1                  1 
    ##       ibackthenats               nats 
    ##                  1                  1 
    ## 
    ## $`594`
    ## drink   use 
    ##     1     1 
    ## 
    ## $`595`
    ##             2nd            best            burn            cant          corner 
    ##               1               1               1               1               1 
    ## deseanjackson11          league            wait 
    ##               1               1               1 
    ## 
    ## $`596`
    ##                               forever                                listen 
    ##                                     1                                     1 
    ##                               weekend                                weeknd 
    ##                                     1                                     1 
    ##                           “rosyceline <f0><ff><U+0098><U+008F><f0><ff><U+009E><U+00A7><U+2764><U+FE0F>” 
    ##                                     1                                     1 
    ## 
    ## $`597`
    ##  always   tired    back  depend forward     one    shit someone    step   steps 
    ##       2       2       1       1       1       1       1       1       1       1 
    ## 
    ## $`598`
    ##                     get                    name                   never 
    ##                       1                       1                       1 
    ##                   nigga                  tatted tho<f0><ff><U+0098><U+0092> 
    ##                       1                       1                       1 
    ## 
    ## $`599`
    ##   actually        can       guys  instagram   pictures       post pretending 
    ##          1          1          1          1          1          1          1 
    ##    realize    someone      steal 
    ##          1          1          1 
    ## 
    ## $`600`
    ## emotional  jaybeerz lightskin    niggah 
    ##         1         1         1         1 
    ## 
    ## $`601`
    ## another  credit deserve    give granted  harder    just  people   prove  reason 
    ##       1       1       1       1       1       1       1       1       1       1 
    ## 
    ## $`602`
    ##                games                  get               hockey 
    ##                    1                    1                    1 
    ##    httptcofptqyebrxk                 made                night 
    ##                    1                    1                    1 
    ##                 pick                 this <f0><ff><U+0098><U+008D> 
    ##                    1                    1                    1 
    ## 
    ## $`603`
    ##          alive dcmarkwhitaker fightinhydrant           four           game 
    ##              1              1              1              1              1 
    ##           haha      happiness   ibackthenats      nationals         season 
    ##              1              1              1              1              1 
    ## 
    ## $`604`
    ##            border              darn          delaware              fall 
    ##                 1                 1                 1                 1 
    ##              good             happy httptcojn0s6iecql            natgeo 
    ##                 1                 1                 1                 1 
    ##              penn            pretty 
    ##                 1                 1 
    ## 
    ## $`605`
    ## cute  hes  lol 
    ##    1    1    1 
    ## 
    ## $`606`
    ##                                     know 
    ##                                        1 
    ## <f0><ff><U+0098><U+008B><f0><ff><U+009C><U+008D> 
    ##                                        1 
    ## 
    ## $`607`
    ##   breweryommegang          drinking httptcobxmstesnoe          ommegang 
    ##                 1                 1                 1                 1 
    ##     publictrustdc             witte                 — 
    ##                 1                 1                 1 
    ## 
    ## $`608`
    ##                                                         babe 
    ##                                                            1 
    ##                                            httptcod3s9h6qano 
    ##                                                            1 
    ##                                                          mcm 
    ##                                                            1 
    ## <f0><ff><U+0099><U+009A><f0><ff><U+0099><U+009A><f0><ff><U+0098><U+008D> 
    ##                                                            1 
    ## 
    ## $`609`
    ## indiaeaton    inikkee    reaveta      right       time 
    ##          1          1          1          1          1 
    ## 
    ## $`610`
    ## homeopathicdana        hormesis             say       sceptiguy         support 
    ##               1               1               1               1               1 
    ##      tetenterre            they 
    ##               1               1 
    ## 
    ## $`611`
    ## hungry<f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092> 
    ##                                                                  1 
    ##                                                               make 
    ##                                                                  1 
    ##                                                               toni 
    ##                                                                  1 
    ##                                                              tryna 
    ##                                                                  1 
    ## 
    ## $`612`
    ##   always gorgeous  jasmine    lahhh     like     look    nikki princess 
    ##        1        1        1        1        1        1        1        1 
    ##     stay 
    ##        1 
    ## 
    ## $`613`
    ## httptco6cys1nhha3 
    ##                 1 
    ## 
    ## $`614`
    ##    amp  bathe boudda   call  night 
    ##      1      1      1      1      1 
    ## 
    ## $`615`
    ## abused    dog justin  owner rather    smh 
    ##      1      1      1      1      1      1 
    ## 
    ## $`616`
    ##       can      deep       get      once something     think   thought 
    ##         1         1         1         1         1         1         1 
    ## 
    ## $`617`
    ##                            bae              httptcosxtwx53ry1 
    ##                              1                              1 
    ## <f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00BB> 
    ##                              1 
    ## 
    ## $`618`
    ## makewards     shirt 
    ##         1         1 
    ## 
    ## $`619`
    ##                           both                         bottom 
    ##                              1                              1 
    ##                            eat                            fun 
    ##                              1                              1 
    ##                            idk                           like 
    ##                              1                              1 
    ##                           lips scclluurpp<f0><ff><U+0098><U+0081> 
    ##                              1                              1 
    ##                           slow                           suck 
    ##                              1                              1 
    ## 
    ## $`620`
    ##                                          forever 
    ##                                                1 
    ##                                             lied 
    ##                                                1 
    ## on<f0><ff><U+0098><U+00A1><f0><ff><U+0098><U+00A1><f0><ff><U+0098><U+00A3>” 
    ##                                                1 
    ##                                              tho 
    ##                                                1 
    ##                                        “lilswahh 
    ##                                                1 
    ## 
    ## $`621`
    ##                                                                               get 
    ##                                                                                 1 
    ##                                                                         hatpurley 
    ##                                                                                 1 
    ##                                                                               hbd 
    ##                                                                                 1 
    ##                                                                              hope 
    ##                                                                                 1 
    ##                                                                              lets 
    ##                                                                                 1 
    ##                                                                               mom 
    ##                                                                                 1 
    ##                                                                           patrick 
    ##                                                                                 1 
    ##                                                                            permit 
    ##                                                                                 1 
    ## <f0><ff><U+0092><U+009B><f0><ff><U+0092><U+0099><f0><ff><U+0092><U+009C><f0><ff><U+0092><U+009A><U+2764><U+FE0F> 
    ##                                                                                 1 
    ## 
    ## $`622`
    ##        alright        doggies indirectionstx 
    ##              1              1              1 
    ## 
    ## $`623`
    ##       aishadahira             group httptcoqjsnvyh3ph              like 
    ##                 1                 1                 1                 1 
    ##           someone              when 
    ##                 1                 1 
    ## 
    ## $`624`
    ##   got guess pizza 
    ##     1     1     1 
    ## 
    ## $`625`
    ##          can         hand        heavy         hold        looks mattsteele23 
    ##            1            1            1            1            1            1 
    ##  pickupiines 
    ##            1 
    ## 
    ## $`626`
    ##                                 idk                                lmao 
    ##                                   1                                   1 
    ## <f0><ff><U+0098><U+0088><f0><ff><U+0092><U+00B0> 
    ##                                   1 
    ## 
    ## $`627`
    ##        caltor ethanamitasty         qodba 
    ##             1             1             1 
    ## 
    ## $`628`
    ##    breasts everything       just       rosa 
    ##          1          1          1          1 
    ## 
    ## $`629`
    ##          finished          grateful httptcoi9yynzxt0s           iceland 
    ##                 1                 1                 1                 1 
    ##           photos…          pictures        processing          stunning 
    ##                 1                 1                 1                 1 
    ##              trip 
    ##                 1 
    ## 
    ## $`630`
    ## days<f0><ff><U+0098><U+00AB><f0><ff><U+0092><U+008D><f0><ff><U+0092><U+0091> 
    ##                                                           1 
    ##                                                      havent 
    ##                                                           1 
    ##                                                         him 
    ##                                                           1 
    ##                                                        seen 
    ##                                                           1 
    ## 
    ## $`631`
    ##           academy       forestville httptcoxe4etbadex          military 
    ##                 1                 1                 1                 1 
    ##          suitland 
    ##                 1 
    ## 
    ## $`632`
    ##  constantly  everything       great        make overanalyze       worry 
    ##           1           1           1           1           1           1 
    ## 
    ## $`633`
    ##           299           315           324          made       minutes 
    ##             1             1             1             1             1 
    ## qlovingronnie           rts        states         topic      trending 
    ##             1             1             1             1             1 
    ## 
    ## $`634`
    ##                burglar                 expect                  fully 
    ##                      1                      1                      1 
    ##                  homer                 kristy                   like 
    ##                      1                      1                      1 
    ##                  rizzz stopstophesalreadydead                   wind 
    ##                      1                      1                      1 
    ## 
    ## $`635`
    ##               133 httptco3bok3lcnge    iamronniebanks            impact 
    ##                 1                 1                 1                 1 
    ##         published     qlovingronnie               rts               the 
    ##                 1                 1                 1                 1 
    ##             trend            trndnl 
    ##                 1                 1 
    ## 
    ## $`636`
    ##         council        debate14 electgeorge2014           never            pass 
    ##               1               1               1               1               1 
    ##            says          school         smaller            will           wrong 
    ##               1               1               1               1               1 
    ## 
    ## $`637`
    ##       twitter       android          ipad        iphone qlovingronnie 
    ##             3             1             1             1             1 
    ##      top3apps 
    ##             1 
    ## 
    ## $`638`
    ##           1st        408627        became       mention        people 
    ##             1             1             1             1             1 
    ## qlovingronnie          seen         since         topic      trending 
    ##             1             1             1             1             1 
    ## 
    ## $`639`
    ##                 33m             baldwin                doug                face 
    ##                   1                   1                   1                   1 
    ##   httptcoaojjh1vdib            maryland mondaynightfootball            seahawks 
    ##                   1                   1                   1                   1 
    ##        seattletimes               warms 
    ##                   1                   1 
    ## 
    ## $`640`
    ##   change leeeeeet minnnnnd 
    ##        1        1        1 
    ## 
    ## $`641`
    ##     better    feeling      helps        hub sportsmans       stub    winning 
    ##          1          1          1          1          1          1          1 
    ##      xanax zimmermitt 
    ##          1          1 
    ## 
    ## $`642`
    ##             got            knew            lien           moose          niggas 
    ##               1               1               1               1               1 
    ##          saying         stupid”             why           years “headhunchonard 
    ##               1               1               1               1               1 
    ## 
    ## $`643`
    ##            can           chat          depth          email   monicawilcox 
    ##              2              1              1              1              1 
    ##          novel          skype           talk tracietnichols           want 
    ##              1              1              1              1              1 
    ## 
    ## $`644`
    ##          bar  blackfinndc    celebrate ibackthenats         just         like 
    ##            1            1            1            1            1            1 
    ##         nats          put         take         they 
    ##            1            1            1            1 
    ## 
    ## $`645`
    ##    contact       game        ill       info        one sabraham16      still 
    ##          1          1          1          1          1          1          1 
    ##      think 
    ##          1 
    ## 
    ## $`646`
    ## america bennett   bless     god   botch   cool”   didnt    look  lyrics    only 
    ##       2       2       2       2       1       1       1       1       1       1 
    ## 
    ## $`647`
    ## bored 
    ##     2 
    ## 
    ## $`648`
    ##         brick       geezaws nicolassizzle     otsutsukl 
    ##             1             1             1             1 
    ## 
    ## $`649`
    ## nigga claim 
    ##     2     1 
    ## 
    ## $`650`
    ## riuuuuh 
    ##       1 
    ## 
    ## $`651`
    ##     bout    cares holycash lmaooooo    looks  talking 
    ##        1        1        1        1        1        1 
    ## 
    ## $`652`
    ##                                         pants 
    ##                                             2 
    ##                                           can 
    ##                                             1 
    ##                                   christopher 
    ##                                             1 
    ##                                           fit 
    ##                                             1 
    ##                                       hunting 
    ##                                             1 
    ##                                           top 
    ##                                             1 
    ## <f0><ff><U+0098><U+00B3><f0><ff><U+0098><U+00B3><f0><ff><U+0098><U+00B3> 
    ##                                             1 
    ## 
    ## $`653`
    ##  duper  super wankin 
    ##      1      1      1 
    ## 
    ## $`654`
    ##            1st        appears        mention            now         states 
    ##              1              1              1              1              1 
    ## thankyouaustin          topic       trending         trndnl         united 
    ##              1              1              1              1              1 
    ## 
    ## $`655`
    ##   gets   lawd tumblr 
    ##      1      1      1 
    ## 
    ## $`656`
    ##               job              cath            health httptcoycokq3r7q6 
    ##                 2                 1                 1                 1 
    ##              jobs               lab             nurse           nursing 
    ##                 1                 1                 1                 1 
    ##           soliant            travel 
    ##                 1                 1 
    ## 
    ## $`657`
    ##  arms booty   now right veins 
    ##     1     1     1     1     1 
    ## 
    ## $`658`
    ##                                    comes 
    ##                                        1 
    ##                                     here 
    ##                                        1 
    ##                                     pics 
    ##                                        1 
    ##                                    puppy 
    ##                                        1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0099><U+0088> 
    ##                                        1 
    ## 
    ## $`659`
    ## brucejohnson9       catania        except      growthdc          gt30 
    ##             1             1             1             1             1 
    ##     markleedc     sedcscoop         stuck         wusa9          year 
    ##             1             1             1             1             1 
    ## 
    ## $`660`
    ## lhhhollywood         like        mafia        movaa         nnaa       shawdy 
    ##            1            1            1            1            1            1 
    ##       soundd          use 
    ##            1            1 
    ## 
    ## $`661`
    ## 9thwonder       idc      lmao       mom      plus       sit       son   traffic 
    ##         1         1         1         1         1         1         1         1 
    ##      want 
    ##         1 
    ## 
    ## $`662`
    ##              home httptcoblyp0d9ljt              much               now 
    ##                 1                 1                 1                 1 
    ##            pretty             right           walking             wmata 
    ##                 1                 1                 1                 1 
    ## 
    ## $`663`
    ##   care   just really    say   wish 
    ##      2      1      1      1      1 
    ## 
    ## $`664`
    ##                                    hickey 
    ##                                         1 
    ##                                       mom 
    ##                                         1 
    ##                                      neck 
    ##                                         1 
    ##                                       saw 
    ##                                         1 
    ##                                       say 
    ##                                         1 
    ##                                 “xxgeeexx 
    ##                                         1 
    ## <f0><ff><U+0098><U+0082>”<f0><ff><U+0092><U+0080> 
    ##                                         1 
    ## 
    ## $`665`
    ##            get      honeymoon            ill            lml            lol 
    ##              1              1              1              1              1 
    ##        married            one           take           want “yelowbonethug 
    ##              1              1              1              1              1 
    ## 
    ## $`666`
    ## favorite      get    gifts   greedy     time  winters     year 
    ##        1        1        1        1        1        1        1 
    ## 
    ## $`667`
    ##   910  gets   idk  weak whats 
    ##     1     1     1     1     1 
    ## 
    ## $`668`
    ## doziee99      eze     love     mama 
    ##        1        1        1        1 
    ## 
    ## $`669`
    ##         either           fine   monicawilcox            one tracietnichols 
    ##              1              1              1              1              1 
    ## 
    ## $`670`
    ##          big       course         dick         know lhhhollywood          ray 
    ##            1            1            1            1            1            1 
    ##        rayyy          wit 
    ##            1            1 
    ## 
    ## $`671`
    ##               corner                early    httptcoud593dwycq 
    ##                    1                    1                    1 
    ##                 late             mornings                night 
    ##                    1                    1                    1 
    ##              started     <U+26C5><U+FE0F> <f0><ff><U+009C><U+0099> 
    ##                    1                    1                    1 
    ## 
    ## $`672`
    ##        adams          bad          her lhhhollywood     morticia       mother 
    ##            1            1            1            1            1            1 
    ##         sooo        wanna 
    ##            1            1 
    ## 
    ## $`673`
    ## heem  txt 
    ##    1    1 
    ## 
    ## $`674`
    ##                 feelings                     like                   matter 
    ##                        1                        1                        1 
    ##                     much                   mutual shit<f0><ff><U+0098><U+0092> 
    ##                        1                        1                        1 
    ##                  someone 
    ##                        1 
    ## 
    ## $`675`
    ##                 bout                 frfr                 fuck 
    ##                    1                    1                    1 
    ##                  get           homecoming                  say 
    ##                    1                    1                    1 
    ##              smacked <f0><ff><U+0098><U+0090> 
    ##                    1                    1 
    ## 
    ## $`676`
    ##   jackson     match   sherman something      that   tonight     watch 
    ##         1         1         1         1         1         1         1 
    ## 
    ## $`677`
    ##             100am httptco5oznh3q18p             sucks              that 
    ##                 1                 1                 1                 1 
    ## 
    ## $`678`
    ##              fuck              give httptcoojl7lzxmkv              life 
    ##                 1                 1                 1                 1 
    ##            really               wiz 
    ##                 1                 1 
    ## 
    ## $`679`
    ## httptcot4gndocncv       yungeggroll 
    ##                 1                 1 
    ## 
    ## $`680`
    ##            football   httptco9rhkfjxjde   httptcoe0z6gmbrqn              monday 
    ##                   1                   1                   1                   1 
    ## mondaynightfootball                 nfl               night            redskins 
    ##                   1                   1                   1                   1 
    ##            seahawks 
    ##                   1 
    ## 
    ## $`681`
    ## accelerated       among         amp      cancer    conflict        east 
    ##           1           1           1           1           1           1 
    ##   increased         ive      middle mikedebonis 
    ##           1           1           1           1 
    ## 
    ## $`682`
    ##            a1day1               bad           bitches httptcown3egc3oa6 
    ##                 1                 1                 1                 1 
    ##              like            spirit             thing              week 
    ##                 1                 1                 1                 1 
    ## 
    ## $`683`
    ##       aka    anyone      care      food       get      join mcdonalds   nightly 
    ##         1         1         1         1         1         1         1         1 
    ##       run 
    ##         1 
    ## 
    ## $`684`
    ##                                    baker 
    ##                                        1 
    ##                                  running 
    ##                                        1 
    ##                                     surf 
    ##                                        1 
    ##                                treadmill 
    ##                                        1 
    ##                                   twiggy 
    ##                                        1 
    ##                                 watching 
    ##                                        1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008B> 
    ##                                        1 
    ## 
    ## $`685`
    ## along child civil crazy  even   get  sake 
    ##     1     1     1     1     1     1     1 
    ## 
    ## $`686`
    ## httptcoxhqul7anhe 
    ##                 1 
    ## 
    ## $`687`
    ##           ima soufsidevonny          talk      tomorrow 
    ##             1             1             1             1 
    ## 
    ## $`688`
    ## enemies  fuckin  niggas   these 
    ##       1       1       1       1 
    ## 
    ## $`689`
    ##   many photos 
    ##      1      1 
    ## 
    ## $`690`
    ## boutaaa  friend   kersh   stamp yeaaaaa 
    ##       1       1       1       1       1 
    ## 
    ## $`691`
    ## built  lhhh   lls  momz  what   yes 
    ##     1     1     1     1     1     1 
    ## 
    ## $`692`
    ## ready 
    ##     1 
    ## 
    ## $`693`
    ##       bigcitymoms biggestbabyshower              wish 
    ##                 1                 1                 1 
    ## 
    ## $`694`
    ##                 menace                   dont on<f0><ff><U+0099><U+009C> 
    ##                      2                      1                      1 
    ##                society 
    ##                      1 
    ## 
    ## $`695`
    ##                360        classkiller          classpass httpstcoj5kixnxq86 
    ##                  1                  1                  1                  1 
    ##             sculpt           sculptdc              tried         washington 
    ##                  1                  1                  1                  1 
    ## 
    ## $`696`
    ## disgust     you 
    ##       1       1 
    ## 
    ## $`697`
    ##   innings   bigdata    giants nationals outscored      runs    scored       the 
    ##         2         1         1         1         1         1         1         1 
    ## 
    ## $`698`
    ##  alike   just  looks mother  nikki 
    ##      1      1      1      1      1 
    ## 
    ## $`699`
    ##       and  19881990      blue    campus    dinner     fives graduated       guy 
    ##         2         1         1         1         1         1         1         1 
    ##      high   mention 
    ##         1         1 
    ## 
    ## $`700`
    ##                                                     teamryan 
    ##                                                            1 
    ##                                                   thevoiceth 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0081><f0><ff><U+0098><U+0081><f0><ff><U+0098><U+0081> 
    ##                                                            1 
    ## 
    ## $`701`
    ##  pick  spot sweet third  yess 
    ##     1     1     1     1     1 
    ## 
    ## $`702`
    ## 98rocknitb       good    heading       mike     should       time 
    ##          1          1          1          1          1          1 
    ## 
    ## $`703`
    ##              azz defensehopefully       impressive              ish 
    ##                1                1                1                1 
    ##             kick             lets             like              mnf 
    ##                1                1                1                1 
    ##              our             puts 
    ##                1                1 
    ## 
    ## $`704`
    ##         isnt      netflix       season supernatural          why 
    ##            1            1            1            1            1 
    ## 
    ## $`705`
    ## httptco5eggq7ok8n 
    ##                 1 
    ## 
    ## $`706`
    ## awwiebabyy   edmyakua kariclaure        yas 
    ##          1          1          1          1 
    ## 
    ## $`707`
    ##         dumb       people toricaitlinn 
    ##            1            1            1 
    ## 
    ## $`708`
    ##       dreams         girl itsreiillyyy     jaybeerz        white         yeet 
    ##            1            1            1            1            1            1 
    ##        young 
    ##            1 
    ## 
    ## $`709`
    ##          damascus          cellular        connection httptcooapslrkp6z 
    ##                 2                 1                 1                 1 
    ##               job              jobs           manager             sales 
    ##                 1                 1                 1                 1 
    ##             store        tccverizon 
    ##                 1                 1 
    ## 
    ## $`710`
    ## ebnoxi0us    gotchu 
    ##         1         1 
    ## 
    ## $`711`
    ##      got      hey natitude      one 
    ##        1        1        1        1 
    ## 
    ## $`712`
    ##           chicken           dcbarno       dcsportsbog            forget 
    ##                 1                 1                 1                 1 
    ## httptco5gfg4ciinh         nationals              sage       scottsallen 
    ##                 1                 1                 1                 1 
    ## 
    ## $`713`
    ##      bag      get    gotta hospital   packed     soon 
    ##        1        1        1        1        1        1 
    ## 
    ## $`714`
    ## actually    didnt  dislike   forget    forso    funny     like     long 
    ##        1        1        1        1        1        1        1        1 
    ##   people  reasons 
    ##        1        1 
    ## 
    ## $`715`
    ##  almost  aspect changed   crazy   every    feel     its    life    like    past 
    ##       1       1       1       1       1       1       1       1       1       1 
    ## 
    ## $`716`
    ##       batman       coming       gotham    halloween        smile       tweeps 
    ##            1            1            1            1            1            1 
    ## whysoserious 
    ##            1 
    ## 
    ## $`717`
    ##      cause       city       dont       know scandalous      trust 
    ##          1          1          1          1          1          1 
    ## 
    ## $`718`
    ##            does dominiquearamos            even          famous             get 
    ##               1               1               1               1               1 
    ##          number           still <f0><ff><U+0098><U+00AC> 
    ##               1               1               1 
    ## 
    ## $`719`
    ##             ahora   carlosramirezl3             corte               dio 
    ##                 1                 1                 1                 1 
    ##              eeuu               gay httptcoplhg16nkoa               luz 
    ##                 1                 1                 1                 1 
    ##        matrimonio     rosaslissette 
    ##                 1                 1 
    ## 
    ## $`720`
    ##  bumping     card    chris      fun gamelets      got      kid     make 
    ##        1        1        1        1        1        1        1        1 
    ##   mcghan     next 
    ##        1        1 
    ## 
    ## $`721`
    ##      ate   cheese  evening     girl    gross     kind majority     much 
    ##        1        1        1        1        1        1        1        1 
    ##      new      oct 
    ##        1        1 
    ## 
    ## $`722`
    ## collegegameday            dak 
    ##              1              1 
    ## 
    ## $`723`
    ##       better         know       mother zeaddddddddd 
    ##            1            1            1            1 
    ## 
    ## $`724`
    ##                                                         good 
    ##                                                            1 
    ##                                                         nats 
    ##                                                            1 
    ##                                                        those 
    ##                                                            1 
    ##                                                          wow 
    ##                                                            1 
    ##                                             <U+2764><U+26BE> 
    ##                                                            1 
    ##                                     <f0><ff><U+0092><U+0083> 
    ##                                                            1 
    ## <f0><ff><U+0099><U+009C><f0><ff><U+0092><U+0083><f0><ff><U+0099><U+009C> 
    ##                                                            1 
    ## 
    ## $`725`
    ## <f0><ff><U+0098><U+00B4><f0><ff><U+0098><U+00B4> 
    ##                              1 
    ## 
    ## $`726`
    ## lifeofdess 
    ##          1 
    ## 
    ## $`727`
    ## fine<U+2764><U+FE0F> maugst8 squints 
    ##       1       1       1 
    ## 
    ## $`728`
    ##            1st          78178         became        mention         people 
    ##              1              1              1              1              1 
    ##           seen          since thankyouaustin          topic       trending 
    ##              1              1              1              1              1 
    ## 
    ## $`729`
    ##           days           made            rts         states thankyouaustin 
    ##              1              1              1              1              1 
    ##          topic       trending         trndnl         tweets         united 
    ##              1              1              1              1              1 
    ## 
    ## $`730`
    ##        twitter        android         client         iphone thankyouaustin 
    ##              3              1              1              1              1 
    ##       top3apps            web 
    ##              1              1 
    ## 
    ## $`731`
    ## httptcooiixekde6g           message            repair              this 
    ##                 1                 1                 1                 1 
    ##              time 
    ##                 1 
    ## 
    ## $`732`
    ##      sum1     think       cus      else insulting   insults   nothing something 
    ##         2         2         1         1         1         1         1         1 
    ##   telling       way 
    ##         1         1 
    ## 
    ## $`733`
    ##                                                         aint 
    ##                                                            1 
    ##                                                        freak 
    ##                                                            1 
    ##                                                          gas 
    ##                                                            1 
    ##                                                        ready 
    ##                                                            1 
    ##                                                         road 
    ##                                                            1 
    ##                                                         side 
    ##                                                            1 
    ## <f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0098><U+0082> 
    ##                                                            1 
    ##             <f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+00A9> 
    ##                                                            1 
    ## 
    ## $`734`
    ## <f0><ff><U+0098><U+0082>    httptco4fz7ryhrnb            mooreofme 
    ##                    4                    1                    1 
    ##             somebody            unfollows             wayneljr 
    ##                    1                    1                    1 
    ##                 when 
    ##                    1 
    ## 
    ## $`735`
    ##         because    doseresponse         drawing        evidence            free 
    ##               1               1               1               1               1 
    ##            hand homeopathicdana        relation       sceptiguy      tetenterre 
    ##               1               1               1               1               1 
    ## 
    ## $`736`
    ##                                                dream 
    ##                                                    1 
    ##                                                  its 
    ##                                                    1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0099><U+0088>cullentaylor 
    ##                                                    1 
    ## 
    ## $`737`
    ##       fuck irrelevant   starting    wizards 
    ##          1          1          1          1 
    ## 
    ## $`738`
    ##                     katie                      love sarah<f0><ff><U+0092><U+009A> 
    ##                         1                         1                         1 
    ## 
    ## $`739`
    ## lahhhollywood          love         nikki 
    ##             1             1             1 
    ## 
    ## $`740`
    ##         630am           amp       another           day           got 
    ##             1             1             1             1             1 
    ##           job         miles productiveday           ran        school 
    ##             1             1             1             1             1 
    ## 
    ## $`741`
    ##  allison     died remember     teen     wolf 
    ##        1        1        1        1        1 
    ## 
    ## $`742`
    ## bitches    aint     and   basic     but    club    lost    ones  saying    shit 
    ##       2       1       1       1       1       1       1       1       1       1 
    ## 
    ## $`743`
    ## aint  got type 
    ##    1    1    1 
    ## 
    ## $`744`
    ##       back     deargo   dribbles fimustauri   happyill       send    smother 
    ##          1          1          1          1          1          1          1 
    ##      tease        til       want 
    ##          1          1          1 
    ## 
    ## $`745`
    ##         getting             god            help        question           ready 
    ##               1               1               1               1               1 
    ##            root           skins sportaholicsusa 
    ##               1               1               1 
    ## 
    ## $`746`
    ## elevators   existed    people    phones     smart    wonder 
    ##         1         1         1         1         1         1 
    ## 
    ## $`747`
    ##     chinese     feeling lightheaded    nauseous        soup       still 
    ##           1           1           1           1           1           1 
    ## 
    ## $`748`
    ##   fucking     idgaf      lets preseason   wizards 
    ##         1         1         1         1         1 
    ## 
    ## $`749`
    ##  babyy better    can   much  nikki    soo 
    ##      1      1      1      1      1      1 
    ## 
    ## $`750`
    ##                aww chickfilaisthebest  happylatebirthday          raykane13 
    ##                  1                  1                  1                  1 
    ##              yummy 
    ##                  1 
    ## 
    ## $`751`
    ##      attempt        draft        dream        going pennsocialdc        phone 
    ##            1            1            1            1            1            1 
    ## 
    ## $`752`
    ## everyone      you 
    ##        1        1 
    ## 
    ## $`753`
    ##                        cold                        hope 
    ##                           1                           1 
    ##                     outside tonight<f0><ff><U+0098><U+0092> 
    ##                           1                           1 
    ## 
    ## $`754`
    ## long  not  now 
    ##    1    1    1 
    ## 
    ## $`755`
    ##        mind    creating  meditation mindfulness     perfect      rather 
    ##           2           1           1           1           1           1 
    ##       relax    relaxing    whatever 
    ##           1           1           1 
    ## 
    ## $`756`
    ##         para cu<e3><U+00A1>ndo         esos    fortaleza      notiuno     preparar 
    ##            2            1            1            1            1            1 
    ## prpdnoticias      rumores r<e3><U+00AD>o        salen 
    ##            1            1            1            1 
    ## 
    ## $`757`
    ## fun<f0><ff><U+0092><U+0081><f0><ff><U+0098><U+009C> 
    ##                                           1 
    ##                                       gonna 
    ##                                           1 
    ##                                        this 
    ##                                           1 
    ##                                     weekend 
    ##                                           1 
    ## 
    ## $`758`
    ##                                                     birthday 
    ##                                                            1 
    ##                                                        happy 
    ##                                                            1 
    ##                                                 lovekisses18 
    ##                                                            1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+009E><U+0089><f0><ff><U+009E><U+0081> 
    ##                                                            1 
    ## 
    ## $`759`
    ##   actually   baseball    believe      hoops    matters mikememoli     thefix 
    ##          1          1          1          1          1          1          1 
    ## 
    ## $`760`
    ##              bridges              burning                 long 
    ##                    1                    1                    1 
    ##                  mad                  ppl                  run 
    ##                    1                    1                    1 
    ## <f0><ff><U+0091><U+0090> 
    ##                    1 
    ## 
    ## $`761`
    ##   boythatsmirah           phone       sometimes           texts           threw 
    ##               1               1               1               1               1 
    ## <f0><ff><U+0098><U+00A9> 
    ##               1 
    ## 
    ## $`762`
    ## buckhantz   excited       for     steve 
    ##         1         1         1         1 
    ## 
    ## $`763`
    ##     cause clamyetti      fire      fuck   jwalt96   moltres      type 
    ##         1         1         1         1         1         1         1 
    ## 
    ## $`764`
    ##     its  monday    need     now weekend 
    ##       1       1       1       1       1 
    ## 
    ## $`765`
    ##                                    baker 
    ##                                        1 
    ##                               motivation 
    ##                                        1 
    ##                                     surf 
    ##                                        1 
    ##                                treadmill 
    ##                                        1 
    ##                                   twiggy 
    ##                                        1 
    ##                                 watching 
    ##                                        1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008B> 
    ##                                        1 
    ## 
    ## $`766`
    ##                                     bipolar 
    ##                                           1 
    ##                                         hes 
    ##                                           1 
    ## lls<f0><ff><U+0098><U+0082><f0><ff><U+0099><U+0088> 
    ##                                           1 
    ##                                        sooo 
    ##                                           1 
    ## 
    ## $`767`
    ##           chicken              fear httptcodnrf8ryirq           urchkin 
    ##                 1                 1                 1                 1 
    ##         willstick 
    ##                 1 
    ## 
    ## $`768`
    ## beautiful<f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+00A9> 
    ##                                                      1 
    ##                                                    boy 
    ##                                                      1 
    ##                                                    fav 
    ##                                                      1 
    ##                                                    omg 
    ##                                                      1 
    ##                                                    one 
    ##                                                      1 
    ##                                                singing 
    ##                                                      1 
    ##                                                  songs 
    ##                                                      1 
    ##                                                  sound 
    ##                                                      1 
    ##                                                  voice 
    ##                                                      1 
    ## 
    ## $`769`
    ##                  get                payed             tomorrow 
    ##                    1                    1                    1 
    ## <f0><ff><U+0092><U+0081> 
    ##                    1 
    ## 
    ## $`770`
    ##     congressional              gets             golf… httptco6h0q5bahvh 
    ##                 2                 1                 1                 1 
    ##             never               old            onsite             today 
    ##                 1                 1                 1                 1 
    ## tridentimagegroup 
    ##                 1 
    ## 
    ## $`771`
    ##         franfisto      ibackthenats nothingbutoctober               san 
    ##                 1                 1                 1                 1 
    ##             treat 
    ##                 1 
    ## 
    ## $`772`
    ##               game              alive           baseball               best 
    ##                  2                  1                  1                  1 
    ##               ever                get httptcoaetkwz91ma”               huge 
    ##                  1                  1                  1                  1 
    ##       ibackthenats               nats 
    ##                  1                  1 
    ## 
    ## $`773`
    ##   get  just wanna 
    ##     1     1     1 
    ## 
    ## $`774`
    ##  barbarianlogic          gotham     interesting mckeevemichelle 
    ##               1               1               1               1 
    ## 
    ## $`775`
    ##   always   chicks    money moroccan     seem 
    ##        1        1        1        1        1 
    ## 
    ## $`776`
    ## attention      dont     dress       get    trashy 
    ##         1         1         1         1         1 
    ## 
    ## $`777`
    ## clothes   coven    want 
    ##       1       1       1 
    ## 
    ## $`778`
    ##                                      dat 
    ##                                        1 
    ##                                     hell 
    ##                                        1 
    ##                                     need 
    ##                                        1 
    ##                                     talk 
    ##                                        1 
    ##                                     text 
    ##                                        1 
    ##                            yothatslexxxo 
    ##                                        1 
    ##                                    you”k 
    ##                                        1 
    ##                                “xxgeeexx 
    ##                                        1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                        1 
    ## 
    ## $`779`
    ## boardcast   chenier      dude      hell      phil      team     where   wizards 
    ##         1         1         1         1         1         1         1         1 
    ## 
    ## $`780`
    ##         lt3        okay veravulture 
    ##           1           1           1 
    ## 
    ## $`781`
    ##              emma          faulkner            folger httptcohwqt38y2hb 
    ##                 1                 1                 1                 1 
    ##           library     litlifeislife               pen penfaulknergala14 
    ##                 1                 1                 1                 1 
    ##       shakespeare             staff 
    ##                 1                 1 
    ## 
    ## $`782`
    ## fucking goooooo    lets 
    ##       1       1       1 
    ## 
    ## $`783`
    ##                                                                                                think 
    ##                                                                                                    2 
    ##                                                                                                 zeta 
    ##                                                                                                    2 
    ##                                                                                                alpha 
    ##                                                                                                    1 
    ##                                                                                    httptcogf68vpronb 
    ##                                                                                                    1 
    ##                                                                                                 pink 
    ##                                                                                                    1 
    ##                                                                                             supports 
    ##                                                                                                    1 
    ## tatas<f0><ff><U+0092><U+0095><f0><ff><U+009E><U+0080><f0><ff><U+0091><U+00AF><f0><ff><U+0091><U+0091><f0><ff><U+008F><U+0088> 
    ##                                                                                                    1 
    ##                                                                                                  tau 
    ##                                                                                                    1 
    ## 
    ## $`784`
    ## lowkeyyyyy   majinjii 
    ##          1          1 
    ## 
    ## $`785`
    ##    clothes       espn fatshaming      fully        fun      great       guys 
    ##          1          1          1          1          1          1          1 
    ##    jaguars        job     making 
    ##          1          1          1 
    ## 
    ## $`786`
    ## makaveliix        win       yall 
    ##          1          1          1 
    ## 
    ## $`787`
    ##      espn inspiring      love       mnf      nice     panel       pro   returns 
    ##         1         1         1         1         1         1         1         1 
    ##       see       she 
    ##         1         1 
    ## 
    ## $`788`
    ##                                    tired 
    ##                                        1 
    ## <f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092> 
    ##                                        1 
    ## 
    ## $`789`
    ##      doe     dont     even      get   sneeze squeezin      tha 
    ##        1        1        1        1        1        1        1 
    ## 
    ## $`790`
    ##       alike eiramarreis       great  hahahahaaa       minds       think 
    ##           1           1           1           1           1           1 
    ## 
    ## $`791`
    ##           average          european httptco67ybfvrngp       interesting 
    ##                 1                 1                 1                 1 
    ##         languages     michelamasson            number        population 
    ##                 1                 1                 1                 1 
    ##            spoken             union 
    ##                 1                 1 
    ## 
    ## $`792`
    ## nothing    talk    wish   wrong 
    ##       1       1       1       1 
    ## 
    ## $`793`
    ##             can forgetsaboutyou            mean             til            wait 
    ##               1               1               1               1               1 
    ##             wcw       wednesday 
    ##               1               1 
    ## 
    ## $`794`
    ##                        welovegiirlzz                                  amp 
    ##                                    2                                    1 
    ##                    httptcouwqs3d4dmr                                  pic 
    ##                                    1                                    1 
    ##                                quote                              samisul 
    ##                                    1                                    1 
    ##                               skinny                                thick 
    ##                                    1                                    1 
    ##                                 will <f0><ff><U+0091><U+008F><U+2764><U+FE0F><f0><ff><U+0093><U+00B7> 
    ##                                    1                                    1 
    ## 
    ## $`795`
    ##        fedexfield          football httptcob5dtq0ywjl            monday 
    ##                 1                 1                 1                 1 
    ##              nats             night               win 
    ##                 1                 1                 1 
    ## 
    ## $`796`
    ##             birthday                  boy          coryescobar 
    ##                    1                    1                    1 
    ##                  lol                today                  wyd 
    ##                    1                    1                    1 
    ## <f0><ff><U+0098><U+009C> 
    ##                    1 
    ## 
    ## $`797`
    ##      100 attitude     fast     from     that     went 
    ##        1        1        1        1        1        1 
    ## 
    ## $`798`
    ##   big bills cause  face faces   fix 
    ##     1     1     1     1     1     1 
    ## 
    ## $`799`
    ##             alert httptco0yx2lhoflq httptcolcaeh6twol              more 
    ##                 1                 1                 1                 1 
    ##             trend            trends            trndnl         vogelsong 
    ##                 1                 1                 1                 1 
    ## 
    ## $`800`
    ##             alert             curly httptcolcaeh6twol httptcovtw4odemak 
    ##                 1                 1                 1                 1 
    ##              more             trend            trends            trndnl 
    ##                 1                 1                 1                 1 
    ## 
    ## $`801`
    ##   goat justin rather    smh 
    ##      1      1      1      1 
    ## 
    ## $`802`
    ##             alert            gerald httptcodvdk5fhfvj httptcolcaeh6twol 
    ##                 1                 1                 1                 1 
    ##              more             trend            trends            trndnl 
    ##                 1                 1                 1                 1 
    ##           wallace 
    ##                 1 
    ## 
    ## $`803`
    ##             alert              game httptcoc73ahgdwqv httptcolcaeh6twol 
    ##                 1                 1                 1                 1 
    ##              more             trend            trends            trndnl 
    ##                 1                 1                 1                 1 
    ## 
    ## $`804`
    ##  broadcast      bulls    comcast gunitradio      shown  sportsnet      weird 
    ##          1          1          1          1          1          1          1 
    ## 
    ## $`805`
    ## bitches    love     all     bad    beat  better   faces     hip     hop    make 
    ##       2       2       1       1       1       1       1       1       1       1 
    ## 
    ## $`806`
    ##             alert httptcolcaeh6twol httptcoud5b63svme              more 
    ##                 1                 1                 1                 1 
    ##    thankyouaustin             trend            trends            trndnl 
    ##                 1                 1                 1                 1 
    ## 
    ## $`807`
    ##     building     everyone        front        leave savageleek16         seen 
    ##            1            1            1            1            1            1 
    ## 
    ## $`808`
    ## feel hell like 
    ##    1    1    1 
    ## 
    ## $`809`
    ##  joinin     lil sisters    stay 
    ##       1       1       1       1 
    ## 
    ## $`810`
    ## bitch   day  dude   leg today 
    ##     1     1     1     1     1 
    ## 
    ## $`811`
    ##   despite       gas impending      love      much 
    ##         1         1         1         1         1 
    ## 
    ## $`812`
    ##     eenytsed embarrassing         just       loving        never         show 
    ##            1            1            1            1            1            1 
    ##         stop      stopped       trying         will 
    ##            1            1            1            1 
    ## 
    ## $`813`
    ## concerned       fix    future   instead     meant     never      past    person 
    ##         1         1         1         1         1         1         1         1 
    ##    trynna 
    ##         1 
    ## 
    ## $`814`
    ## ayoookaee      heyy 
    ##         1         1 
    ## 
    ## $`815`
    ## cute  lol shes 
    ##    1    1    1 
    ## 
    ## $`816`
    ##           peer   ekeeleymoore           gfry     healthcare     kzcallihan 
    ##              2              1              1              1              1 
    ##           like          looks noahsdaddotcom       reaching        support 
    ##              1              1              1              1              1 
    ## 
    ## $`817`
    ## bigbangtheory       haircut    horrendous        pennys 
    ##             1             1             1             1 
    ## 
    ## $`818`
    ##       day       ive      just wonderful 
    ##         1         1         1         1 
    ## 
    ## $`819`
    ## bae<f0><ff><U+0092><U+0094>       httptcommykdniowe                    miss 
    ##                       1                       1                       1 
    ## 
    ## $`820`
    ##   bus  fuck nigga  they 
    ##     1     1     1     1 
    ## 
    ## $`821`
    ##    lose  pounds product  sample    send   tyrna     who    will 
    ##       1       1       1       1       1       1       1       1 
    ## 
    ## $`822`
    ## assholes    cards    green     need   puerto   ricans 
    ##        1        1        1        1        1        1 
    ## 
    ## $`823`
    ##    alone annoying   enough  getting   guilty     hope innocent      its 
    ##        1        1        1        1        1        1        1        1 
    ##    leave     need 
    ##        1        1 
    ## 
    ## $`824`
    ##                drive            elizabeth                  ems 
    ##                    1                    1                    1 
    ##                  gas                  ill                  lol 
    ##                    1                    1                    1 
    ##                  pay              someone <f0><ff><U+0098><U+0082> 
    ##                    1                    1                    1 
    ## 
    ## $`825`
    ## comes  game  here 
    ##     1     1     1 
    ## 
    ## $`826`
    ##            buddy           desean         england”             good 
    ##                1                1                1                1 
    ##              hes              new             went “houghgoessplash 
    ##                1                1                1                1 
    ## 
    ## $`827`
    ##        girl         ive        just laurentayyy     monster       never 
    ##           1           1           1           1           1           1 
    ##        seen     started 
    ##           1           1 
    ## 
    ## $`828`
    ## harpershomeboyz 
    ##               1 
    ## 
    ## $`829`
    ##          heck bythekil0watt          cute        taylor 
    ##             2             1             1             1 
    ## 
    ## $`830`
    ##                                           bad 
    ##                                             1 
    ##                                     irritated 
    ##                                             1 
    ##                                          real 
    ##                                             1 
    ## <f0><ff><U+0098><U+00AB><f0><ff><U+0098><U+00AB><f0><ff><U+0091><U+00BF> 
    ##                                             1 
    ## 
    ## $`831`
    ##          chills          giving             guy            this           voice 
    ##               1               1               1               1               1 
    ## <f0><ff><U+0098><U+00A9> 
    ##               1 
    ## 
    ## $`832`
    ## bulletsforever            can          court           home         jersey 
    ##              1              1              1              1              1 
    ##           logo      shrinking           stay           they            use 
    ##              1              1              1              1              1 
    ## 
    ## $`833`
    ##                          blows                       graphing 
    ##                              1                              1 
    ##                           math <f0><ff><U+0098><U+00A9><f0><ff><U+0098><U+00A1> 
    ##                              1                              1 
    ## 
    ## $`834`
    ##  bsweeneydc       howie metsbullpen   reference        rose      tweets 
    ##           1           1           1           1           1           1 
    ## 
    ## $`835`
    ##     aint arranged   family    lahhh    least marriage   nikkis     shit 
    ##        1        1        1        1        1        1        1        1 
    ## 
    ## $`836`
    ##   amazing      been centuries     flaws listening     night     songs 
    ##         1         1         1         1         1         1         1 
    ## 
    ## $`837`
    ##                                         girl 
    ##                                            1 
    ## girl<f0><ff><U+0092><U+0095><f0><ff><U+0092><U+0095> 
    ##                                            1 
    ##                            httptcojqqlc0tr1j 
    ##                                            1 
    ##                                          mcm 
    ##                                            1 
    ## 
    ## $`838`
    ## belleboogiee          lol 
    ##            1            1 
    ## 
    ## $`839`
    ##                                     5sos 
    ##                                        1 
    ##                                   record 
    ##                                        1 
    ##                                superhero 
    ##                                        1 
    ##                                      you 
    ##                                        1 
    ## <f0><ff><U+0098><U+008F><f0><ff><U+0098><U+008F> 
    ##                                        1 
    ## 
    ## $`840`
    ##        can crystalcpr foundation   jtthekid        pls      thank    working 
    ##          1          1          1          1          1          1          1 
    ## 
    ## $`841`
    ##            cant          chilis           drive             get <f0><ff><U+0098><U+00A9> 
    ##               1               1               1               1               1 
    ## 
    ## $`842`
    ##   wit  just quiet  ride 
    ##     2     1     1     1 
    ## 
    ## $`843`
    ##        fall         day     falling       looks        love       model 
    ##           2           1           1           1           1           1 
    ##         one personality       super        when 
    ##           1           1           1           1 
    ## 
    ## $`844`
    ##       anything           else       everyone fredsavagefan1        getting 
    ##              1              1              1              1              1 
    ##           gosh         havent          heard        killing       suspense 
    ##              1              1              1              1              1 
    ## 
    ## $`845`
    ## hahahahahah   roromusso 
    ##           1           1 
    ## 
    ## $`846`
    ##        taeebandzz     birdfreestyle httptcokdlcrrfgds            listen 
    ##                 2                 1                 1                 1 
    ##             music               new        soundcloud     user209513674 
    ##                 1                 1                 1                 1 
    ## 
    ## $`847`
    ##                           talk <f0><ff><U+009A><U+00A8><f0><ff><U+009A><U+00A8> 
    ##                              1                              1 
    ## 
    ## $`848`
    ##                                        sammlite 
    ##                                               1 
    ## welcome<f0><ff><U+0098><U+009A><f0><ff><U+0092><U+0081> 
    ##                                               1 
    ## 
    ## $`849`
    ## bythekil0watt          heck          said         twice           wow 
    ##             1             1             1             1             1 
    ## 
    ## $`850`
    ##              doctors                 moms              staying 
    ##                    1                    1                    1 
    ##             tomorrow              tonight <f0><ff><U+0098><U+0092> 
    ##                    1                    1                    1 
    ## 
    ## $`851`
    ##        almonds pareesabuerger 
    ##              1              1 
    ## 
    ## $`852`
    ##   audreymcclellan biggestbabyshower             great         safety1st 
    ##                 1                 1                 1                 1 
    ##               see 
    ##                 1 
    ## 
    ## $`853`
    ##          backside            beauty           djzeeti   lolo2irrelevant 
    ##                 1                 1                 1                 1 
    ##            lovely mentionperfection           natural               pic 
    ##                 1                 1                 1                 1 
    ##             round          smashing 
    ##                 1                 1 
    ## 
    ## $`854`
    ## everybody   getting    nerves <U+203C><U+FE0F> 
    ##         1         1         1         1 
    ## 
    ## $`855`
    ## everything        lot   maturity 
    ##          1          1          1 
    ## 
    ## $`856`
    ##             album              bout httptcoczfkx0x2tk            khaled 
    ##                 1                 1                 1                 1 
    ##               man              next              this 
    ##                 1                 1                 1 
    ## 
    ## $`857`
    ##                                   naedyy 
    ##                                        1 
    ##                                    thank 
    ##                                        1 
    ##                                    youuu 
    ##                                        1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+0098> 
    ##                                        1 
    ## <f0><ff><U+0098><U+009A><f0><ff><U+0098><U+009A> 
    ##                                        1 
    ## 
    ## $`858`
    ##        better eatmoreesskay          game           get          much 
    ##             1             1             1             1             1 
    ##       package       partial    postseason        prices        season 
    ##             1             1             1             1             1 
    ## 
    ## $`859`
    ##    gunsight        hood   karenfink       makes melanierizk    ornament 
    ##           1           1           1           1           1           1 
    ##        that        wish   zoyaroses 
    ##           1           1           1 
    ## 
    ## $`860`
    ##  aint  ayee cubes   got  like  milo thing  type 
    ##     1     1     1     1     1     1     1     1 
    ## 
    ## $`861`
    ##     sorry     cough  everyone    school   serious something      this  tomorrow 
    ##         2         1         1         1         1         1         1         1 
    ## 
    ## $`862`
    ##  dont panic 
    ##     1     1 
    ## 
    ## $`863`
    ##                                                                                                             abstractsoui 
    ##                                                                                                                        1 
    ##                                                                                                                 clitoria 
    ##                                                                                                                        1 
    ##                                                                                                                 daughter 
    ##                                                                                                                        1 
    ##                                                                                                                     name 
    ##                                                                                                                        1 
    ##                                                                                                                     will 
    ##                                                                                                                        1 
    ## <f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                                                                                        1 
    ## 
    ## $`864`
    ##     media    social    2cents      feel      like    people       put       say 
    ##         2         2         1         1         1         1         1         1 
    ## something       two 
    ##         1         1 
    ## 
    ## $`865`
    ##     beautiful georgiaspeach         hello 
    ##             1             1             1 
    ## 
    ## $`866`
    ## everything      means    nothing      thats 
    ##          1          1          1          1 
    ## 
    ## $`867`
    ## named numeric(0)
    ## 
    ## $`868`
    ##       audreyslade              four       fsbaltimore       fsfoodtruck 
    ##                 1                 1                 1                 1 
    ##           fstaste               fun httptcob8mexbwqjf              much 
    ##                 1                 1                 1                 1 
    ##    oliverbeckert1          seasons… 
    ##                 1                 1 
    ## 
    ## $`869`
    ##     easy esssbeee      fun     gets     hard   really   starts   though 
    ##        1        1        1        1        1        1        1        1 
    ## 
    ## $`870`
    ##        ass       deal everywhere       fake   happened       lhhh        now 
    ##          1          1          1          1          1          1          1 
    ##       real    titties       what 
    ##          1          1          1 
    ## 
    ## $`871`
    ##    believe    brought     gotham        ive jinxedjo15       just  listening 
    ##          1          1          1          1          1          1          1 
    ##      never    podcast       seen 
    ##          1          1          1 
    ## 
    ## $`872`
    ## additional       even      great        may  sacrifice  schuh2014      trade 
    ##          1          1          1          1          1          1          1 
    ## 
    ## $`873`
    ##                                                       caused 
    ##                                                            1 
    ##                                                      eatting 
    ##                                                            1 
    ##                                                        ebola 
    ##                                                            1 
    ##                                                       gluten 
    ##                                                            1 
    ##                                                        guess 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                            1 
    ## 
    ## $`874`
    ##         examen            con          hasta          hogar            mas 
    ##              2              1              1              1              1 
    ##            mis        perrito presentaciones          puedo           solo 
    ##              1              1              1              1              1 
    ## 
    ## $`875`
    ##                                    wilin 
    ##                                        1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                        1 
    ## 
    ## $`876`
    ##       need        see starssssss  telescope        you 
    ##          1          1          1          1          1 
    ## 
    ## $`877`
    ##                                                                       factor 
    ##                                                                            1 
    ##                                                                          the 
    ##                                                                            1 
    ##                                                                   ukintheusa 
    ##                                                                            1 
    ##                                                                     watching 
    ##                                                                            1 
    ## <f0><ff><U+0099><U+008F><f0><ff><U+0091><U+008F><f0><ff><U+0099><U+009C><f0><ff><U+0098><U+00B1><U+2764><U+FE0F> 
    ##                                                                            1 
    ## 
    ## $`878`
    ##                                       anngie 
    ##                                            1 
    ## bruh<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                            1 
    ##                                        going 
    ##                                            1 
    ##                                         hell 
    ##                                            1 
    ## 
    ## $`879`
    ## volume  alarm   dumb    ios ringer that’s   wait 
    ##      2      1      1      1      1      1      1 
    ## 
    ## $`880`
    ## talk <U+2693><U+FE0F> 
    ##    1    1 
    ## 
    ## $`881`
    ##            ayoookaee <f0><ff><U+0098><U+0094> 
    ##                    1                    1 
    ## 
    ## $`882`
    ## foodiechats    tweeting 
    ##           1           1 
    ## 
    ## $`883`
    ## another blowout    game    late   night 
    ##       2       1       1       1       1 
    ## 
    ## $`884`
    ##    lol    may  shock t3rr3z 
    ##      1      1      1      1 
    ## 
    ## $`885`
    ##  good looks nikki 
    ##     1     1     1 
    ## 
    ## $`886`
    ##          cali         drums         first jordanstewart         kevin 
    ##             1             1             1             1             1 
    ##        ogezra          play           tho          tour           yot 
    ##             1             1             1             1             1 
    ## 
    ## $`887`
    ##           dad          care        family       friends           god 
    ##             2             1             1             1             1 
    ##          lost           may         nicki nickirichards          song 
    ##             1             1             1             1             1 
    ## 
    ## $`888`
    ##     better    dollars   hayfield       lose        not     paying straightup 
    ##          1          1          1          1          1          1          1 
    ##        win 
    ##          1 
    ## 
    ## $`889`
    ##  getting      god involved     kane    orton   please 
    ##        1        1        1        1        1        1 
    ## 
    ## $`890`
    ##                dude           hilarious httpstcoedc9moespa”                late 
    ##                   1                   1                   1                   1 
    ##                 lol              popeye      realzachgarber      scottgarber324 
    ##                   1                   1                   1                   1 
    ##               think     “badlipreadings 
    ##                   1                   1 
    ## 
    ## $`891`
    ##      bill   getting      ipod      just       lls  messages     phone    reason 
    ##         1         1         1         1         1         1         1         1 
    ##       set sometimes 
    ##         1         1 
    ## 
    ## $`892`
    ##                                            like 
    ##                                               2 
    ##                                            days 
    ##                                               1 
    ##                                         feeling 
    ##                                               1 
    ##                                            fuck 
    ##                                               1 
    ##                                            just 
    ##                                               1 
    ##                                           ready 
    ##                                               1 
    ## relationship<f0><ff><U+0098><U+008D><f0><ff><U+0091><U+00AB> 
    ##                                               1 
    ##                                            some 
    ##                                               1 
    ##                        <f0><ff><U+0098><U+0092> 
    ##                                               1 
    ## 
    ## $`893`
    ##                                         about 
    ##                                             1 
    ##                                          care 
    ##                                             1 
    ## nigga<f0><ff><U+0092><U+008D><f0><ff><U+0098><U+008D> 
    ##                                             1 
    ##                                           one 
    ##                                             1 
    ##                                          only 
    ##                                             1 
    ## 
    ## $`894`
    ##    came  clutch jermari 
    ##       1       1       1 
    ## 
    ## $`895`
    ##               air           almost…            autumn           caramel 
    ##                 1                 1                 1                 1 
    ##              cool            fiddle httptcoi34ewxxiyo             mocha 
    ##                 1                 1                 1                 1 
    ##              oven       restaurants 
    ##                 1                 1 
    ## 
    ## $`896`
    ##                                                                                     <U+2049><U+FE0F> 
    ##                                                                                                    1 
    ##                                                                             <f0><ff><U+0090><U+0089> 
    ##                                                                                                    1 
    ##                                                                             <f0><ff><U+0090><U+0092> 
    ##                                                                                                    1 
    ##                                                                             <f0><ff><U+0090><U+0098> 
    ##                                                                                                    1 
    ##                                                                             <f0><ff><U+0091><U+00B8> 
    ##                                                                                                    1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D> 
    ##                                                                                                    1 
    ## 
    ## $`897`
    ##  httr  just watch 
    ##     1     1     1 
    ## 
    ## $`898`
    ## adrenaline    awkward       body  centuries      couch       hype       just 
    ##          1          1          1          1          1          1          1 
    ##      makes        run    sitting 
    ##          1          1          1 
    ## 
    ## $`899`
    ##      head ooooooooh    waking 
    ##         1         1         1 
    ## 
    ## $`900`
    ##               ais annapolisjunction         developer          dynamics 
    ##                 1                 1                 1                 1 
    ##           general httptco5tbzcpfnta              java               job 
    ##                 1                 1                 1                 1 
    ##              jobs          software 
    ##                 1                 1 
    ## 
    ## $`901`
    ##     are chapped    lips 
    ##       1       1       1 
    ## 
    ## $`902`
    ##               battle               friday             sections 
    ##                    1                    1                    1 
    ##              student <f0><ff><U+0099><U+009C> 
    ##                    1                    1 
    ## 
    ## $`903`
    ##  uglier   every looking quarter samsung 
    ##       2       1       1       1       1 
    ## 
    ## $`904`
    ##  better deserve    dime   lahhh   mally   nikki 
    ##       1       1       1       1       1       1 
    ## 
    ## $`905`
    ## witcha   eyes  hands    see 
    ##      2      1      1      1 
    ## 
    ## $`906`
    ## downnnngrade<U+203C><U+FE0F><U+203C><U+FE0F><U+203C><U+FE0F><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+00AD> 
    ##                                                  1 
    ##                                           flasshya 
    ##                                                  1 
    ## 
    ## $`907`
    ##             bowie           advisor            beauty         cosmetics 
    ##                 3                 1                 1                 1 
    ## httptcopw2pxne3zy               job             macys            makeup 
    ##                 1                 1                 1                 1 
    ##             sales          seasonal 
    ##                 1                 1 
    ## 
    ## $`908`
    ## jahciknick 
    ##          1 
    ## 
    ## $`909`
    ##          say         aint        bitch          den         gone lhhhollywood 
    ##            2            1            1            1            1            1 
    ##         like         look        movaa         nahh 
    ##            1            1            1            1 
    ## 
    ## $`910`
    ## crime<f0><ff><U+0091><U+00AD><f0><ff><U+0092><U+0097><f0><ff><U+0098><U+00BB> 
    ##                                                       1 
    ##                                       httptcoxna6msxsf6 
    ##                                                       1 
    ##                                                     mcm 
    ##                                                       1 
    ##                                                 partner 
    ##                                                       1 
    ##                                                  twinny 
    ##                                                       1 
    ## 
    ## $`911`
    ##                                                                                boots 
    ##                                                                                    1 
    ##                                                                               combat 
    ##                                                                                    1 
    ## guys<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                                                    1 
    ##                                                                              looking 
    ##                                                                                    1 
    ##                                                                                  ppl 
    ##                                                                                    1 
    ##                                                                                 shit 
    ##                                                                                    1 
    ##                                                                             stepping 
    ##                                                                                    1 
    ##                                                                        sydneyyabrams 
    ##                                                                                    1 
    ## 
    ## $`912`
    ## wizards   bulls    lets 
    ##       2       1       1 
    ## 
    ## $`913`
    ##       get      hype       idk sometimes 
    ##         1         1         1         1 
    ## 
    ## $`914`
    ##   lol nip78 
    ##     1     1 
    ## 
    ## $`915`
    ##           birthday         fourchette              happy httpstcoewgdjmffz8 
    ##                  1                  1                  1                  1 
    ##            nyemadi         washington 
    ##                  1                  1 
    ## 
    ## $`916`
    ## biggestbabyshower              come          congrats              many 
    ##                 1                 1                 1                 1 
    ##         safety1st             years 
    ##                 1                 1 
    ## 
    ## $`917`
    ##        act especially      ivaxa     single      wanna 
    ##          1          1          1          1          1 
    ## 
    ## $`918`
    ##      ham   justin   rather sandwich      smh 
    ##        1        1        1        1        1 
    ## 
    ## $`919`
    ##        her       like       lmao       look        mom      oreo”    spoiled 
    ##          1          1          1          1          1          1          1 
    ## “ayeverree 
    ##          1 
    ## 
    ## $`920`
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                beyonc<e3><U+00A9> 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              dead 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              like 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              sing 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             sound 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             thats 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      “bornstuntaa 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ## <f0><ff><U+0098><U+009C><f0><ff><U+0098><U+0085>”<f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080><f0><ff><U+0092><U+0080> 
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1 
    ## 
    ## $`921`
    ##           autumnhoes <f0><ff><U+0099><U+0087> 
    ##                    1                    1 
    ## 
    ## $`922`
    ##   bulls wizards 
    ##       1       1 
    ## 
    ## $`923`
    ## httptconryqdhkelz              need               now          ohmygoff 
    ##                 1                 1                 1                 1 
    ##          redskins               win 
    ##                 1                 1 
    ## 
    ## $`924`
    ##                                       but 
    ##                                         1 
    ##                                 messaging 
    ##                                         1 
    ##                                     still 
    ##                                         1 
    ##                                      time 
    ##                                         1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><U+271C><U+FE0F> 
    ##                                         1 
    ## 
    ## $`925`
    ##     distributes           going lindseyschnabel            look             not 
    ##               1               1               1               1               1 
    ##            sure 
    ##               1 
    ## 
    ## $`926`
    ## anywhere<f0><ff><U+0092><U+008D><f0><ff><U+0092><U+0091> 
    ##                                                1 
    ##                                             cant 
    ##                                                1 
    ##                                              him 
    ##                                                1 
    ##                                              see 
    ##                                                1 
    ## 
    ## $`927`
    ##                           idek <f0><ff><U+009E><U+00AD><f0><ff><U+009E><U+00AD> 
    ##                              1                              1 
    ## 
    ## $`928`
    ##            attractive                  cute               djzeeti 
    ##                     1                     1                     1 
    ## mentionsomeonepopular            mixedtingg                   pic 
    ##                     1                     1                     1 
    ##             teammixed     womancrushforever 
    ##                     1                     1 
    ## 
    ## $`929`
    ##         afternoon              food           friends              girl 
    ##                 1                 1                 1                 1 
    ##         goodtimes httptcouztcrtc3jb              love      mondayfunday 
    ##                 1                 1                 1                 1 
    ##             pizza              yum… 
    ##                 1                 1 
    ## 
    ## $`930`
    ##     name clitoria daughter   entire     just    laugh     life   really 
    ##        2        1        1        1        1        1        1        1 
    ## 
    ## $`931`
    ##   beach  desean     hve jackson   known  lalong    lets    many matches richard 
    ##       1       1       1       1       1       1       1       1       1       1 
    ## 
    ## $`932`
    ##                                   months 
    ##                                        1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0092><U+008D> 
    ##                                        1 
    ## 
    ## $`933`
    ##   black  closet    love     see wearing 
    ##       1       1       1       1       1 
    ## 
    ## $`934`
    ##             can     comfortable            like          mother           never 
    ##               1               1               1               1               1 
    ##         talking <f0><ff><U+0098><U+00B3> 
    ##               1               1 
    ## 
    ## $`935`
    ##                         around                            dat 
    ##                              1                              1 
    ##                           less                            moe 
    ##                              1                              1 
    ##                        smoking               <U+26FD><U+26FD> 
    ##                              1                              1 
    ##                       <U+270B> <f0><ff><U+009A><U+00AC><f0><ff><U+009A><U+00AC> 
    ##                              1                              1 
    ## 
    ## $`936`
    ##          5sos 5sosgoodgirls  adorableness       consist       feeling 
    ##             1             1             1             1             1 
    ##         gonna          haha           idk           see         video 
    ##             1             1             1             1             1 
    ## 
    ## $`937`
    ##  call phone 
    ##     1     1 
    ## 
    ## $`938`
    ##    seattle      upset washington       will 
    ##          1          1          1          1 
    ## 
    ## $`939`
    ## nats  win 
    ##    1    1 
    ## 
    ## $`940`
    ##         aww caitblanche 
    ##           1           1 
    ## 
    ## $`941`
    ##                                                          lrt 
    ##                                                            1 
    ##                                                   raiabadass 
    ##                                                            1 
    ##                                                      tuuucky 
    ##                                                            1 
    ##                                                         yall 
    ##                                                            1 
    ## <f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082> 
    ##                                                            1 
    ## 
    ## $`942`
    ## tf<f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0091> 
    ##                                          1 
    ## 
    ## $`943`
    ##     fanatic haleyfrasca    mcnugget         say   surprised 
    ##           1           1           1           1           1 
    ## 
    ## $`944`
    ## named numeric(0)
    ## 
    ## $`945`
    ##        dad      found    freaked   listened       lmao      oomfs perfection 
    ##          1          1          1          1          1          1          1 
    ##       song      voice 
    ##          1          1 
    ## 
    ## $`946`
    ## doe<e2><U+00BF> thenamesolive 
    ##             1             1 
    ## 
    ## $`947`
    ##                bar httpstcofab1kzanjc              pilar         washington 
    ##                  1                  1                  1                  1 
    ## 
    ## $`948`
    ##              bout         everybody httptcodyllh7nm5q            niggas 
    ##                 1                 1                 1                 1 
    ##            talkin          <U+271A> 
    ##                 1                 1 
    ## 
    ## $`949`
    ## calculatorre          one         will 
    ##            1            1            1 
    ## 
    ## $`950`
    ##           2005         albums    davejglaser glaserhallfull          going 
    ##              1              1              1              1              1 
    ##          least         listen   paulwallbaby            put          since 
    ##              1              1              1              1              1 
    ## 
    ## $`951`
    ##              2600   arbeiaromanfort               bce              diff 
    ##                 1                 1                 1                 1 
    ##   hadrianswallqst         headgears             heads             here3 
    ##                 1                 1                 1                 1 
    ## httptcozjqljusfnj       indusvalley 
    ##                 1                 1 
    ## 
    ## $`952`
    ##  lol sike 
    ##    1    1 
    ## 
    ## $`953`
    ##     drivers   karenfink        mean melanierizk    virginia   zoyaroses 
    ##           1           1           1           1           1           1 
    ## 
    ## $`954`
    ## butler  gonna  jimmy stupid   year 
    ##      1      1      1      1      1 
    ## 
    ## $`955`
    ## 5sosgoodgirls          hear          live          omfg        passes 
    ##             1             1             1             1             1 
    ##          poor       quality      remember          song          time 
    ##             1             1             1             1             1 
    ## 
    ## $`956`
    ##     carlyepagano carlyetotacobell 
    ##                1                1 
    ## 
    ## $`957`
    ## carried<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+0082><f0><ff><U+0098><U+009C> 
    ##                                                                                       1 
    ##                                                                                 excited 
    ##                                                                                       1 
    ##                                                                                     get 
    ##                                                                                       1 
    ##                                                                                   hazel 
    ##                                                                                       1 
    ##                                                                                     see 
    ##                                                                                       1 
    ## 
    ## $`958`
    ##               yassss                bitch             erikaaxo 
    ##                    2                    1                    1 
    ## <f0><ff><U+0098><U+0098> 
    ##                    1 
    ## 
    ## $`959`
    ## cmon  man 
    ##    1    1 
    ## 
    ## $`960`
    ##             die           fruit             now           right           would 
    ##               1               1               1               1               1 
    ## <f0><ff><U+0098><U+00AB> 
    ##               1 
    ## 
    ## $`961`
    ##                                                      get 
    ##                                                        1 
    ##                                                   little 
    ##                                                        1 
    ##                                                     love 
    ##                                                        1 
    ##                                                   nerves 
    ##                                                        1 
    ##                                                   sister 
    ##                                                        1 
    ##                                              “aniyal8ter 
    ##                                                        1 
    ## ”<f0><ff><U+0098><U+0092><f0><ff><U+0098><U+0092><f0><ff><U+009C><U+00B5> 
    ##                                                        1 
    ##                                 <f0><ff><U+0099><U+009C> 
    ##                                                        1 
    ## 
    ## $`962`
    ## berg cant with yung 
    ##    1    1    1    1 
    ## 
    ## $`963`
    ##  really thought trahara <U+2757><U+FE0F> 
    ##       1       1       1       1 
    ## 
    ## $`964`
    ##                     are asf<f0><ff><U+0098><U+0092>                   extra 
    ##                       1                       1                       1 
    ##                  lamphh                   these 
    ##                       1                       1 
    ## 
    ## $`965`
    ##  alive gonats stayin 
    ##      1      1      1 
    ## 
    ## $`966`
    ##      raw    couch    going   happen  sitting     wait watching   wonder 
    ##        2        1        1        1        1        1        1        1 
    ##      wow 
    ##        1 
    ## 
    ## $`967`
    ## derrick    rose 
    ##       1       1 
    ## 
    ## $`968`
    ##              lake              eels           germany             great 
    ##                 2                 1                 1                 1 
    ##             gross           growing httptcohrwbmgwz12              like 
    ##                 1                 1                 1                 1 
    ##               lol               pic 
    ##                 1                 1 
    ## 
    ## $`969`
    ##  connor564 everything      ruins       that    wowwwww 
    ##          1          1          1          1          1 
    ## 
    ## $`970`
    ##                                     mans 
    ##                                        1 
    ##                 <f0><ff><U+0092><U+00A0> 
    ##                                        1 
    ## <f0><ff><U+0098><U+0088><f0><ff><U+0092><U+009E> 
    ##                                        1 
    ## 
    ## $`971`
    ##      bum dhberman      hes 
    ##        1        1        1 
    ## 
    ## $`972`
    ##           come           httr           just         looked       redskins 
    ##              1              1              1              1              1 
    ## redskinsnation ryankerrigan91            saw       seavswas         tunnel 
    ##              1              1              1              1              1 
    ## 
    ## $`973`
    ##     easily  extremely frustrated        get        low       math   patients 
    ##          1          1          1          1          1          1          1 
    ## 
    ## $`974`
    ##     cant  outlets saturday      the     till     wait 
    ##        1        1        1        1        1        1 
    ## 
    ## $`975`
    ##          escuchar            animar              como          deseando 
    ##                 2                 1                 1                 1 
    ## encanta<e2><U+00A1>estoy             lunes        martinmaez             mejor 
    ##                 1                 1                 1                 1 
    ##              para               que 
    ##                 1                 1 
    ## 
    ## $`976`
    ##    can  marry matter  plant    sex  smoke    you 
    ##      1      1      1      1      1      1      1 
    ## 
    ## $`977`
    ## header<f0><ff><U+0098><U+008B>                      wants 
    ##                          1                          1 
    ## 
    ## $`978`
    ## ebnoxi0us 
    ##         1 
    ## 
    ## $`979`
    ##                               hazel                                omfg 
    ##                                   1                                   1 
    ##                                shit                                ugly 
    ##                                   1                                   1 
    ## <f0><ff><U+0092><U+00A9><f0><ff><U+0092><U+0080> 
    ##                                   1 
    ## 
    ## $`980`
    ##      changed jimmytooreal        twice 
    ##            1            1            1 
    ## 
    ## $`981`
    ##    httptcovze7a77oxb <f0><ff><U+0092><U+009A> 
    ##                    1                    1 
    ## 
    ## $`982`
    ##     alone beautiful      ever    female      said      seen       she something 
    ##         1         1         1         1         1         1         1         1 
    ## 
    ## $`983`
    ##                        attractive                              cute 
    ##                                 1                                 1 
    ##                           djzeeti mentionpeoplethatyouloveontwitter 
    ##                                 1                                 1 
    ##                               pic                       teamvasquez 
    ##                                 1                                 1 
    ##                             thick                        vasquezxx3 
    ##                                 1                                 1 
    ##                 womancrushforever 
    ##                                 1 
    ## 
    ## $`984`
    ##    anytime       days       dear        lol       mine missmichy1 
    ##          1          1          1          1          1          1 
    ## 
    ## $`985`
    ## jwalt96     see 
    ##       1       1 
    ## 
    ## $`986`
    ##         always annieatanner15          belts          birth        control 
    ##              1              1              1              1              1 
    ##          found            god      insurance      joesw0rld       medicine 
    ##              1              1              1              1              1 
    ## 
    ## $`987`
    ##   around     baby     even     face    gonna      its pictures     real 
    ##        1        1        1        1        1        1        1        1 
    ##     show   shower 
    ##        1        1 
    ## 
    ## $`988`
    ## baseball    field     home    least    major   matter possibly   sports 
    ##        1        1        1        1        1        1        1        1 
    ##   thefix 
    ##        1 
    ## 
    ## $`989`
    ##     all balance     out  trynna 
    ##       1       1       1       1 
    ## 
    ## $`990`
    ## bulletsforever           phil          steve 
    ##              1              1              1 
    ## 
    ## $`991`
    ##                                                                    danielsharman 
    ##                                                                                1 
    ##                                                                             holy 
    ##                                                                                1 
    ##                                                                        originals 
    ##                                                                                1 
    ##                                                                             shit 
    ##                                                                                1 
    ## <f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D><f0><ff><U+0098><U+008D> 
    ##                                                                                1 
    ## 
    ## $`992`
    ##     cant contacts      mnf      see     wish     wore 
    ##        1        1        1        1        1        1 
    ## 
    ## $`993`
    ##      dabulls         funk         neil         rock       seered stacey21king 
    ##            1            1            1            1            1            1 
    ## 
    ## $`994`
    ##    apartment      country         free        guess          hey       horses 
    ##            1            1            1            1            1            1 
    ## lhhhollywood       stable     supposed        sworn 
    ##            1            1            1            1 
    ## 
    ## $`995`
    ##      get     shit straight virginia      yoo 
    ##        1        1        1        1        1 
    ## 
    ## $`996`
    ##            burnie            church             gbumc              glen 
    ##                 1                 1                 1                 1 
    ## httptcofhlzsqkc2s         methodist         onechurch          progress 
    ##                 1                 1                 1                 1 
    ##         recording            united 
    ##                 1                 1 
    ## 
    ## $`997`
    ##          chauffeur httptcodc50j81gqz”             meeeee        “leeahprint 
    ##                  1                  1                  1                  1 
    ##   <U+2764><U+FE0F> 
    ##                  1 
    ## 
    ## $`998`
    ##    hate    keep     ppl talking 
    ##       1       1       1       1 
    ## 
    ## $`999`
    ##                          cant                         drive 
    ##                             1                             1 
    ##                           til                         truck 
    ##                             1                             1 
    ## wednesday<f0><ff><U+0098><U+0092> 
    ##                             1 
    ## 
    ## $`1000`
    ##                 girla<f0><ff><U+0098><U+0092> 
    ##                                             1 
    ## homecoming<f0><ff><U+0098><U+0082><f0><ff><U+0098><U+00A9> 
    ##                                             1 
    ##                                           lol 
    ##                                             1 
    ##                                       tonight 
    ##                                             1 
    ## 
    ## $`1001`
    ##          ambiente autoriz<e3><U+00B3>           auxilio constituci<e3><U+00B3>n 
    ##                 1                 1                 1                 1 
    ## httptcoesiqu2qcx0           maracay      rtinfoaragua              tala 
    ##                 1                 1                 1                 1 
    ## <e3><U+00A1>rboles 
    ##                 1 
    ## 
    ## $`1002`
    ##              casa httptcoysvkzjugpm            kurrus     mollythedoxie 
    ##                 1                 1                 1                 1 
    ##           snoozer 
    ##                 1 
    ## 
    ## $`1003`
    ##            bethany               blue              grill httpstco8y0odzubgz 
    ##                  1                  1                  1                  1 
    ##               httr           leesburg              ridge               with 
    ##                  1                  1                  1                  1 
    ## 
    ## $`1004`
    ##       bigcitymoms biggestbabyshower              come      thedealmatch 
    ##                 1                 1                 1                 1 
    ##          virginia              west 
    ##                 1                 1 
    ## 
    ## $`1005`
    ##     <U+2049><U+FE0F> <f0><ff><U+0090><U+0089> <f0><ff><U+0090><U+0092> 
    ##                    1                    1                    1 
    ## <f0><ff><U+0090><U+0098> <f0><ff><U+0091><U+00B8> 
    ##                    1                    1 
    ## 
    ## $`1006`
    ##       can      dude      even      mids     smell    smokin surprised 
    ##         1         1         1         1         1         1         1 
    ## 
    ## $`1007`
    ##    fister nationals    please    resign 
    ##         1         1         1         1 
    ## 
    ## $`1008`
    ## hours 
    ##     1 
    ## 
    ## $`1009`
    ##   aint    bad  bitch change   even   life    lol  tryna 
    ##      1      1      1      1      1      1      1      1 
    ## 
    ## $`1010`
    ##          database     administrator          clinical         frederick 
    ##                 2                 1                 1                 1 
    ## httptco4qfflqv0x3               job              jobs        management 
    ##                 1                 1                 1                 1 
    ##          research        veteranjob 
    ##                 1                 1 
    ## 
    ## $`1011`
    ## airiel2    know    nice 
    ##       1       1       1 
    ## 
    ## $`1012`
    ##         hazel lahhhollywood          ugly 
    ##             1             1             1 
    ## 
    ## $`1013`
    ##            curley           flooded httptcogazqlpxbvq            iphone 
    ##                 1                 1                 1                 1 
    ##            opened           request            street               via 
    ##                 1                 1                 1                 1 
    ## 
    ## $`1014`
    ## narcisisticego          never        stopped 
    ##              1              1              1 
    ## 
    ## $`1015`
    ##        able        gets       he’ll joelhousman       phone        pics 
    ##           1           1           1           1           1           1 
    ##      pocket      sfeuer        take      taking 
    ##           1           1           1           1 
    ## 
    ## $`1016`
    ## irritated 
    ##         1 
    ## 
    ## $`1017`
    ## arbeiaromanfort hadrianswallqst             how lovearchaeology             old 
    ##               1               1               1               1               1 
    ##           stone 
    ##               1 
    ## 
    ## $`1018`
    ##           def          game kingkennethii           lol      redskins 
    ##             1             1             1             1             1 
    ##      watching 
    ##             1 
    ## 
    ## $`1019`
    ##    back derrick    rose 
    ##       1       1       1 
    ## 
    ## $`1020`
    ##           facts    uniquewoodie <f0><ff><U+0092><U+00AF> 
    ##               1               1               1 
    ## 
    ## $`1021`
    ##      classic        dance          end       lmaooo mrdcsportssr        video 
    ##            1            1            1            1            1            1 
    ## 
    ## $`1022`
    ## flasshya      new 
    ##        1        1 
    ## 
    ## $`1023`
    ## cheese justin  piece rather    smh 
    ##      1      1      1      1      1 
    ## 
    ## $`1024`
    ##                                      get 
    ##                                        1 
    ##                                   hahaha 
    ##                                        1 
    ##                                     hair 
    ##                                        1 
    ##                                     long 
    ##                                        1 
    ##                                   people 
    ##                                        1 
    ##                                   shower 
    ##                                        1 
    ##                                 struggle 
    ##                                        1 
    ## <f0><ff><U+0098><U+0081><f0><ff><U+008D><U+0091> 
    ##                                        1 
    ## 
    ## $`1025`
    ##            attractive               djzeeti mentionsomeonepopular 
    ##                     1                     1                     1 
    ##         mikeytwilight                   pic            teamlauren 
    ##                     1                     1                     1 
    ##        thickbeautiful     womancrushforever 
    ##                     1                     1 
    ## 
    ## $`1026`
    ## ass”crying       berg     hazele   lmaooooo       nose        see     strong 
    ##          1          1          1          1          1          1          1 
    ## “keezykeez 
    ##          1 
    ## 
    ## $`1027`
    ##           faves             get heartheartheart             lol            mike 
    ##               1               1               1               1               1 
    ##        retweets <U+2661><U+2661><U+2661><U+2661><U+2661> 
    ##               1               1 
    ## 
    ## $`1028`
    ## blame 
    ##     1 
    ## 
    ## $`1029`
    ##        debate14           early electgeorge2014         funding   georgearlotto 
    ##               1               1               1               1               1 
    ##             get          needed         schools           start        supports 
    ##               1               1               1               1               1 
    ## 
    ## $`1030`
    ##  httr  time upset 
    ##     1     1     1 
    ## 
    ## $`1031`
    ##  crucible      drop glpodcast       got    random 
    ##         1         1         1         1         1 
    ## 
    ## $`1032`
    ##       early     evening        goes hotlinejosh       lastl   otherwise 
    ##           1           1           1           1           1           1 
    ##    probably    thursday 
    ##           1           1 
    ## 
    ## $`1033`
    ##     derrick jessirose14        rose 
    ##           1           1           1 
    ## 
    ## $`1034`
    ##        feeess         payed serendippityy 
    ##             1             1             1 
    ## 
    ## $`1035`
    ##   karenfink melanierizk      people        take       taxis   zoyaroses 
    ##           1           1           1           1           1           1 
    ## 
    ## $`1036`
    ##    back   bball    bruh     pre  season   siced wizards 
    ##       1       1       1       1       1       1       1 
    ## 
    ## $`1037`
    ## followers       lob  somebody 
    ##         1         1         1 
    ## 
    ## $`1038`
    ## weirdo 
    ##      1 
    ## 
    ## $`1039`
    ##                chase    httptcouh2sttrddp                 like 
    ##                    1                    1                    1 
    ##                 look                 mean             pictures 
    ##                    1                    1                    1 
    ##               reason               taking <f0><ff><U+0098><U+008D> 
    ##                    1                    1                    1 
    ## 
    ## $`1040`
    ## fuck  you 
    ##    1    1 
    ## 
    ## $`1041`
    ## buddys  lolol   wild 
    ##      1      1      1 
    ## 
    ## $`1042`
    ##           stadium             empty              fedx             field 
    ##                 2                 1                 1                 1 
    ##          football httptcootbvrapyg3            monday             night 
    ##                 1                 1                 1                 1 
    ##          redskins 
    ##                 1 
    ## 
    ## $`1043`
    ##    scary seahawks 
    ##        1        1 
    ## 
    ## $`1044`
    ## hotlinejosh        nats        park    thursday 
    ##           1           1           1           1 
    ## 
    ## $`1045`
    ##  else wants   who 
    ##     1     1     1 
    ## 
    ## $`1046`
    ##               art            framed               got httptcocgnx1df7rm 
    ##                 1                 1                 1                 1 
    ##            mexico          postcard               the 
    ##                 1                 1                 1 
    ## 
    ## $`1047`
    ##         and colindickey      debcha      jzimms        love     ostrich 
    ##           1           1           1           1           1           1 
    ##       right       snout        sock       swans 
    ##           1           1           1           1 
    ## 
    ## $`1048`
    ## wtf 
    ##   1 
    ## 
    ## $`1049`
    ##                         fuckkk                           juss 
    ##                              1                              1 
    ##                   lhhhollywood                        trynaaa 
    ##                              1                              1 
    ## <f0><ff><U+0098><U+00AD><f0><ff><U+0098><U+00AD> 
    ##                              1 
    ## 
    ## $`1050`
    ## kayeekulit      thank 
    ##          1          1 
    ## 
    ## $`1051`
    ##   bae   fun wanna 
    ##     1     1     1 
    ## 
    ## $`1052`
    ##    bitch      boy   diming  gotdamn practice 
    ##        1        1        1        1        1 
    ## 
    ## $`1053`
    ## berniebaby94         good       missed        night      tonight 
    ##            1            1            1            1            1 
    ## 
    ## $`1054`
    ## here 
    ##    1 
    ## 
    ## $`1055`
    ## breminardi21          can          get         mess         till     together 
    ##            1            1            1            1            1            1 
    ##         wait      weekend 
    ##            1            1 
    ## 
    ## $`1056`
    ##  gawd   get  hate  kane orton 
    ##     1     1     1     1     1 
    ## 
    ## $`1057`
    ##                            beauty                              body 
    ##                                 1                                 1 
    ##                           djzeeti                         iamtaixox 
    ##                                 1                                 1 
    ## mentionpeoplethatyouloveontwitter                               pic 
    ##                                 1                                 1 
    ##                          pleasing                          smashing 
    ##                                 1                                 1 
    ##                           teamtai                 womancrushforever 
    ##                                 1                                 1 
    ## 
    ## $`1058`
    ##              hair           harpers httptcodzv7vzibtg 
    ##                 1                 1                 1
