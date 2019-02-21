## What is Bayesian modeling?

Bayesian methods have become very commonplace in systematic research. These methods seek to apply mathematical models to questions of phylogenetics, phylogeography, divergence time estimation, and comparative methods in order to estimate a distribution of plausible solutions to those questions. 
Initially described in the 18th century, Bayesian methods are not unique to systematics, having been applied to nearly every field of study over the past century.
Fundamentally, and across all fields, a Bayesian model involves three pieces: a likelihood model, prior distributions, and the posterior distribution.
Analytical software to make Bayesian methods available to biologists was first introduced in the 1990's. 
Since that time, many models have been implemented for the analysis of biological data in a Bayesian context.

### What is a model?

At the heart of the discussion of Bayesian methology is a discussion of models.
A model is a mathematical construct used to describe the process that generated a set of observed data.
Models are defined by their assumptions.
Assumptions are statements of what we believe to be true about our data.
For example, a common statistical assumption is that observations are independent and identically distributed.
If this were true, this would mean that each data point is independent, or that it does not depend on any other data point in the data that have been collected.
It would also mean that every data point in the data set is equally well-described by the model.
In model-based systematics, the given model makes assumptions about the process of evolution that generated the data that have been collected (also called the observed data). 
Because a model is a mathematical construct, the assumptions will then be translated in to parameters, or quantities describing some facet of the data. 

Let us take a dataset from Barden and Grimaldi (2016). 
In this dataset, we have 42 species from the fossil record and 42 characters, some binary (2 state, usually 0 and 1) and some multistate (2 or more states).
One common model, unweighted parsimony, makes the assumption that any state at a character is equally likely to transition to any other character state, and that each of the 42 characters in the character matrix can have their own topology and length on that topology.
Another common model, the Mk model of Lewis, makes the same assumptions about character state transitions (called exchangeabilities), but assumes that the 42 characters share a common underlying tree and branch lengths. 

In the case of the Mk model, the model parameters will be a tree, a set of branch lengths on that tree, and a rate of exchange between different states in the model.

[ insert equation here ]

The likelihood of this data can then be calculated according to the model. The above statement can be read as "the likelihood of the data given the tree, branch lengths, and exchangeability".
In the case of parsimony, the model is similar, except every character can have its own tree and length on that tree, potentially expanding to 42 separate trees, and each with unique branch lengths.

[ insert equation here ]

There are other models that make more complex assumptions, and make assumptions not solely about the exchangeabilities of characters, but about the distribution of speciation events on a tree, or about correlation between characters.
We will discuss these methods more below.
Regardless of what precise models are being discussed, the key point for systematists to understand is that every model makes assumptions, and it is crucial to think about how well-aligned a particular model is to the observed data.

Bayesian methods are not the only methods to use models.
Parsimony can be considered a model.
Maximum likelihood estimation assumes a model of character evolution.
Under maximum likelihood, combinations of parameters are scored for their likelihood until a combination of parameters is found that maximizes the likelihood of the data.
The key difference between maximum likleihood and Bayesian modeling is described in the next section.

### What is a prior?

Crucial to the Bayesian methodology is the incorporation of uncertainty. 
In the case of our 42 characters, we may be able to make some statements about that which we believe to be true about the underlying tree, branch lengths, and exchangeabilities.
For example, the data were collected in order to maximize the inclusion of stem ants sampled from the Cretaceous period.
Some of these stem ants retain some characteristics of the wasp outgroup.
These features are subsequently lost. 
In these characters, we might expect to see more transitions from a "presence" character state to an "absence" character state.
But how strong should this bias be? 
Are these the only characters in which we expect to see this bias? 
Bayesian modeling enables us to use a prior distribution to describe our beliefs about parameters of our model.

In Bayesian inference, the value a parameter can take may be fixed, meaning it is given to the analysis by the researcher, and not estimated from the data. Alternatively, the parameter value may be a random variable, meaning it can take on different values at random. 
The prior allows researchers to place a probability distribution on how likely a random variable is to take on a specific set of values.
A probability distribution provides the probabilities of different outcomes or solutions in an experiment. 

The type of prior a researcher places on a parameter will dictate the types of estimated values one is likely to see in the results of a Bayesian analysis.
For example, using an exponential distribution with a rate of 10 on branch lengths is quite common.
This distribution can be seen in Figure 1.
The reason for this choice is that most branch lengths are observed to be fairly short.
The exponential(10) distribution specifies this, while also allowing for some branches to be longer. 

As we will discuss below, the prior is not absolute. 
A prior can be enforced with different weights, according to the researcher's prior beliefs.
For example, a lightly-enforced prior can easily be overturned by the weight of evidence.
However, a more strongly-enforced prior will need stronger observed data to overturn it.

### How do the prior and the posterior fit together? 

Bayesian modeling differs from other types of model-based inference due to the incorporation of the prior.
Bayes' theorem is given in eq. 2.

[Bayes theorem will go here ] 

In Bayes' theorem, the probability of the observed data given some hypothesis is multiplied by the prior probability of that hypothesis. 
This is divided by the total probability of the observed data.
The end result is the probability of the hypothesis given the observed data.
This probability is called the posterior probability. 


This is a challenging quantity to calculate - what is the likelihood of the data?
We evaluate this using Markov Chain Monte Carlo, or MCMC, simulation.
MCMC allows new random values for each parameter to be proposed, so that the solutions can be evaluated. 
In the MCMC algorithm, an initial set of values for the model parameters is proposed.
These values are then perturbed, and new values obtained.
This is the "Monte Carlo" aspect of the name: we choose new values at random, though often within some constraining conditions.
The act of changing the the values for the parameters is often referred to as a "move."
These new parameters are then evaluated.
If they improve on the old values, or are the same, or even slightly negative, they may be kept and used as the basis for the next set of parameters.


A move may be large in scale, changing a particular parameter by a lot, or it may be small in scale, making only minor changes to a parameter.
Moves also vary in how often they are performed.
More important model parameters may be "moved" more often in order to estimate good solutions for them. 
Previous states made by the chain are not considered when making moves.
That is why this process is a "Markov Chain", or memoryless process.
Because the process is memoryless, and previously-visited solutions are  not removed from the propulation of possible solutions, a truly good solution will be revisited many times.
The MCMC process will sample from the prior for each parameter.
A well-specified model will eventually converge to the true distribution of each random variable. 
By sampling many possible combinations of parameters over the course of a phylogenetic estimation, we approximate the value of the probability of the data. 
This, in turn allows us to complete the equation shown in eq. 2 in order to calculate the posterior probability.

While MCMC does not consider its previous steps in taking new ones, most phylogenetics software packages do write out the previous combinations of parameters.
What is produced is often termed the 'posterior sample', a log of all the trees, branch lengths, and model parameters that were examined during the phylogenetic analysis.
Summary trees can then be built from this sample, and the degree of confidence in any particular bipartition on the tree assessed.
How often different solutions for any particular parameter were visited can also be assessed.
The consideration of a sample of phylogenetic trees is somewhat different than other ways of estimating trees, and as I will argue below, has implications for how researchers should consider broader macroevolutionary analyses.


## What is morphological data? 

In the section "What is a model", I outlined a model Lewis' Mk model for estimating phylogeny from discrete morphological data. 
Before we think about coherent models of morphological evolution, we need to think about what morphological data are.
What are the properties of morphological data, and how are morphological data collected? 
Broadly, morphological data often fall into two categories, discrete and continuous, which I will define and discuss below.

### Discrete morphological data

Discrete data can be found in many fields, not just phylogenetics.
Any data that can be broken into distinct and non-overlapping characteristics may be considered discrete.
In the study of morphology, most datasets used are discrete.
In these cases, an individual character is broken down into states, each with diagnostic morphology.
Our example matrix from Barden and Grimaldi is made up of discrete characters. 


Discrete characters can be broken down into categories.
Binary data are characters which have two states, typically 0 and 1.
These states may correspond to presence and absence, or they may have more complex diagnoses, such as specific morphological features assigned to each.
An example of this type of character from the Barden and Grimaldi matrix is "Anterior margin of clypeus with row of peg-like denticles", referring to setae on the margin of the clypeus.
In this case, we have a trait that is described qualitatively.
This character is broken down into present (1) and absent (0).
Multistate characters are those characters which are broken down into more than two character states.
In these characters, each state corresponds to a specific morphology, though 0 may still correspond to absent.
An example of this character type from the Barden and Grimaldi dataset is the "Mandibular shape", which is broken into six states, each with a clear definition of the morphology of the state.

Characters may be coded with respect to what is called "polarity". 
In these cases, the phylogeny has informed the way in which the character is coded.
The result of this is that one character state is designated pleisiomorphic, and one is denoted apomorphic a priori. 
This is often seen in the form of the 0 state representing the state possessed by outgroup, or the purported ancestral state. 

The act of choosing which characters to use, and what the states should be is typically performed by an expert examining populations of samples, and deciding which facets of organismal form vary, and which vary that is considered phylogenetically informative.
Phylogenetically informative refers to whether or not a character can be used to favor one set of bipartitions on a tree over another under the parsimony criterion. 
For example, a character which does not vary in the set of taxa on the tree is not considered to be phylogenetically informative because it will have the same parsimony score on any set of bipartitions. 
Likewise, a character for which every taxon has a different character state is not considered phylogenetically informative because it will also have the same parsimony score on any set of bipartitions.
A character which varies among the set of taxa, but is shared by at least two tips on the tree is considered phylogenetically informative.
A schematic of this concept is on Fig. 1. 

All of the above concepts - character coding, polarity, phylogenetic informativity - have implications for modeling the data, and will be discussed in the section "Bayesian modeling of morphology for phylogenetic estimation".

### Continuous characters

Continuous characters are those characters that cannot be broken into discrete states.
Examples of these types of characters may include height, or the length of a structure on the body.
These traits can take on the value of any real number, and may represent a specific morphometric observation from one individual, or another measurement, such as the mean of some trait in a population of individuals.
As such, continuous characters are often also referred to as quantitative characters.

In the case of discrete data, there is typically an expert observer choosing which characters are worth collecting, as outlined in the previous section.
This is typically the case for continuous data, as well.
When a specific structure is being measured, this is typically chosen by an expert observer because it varies within the set of taxa that will be placed on the tree.
Some researchers choose to then discretize the data into categories of variation.

In the case of landmark-based morphometrics, the data are the coordinates of the location of distinct anatomical features on the organism.
The landmarks are typically decided upon by an expert, and are homologous across the sample of organisms. 
This may be done in 2-D, such as from an image, or in 3-D, such as from a computer-based anatomical scan. 
While an expert has traditionally been required for this type of analysis, recent work has explored crowd-sourcing this type of data collection.

Landmarks can also be defined automatically, without the use of an expert. 
Automated landmarking typically requires a high-quality 3-D scan of the specimen to be quantified, and some way to normalize the size and view of the scan.
These methods are promising beause they allow the collection of larger datasets with less time investment, but also they avoid observer bias about which facets of the individual are important, and have error sources that are easier to detect and correct.

Lesser-used forms of continuous data may include sonic information, and behavioral information.
Traits of this nature are typically not used in phylogenetic estimation, but rather the inference of macroevolutionary patterns. 


## Bayesian modeling of morphology for phylogenetic estimation

### Discrete morphological data

The manner in which discrete morphological data are collected introduces potential biases in to phylogenetic estimation.
Since many of the modern methods for handling phylogenetic data were described to handle molecular data, it is instructive to contrast molecular and morphological data.
Molecular sequence data has a defined number of states (4 for nucleotides, 16 for amino acids), and it is generally assumed that an instance of one particular molecule will have the same properties across the sequence.
On the other hand, a a change between one state and another (say between 0 and 1) at one character might require only a small underlying genetic change.
That same change at another character may involve wholly different underlying molecular machinery, and be of a much larger magnitude.
The lack of ability to specify a single mechanism, or a small set of mechanisms, has long hamstrung the types of models that can be considered for morphology.

Due to these difficulties, discrete morphology has been analysed under a very simple model. 
This mode is often referred to as the Mk (Markov K-States) model of morphological evolution.
This is a generalization of the Jukes-Cantor model for molecular sequence evolution.
As such, it makes the same set of assumptions.
We will now discuss what these assumptions are, what they mean for character evolution, and how priors on these assumptions can be used to enable more flexible models of evolution.

Exchangeabilities  define how likely we are to observe a given change between two character states.
In the Jukes-Cantor model, the exchangeabilities between any state and any other state are held to be equal.
A Q-matrix, the matrix specifying the likelihood of different transitions, can be seen in equation 3a.
In the case of morphological data, this means that the probability of transitioning between one character state and another are equal.
If we have binary, presence-absence data, these data would be equally likely to be gained as lost.
However, in molecular phylogenetics, the probability of observing a change depends on two quantities: the exchangeability, and the equilibrium frequency of the starting state.
The Jukes-Cantor model assumes the character states have equal frequency.
This can be seen in 3b.
Equilibrium state frequencies define how many of each state we would expect to see if the process of evolution were run out to infinity (allowed to equilibrate). 
Even if the exchangeability between two states is high, if the starting state is rare, we will observe that change rarely.
The default assumption of the Mk and Jukes-Cantor models is that character frequencies is equal.
Taken with the assumption of equal exchangeabilities, this disallows states to have different rates of change.

This assumption likely strikes many readers as unrealistic.
Bayesian methods provide us a solution to escape this problematic assumption. 
In a Bayesian context, assumptions are set up as model parameters. 
The value of a parameter is a random variable, and we can use priors to create distributions of values that the random variable is likely to take.
Under parsimony, individual characters having differential probabilities on state transitions is often handled by a priori specifying a transition matrix with the correct weights on different changes.
For example, if it is considered more likely to lose a character state (transition from a 1 state to a 0 state) than to gain one (transition from a 0 state to a 1 state), a step matrix can be specified for that character that penalizes 0 to 1 transitions.
In order to do this, one must have a suspicion that certain transitions between states at a character are more or less likely.

In a Bayesian framework, one can place a prior on the state frequencies, allowing them to take on multiple possible values. 
In the case of state frequencies, one approach to allow variation has been to use a Beta prior for binary data, or a Dirichlet prior for multistate data.
These priors do not change the underlying structure of the model: the parameters do not change.
But they allow researchers to sample different numbers for the values of those parameters. 
In practice, this allows for different rates of change between states, informed by the data, rather than by a priori assumptions. 

Bayesian methods open the door to modeling data as mixture models. Mixture models treat the total dataset as an aggregate of smaller populations, which may have different paramter values.
A common example of this is the use of among-character rate variation. 
One long-acknowledged issue in phylogenetics is that not all sites in a molecular alignment, or characters in a data matrix will evolve at the same rate.
Declining to correct for this can lead to incorrect inferences, particularly when characters have high rates of evolution.
Under this model, the rate of evolution at any one character is assumed to be drawn from a Gamma distribution.
Because approximating a continuous Gamma distribution would be too computationally intensive, a discrete Gamma distribution with a user-specified number of categories is used.
Four has been supported in some empirical studies, and is a common value chosen.
When this value is chosen, there are four rate categories used to describe the data (i.e., for subpopulations in the mixture model).

This same framework can be applied to other parameters.
Relaxing character change symmetry has been accomplished using similar principles. 
When we place a prior on character frequencies, this is typically done as a mixture model.
In this case, the Beta distribution (binary data) or Dirichlet distribution (multistate data) is typically discretized into several categories.
The likelihood is then computed according each category and summed to generate a character likelihood.
Treating character rates, or character change asymmetry, as a mixture model allows the dataset to potentially have multiple classes for each of dataset, with each class specifying the same model parameters, but allowing those parameters to take on different values.
In this way, there can be multiple rates of evolution, or multiple 0 to 1 transition rates, in the dataset.

In Bayesian analysis, it can be confusing for researchers to understand what is the model, what is the prior, and how each part affects the analysis.
Parameters define what a researcher believes are the key facets of the process by which the data were generated.
A prior specifies a range of values for that parameter that the researchers consider reasonable a priori.

### Continuous Data

Continuous data have been less commonly used for phylogenetic inference.
As discussed in the section "What is morphological data", continuous data are often discretized in and modeled according to the Mk model.
This, however, introduces an element of user interpretation to the data that does not otherwise need to exist, and is not accounted for in the model. 
Continuous data have often been used for what is termed comparative analysis or macroevolutionary analysis, which is discussed in the section "Further evolutionary analysis with morphology". 
Despite their relatively rare use, these data have been demonstrated to contain phylogenetic signal.

The rich history of using continuous characters for comparative analysis enables those same models to be used for phylogenetic estimation.
Brownian motion has been used to model trait data for phylogenetic estimation. 
Brownian motion is used to model the value of continuously-varying data over time.
This model is often referred to as the "random walk," due to the fact that in any time interval, the value of a trait can change randomly in both direction (positive or negative) and magnitude (small or large changes). 
Brownian motion was originally used to describe the movement of particles suspended in fluids.
In biology, Brownian motion may be compatible with a number (or combination) of evolutionary forces. 

In a Brownian motion model, evolution is typically described by two parameters: the mean trait value, x, at the start of a particular time interval, and the evolutionary rate parameter, sigma. 
X will be the value from which the trait can "walk" during the time interval. 
Sigma will determine the magnitude with with the trait will step away from x. 
Changes are expected to be distributed according to a normal distribution with mean 0 and variance proportional to the rate and size of the time interval.
At very short time intervals, we expect to see little change. 
For long intervals, we expect the normal to become wider and wider,
indicating that the amount of change has the potential to be larger.

Brownian motion has been used for a long time to model the evolution of traits on a tree.
Recently, it has been implemented for phylogenetic estimation in both dated and undated trees.
Simulation research indicates that continuous characters modeled under Brownian motion can lead to lower topological error than discrete morphological traits.
In particular, this is true in datasets with multiple rates of evolution. 
Wright and Hillis (2014) demonstrated that in discrete morphological traits, phylogenetic error is very high for low rate of evolution characters (due to low signal), and very high rate of evolutioon characters (due to saturation of changes).
Continuous characters do not display this relationship as strongly.

Use of continuous characters is promising because the Brownian motion model is fairly lightweight. 
This allows for each character to have its own sigma, enabling multiple mechanisms in a dataset without having to calculate a character likelihood according to multiple Beta categories.
Expectations about the evolution of continuous character are complex, but Brownian motion can be expanded to accommodate them.
Characters are expected to covary in a Brownian motion framework. 
This character correlation can be accounted for by estimating a correlation matrix from individuals in a lineage.
If within-lineage variation is not accounted for, morphological evolution rates will be overestimated. 
The correlation matrix can be used to correct within-lineage character correlation.
Because the lineages are all connected by an underlying phylogeny, character correlation may also occur among lineages. 
The correlation matrix can then be used to establish a correlation matrix among lineages, as well.

The use of continuous characters in morphological phylogenetics is an exciting prospect along several lines.
Firstly, Brownian motion is one of many comparative models of evolution.
Others could be substituted, or multiple models used among characters.
Even in the case that other models are not explored, the Brownian motion can correspond to different biological interpretations. 
Brownian motion is typically interpreted to be analogous to traits evolving under drift, having no selective optima.
Prior work (Hansen and Martins 1996) demonstrates that several models incorporating selection still appear indistinguishable from Brownian motion. 
In sum, there are a variety of mechanisms that could be described by Brownian motion, and the research does not have to explictly choose to model them a priori.

Secondly, these implementations are exciting because they enable the use of a third independent data source, modeled under different assumptions.
Modeling traits according to Brownian motion to estimate a phylogeny from continuous trait data allows researchers to work in the same MCMC framework for continuous, discrete, and discrete molecular data. 
Using all available data will enable researchers to validate the tree among sources, and formulate testable hypotheses of how model assumptions may impact the tree estimated.
This also opens the path to perform joint estimation across multiple types of data.
Indeed, fossil datasets are often limited in size. 
Opening up new avenues to collect data, particularly if automation of data collection becomes commonplace, will allow researchers to make more complete use of specimens. 


## How does Bayesian modeling differ from parsimony?

I have said very little thus far in this review about parsimony.
My main purpose has been to lay out how Bayesian modeling of morphology works.
Parsimony is still a dominant optimality criterion in morphological phylogenetics.
It is informative to look at how the assumptions, mechanisms, and interpretation of Bayesian and parsimony methods are similiar, and how they are different.
There are three main comparisons I would like to make between the two criteria: assumptions made about the evolutionary process, interpretation of parsimony and Bayesian analysis.

### Assumptions about the evolutionary process

Parsimony can come in several variations, just as we can relax various assumptions of the Mk model. 
The most common variation is unweighted parsimony.
This typically refers to an application of parsimony in which it is held that any change between any two character states is weighted equally.
In this case, a change from 0 to a 1 state is as likely as a reversal between the two. 
Surficially, this is quite similar to one of the chief assumptions of the Mk model - that character changes are symmetrical. 

However, there are core difference between parsimony and Bayesian approaches which change the results and interpretation of these two ways of estimating trees. 
In a Bayesian analysis, values are sampled for each of the parameters in the model, including branch lengths.
Branch lengths are typically sampled as number of expected character changes per character.
In a parsimony analysis, the tree that is favored is the one that minimizes the number of changes in the dataset across that tree.
However, each character may have its own length on the tree.
The final branch lengths represent the number of changes in the dataset along each branch, as a whole number, rather than a rate.
This has desireable properties - in a maximum parsimony analysis, character changes can be mapped to specific branches.
In Bayesian estimation, either more complex models or post-hoc analyses are required to do this.


Advocates for parsimony often point to the aforementioned as a positive.
Parsimony is often referred to as a "No Common Mechanisms model", which allows every character in the character matrix to have its own length on a common tree.
This is intuitively appealing - it's unlikely that every character in a matrix evolves at the same rate.
Allowing each site to have its own rate of evolution means that no matter how different the rates actually are, they can be accommodated.
However, this same assumption makes it impossible to choose a likelihood implementation of the No Common Mechanisms model via even liberal information criteria due to its parameter richness.
Model fitting techniques typically attempt to balance parameter richness with how much the fit of the model to the data improves with those additional parameters.
What a model fitting technique cannot tell you is if the added parameters add biological realism. 
There may very well be reasons why, even in the absence of statistical evidence, the assumptions of parsimony make more sense for their data.
The purpose of this review is not to argue for one method over another, but to lay the groundwork for researchers to understand the underlying assumptions of these two different types of phylogenetic estimation.

Bayesian estimation can enable researchers to relax the assumptions of the Mk model.
Parsimony also allows users to do this.
A parsimony step matrix can be specified, which allows researchers to place different weights on various character state transitions.
For example, if a researcher believed it would be easy to lose a trait, but hard to regain it, they could weight the loss lightly, and the gain heavily.
Then, when the parsimony tree is esitmated, trees that contain gains of the trait will have to compensate by minimizing parsimony steps in other parts of the dataset.
This penalizes trees not containing the penalized gain.
Researchers can specify custom step matrices for every character in the matrix, if desired.
This flexibility enables researchers to, in effect, completely control the tree esitmated through a priori specifications of the types of changes that can be seen.
Specifying a step matrix can be thought of as a type of very strong prior, which cannot be overcome by the signal in the data.

There is another type of weighting that has become popular. 
This type is referred to as "character weighting."
In a dataset with character weighting applied, changes in certain characters are held to cout more towards the parsimony score than others.
This often takes the form of downweighting characters thought to be highly homoplasious.
When a researcher does this, they specify that certain characters are less reliable indicators of the true phylogeny than others.
This may be done a priori, by the researcher specifying that a change in one character (the character thought to hold the truthful signal) must be balanced by multiple (2 or more) changes in others. 
This can also be automated, a process often referred to as "implied weighting." 
Under this approach, the first time a character changes state on a tree, the change is given the weight of one. 
Subsequent changes are given smaller weights.
In effect, this means that the more a character changes, the less it is allowed to influence the estimated tree.
The implied weighting approach allows for the process of weighting to be more reproducible, and less dependent on observer bias.

### Interpretation of Bayesian and parsimony analyses

Parsimony aims to estimate the most parsimonious tree, i.e, the tree that minimizes the number of changes in the dataset along that tree.
This is fairly straightforward to understand.
Multiple most parsimonious trees may be estimated from the same dataset, if multiple sets of relationships, or branch length distributions, are equally parsimonious.
We can think of parsimony methods as aiming to estimate one tree, but that this may not be possible for a particular dataset.

Bayesian methods, however, provide a distribution of trees and parameter values sampled during the tree search.
These are the values and trees proposed and evaluated by the MCMC algorithm during estimation.
This posterior distribution can be used to test if the estimation has converged, or drawn enough independent samples that the true posterior has been approximated. 
A Bayesian estimation is not expected to provide one evolutionary history, and set of parameters. 
Rather, visualizing this uncertainty is considered by many to be integral to Bayesian estimation.
For example, in a dated phylogeny, node ages are typically shown as distributions of possible ages, rather than point estimates.
The shape are spread of the distribution itself is important information - a very wide distribution might indicate little confidence in the value.

Both parsimony and Bayesian methods often rely on building a consensus tree.
There are many ways to estimate a consensus tree (for a review see ), but fundamentally, a consensus tree summarizes the bipartitions on the tree, and turns a sample of trees into a single tree object.
In a Bayesian analysis, those trees are normally labeled with the posterior probability, or the probability of the parameters and tree given the data. 
In parsimony analyses, further estimations, such as bootstrap, must be performed in order to assess confidence in a particular bipartition.
These approaches subsample columns of data in a phylogenetic matrix and re-estimate trees from the generated samples.
Whereas Bayesian posterior probability can be thought of as the probability of the phylogenetic hypothesis given the data, the bootstrap can be thought of as a measure of repeatability of the hypothesis given the data.
For example, if 1000 subsampled replicate datasets are used to estimate trees, and 999 of them support the same tree, this is the evidence that the collected data strongly support the estimated topology.
However, collection of additional data could change the bootstrap values.

These two approaches have implications for how model fit and adequacy can be addressed.
As discussed above, both Bayesian methods are parsimony make assumptions about the data. 
In a Bayesian method, the fit of the model to the data can be described by calculating the marginal likelihood of the data, the probability of the data given the model.
The calculation of this quantity can be complex, and beyond the scope of this paper, but for an example see here.
This quantity allows for the comparison of models using the Bayes Factor, a standardized statistical framework for comparing the weight of evidence for different models.
In this way, assumptions about the data either via the model or the prior can be tested.
An equivalent framework does not exist for parsimony. 
Under the maximum parsimony criterion, if a shorter tree is returned, that is considered to be the better tree.


## Brave new worlds of data-intensive morphology

As we've seen in the previous sections, estimating phylogenetic trees from morphological information is an evolving science.
Between parsimony estimation and Bayesian methods, many combinations of assumptions can be made to suit a given dataset.
Researchers have a greater range of choices than at any point in the past to try, and create, new models for understanding the evolution of taxa and traits.
Below, I will highlight two that are particularly interesting.

### Modularity of the prior and the model

Historically, many Bayesian estimation software suites have allowed only limited choices of priors on any model parameter, and limited control over the shapes that the prior distribution can take.
More modern software allows researchers to experiment more broadly with novel combinations of parameters and priors. 

In the section "Bayesian modeling of morphological data", we discuss placing priors on ACRV and character state frequencies. 
In the previous generation of phylogenetics software (i.e., MrBayes), priors of this nature had to be coded into the software by the developers.
If a user wanted to use a certain prior distribution with a new data type, they may have needed to program it in in the actual software.
Current generation software (Beast2, RevBayes) allows users to generate new combinations of parameters and priors, and to contribute the scripts to do so back so other users may find them via contributions to their open-access software repositories. 

This philosophy of flexibility is important to progress in this field.
Firstly, many of the models we use to estimate phylogenies were not generated with morphology in mind.
For example, prior work indicates that using the Gamma distribution to model ACRV may not be optimal for morphological data.
On its face, this makes a great deal of sense: traditionally, invariant characters have not been collected by researchers.
Nor have characters that change only once on a tree.
This means that we typically must correct for this omission, often referred to as correcting for ascertainment bias.
But when we use Gamma-distributed rate variation, we assume that there are some extreme low-rate characters in the dataset.
For morphology, this is unlikely to be true.
A modular framework allows a user to simply substitute another prior distribution.

An implicit benefit to this is that researchers can realize models that they believe will fit their data without needing to involve a developer of the software. 
In previous software generations, when a researcher needed a new model, they would contact the developer, and let them know what they needed.
Depending on how much time the developer had to handle user requests, perhaps they would implement it. 
Modular software allows researchers to be the developer of the model.
This enables the expert on the data to create models to describe those data, without having to wait on a software expert.
Likewise, the software expert is also freed from needing to constantly balance user requests with their own work.
Embracing open source contribution also allows users to contribute back their scripts for analyses, such that other users can find and use them.
Modularity and openess enable faster scientific progress as researchers can implement new models quickly, and disseminate those results with an interested community of scientific practice.

### 

