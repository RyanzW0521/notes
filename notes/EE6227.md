# EE6227 Genetic Algorithem & Machine learing
____
## Part I Genetic Algorithms

#### Chapter 3 Evolutionary Algorithms

1. __About EA__
    * __main components:__
    
        representation / evaluation / population 种群
        parent selection / survivor selection
        recombination / mutation 变异
    
    * __general scheme__
    
        Initialization ---- population ---- parent selection ---- parents ---- recombination / mutation ---- offspring(后代) ----survovor selection ---- population ----Termination 
     
    * __pseudo-code__
    

        ```
        BEGIN 
            _INITIALISE_ _population_ with random candidate solutions;
            _EVALUATE_ each candidate; //？
            REPEAT UNTIL ( TERMINATION CONDITION is satisfied ) DO
                1 _SELECT_ parents;
                2 _RECOMBINE_ pairs of parents;
                3 _MUTATE_ the resulting offspring;
                4 _EVALUATE_ new candidates;
                5 _SELECT_ individuals for the next generation;
             OD
         END
         ```
     
     * __common model of process__
         
         individuals with fitness form population and via variation operators which are crossover and mutation to selection towards higher fitness -- survival and mating of the fittest
         
         evolutionary progress actually a optimization progress
         
      * __2 pillars(支柱)__
      
          two competing forces: 
          
          genetic operators: increasing diversity, push towards novelty(新颖性)
          
          selection operators: decreasing diversity, push towards quality( select parents and survivors )
      
      * __Representation__
      
          roles: provides code which can be manipulated(操作) by variation operators for candidate solutions
          
          phenotype: the outside
          
          genotype: the inside, digital DNA
          
          mappings: 
              
              encoding: phenotyoe => genotype ( not must 1 to 1)
              
              decoding: genotype => phenotype ( 1 to 1)
          
          
          
      * __Evaluation (fitness function)__
          
          see as "environment", means the task to solve and the requirement to be adapt.
          
          Assigns a single real-valued fitness to each phenotype which forms the basis for selection – So the more discrimination (different values) the better
          
      * __Population__
      
          holds the candidate solutions of the problem as individuals (genotypes)
           >Population is the basic unit of evolution, i.e., the population is evolving, not the individuals • Selection operators act on population level • Variation operators act on individual level
       
      * __slection Mechanism__  aka replacement 
                                  
          individuals----parents/survive
          
          push population to higher fitness ----probabilistic ----even ur the worst maybe canbe chosen. escape from __local optima__
          
          use fixed population size so need a way of going from (parents + offspring) to next generation
          
      * __Variation operators__
      
          generate new candidate solutions
          
          Usually divided into two types according to their arity
          
          (number of inputs):
          
            – Arity 1 : mutation operators
            
            – Arity >1 : recombination operators
            
            – Arity = 2 typically called crossover
            
            – Arity > 2 is formally possible, seldom used in EC
          
          
      * __Mutation__
      
          causes small, random variance
          
          Acts on one genotype and delivers another
          
          May guarantee connectedness of search space and hence convergence proofs
          
      * __Recombination__
      
          merges information (chosen stochastic) from parents into offspring
          
2. __Example__

    * __8-queen problem__
    
        Place 8 queens on an 8x8 chessboard in such a way that they cannot check each other
        * Phenotype: a board configuration
        
        * Genotype: a permutation of the numbers 1–8
        
        * Penalty of one queen: the number of queens she can check. The sum need to be minimized
        
        * Fitness of a configuration: inverse penalty to be maximized
        
        * Mutation: Small variation in one permutation, e.g.: swapping values of two randomly chosen positions, 
        
        * Recombination: 2 permutations 排列组合 to 2 ~. 
        
        <img src= 42538.jpg align=center />
        
        * Parent selection:

             – Pick 5 parents and take best two to undergo crossover
             
        * Survivor selection (replacement)
            
            progress: 1. sorting the population by decreasing fitness ---- 2. enumerating list from high to low ---- 3. replace the first with a finess lower than the given.
            
            
            
3. __Typical EA behaviour__


    niching 挑剔
    但是这边到底是啥意思…… 看不懂
    
    
      <img src= 144934.png align=center />
    
    * progression of fitness
    
      long runs not always beneficial -- depends on the last bit you want how much
    
      worth expending effort on smart initialisation -- if there is good solutions.
    
    
