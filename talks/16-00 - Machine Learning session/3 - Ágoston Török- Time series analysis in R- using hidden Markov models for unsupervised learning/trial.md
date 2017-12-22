

<style>
.footer {
    color: #333333;
    position: fixed;
    top: 90% !important;
    text-align:right;
    width:100%;
}
.subtitle {
    color: #333333;
    font-size : 40em;
}

.reveal h1,  .reveal h3 {
  color: #ff5000;
  word-wrap: normal;
  -moz-hyphens: none;
  font-size : 2em;
}

.reveal h2 {
  font-size : 1.5em;
}


.section .reveal .state-background {
    background: #FF5000;
    background-image: url("satRday_talk-figure/r-brain-back.png");
    background-repeat: no-repeat;
    background-size:100% 100%;
    background-attachment: fixed;
    background-position: right right; 
}
    
    

</style>

Time Series Analysis In R
========================================================
autosize: true
width: 1920 
height: 1080

<div class="subtitle">
  <font size="300">
      Using Hidden Markov Models <br>
      For Unsupervised Learning
    <br>
  </font>
</div>


<p align="left", color="#ff5000">
     <font size="300">
    Agoston Torok 
    </font>
     <br> @torokagoston <br> <br> <br>
    data scientist @SynetiqLab
</p>

<div class = "footer">satRday conference <br> Budapest, 09/03/2016 </div>


<head>
  <script type="text/javascript"
     src="https://d3eoax9i5htok0.cloudfront.net/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
  </script>
</head>


Time series analysis
========================================================
<hr>

## Time series: 
<center><i>A series of datapoints in time order</i> </center>

## Methods of time series analysis:
- Season trend decomposition (e.g. [STL](http://www.wessa.net/download/stl.pdf))
- Autoregressive modeling (AR, ARMA, ARIMA)
- State space models (e.g. [HMM](https://cran.r-project.org/web/packages/depmixS4/vignettes/depmixS4.pdf))




<img src="trial-figure/unnamed-chunk-3-1.png" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" style="display: block; margin: auto;" />

<div class="footer">@torokagoston</div>

A (very) short intro to hidden Markov models
========================================================
<hr>
We hypothesize latent states/regimes in the time series (for example past temperature and tree rings [[1](https://www.cs.sjsu.edu/~stamp/RUA/HMM.pdf)])

$$ \begin{array} & X_{1}\overset{P_T}{\longrightarrow} & X_{2}\overset{P_T}{\longrightarrow} & \dots  \overset{P_T}{\longrightarrow} & X_N\\ 
\downarrow P_O & \downarrow P_O & & \downarrow P_O \\ 
O_1 & O_2 & \dots & O_N\end{array} $$

- We have a series of $O$ observations resulting from $X$ latent states. 
- We cannot directly observe the $X$ time series, but we can infer it 
- Based on the $P_T$ transition probability matrix, and $P_O$ probability matrix
- Passes between $O$, $X$, and $Model$ 



<div class="footer">@torokagoston</div>


Human biosignals and HMM
========================================================
<hr>

## Has been used to find patterns in:
- EEG (sleep cycles) [[3](http://ieeexplore.ieee.org/xpl/login.jsp?tp=&arnumber=1224760&url=http%3A%2F%2Fieeexplore.ieee.org%2Fxpls%2Fabs_all.jsp%3Farnumber%3D1224760http://ieeexplore.ieee.org/xpl/login.jsp?tp=&arnumber=1224760&url=http%3A%2F%2Fieeexplore.ieee.org%2Fxpls%2Fabs_all.jsp%3Farnumber%3D1224760), [4](http://link.springer.com/article/10.1007/s11517-012-0871-2)]
- Eye movements (face recognition) [[5](http://jov.arvojournals.org/article.aspx?articleid=2193875)]
- Heart rate (cardiac events) [[6](http://www.inderscienceonline.com/doi/abs/10.1504/IJBET.2010.032695)]

<div class="footer">@torokagoston</div>

Can we use it to recognise emotional states?
========================================================
<hr>
- Results show that emotions have different physiological correlates [[7](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0066032)] 
- It is reasonable to say that the transition probabilities are not uniform
- Some emotions lasts longer (fear), others fade rather fast (surprise)


<br><br><br><br><br>
<p align="center"; style="font-style: italic; font-size:1.2em"> "Emotions are just as easy to access as any Higgs boson." </p>



<div class="footer">@torokagoston</div>


The dataset
========================================================
<hr>


## Emotional reactions to [Paperman (2012, John Kahrs)](http://www.imdb.com/title/tt2388725/)
- average of 62 testers' data
![paperman](https://67.media.tumblr.com/7c3ea782774eac0291502e53fd36ca64/tumblr_nwnk05OAq31uhr8t5o1_500.gif)

<div style="width: 100%; position: fixed">
<img src="trial-figure/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" style="display: block; margin: auto;" />
</div>

*** 
<hr>
## Psychophysiological recordings
- **FEA** Frontal EEG asymmetry (Attraction)
- **RMSSD** Heart rate variability (Engagement)
- **NSCR** Skin conductance (Excitement)

<div class="footer">@torokagoston</div>

Why depmixS4?
========================================================
<hr>

**Dep**endent **mix**ture models by [Ingmar Visser](http://www.ingmar.org/) and [Maarten Speekenbrink](https://www.ucl.ac.uk/pals/research/experimental-psychology/person/maarten-speekenbrink/)

 - custom functions
 - custom constraints
 - covariates (time dependent $P_T$)
 - custom distribution families
 - prior, transition and response model
 


 
***

```r
# given two distributions
a = rnorm(1000, mean=5, sd=3)
b = rnorm(1000, mean=25, sd=9)

# their joint distribution on histogram
hist(rbind(a,b), breaks = 30)
```

<img src="trial-figure/unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" style="display: block; margin: auto;" />

<div class="footer">@torokagoston</div>


Using depmixS4 for uncovering emotional states
========================================================
<hr>

- Unsupervised learning
- Caution: the fitting algorithm not necessarily recovers the meaningful states --> Setup


```r
library(depmixS4)
expectedNumberOfStates <- 4

myModel <- depmix(list(FEA~1, RMSSD~1, NSCR~1), data=df, # the formulae
              nstates=expectedNumberOfStates, # the number of hidden states we assume
              instart = c(1,0,0,0), # assume that we start from state 1
              trstart = runif(expectedNumberOfStates^2), # this is an N x N matrix
              family = rep(list(gaussian()),3)) # families of the responses/observations


#fit the model 
fittedMyModel <- fit(myModel, verbose = FALSE) # if verbose=TRUE it shows the status at every 5th iteration
```

```
converged at iteration 58 with logLik: 494.1904 
```



<div class="footer">@torokagoston</div>

The fitted model
========================================================
<hr>


```r
summary(fittedMyModel) 
```

```
Initial state probabilties model 
pr1 pr2 pr3 pr4 
  1   0   0   0 

Transition matrix 
        toS1  toS2  toS3  toS4
fromS1 0.769 0.000 0.124 0.106
fromS2 0.000 0.888 0.078 0.034
fromS3 0.012 0.050 0.938 0.000
fromS4 0.000 0.121 0.000 0.879

Response parameters 
Resp 1 : gaussian 
Resp 2 : gaussian 
Resp 3 : gaussian 
    Re1.(Intercept) Re1.sd Re2.(Intercept) Re2.sd Re3.(Intercept) Re3.sd
St1           0.125  0.252           0.754  0.335           1.061  0.551
St2           0.896  0.199           1.070  0.086           1.400  0.115
St3           0.741  0.268           0.827  0.106           1.399  0.122
St4           0.973  0.191           1.349  0.113           1.451  0.110
```

Does it make any sense?
========================================================
<hr>

<p style="text-align:center;">
<img src="paperman_20_25.gif" width="400" />
<img src="paperman_90_95.gif" width="400"/>
<img src="paperman_203_208.gif" width="400"/>
<img src="paperman_295_299.gif" width="400"/></p>


<p style="text-align:center;"><img src="hmm_states_paperman.jpeg"/> 
<img src="paperman_37_42.gif" width="400"/>
<img src="paperman_167_172.gif" width="400"/>
<img src="paperman_252_257.gif" width="400"/></p>



<div class="footer">@torokagoston</div>

Conclusions
========================================================
<hr>

- We were able to uncover emotional states
- The emotional states made sense 
- And were consistent with self-report data

# Future directions
- Everyone experience emotions somewhat differently
- Personalized content, storyline
- Trageted advertising: neuromarketing

<div class="footer">@torokagoston</div>

========================================================
<hr>
<br><br><br><br>
# Thank you for your attention!

<br>
@agostontorok

References
========================================================
[Stamp, M. (2004). A revealing introduction to hidden Markov models. Department of Computer Science San Jose State University.](https://www.cs.sjsu.edu/~stamp/RUA/HMM.pdf)

[Visser, I., & Speekenbrink, M. (2010). depmixS4: An R-package for hidden Markov models. Journal of Statistical Software, 36(7), 1-21.](http://dare.uva.nl/record/1/336266)	

[Lee, H., & Choi, S. (2003). Pca+ hmm+ svm for eeg pattern classification. In Signal Processing and Its Applications, 2003. Proceedings. Seventh International Symposium on (Vol. 1, pp. 541-544). IEEE.](http://ieeexplore.ieee.org/xpl/login.jsp?tp=&arnumber=1224760&url=http%3A%2F%2Fieeexplore.ieee.org%2Fxpls%2Fabs_all.jsp%3Farnumber%3D1224760http://ieeexplore.ieee.org/xpl/login.jsp?tp=&arnumber=1224760&url=http%3A%2F%2Fieeexplore.ieee.org%2Fxpls%2Fabs_all.jsp%3Farnumber%3D1224760)

[Lederman, D., & Tabrikian, J. (2012). Classification of multichannel EEG patterns using parallel hidden Markov models. Medical & biological engineering & computing, 50(4), 319-328.](http://link.springer.com/article/10.1007/s11517-012-0871-2)

[Chuk, T., Chan, A. B., & Hsiao, J. H. (2014). Understanding eye movements in face recognition using hidden Markov models. Journal of vision, 14(11), 8-8.](http://jov.arvojournals.org/article.aspx?articleid=2193875)

[Mendez, M. O., Matteucci, M., Castronovo, V., Ferini-Strambi, L., Cerutti, S., & Bianchi, A. (2010). Sleep staging from heart rate variability: time-varying spectral features and hidden Markov models. International Journal of Biomedical Engineering and Technology, 3(3-4), 246-263.](http://www.inderscienceonline.com/doi/abs/10.1504/IJBET.2010.032695)

[Kassam, K. S., Markey, A. R., Cherkassky, V. L., Loewenstein, G., & Just, M. A. (2013). Identifying emotions on the basis of neural activation. PloS one, 8(6), e66032.](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0066032)
