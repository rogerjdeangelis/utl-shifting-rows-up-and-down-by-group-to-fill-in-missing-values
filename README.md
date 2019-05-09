# utl-shifting-rows-up-and-down-by-group-to-fill-in-missing-values
Shifting rows up and down by group to fill in missing values
    Shifting rows up and down by group to fill in missing values                                                                        
                                                                                                                                        
     Solutions                                                                                                                          
                                                                                                                                        
         1. Harcoded mutiple merging                                                                                                    
                                                                                                                                        
         2. Dynamic mutiple merging (do_over and an array of merges)                                                                    
                                                                                                                                        
         3. Arrays by Astounding                                                                                                        
            Astounding                                                                                                                  
            https://communities.sas.com/t5/user/viewprofilepage/user-id/4954                                                            
                                                                                                                                        
         4. Sort data so we only need to retain 'lag1'                                                                                  
            Heffo                                                                                                                       
            https://communities.sas.com/t5/user/viewprofilepage/user-id/261618                                                          
                                                                                                                                        
         5. SQL                                                                                                                         
            Jagadishkatam                                                                                                               
            https://communities.sas.com/t5/user/viewprofilepage/user-id/12151                                                           
                                                                                                                                        
         6. SUMMARY and HASH by Scott Bass ( easy HASH? - I need to muck around Scotts Macros?)                                         
            Thanks Scott!!                                                                                                              
            Need these macros                                                                                                           
            https://github.com/scottbass/SAS/blob/master/Macro/hash_define.sas and                                                      
            https://github.com/scottbass/SAS/blob/master/Macro/hash_lookup.sas                                                          
            https://github.com/scottbass/SAS/blob/master/Macro/parmv.sas                                                                
            https://github.com/scottbass/SAS/blob/master/Macro/loop.sas                                                                 
            https://github.com/scottbass/SAS/blob/master/Macro/seplist.sas                                                              
            Macros also on end and in my repo, run code to store in your autocall library                                               
                                                                                                                                        
         7. SORT and HASH by KSharp                                                                                                     
            KSharp                                                                                                                      
            https://communities.sas.com/t5/user/viewprofilepage/user-id/18408                                                           
                                                                                                                                        
    github                                                                                                                              
    https://tinyurl.com/yxjl6ast                                                                                                        
    https://github.com/rogerjdeangelis/utl-shifting-rows-up-and-down-by-group-to-fill-in-missing-values                                 
                                                                                                                                        
    SAS Forum                                                                                                                           
    https://tinyurl.com/y3blel56                                                                                                        
    https://communities.sas.com/t5/SAS-Programming/Copying-data-down-a-column-in-SAS/m-p/557341                                         
                                                                                                                                        
    I have a dataset that has blanks and I would like to fill in the                                                                    
    spaces based on other columns. HT, WT, and BI are filled in for                                                                     
    patient and visit by variable. I would like to copy over the                                                                        
    values of each patient and visit into the other variables.                                                                          
    Can this be done by writing if patient and visit match,                                                                             
    then copy value over? Or is there another way?                                                                                      
                                                                                                                                        
    *_                   _                                                                                                              
    (_)_ __  _ __  _   _| |_                                                                                                            
    | | '_ \| '_ \| | | | __|                                                                                                           
    | | | | | |_) | |_| | |_                                                                                                            
    |_|_| |_| .__/ \__,_|\__|                                                                                                           
            |_|                                                                                                                         
    ;                                                                                                                                   
                                                                                                                                        
    data have;                                                                                                                          
    input Patient $ Visit Variable $ HT WT;                                                                                             
    cards4;                                                                                                                             
    A 1 H 1 .                                                                                                                           
    A 2 H 3 .                                                                                                                           
    A 3 H 4 .                                                                                                                           
    A 1 W . 2                                                                                                                           
    A 2 W . 5                                                                                                                           
    A 3 W . 9                                                                                                                           
    B 1 H 1 .                                                                                                                           
    B 2 H 3 .                                                                                                                           
    B 3 H 1 .                                                                                                                           
    B 1 W . 2                                                                                                                           
    B 2 W . 4                                                                                                                           
    B 3 W . 1                                                                                                                           
    ;;;;                                                                                                                                
    run;quit;                                                                                                                           
                                                                                                                                        
                                                                                                                                        
    Up to 40 obs WORK.HAVE total obs=12               | RULES                                                                           
                                                      |                                                                                 
    Obs    PATIENT    VISIT    VARIABLE    HT    WT   | HT    WT                                                                        
                                                      |                                                                                 
      1       A         1         H         1     .   |  1     2                                                                        
      2       A         2         H         3     .   |  3     5                                                                        
      3       A         3         H         4     .   |  4 |   9  ^ Shift up                                                            
                                                           V      |                                                                     
      4       A         1         W         .     2   |  1     2                                                                        
      5       A         2         W         .     5   |  3     5                                                                        
      6       A         3         W         .     9   |  4     9                                                                        
                                                                                                                                        
      7       B         1         H         1     .   |  1     2                                                                        
      8       B         2         H         3     .   |  3     4                                                                        
      9       B         3         H         1     .   |  1     1                                                                        
                                                                                                                                        
     10       B         1         W         .     2   |  1     2                                                                        
     11       B         2         W         .     4   |  3     4                                                                        
     12       B         3         W         .     1   |  1     1                                                                        
                                                                                                                                        
                                                                                                                                        
    *            _               _                                                                                                      
      ___  _   _| |_ _ __  _   _| |_                                                                                                    
     / _ \| | | | __| '_ \| | | | __|                                                                                                   
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                    
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                   
                    |_|                                                                                                                 
    ;                                                                                                                                   
                                                                                                                                        
    Up to 40 obs from WANT total obs=12                                                                                                 
                                                                                                                                        
    Obs    PATIENT    VISIT    VARIABLE    HT    WT                                                                                     
                                                                                                                                        
      1       A         1         H         1     2                                                                                     
      2       A         2         H         3     5                                                                                     
      3       A         3         H         4     9                                                                                     
      4       A         1         W         1     2                                                                                     
      5       A         2         W         3     5                                                                                     
      6       A         3         W         4     9                                                                                     
      7       B         1         H         1     2                                                                                     
      8       B         2         H         3     4                                                                                     
      9       B         3         H         1     1                                                                                     
     10       B         1         W         1     2                                                                                     
     11       B         2         W         3     4                                                                                     
     12       B         3         W         1     1                                                                                     
                                                                                                                                        
    *                                                                                                                                   
     _ __  _ __ ___   ___ ___  ___ ___                                                                                                  
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                                                 
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                 
    | .__/|_|  \___/ \___\___||___/___/                                                                                                 
    |_|                                                                                                                                 
    ;                                                                                                                                   
                                                                                                                                        
                                                                                                                                        
    *****************************                                                                                                       
    1. Harcoded mutiple merging                                                                                                         
    *****************************;                                                                                                      
                                                                                                                                        
    data want;                                                                                                                          
        merge have(where=(ht=.) drop=wt) have(where=(ht ne .) drop= wt)                                                                 
              have(where=(wt=.) drop=ht) have(where=(wt ne .) drop= ht);                                                                
        output;                                                                                                                         
        output;                                                                                                                         
        drop variable; * does nat make sense to keep?;                                                                                  
    run;quit;                                                                                                                           
                                                                                                                                        
    ***************************                                                                                                         
    2. Dynamic mutiple merging                                                                                                          
    ***************************;                                                                                                        
                                                                                                                                        
    %array(ht,values=ht wt);                                                                                                            
    %array(wt,values=wt ht);                                                                                                            
                                                                                                                                        
    data want;                                                                                                                          
        merge                                                                                                                           
           %do_over(htwt wtht,phrase=                                                                                                   
              have(where=(?ht=.) drop=?wt) have(where=(?ht ne .) drop= ?wt)                                                             
           );                                                                                                                           
        output;                                                                                                                         
        output;                                                                                                                         
        drop=variable;                                                                                                                  
    run;quit;                                                                                                                           
                                                                                                                                        
    **************************                                                                                                          
    3. Arrays by Astounding                                                                                                             
    **************************;                                                                                                         
                                                                                                                                        
    data want ;                                                                                                                         
       array htvals {3} _temporary_;                                                                                                    
       array wtvals {3} _temporary_;                                                                                                    
       do until (last.patient);                                                                                                         
          set have;                                                                                                                     
          by patient;                                                                                                                   
          if ht > . then htvals{visit} = ht;                                                                                            
          if wt > . then wtvals{visit} = wt;                                                                                            
       end;                                                                                                                             
       do until (last.patient);                                                                                                         
          set have;                                                                                                                     
          by patient;                                                                                                                   
          if ht = . then ht = htvals{visit};                                                                                            
          if wt = . then wt = wtvals{visit};                                                                                            
          output;                                                                                                                       
       end;                                                                                                                             
       do _n_=1 to 3;                                                                                                                   
          htvals{_n_} = .;                                                                                                              
          wtvals{_n_} = .;                                                                                                              
       end;                                                                                                                             
    run;                                                                                                                                
                                                                                                                                        
                                                                                                                                        
    **********************************************                                                                                      
    4. Sort data so we only need to retain 'lag1'                                                                                       
    **********************************************;                                                                                     
                                                                                                                                        
    proc sort data=have out=have2;                                                                                                      
      by patient visit ;                                                                                                                
    run;                                                                                                                                
                                                                                                                                        
    data want (rename=(HT_=HT   WT_=WT));                                                                                               
                                                                                                                                        
      set have2;                                                                                                                        
      by patient visit ;                                                                                                                
                                                                                                                                        
      *We want to keep the values from the variables,                                                                                   
       but can only do this if they are not from the input data set.;                                                                   
                                                                                                                                        
      retain HT_ WT_ ;                                                                                                                  
                                                                                                                                        
      *Put the right values in the right variable.;                                                                                     
                                                                                                                                        
      if variable = "H" then HT_ = HT;                                                                                                  
      else if variable = "W" then WT_ = WT;                                                                                             
                                                                                                                                        
      if last.visit then output;                                                                                                        
                                                                                                                                        
      drop variable HT WT;                                                                                                              
                                                                                                                                        
      run;                                                                                                                              
                                                                                                                                        
                                                                                                                                        
    **********************************************                                                                                      
    5. SQL                                                                                                                              
    **********************************************;                                                                                     
                                                                                                                                        
    proc sql;                                                                                                                           
       create                                                                                                                           
          table want as                                                                                                                 
       select                                                                                                                           
          a.patient                                                                                                                     
         ,a.visit                                                                                                                       
         ,a.variable                                                                                                                    
         ,b.ht                                                                                                                          
         ,b.wt                                                                                                                          
      from                                                                                                                              
         have as a                                                                                                                      
      inner join                                                                                                                        
            (                                                                                                                           
             select                                                                                                                     
                patient                                                                                                                 
               ,visit                                                                                                                   
               ,max(ht) as ht                                                                                                           
               ,max(wt) as wt                                                                                                           
             from                                                                                                                       
               have                                                                                                                     
             group by                                                                                                                   
               patient                                                                                                                  
              ,visit                                                                                                                    
           ) as b                                                                                                                       
      on                                                                                                                                
           a.patient = b.patient and                                                                                                    
           a.visit   = b.visit                                                                                                          
      order by                                                                                                                          
           a.patient                                                                                                                    
          ,a.variable                                                                                                                   
          ,a.visit                                                                                                                      
    ;quit;                                                                                                                              
                                                                                                                                        
                                                                                                                                        
    **********************************************                                                                                      
    6. HASH by Scott Bass ( easy HASH? )                                                                                                
    **********************************************;                                                                                     
                                                                                                                                        
    proc summary data=have nway;                                                                                                        
       class Patient Visit;                                                                                                             
       var HT WT;                                                                                                                       
       output out=max(drop=_:) max=;                                                                                                    
    run;                                                                                                                                
                                                                                                                                        
    data want;                                                                                                                          
       * set PDV order (optional) ;                                                                                                     
       if 0 then set have;                                                                                                              
                                                                                                                                        
       * initialize &_hashnum_ to zero then declare hash objects ;                                                                      
       %let _hashnum_=0;                                                                                                                
       %hash_define(data=max, keys=Patient Visit, vars=HT WT);                                                                          
                                                                                                                                        
       set have;                                                                                                                        
       %hash_lookup;                                                                                                                    
                                                                                                                                        
       drop _rc:;                                                                                                                       
    run;                                                                                                                                
                                                                                                                                        
                                                                                                                                        
    **********************************************                                                                                      
    7. SORT and HASH by KSharp                                                                                                          
    **********************************************;                                                                                     
                                                                                                                                        
    proc sort data=have(drop= Variable ) out=temp;                                                                                      
     by Patient  Visit;                                                                                                                 
    run;                                                                                                                                
                                                                                                                                        
    data key;                                                                                                                           
     update temp(obs=0) temp;                                                                                                           
     by Patient  Visit;                                                                                                                 
    run;                                                                                                                                
                                                                                                                                        
    data want;                                                                                                                          
                                                                                                                                        
     if _n_=1 then do;                                                                                                                  
       if 0 then set key;                                                                                                               
       declare hash h(dataset:'key');                                                                                                   
       h.definekey( 'Patient' ,'Visit');                                                                                                
       h.definedata('HT', 'WT', 'BI');                                                                                                  
       h.definedone();                                                                                                                  
     end;                                                                                                                               
                                                                                                                                        
    set have(drop=HT WT );                                                                                                              
      call missing(HT ,WT );                                                                                                            
      h.find();                                                                                                                         
    run;                                                                                                                                
                                                                                                                                        
                                                                                                                                        
    *                                                                                                                                   
     _ __ ___   __ _  ___ _ __ ___  ___                                                                                                 
    | '_ ` _ \ / _` |/ __| '__/ _ \/ __|                                                                                                
    | | | | | | (_| | (__| | | (_) \__ \                                                                                                
    |_| |_| |_|\__,_|\___|_|  \___/|___/                                                                                                
                                                                                                                                        
    ;                                                                                                                                   
    *_               _            _       __ _                                                                                          
    | |__   __ _ ___| |__      __| | ___ / _(_)_ __   ___                                                                               
    | '_ \ / _` / __| '_ \    / _` |/ _ \ |_| | '_ \ / _ \                                                                              
    | | | | (_| \__ \ | | |  | (_| |  __/  _| | | | |  __/                                                                              
    |_| |_|\__,_|___/_| |_|___\__,_|\___|_| |_|_| |_|\___|                                                                              
                         |_____|                                                                                                        
    ;                                                                                                                                   
                                                                                                                                        
    * just change path below and run - all three macros will be written to you autocall;                                                
                                                                                                                                        
                                                                                                                                        
    %let oto=c:/oto;                                                                                                                    
                                                                                                                                        
    %put &=oto;                                                                                                                         
                                                                                                                                        
                                                                                                                                        
    filename ft15f001 "&oto./hash_define.sas";                                                                                          
    parmcards4;                                                                                                                         
    /*====================================================================                                                              
    Program Name            : hash_define.sas                                                                                           
    Purpose                 : Defines a hash object for later lookup.                                                                   
    SAS Version             : SAS 9.1.3                                                                                                 
    Input Data              : SAS input dataset                                                                                         
    Output Data             : None                                                                                                      
    Macros Called           : parmv, seplist                                                                                            
    Originally Written by   : Scott Bass                                                                                                
    Date                    : 12MAY2010                                                                                                 
    Program Version #       : 1.0                                                                                                       
    ======================================================================                                                              
    Copyright (c) 2016 Scott Bass (sas_l_739@yahoo.com.au)                                                                              
    This code is licensed under the Unlicense license.                                                                                  
    For more information, please refer to http://unlicense.org/UNLICENSE.                                                               
    =======================================================================                                                             
    Modification History    :                                                                                                           
    Programmer              : Scott Bass                                                                                                
    Date                    : 22JUL2011                                                                                                 
    Change/reason           : Added support for dataset options for hash                                                                
                              object since they are now supported in                                                                    
                              SAS 9.2                                                                                                   
    Program Version #       : 1.1                                                                                                       
    Programmer              : Scott Bass                                                                                                
    Date                    : 15AUG2011                                                                                                 
    Change/reason           : Added multidata option.                                                                                   
    Program Version #       : 1.2                                                                                                       
    Programmer              : Scott Bass                                                                                                
    Date                    : 04FEB2016                                                                                                 
    Change/reason           : Added hashname option.                                                                                    
    Program Version #       : 1.3                                                                                                       
    Programmer              : Scott Bass                                                                                                
    Date                    : 30MAY2016                                                                                                 
    Change/reason           : Added duplicate option.                                                                                   
    Program Version #       : 1.4                                                                                                       
    ====================================================================*/                                                              
                                                                                                                                        
    /*--------------------------------------------------------------------                                                              
    Usage:                                                                                                                              
    data source;                                                                                                                        
       length key1 key2 svar1 svar2 $1;                                                                                                 
       input  key1 key2 svar1 svar2;                                                                                                    
       datalines;                                                                                                                       
    E  F  1  2                                                                                                                          
    A  B  3  4                                                                                                                          
    C  D  5  6                                                                                                                          
    ;                                                                                                                                   
    run;                                                                                                                                
    data lookup;                                                                                                                        
       length key1 key2 lvar3 lvar4 dontwant $1;                                                                                        
       input  key1 key2 lvar3 lvar4 dontwant;                                                                                           
       datalines;                                                                                                                       
    C  D  7  8  X                                                                                                                       
    E  F  9  0  X                                                                                                                       
    ;                                                                                                                                   
    run;                                                                                                                                
    * "Standard" lookup ;                                                                                                               
    data joined;                                                                                                                        
       * set PDV order (optional) ;                                                                                                     
       if 0 then set source;                                                                                                            
       * initialize &_hashnum_ to zero then declare hash objects ;                                                                      
       %let _hashnum_=0;                                                                                                                
       %hash_define(data=lookup, keys=key1 key2, vars=lvar3);                                                                           
       %hash_define(data=lookup, keys=key1,      vars=lvar4);  * multiple hashes can be declared ;                                      
       set source;                                                                                                                      
       %hash_lookup;                                                                                                                    
       if _rc_h1 ne 0 then put "Lookup failed for hash #1 " key1= key2=;                                                                
       if _rc_h2 ne 0 then put "Lookup failed for hash #2 " key1= key2=;                                                                
       if sum(of _rc_h:) ne 0 then put "Some lookup failed, generic error processing";                                                  
       if (_n_ = 3) then do;                                                                                                            
          %hash_lookup(hashnum=2);                                                                                                      
          put key1= key2= lvar4=;                                                                                                       
       end;                                                                                                                             
    *  drop _rc_h:;  * optional ;                                                                                                       
    run;                                                                                                                                
    proc print;                                                                                                                         
    run;                                                                                                                                
    ======================================================================                                                              
    * "Standard" lookup with renames ;                                                                                                  
    data joined;                                                                                                                        
       * initialize &_hashnum_ to zero then declare hash objects ;                                                                      
       %let _hashnum_=0;                                                                                                                
       %hash_define(data=lookup, keys=key1 key2, vars=lvar3 lvar4, rename=key1=foo key2=bar lvar3=blah lvar4=blech);                    
       set source (keep=key1 key2 rename=(key1=foo key2=bar));                                                                          
       %hash_lookup;                                                                                                                    
    *  drop _rc_h:;  * optional ;                                                                                                       
    run;                                                                                                                                
    proc print;                                                                                                                         
    run;                                                                                                                                
    ======================================================================                                                              
    * Multidata lookup - better source/lookup example datasets ;                                                                        
    data lookup;                                                                                                                        
      set                                                                                                                               
        sashelp.class                                                                                                                   
        sashelp.class                                                                                                                   
        sashelp.class                                                                                                                   
      ;                                                                                                                                 
    run;                                                                                                                                
    proc sort;                                                                                                                          
      by name;                                                                                                                          
    run;                                                                                                                                
    * build dummy date ranges ;                                                                                                         
    data lookup;                                                                                                                        
      set lookup;                                                                                                                       
      date=(_n_-1)*3;                                                                                                                   
      format date date7.;                                                                                                               
    run;                                                                                                                                
    * Multidata lookup with single %hash_lookup macro call ;                                                                            
    data joined;                                                                                                                        
      * set PDV order (optional) ;                                                                                                      
      if 0 then set lookup;                                                                                                             
      * initialize &_hashnum_ to zero then declare hash objects ;                                                                       
      %let _hashnum_=0;                                                                                                                 
      %hash_define(data=lookup, keys=name, vars=_all_, multidata=Y)                                                                     
      set sashelp.class (keep=name);                                                                                                    
      %hash_lookup(hashnum=1, lookup="01JAN60"d le date le "15JAN60"d, link=derive, return=Y)                                           
      * some dummy derivations for illustration only ;                                                                                  
      derive:                                                                                                                           
        height=height*1000;                                                                                                             
        weight=age*1000;                                                                                                                
        new=date;                                                                                                                       
        format new yymmddd.;                                                                                                            
      return;                                                                                                                           
    *  drop _rc_h:;  * optional ;                                                                                                       
    run;                                                                                                                                
    proc print;                                                                                                                         
    run;                                                                                                                                
    ======================================================================                                                              
    * Multidata lookup with multiple %hash_lookup macro calls ;                                                                         
    * (for code generation illustration only - normally different lookup datasets would be used) ;                                      
    data joined;                                                                                                                        
      * set PDV order (optional) ;                                                                                                      
      if 0 then set lookup;                                                                                                             
      * initialize &_hashnum_ to zero then declare hash objects ;                                                                       
      %let _hashnum_=0;                                                                                                                 
      %hash_define(data=lookup, keys=name, vars=_all_, multidata=Y)                                                                     
      %hash_define(data=lookup, keys=name, vars=_all_, multidata=Y)                                                                     
      set sashelp.class (keep=name);                                                                                                    
      %hash_lookup(hashnum=1, lookup="01JAN60"d le date le "15JAN60"d, link=derive, return=N)                                           
      %hash_lookup(hashnum=2, lookup=age le 12,                        link=derive, return=Y)                                           
      * some dummy derivations for illustration only ;                                                                                  
      derive:                                                                                                                           
        height=height*1000;                                                                                                             
        weight=age*1000;                                                                                                                
        new=date;                                                                                                                       
        format new yymmddd.;                                                                                                            
      return;                                                                                                                           
    *  drop _rc_h:;  * optional ;                                                                                                       
    run;                                                                                                                                
    proc print;                                                                                                                         
    run;                                                                                                                                
    ======================================================================                                                              
    * Hash names used instead of hash numbers ;                                                                                         
    data joined;                                                                                                                        
      * set PDV order (optional) ;                                                                                                      
      if 0 then set lookup;                                                                                                             
      %let _hashnum_=0;                                                                                                                 
      %hash_define(hashname=foo, data=lookup, keys=name, vars=_all_)                                                                    
      * This next invocation does not specify an explicit name, ;                                                                       
      * so its name is the default name _h2 ;                                                                                           
      %hash_define(              data=lookup, keys=name, vars=_all_)                                                                    
      %hash_define(hashname=_bar,data=lookup, keys=name, vars=_all_)                                                                    
                                                                                                                                        
      * At this point, &_hashname_=foo | _h2 | _bar ;                                                                                   
      set sashelp.class (keep=name);                                                                                                    
                                                                                                                                        
      * even though a hash name was used, the default hash_lookup ;                                                                     
      * will process all hash objects ;                                                                                                 
      %hash_lookup;                                                                                                                     
                                                                                                                                        
      * or you can process by number ;                                                                                                  
      %hash_lookup(hashnum=1)                                                                                                           
      %hash_lookup(hashnum=2)                                                                                                           
      %hash_lookup(hashnum=3)                                                                                                           
                                                                                                                                        
      * or you can process by name (in any order) ;                                                                                     
      %hash_lookup(hashname=_bar)                                                                                                       
      %hash_lookup(hashname=foo)                                                                                                        
      if _rc_foo ne 0 then put "Lookup failed for hash foo " key1= key2=;                                                               
      if _rc_bar ne 0 then put "Lookup failed for hash bar " key1= key2=;                                                               
    *  drop _rc_:;  * optional ;                                                                                                        
    run;                                                                                                                                
    ----------------------------------------------------------------------                                                              
    Notes:                                                                                                                              
    Set &_hashnum_ = 0 before calling this macro for the first time.                                                                    
    &_hashnum_ is incremented each time this macro is called.                                                                           
    (Technically you do not have to set _hashnum_ in the calling program,                                                               
     but it will get created on the first %hash_define invocation,                                                                      
     and incremented each time %hash_define is called.                                                                                  
     This would be a problem in a development environment like EG or DMS,                                                               
     but would not be an issue in batch processing.                                                                                     
     Do yourself a favor and just set _hashnum_=0 in the calling program.)                                                              
    You do not need to explicitly set &_hashname_.                                                                                      
    It is set to blank when &_hashnum_=1, and appended to each time                                                                     
    %hash_define is called.                                                                                                             
    Neither the source nor the lookup datasets need to be sorted.                                                                       
    Each time the find() command executes (%hash_lookup), the current                                                                   
    key values in the PDV are used to lookup the corresponding record in                                                                
    the lookup dataset(s).  If found, the satellite variables are assigned                                                              
    in the PDV.                                                                                                                         
    The default hash object name is _h1, _h2, ..., _h<n>, where "n"                                                                     
    is the number of times %hash_define was called.                                                                                     
    If the HASHNAME parameter is specified, that name is used for the                                                                   
    hash object, and for the return code set by %hash_lookup.                                                                           
    The default return code variable is _rc_h1, _rc_h2,..., _rc_n<n>.                                                                   
    If the HASHNAME parameter was specified in %hash_define, the                                                                        
    return code variable is named _rc_<hashname>.  If the hashname                                                                      
    contains a leading underscore, it is removed from the return                                                                        
    code variable.                                                                                                                      
    For example, if the HASHNAME parameter = _myhash,                                                                                   
    the generated code will be                                                                                                          
    _rc_myhash = _myhash.find(), instead of                                                                                             
    _rc__myhash = _myhash.find().                                                                                                       
    The return code indicates whether the lookup was successful,                                                                        
    and is similar to IN= dataset option on a merge.                                                                                    
    If the VARS parameter is not specified, then no additional variables                                                                
    are joined with the master dataset.  Typically this is used when the                                                                
    only thing you are interested in is the existence of the key variables                                                              
    in the lookup dataset.  This is similar to a data step merge with no                                                                
    additional variables other than the keys, and processing the IN=                                                                    
    dataset option.  Check the value _rc_h# = 0 for successful key lookup.                                                              
    If the VARS parameter is set to _ALL_, then the ALL parameter is                                                                    
    specified in the hash object declaration, and ALL variables from the                                                                
    lookup dataset are joined with the master dataset.                                                                                  
    If the RENAME parameter is specified, it must be specified as                                                                       
    physical_name1=virtual_name1 physical_name2 = virtual_name2, etc.                                                                   
    Spaces are allowed between the equals sign and the oldname/newname                                                                  
    variable pairs.                                                                                                                     
    If the RENAME parameter is specified, the KEYS and VARS parameters must                                                             
    be specified from the perspective of the virtual names, i.e. the                                                                    
    renamed variables.                                                                                                                  
    By default, the first key in the load dataset is added to the hash                                                                  
    object, and all other keys are ignored.                                                                                             
    If REPLACE=R, then the last key in the load dataset is added to the hash object.                                                    
    If REPLACE=E, then duplicate keys in the load dataset will generate an error.                                                       
    If MULTIDATA=Y, then multiple values of the keys are permitted in the                                                               
    lookup hash object.  Otherwise, only the first occurence of each key                                                                
    value will be added to the lookup hash object.                                                                                      
    If MULTIDATA=Y, you would usually pair this with a LOOKUP=<subsetting                                                               
    criteria> %hash_lookup macro call.  See usage example above for details.                                                            
    The hash lookup is a lookup/join, not a merge.  It does NOT merge the                                                               
    additional observations from the lookup dataset. The resulting number                                                               
    of observations will match the number of observations in the source                                                                 
    dataset.                                                                                                                            
    --------------------------------------------------------------------*/                                                              
                                                                                                                                        
                                                                                                                                        
    %macro hash_define                                                                                                                  
    /*--------------------------------------------------------------------                                                              
    Defines a hash object for later lookup                                                                                              
    --------------------------------------------------------------------*/                                                              
    (DATA=         /* Lookup dataset (Opt).                             */                                                              
                   /* If specified, the lookup dataset loads the hash   */                                                              
                   /* object.  If not specified, an empty hash object   */                                                              
                   /* is created.                                       */                                                              
    ,KEYS=         /* Lookup keys (REQ).                                */                                                              
    ,VARS=         /* Lookup satellite variables (Opt).                 */                                                              
                   /* Default is no additional variables from the lookup*/                                                              
                   /* dataset.  Specify _ALL_ to return all variables   */                                                              
                   /* from the lookup dataset.                          */                                                              
    ,RENAME=       /* Rename lookup keys or satellite variables (Opt).  */                                                              
                   /* If specified, then specify as oldname1=newname1   */                                                              
                   /* oldname2=newname2 etc.                            */                                                              
    ,WHERE=        /* Where clause to apply to the lookup dataset(Opt). */                                                              
    ,KEEP=         /* Additional keep variables (Opt).                  */                                                              
                   /* KEYS= and VARS= variables are automatically kept, */                                                              
                   /* specify KEEP= if you have variables needed for a  */                                                              
                   /* WHERE= clause that are neither key nor satellite  */                                                              
                   /* variables.                                        */                                                              
    ,ORDERED=N     /* Store lookup table in sorted order? (REQ).        */                                                              
                   /* Valid values are N=none, A=ascending, D=descending*/                                                              
                   /* (case insensitive).                               */                                                              
                   /* Default=none.                                     */                                                              
    ,MULTIDATA=N   /* Multiple key values allowed in the lookup hash    */                                                              
                   /* object? (REQ).                                    */                                                              
                   /* Default value is NO.  Valid values are:           */                                                              
                   /* 0 1 OFF N NO F FALSE and ON Y YES T TRUE          */                                                              
                   /* OFF N NO F FALSE and ON Y YES T TRUE              */                                                              
                   /* (case insensitive) are acceptable aliases for     */                                                              
                   /* 0 and 1 respectively.                             */                                                              
    ,REPLACE=      /* Replace keys that have already been loaded into   */                                                              
                   /* the hash object? (Opt).                           */                                                              
                   /* Default value is <blank>, the first key is loaded */                                                              
                   /* and all other keys are ignored.                   */                                                              
                   /* If REPLACE=R | REPLACE, duplicate keys overwrite  */                                                              
                   /* previous keys, i.e. the last key is loaded.       */                                                              
                   /* If REPLACE=E | ERROR, duplicate keys will         */                                                              
                   /* generate an error.                                */                                                              
    ,HASHNAME=     /* Explicit hash object name (Opt.)                  */                                                              
                   /* If not specified, the default name                */                                                              
                   /* _h<hashnum> is used.                              */                                                              
    );                                                                                                                                  
                                                                                                                                        
    %local macro parmerr keep rx hn;                                                                                                    
    %let macro = &sysmacroname;                                                                                                         
                                                                                                                                        
    %* check input parameters ;                                                                                                         
    %parmv(DATA,         _req=0,_words=0,_case=N)                                                                                       
    %parmv(KEYS,         _req=1,_words=1,_case=N)                                                                                       
    %parmv(VARS,         _req=0,_words=1,_case=N)                                                                                       
    %parmv(RENAME,       _req=0,_words=1,_case=N)                                                                                       
    %parmv(WHERE,        _req=0,_words=1,_case=N)                                                                                       
    %parmv(KEEP,         _req=0,_words=1,_case=N)                                                                                       
    %parmv(ORDERED,      _req=0,_words=0,_val=A D N)                                                                                    
    %parmv(MULTIDATA,    _req=1,_words=0,_case=U,_val=0 1)                                                                              
    %parmv(REPLACE,      _req=0,_words=0,_case=U,_val=R REPLACE E ERROR)                                                                
    %parmv(HASHNAME,     _req=0,_words=0,_case=N)                                                                                       
                                                                                                                                        
    %if (&parmerr) %then %goto quit;                                                                                                    
                                                                                                                                        
    %* we need to transform the keys and vars variables expressed as ;                                                                  
    %* the physical variable names into the renamed variables ;                                                                         
    %* keys and vars must match the actual variable names ;                                                                             
    %if (%upcase(&vars) ne _ALL_) %then %let keep=&keys &vars &keep;                                                                    
                                                                                                                                        
    %* remove spaces around equals sign ;                                                                                               
    %let rx=%sysfunc(prxparse(s/\s*=\s*/=/));                                                                                           
    %let rename=%sysfunc(prxchange(&rx,-1,&rename));                                                                                    
    %syscall prxfree(rx);                                                                                                               
                                                                                                                                        
    %* convert the keep variables to the physical names ;                                                                               
    %macro _convert_;                                                                                                                   
      %let rx=%sysfunc(prxparse(s/%scan(&word,2,=)/%scan(&word,1,=)/i));                                                                
      %let keep=%sysfunc(prxchange(&rx,-1,&keep));                                                                                      
      %syscall prxfree(rx);                                                                                                             
    %mend;                                                                                                                              
    %loop(%superq(rename),mname=_convert_)                                                                                              
                                                                                                                                        
    %* declare _hashnum_ as a global variable for use in %hash_lookup macro ;                                                           
    %if (^%symexist(_hashnum_)) %then %do;                                                                                              
      %global _hashnum_;                                                                                                                
      %let _hashnum_=0;                                                                                                                 
    %end;                                                                                                                               
                                                                                                                                        
    %* declare _hashname_ as a global variable for use in %hash_lookup macro ;                                                          
    %if (^%symexist(_hashname_)) %then %do;                                                                                             
      %global _hashname_;                                                                                                               
      %let _hashname_= ;                                                                                                                
    %end;                                                                                                                               
                                                                                                                                        
    %* increment _hashnum_ ;                                                                                                            
    %let _hashnum_ = %eval(&_hashnum_+1);                                                                                               
                                                                                                                                        
    %* if this is the first iteration set _hashname_ to blank ;                                                                         
    %if (&_hashnum_ eq 1) %then %let _hashname_ = ;                                                                                     
                                                                                                                                        
    %* if an explicit hash name was specified use it, ;                                                                                 
    %* otherwise set the default hash name of _h<hashnum> ;                                                                             
    %if (&hashname eq ) %then                                                                                                           
       %let hn=_h&_hashnum_;                                                                                                            
    %else                                                                                                                               
       %let hn=&hashname;                                                                                                               
                                                                                                                                        
    %* append the hashname to &_hashname_ with a pipe (|) delimiter ;                                                                   
    %if (&_hashnum_ gt 1) %then %let _hashname_ = &_hashname_ |;                                                                        
    %let _hashname_ = &_hashname_ &hn;                                                                                                  
                                                                                                                                        
    %* build dataset options ;                                                                                                          
    %local options;                                                                                                                     
                                                                                                                                        
    %if (%superq(keep)%superq(rename)%superq(where) ne ) %then                                                                          
      %let options=%str(%();                                                                                                            
    %if (%superq(keep)    ne ) %then                                                                                                    
      %let options=%superq(options) keep=%superq(keep);                                                                                 
    %if (%superq(rename)  ne ) %then                                                                                                    
      %let options=%superq(options) rename=(%superq(rename));                                                                           
    %if (%superq(where)   ne ) %then                                                                                                    
      %let options=%superq(options) where=(%superq(where));                                                                             
    %if (%superq(options) ne ) %then                                                                                                    
      %let options=%superq(options) %str(%));                                                                                           
    %let options=%unquote(&options);                                                                                                    
                                                                                                                                        
    %* declare hash object(s) on first iteration of the datastep ;                                                                      
    if (_n_ = 1) then do;                                                                                                               
      %if (&data ne ) %then %do;                                                                                                        
      %* set the variable attributes, but do not read any records ;                                                                     
      if 0 then set &data &options;                                                                                                     
      %end;                                                                                                                             
                                                                                                                                        
      %* declare the hash object ;                                                                                                      
      declare hash &hn (                                                                                                                
        %if (&data ne ) %then %do;                                                                                                      
        dataset: "&data &options" ,                                                                                                     
        %end;                                                                                                                           
        hashexp: 16                                                                                                                     
        , ordered: "&ordered"                                                                                                           
        %if (&replace eq R or &replace eq REPLACE) %then %do;                                                                           
        , duplicate: "R"                                                                                                                
        %end;                                                                                                                           
        %else                                                                                                                           
        %if (&replace eq E or &replace eq ERROR) %then %do;                                                                             
        , duplicate: "E"                                                                                                                
        %end;                                                                                                                           
        %if (&multidata) %then %do;                                                                                                     
        , multidata: "Y"                                                                                                                
        %end;                                                                                                                           
      );                                                                                                                                
                                                                                                                                        
      %* define keys and satellite variables ;                                                                                          
      &hn..defineKey(%seplist(&keys,nest=QQ));                                                                                          
                                                                                                                                        
      %if (%upcase(&vars) eq _ALL_) %then %do;                                                                                          
        &hn..defineData(ALL: "YES");                                                                                                    
      %end;                                                                                                                             
      %else                                                                                                                             
      %if (&vars ne ) %then %do;                                                                                                        
        &hn..defineData(%seplist(&vars,nest=QQ));                                                                                       
      %end;                                                                                                                             
                                                                                                                                        
      %* end hash declaration ;                                                                                                         
      &hn..defineDone();                                                                                                                
    end;                                                                                                                                
                                                                                                                                        
    %* explicitly set hash variables to missing so they are not ;                                                                       
    %* retained across lookups if a lookup fails ;                                                                                      
    %if (&vars ne ) %then %do;                                                                                                          
       call missing(of &vars);                                                                                                          
    %end;                                                                                                                               
                                                                                                                                        
    %quit:                                                                                                                              
                                                                                                                                        
    %mend hash_define;                                                                                                                  
    ;;;;                                                                                                                                
    run;quit;                                                                                                                           
                                                                                                                                        
                                                                                                                                        
    *_               _         _             _                                                                                          
    | |__   __ _ ___| |__     | | ___   ___ | | ___   _ _ __                                                                            
    | '_ \ / _` / __| '_ \    | |/ _ \ / _ \| |/ / | | | '_ \                                                                           
    | | | | (_| \__ \ | | |   | | (_) | (_) |   <| |_| | |_) |                                                                          
    |_| |_|\__,_|___/_| |_|___|_|\___/ \___/|_|\_\\__,_| .__/                                                                           
                         |_____|                       |_|                                                                              
    ;                                                                                                                                   
                                                                                                                                        
    filename ft15f001 "&oto./hash_lookup.sas";                                                                                          
    parmcards4;                                                                                                                         
    /*====================================================================                                                              
    Program Name            : hash_lookup.sas                                                                                           
    Purpose                 : Lookup satellite variables from a hash object.                                                            
    SAS Version             : SAS 9.1.3                                                                                                 
    Input Data              : SAS input dataset loaded by %hash_define                                                                  
    Output Data             : None                                                                                                      
    Macros Called           : parmv                                                                                                     
    Originally Written by   : Scott Bass                                                                                                
    Date                    : 12MAY2010                                                                                                 
    Program Version #       : 1.0                                                                                                       
    ======================================================================                                                              
    Copyright (c) 2016 Scott Bass (sas_l_739@yahoo.com.au)                                                                              
    This code is licensed under the Unlicense license.                                                                                  
    For more information, please refer to http://unlicense.org/UNLICENSE.                                                               
    =======================================================================                                                             
    Modification History    :                                                                                                           
    Programmer              : Scott Bass                                                                                                
    Date                    : 20JAN2012                                                                                                 
    Change/reason           : Commented out error checking on hashnum parameter                                                         
    Program Version #       : 1.1                                                                                                       
    Programmer              : Scott Bass                                                                                                
    Date                    : 04FEB2016                                                                                                 
    Change/reason           : Added hashname option.                                                                                    
    Program Version #       : 1.3                                                                                                       
    +===================================================================*/                                                              
                                                                                                                                        
    /*--------------------------------------------------------------------                                                              
    Usage:                                                                                                                              
    See Usage in %hash_define macro header.                                                                                             
    However, a multidata lookup deserves a special example.                                                                             
    Note that this example illustrates the *concepts* of the macro code,                                                                
    not the macro code itself.                                                                                                          
    * Multidata lookup - better source/lookup example datasets ;                                                                        
    data lookup;                                                                                                                        
      set                                                                                                                               
        sashelp.class                                                                                                                   
        sashelp.class                                                                                                                   
        sashelp.class                                                                                                                   
      ;                                                                                                                                 
    run;                                                                                                                                
    proc sort;                                                                                                                          
      by name;                                                                                                                          
    run;                                                                                                                                
    * build dummy date ranges ;                                                                                                         
    data lookup;                                                                                                                        
      set lookup;                                                                                                                       
      date=(_n_-1)*3;                                                                                                                   
      format date date7.;                                                                                                               
    run;                                                                                                                                
    * it is useful walking this through the debugger ;                                                                                  
    * remove "/ debug" if you do not want the debugger ;                                                                                
    data joined / debug;                                                                                                                
      * set PDV variable attributes ;                                                                                                   
      if 0 then set lookup;                                                                                                             
      * create hash object ;                                                                                                            
      if _n_=1 then do;                                                                                                                 
        declare hash _h1 (dataset: "lookup", multidata: "Y");                                                                           
        _h1.defineKey("name");                                                                                                          
        _h1.defineData(ALL: "Y");                                                                                                       
        _h1.defineDone();                                                                                                               
      end;                                                                                                                              
      call missing(of _all_);                                                                                                           
      * the lookup key is the name,                                                                                                     
      * which has repeated values in the lookup dataset ;                                                                               
      set sashelp.class (keep=name);                                                                                                    
      * find the first instance of the key ;                                                                                            
      _rc_h1=_h1.find();                                                                                                                
      * find the remaining instances of the keys, ;                                                                                     
      * then apply further subsetting logic via the if statement ;                                                                      
      * link out of the loop to derive all downstream variables ;                                                                       
      * based on the lookup results. ;                                                                                                  
      * we need an explicit output statement to only output the records ;                                                               
      * which match the subsetting criteria. ;                                                                                          
      do while (_rc_h1=0);                                                                                                              
        if ("01JAN60"d le date le "15JAN60"d) then do;                                                                                  
          link derive;                                                                                                                  
          output;                                                                                                                       
        end;                                                                                                                            
        _rc_h1=_h1.find_next();                                                                                                         
      end;                                                                                                                              
      * return;                                                                                                                         
      * if doing several multidata lookups, only the last lookup should include the return statement ;                                  
      * normally this would be done with different lookup hash objects ;                                                                
      * need to do another find since at this point _rc_h1 ne 0 ;                                                                       
      _rc_h1=_h1.find();                                                                                                                
      do while (_rc_h1=0);                                                                                                              
        if (age le 12) then do;                                                                                                         
          link derive;                                                                                                                  
          output;                                                                                                                       
        end;                                                                                                                            
        _rc_h1=_h1.find_next();                                                                                                         
      end;                                                                                                                              
      return;                                                                                                                           
      derive:                                                                                                                           
        height=height*1000;                                                                                                             
        weight=age*1000;                                                                                                                
        new=date;                                                                                                                       
        format new yymmddd.;                                                                                                            
      return;                                                                                                                           
    run;                                                                                                                                
    proc print;                                                                                                                         
    run;                                                                                                                                
    ----------------------------------------------------------------------                                                              
    Notes:                                                                                                                              
    Use %hash_define to load the lookup dataset(s).  This sets the                                                                      
    _hashnum_ macro variable used in this macro.                                                                                        
    The return code from the find() command is returned in data step                                                                    
    variables _rc_h1 - _rc_h<n>, where "n" matches the number of times                                                                  
    %hash_define was called.  No error checking is done on the return                                                                   
    code from within this macro, but error checking could be done in                                                                    
    the calling code.                                                                                                                   
    Alternatively, if the hashname parameter was used, the return code                                                                  
    from the find() command is returned as _rc_<hashname>, eg.                                                                          
    _rc_foo, _rc_bar, _rc_myhash, etc.                                                                                                  
    If the hashname has a leading underscore, only one underscore is used                                                               
    in the return code.  In other words:                                                                                                
       If the hashname is myhash, the hash object is myhash.find(),                                                                     
          and the return code is _rc_myhash.                                                                                            
       If the hashname is _myhash, the hash object is _myhash.find(),                                                                   
          and the return code is _rc_myhash, NOT _rc__myhash.                                                                           
    By default a lookup will be done across all hash objects defined by the                                                             
    %hash_define macro.  Use either the hashnum= or hashname= option                                                                    
    to limit the lookup to a single hash object.                                                                                        
    If the LOOKUP parameter is specified:                                                                                               
      It must be syntactically correct subsetting criteria using if                                                                     
      statement syntax which is used to further subset additional items                                                                 
      which match the lookup key.                                                                                                       
      You would normally also specify the HASHNUM or HASHNAME parameter,                                                                
      unless the logic criteria for all lookups was identical                                                                           
      (highly unlikely).                                                                                                                
      The HASHNUM associated with a LOOKUP parameter lookup must be                                                                     
      associated with a MULTIDATA %hash_define statement.  Otherwise, you                                                               
      will get a runtime error when the find_next() method is executed                                                                  
      against a non-multidata hash object.                                                                                              
      The LOOKUP= %hash_lookup macro call should be the LAST lookup                                                                     
      specified, since by default it also adds the return statement to the                                                              
      generated code.                                                                                                                   
      If several multidata lookups are conducted in the same data step, only                                                            
      the final multidata %hash_lookup invocation should issue the return                                                               
      statement (RETURN=Y).                                                                                                             
      If there are downstream derived variables that rely on the values                                                                 
      returned from the lookups, then you should also specify the LINK                                                                  
      parameter, which should be a data step label marking the start of your                                                            
      derived variables.                                                                                                                
    --------------------------------------------------------------------*/                                                              
                                                                                                                                        
    %macro hash_lookup                                                                                                                  
    /*--------------------------------------------------------------------                                                              
    Lookup satellite variables from a hash object                                                                                       
    --------------------------------------------------------------------*/                                                              
    (HASHNUM=      /* Limit lookup to a specific hash by number (Opt).  */                                                              
    ,HASHNAME=     /* Limit lookup to a specific hash by name (Opt).    */                                                              
    ,LOOKUP=       /* Perform a multidata item lookup? (Opt).           */                                                              
                   /* If specified, then this criteria is used to       */                                                              
                   /* perform addtional multidata item lookups          */                                                              
                   /* and filter the results.                           */                                                              
    ,LINK=         /* Link to additional variable derivations? (Opt).   */                                                              
                   /* If specified, it must be a data step link label   */                                                              
                   /* defined outside this macro that marks the start   */                                                              
                   /* of the additional variable derivations.           */                                                              
    ,RETURN=N      /* Issue a return statement? (REQ).                  */                                                              
                   /* Default value is NO.  Valid values are:           */                                                              
                   /* 0 1 OFF N NO F FALSE and ON Y YES T TRUE          */                                                              
                   /* OFF N NO F FALSE and ON Y YES T TRUE              */                                                              
                   /* (case insensitive) are acceptable aliases for     */                                                              
                   /* 0 and 1 respectively.                             */                                                              
    );                                                                                                                                  
                                                                                                                                        
    %local macro parmerr _num_;                                                                                                         
    %let macro = &sysmacroname;                                                                                                         
                                                                                                                                        
    %* check input parameters ;                                                                                                         
    %if (not %symexist(_hashnum_)) %then                                                                                                
    %parmv(_msg=%nrstr(The _hashnum_ global macro variable does not exist.  %hash_define must be called before calling this macro))     
                                                                                                                                        
    %if (&hashname ne ) %then                                                                                                           
    %if (%sysfunc(findw(&_hashname_,&hashname,|,IRST)) eq 0) %then                                                                      
    %parmv(_msg=%nrstr(The &hashname is not in the &_hashname_ list.  Please review the %hash_define invocations.))                     
                                                                                                                                        
    %parmv(HASHNUM,      _req=0,_words=0,_val=POSITIVE)                                                                                 
    %parmv(HASHNAME,     _req=0,_words=0,_case=N);                                                                                      
    %parmv(LOOKUP,       _req=0,_words=1,_case=N)                                                                                       
    %parmv(LINK,         _req=0,_words=0,_case=N)                                                                                       
    %parmv(RETURN,       _req=1,_words=0,_case=U,_val=0 1)                                                                              
                                                                                                                                        
    /*                                                                                                                                  
    %if (&hashnum ne ) %then                                                                                                            
    %if (&hashnum gt &_hashnum_) %then                                                                                                  
    %parmv(_msg=The hashnum parameter is invalid);                                                                                      
    */                                                                                                                                  
                                                                                                                                        
    %if (&parmerr) %then %goto quit;                                                                                                    
                                                                                                                                        
    %* define utility macro to perform multidata lookups ;                                                                              
    %* since we are ONLY outputting when we have found a record, ;                                                                      
    %* we do not need to set the satellite variables to missing. ;                                                                      
    %* we know they must have been set when the lookup was successful. ;                                                                
    %macro multidata_lookup(criteria);                                                                                                  
      %* the initial lookup was successful ;                                                                                            
      do while (_rc&hn=0);                                                                                                              
        if (&criteria) then do;                                                                                                         
          %if (&link ne ) %then %do;                                                                                                    
          link &link;                                                                                                                   
          %end;                                                                                                                         
          output;                                                                                                                       
        end;                                                                                                                            
        _rc&hn=&hn..find_next();                                                                                                        
      end;                                                                                                                              
    %mend;                                                                                                                              
                                                                                                                                        
    %* perform lookup for each lookup table ;                                                                                           
    %if (&hashname ne ) %then %do;                                                                                                      
      %let hn = &hashname;                                                                                                              
      %let rc = _rc;                                                                                                                    
      %if (%substr(&hn,1,1) eq _) %then                                                                                                 
        %let rc=&rc.&hn;                                                                                                                
      %else                                                                                                                             
        %let rc=&rc._&hn;                                                                                                               
      &rc = &hn..find();                                                                                                                
                                                                                                                                        
      %if (%superq(lookup) ne ) %then %do;                                                                                              
        %multidata_lookup(&lookup)                                                                                                      
      %end;                                                                                                                             
    %end;                                                                                                                               
    %else                                                                                                                               
    %if (&hashnum ne ) %then %do;                                                                                                       
      %let hn = %scan(&_hashname_,&hashnum,|);                                                                                          
      %let rc = _rc;                                                                                                                    
      %if (%substr(&hn,1,1) eq _) %then                                                                                                 
        %let rc=&rc.&hn;                                                                                                                
      %else                                                                                                                             
        %let rc=&rc._&hn;                                                                                                               
      &rc = &hn..find();                                                                                                                
                                                                                                                                        
      %if (%superq(lookup) ne ) %then %do;                                                                                              
        %multidata_lookup(&lookup)                                                                                                      
      %end;                                                                                                                             
    %end;                                                                                                                               
    %else                                                                                                                               
    %do _num_=1 %to &_hashnum_;                                                                                                         
      %let hn = %scan(&_hashname_,&_num_,|);                                                                                            
      %let rc = _rc;                                                                                                                    
      %if (%substr(&hn,1,1) eq _) %then                                                                                                 
        %let rc=&rc.&hn;                                                                                                                
      %else                                                                                                                             
        %let rc=&rc._&hn;                                                                                                               
      &rc = &hn..find();                                                                                                                
                                                                                                                                        
      %if (%superq(lookup) ne ) %then %do;                                                                                              
        %multidata_lookup(&lookup)                                                                                                      
      %end;                                                                                                                             
    %end;                                                                                                                               
                                                                                                                                        
    %if (&return) %then %do;                                                                                                            
      return;                                                                                                                           
    %end;                                                                                                                               
                                                                                                                                        
    %quit:                                                                                                                              
                                                                                                                                        
    %mend hash_lookup;                                                                                                                  
    ;;;;                                                                                                                                
    run;quit;                                                                                                                           
                                                                                                                                        
    %put &=oto;                                                                                                                         
                                                                                                                                        
    *                                                                                                                                   
     _ __   __ _ _ __ _ __ _____   __                                                                                                   
    | '_ \ / _` | '__| '_ ` _ \ \ / /                                                                                                   
    | |_) | (_| | |  | | | | | \ V /                                                                                                    
    | .__/ \__,_|_|  |_| |_| |_|\_/                                                                                                     
    |_|                                                                                                                                 
    ;                                                                                                                                   
                                                                                                                                        
    filename ft15f001 "&oto./parmv.sas";                                                                                                
    parmcards4;                                                                                                                         
    /*=====================================================================                                                             
    Program Name            : parmv.sas                                                                                                 
    Purpose                 : Macro parameter validation utility.                                                                       
                              Returns parmerr=1 and writes ERROR message                                                                
                              to log for parameters with invalid values.                                                                
    SAS Version             : SAS 8.2                                                                                                   
    Input Data              : N/A                                                                                                       
    Output Data             : N/A                                                                                                       
    Macros Called           : None                                                                                                      
    Originally Written by   : Tom Hoffman                                                                                               
    Date                    : 09SEP1996                                                                                                 
    Program Version #       : 1.0                                                                                                       
    =======================================================================                                                             
    Copyright (c) 2016 Scott Bass (sas_l_739@yahoo.com.au)                                                                              
    This code is licensed under the Unlicense license.                                                                                  
    For more information, please refer to http://unlicense.org/UNLICENSE.                                                               
    =======================================================================                                                             
    Modification History    :                                                                                                           
    Programmer              : Tom Hoffman                                                                                               
    Date                    : 16MAR1998                                                                                                 
    Change/reason           : Replaced QTRIM autocall macro with QSYSFUNC                                                               
                              and TRIM in order to avoid conflict with the                                                              
                              i command line macro.                                                                                     
    Program Version #       : 1.1                                                                                                       
    Programmer              : Tom Hoffman                                                                                               
    Date                    : 04OCT1999                                                                                                 
    Change/reason           : Added _val=NONNEGATIVE. Converted _val=0 1 to                                                             
                              map N NO F FALSE OFF --> 0 and Y YES T TRUE                                                               
                              ON --> 1. Added _varchk parameter to support                                                              
                              variables assumed to be defined before macro                                                              
                              invocation.                                                                                               
    Program Version #       : 1.2                                                                                                       
    Programmer              : Tom Hoffman                                                                                               
    Date                    : 12APR00                                                                                                   
    Change/reason           : Changed the word 'parameter' in the message                                                               
                              text to 'macro variable' when _varchk=1.                                                                  
                              Fixed NONNEGATIVE option.                                                                                 
    Program Version #       : 1.3                                                                                                       
    Programmer              : Tom Hoffman                                                                                               
    Date                    : 10JUN01                                                                                                   
    Change/reason           : Added _DEF parameter. Returned S_MSG global                                                               
                              macro variable.                                                                                           
    Program Version #       : 1.4                                                                                                       
    =====================================================================*/                                                             
                                                                                                                                        
    /*---------------------------------------------------------------------                                                             
    Usage:                                                                                                                              
    %macro test;                                                                                                                        
    %local macro parmerr;                                                                                                               
    %let macro = TEST;                                                                                                                  
    %parmv(INTERVAL,   _req=1,_words=1)                                                                                                 
    %parmv(IVAR,       _req=1)                                                                                                          
    %if (%length(&visit) > 7) %then                                                                                                     
       %parmv(IVAR,    _msg=SAS name containing 7 or less characters)                                                                   
    ;                                                                                                                                   
    %parmv(LZERO,      _req=1)                                                                                                          
    %parmv(UZERO,      _req=1)                                                                                                          
    %parmv(HIGH,       _req=1,_val=0 1)                                                                                                 
    %parmv(DAY,        _req=1)                                                                                                          
    %parmv(PRINT,      _req=1,_val=0 1)                                                                                                 
    %if (&parmerr) %then %goto quit;                                                                                                    
    ....                                                                                                                                
    %quit:                                                                                                                              
    %mend test;                                                                                                                         
    -----------------------------------------------------------------------                                                             
    Notes:                                                                                                                              
    =======================================================================                                                             
    This code was developed by HOFFMAN CONSULTING as part of a FREEWARE                                                                 
    macro tool set. Its use is restricted to current and former clients of                                                              
    HOFFMAN CONSULTING as well as other professional colleagues. Questions                                                              
    and suggestions may be sent to TRHoffman@sprynet.com.                                                                               
    Note:  I have received permission from Tom Hoffman to use this macro.                                                               
    Scott Bass                                                                                                                          
    =======================================================================                                                             
    The calling macro requires two local variables, PARMERR and MACRO,                                                                  
    where MACRO's value equals the name of the calling macro.                                                                           
    Invoke macro %parmv once for each macro parameter. After the last                                                                   
    invocation branch to the macro's end whenever PARMERR equals 1 (e.g.,                                                               
    %if (&parmerr) %then %goto quit;).                                                                                                  
    Macro %parmv can be disabled (except for changing case) by setting the                                                              
    global macro variable S_PARMV to 0.                                                                                                 
    Macros using the %parmv tool may not have any parameters in common                                                                  
    with parmv parameters.                                                                                                              
    Use the _MSG parameter to set parmerr to 1 and issue a message based on                                                             
    validation criteria within the calling program.                                                                                     
    Note that for efficiency reasons, parmv does not validate its own                                                                   
    parameters. Only code valid values for the _REQ, _WORDS, _CASE, and                                                                 
    _VARCHK parameters.                                                                                                                 
    For macros that require 'many' non-parameter macro variables that may                                                               
    not be defined by the programming environment, consider using the                                                                   
    CHKMVARS macro rather than setting _varchk=1. Both methods will work,                                                               
    but note that DICTIONARY.MACROS is opened each time that parmv is                                                                   
    invoked with _varchk=1.                                                                                                             
    --------------------------------------------------------------------*/                                                              
                                                                                                                                        
    %macro parmv                                                                                                                        
    /*---------------------------------------------------------------------                                                             
    Macro parameter validation utility. Returns parmerr=1 and writes                                                                    
    ERROR message to log for parameters with invalid values.                                                                            
    ---------------------------------------------------------------------*/                                                             
    (_PARM     /* Macro parameter name (REQ)                             */                                                             
    ,_VAL=     /* List of valid values or POSITIVE for any positive      */                                                             
               /* integer or NONNEGATIVE for any non-negative integer.   */                                                             
               /* When _val=0 1, OFF N NO F FALSE and ON Y YES T TRUE    */                                                             
               /* (case insensitive) are acceptable aliases for 0 and 1  */                                                             
               /* respectively.                                          */                                                             
    ,_REQ=0    /* Value required? 0=No, 1=Yes.                           */                                                             
    ,_WORDS=0  /* Multiple values allowed? 0=No ,1=Yes                   */                                                             
    ,_CASE=U   /* Convert case of parameter value & _val? U=upper,       */                                                             
               /* L=lower,N=no conversion.                               */                                                             
    ,_MSG=     /* When specified, set parmerr to 1 and writes _msg as the*/                                                             
               /* last error message.                                    */                                                             
    ,_VARCHK=0 /* 0=Assume that variable defined by _parm exists.        */                                                             
               /* 1=Check for existence - issue global statement if not  */                                                             
               /* defined (and _req=0).                                  */                                                             
    ,_DEF=     /* Default parameter value when not assigned by calling   */                                                             
               /* macro.                                                 */                                                             
    );                                                                                                                                  
                                                                                                                                        
    %local _word _n _vl _pl _ml _error _parm_mv;                                                                                        
    %global s_parmv s_msg;  /* in case not in global environment */                                                                     
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Initialize error flags, and valid (vl) flag.                                                                                        
    ----------------------------------------------------------------------;                                                             
    %if (&parmerr = ) %then %do;                                                                                                        
       %let parmerr = 0;                                                                                                                
       %let s_msg = ;                                                                                                                   
    %end;                                                                                                                               
    %let _error = 0;                                                                                                                    
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Support undefined values of the _PARM macro variable.                                                                               
    ----------------------------------------------------------------------;                                                             
    %if (&_varchk) %then %do;                                                                                                           
       %let _parm_mv = macro variable;                                                                                                  
       %if ^%symexist(&_parm) %then %do;                                                                                                
          %if (&_req) %then                                                                                                             
             %local &_parm                                                                                                              
          ;                                                                                                                             
          %else                                                                                                                         
             %global &_parm                                                                                                             
          ; ;                                                                                                                           
       %end;                                                                                                                            
    %end;                                                                                                                               
    %else %let _parm_mv = parameter;                                                                                                    
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Get lengths of _val, _msg, and _parm to use as numeric switches.                                                                    
    ----------------------------------------------------------------------;                                                             
    %let _vl = %length(&_val);                                                                                                          
    %let _ml = %length(&_msg);                                                                                                          
    %if %length(&&&_parm) %then                                                                                                         
       %let _pl = %length(%qsysfunc(trim(&&&_parm)));                                                                                   
    %else %if %length(&_def) %then %do;                                                                                                 
       %let _pl = %length(&_def);                                                                                                       
       %let &&_parm = &_def;                                                                                                            
    %end;                                                                                                                               
    %else %let _pl = 0;                                                                                                                 
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    When _MSG is not specified, change case of the parameter and valid                                                                  
    values conditional on the value of the _CASE parameter.                                                                             
    ----------------------------------------------------------------------;                                                             
    %if ^(&_ml) %then %do;                                                                                                              
       %let _parm = %upcase(&_parm);                                                                                                    
       %let _case = %upcase(&_case);                                                                                                    
                                                                                                                                        
       %if (&_case = U) %then %do;                                                                                                      
          %let &_parm = %qupcase(&&&_parm);                                                                                             
          %let _val = %qupcase(&_val);                                                                                                  
       %end;                                                                                                                            
       %else %if (&_case = L) %then %do;                                                                                                
          %if (&_pl) %then %let &_parm = %qsysfunc(lowcase(&&&_parm));                                                                  
          %if (&_vl) %then %let _val = %qsysfunc(lowcase(&_val));                                                                       
       %end;                                                                                                                            
       %else %let _val = %quote(&_val);                                                                                                 
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    When _val=0 1, map supported aliases into 0 or 1.                                                                                   
    ----------------------------------------------------------------------;                                                             
       %if (&_val = 0 1) %then %do;                                                                                                     
          %let _val=%quote(0 (or OFF NO N FALSE F) 1 (or ON YES Y TRUE T));                                                             
          %if %index(%str( OFF NO N FALSE F ),%str( &&&_parm )) %then                                                                   
             %let &_parm = 0;                                                                                                           
          %else %if %index(%str( ON YES Y TRUE T ),%str( &&&_parm )) %then                                                              
             %let &_parm = 1;                                                                                                           
       %end;                                                                                                                            
    %end;                                                                                                                               
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Bail out when no parameter validation is requested                                                                                  
    ----------------------------------------------------------------------;                                                             
    %if (&s_parmv = 0) %then %goto quit;                                                                                                
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Error processing - parameter value not null                                                                                         
    Error 1: Invalid value - not a positive integer                                                                                     
    Error 2: Invalid value - not in valid list                                                                                          
    Error 3: Single value only                                                                                                          
    Error 4: Value required.                                                                                                            
    Error 5: _MSG specified                                                                                                             
    ----------------------------------------------------------------------;                                                             
    %if (&_ml) %then %let _error = 5;                                                                                                   
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Macro variable specified by _PARM is not null.                                                                                      
    ----------------------------------------------------------------------;                                                             
    %else %if (&_pl) %then %do;                                                                                                         
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Loop through possible list of words in the _PARM macro variable.                                                                    
    -----------------------------------------------------------------------;                                                            
       %if ((&_vl) | ^(&_words)) %then %do;                                                                                             
          %let _n = 1;                                                                                                                  
          %let _word = %qscan(&&&_parm,1,%str( ));                                                                                      
    %*---------------------------------------------------------------------                                                             
    Check against valid list for each word in macro parameter                                                                           
    ----------------------------------------------------------------------;                                                             
          %do %while (%length(&_word));                                                                                                 
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Positive integer check.                                                                                                             
    ----------------------------------------------------------------------;                                                             
             %if (&_val = POSITIVE) %then %do;                                                                                          
                %if %sysfunc(verify(&_word,0123456789)) %then                                                                           
                   %let _error = 1;                                                                                                     
                %else %if ^(&_word) %then %let _error = 1;                                                                              
             %end;                                                                                                                      
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Non-negative integer check.                                                                                                         
    ----------------------------------------------------------------------;                                                             
             %else %if (&_val = NONNEGATIVE) %then %do;                                                                                 
                %if %sysfunc(verify(&_word,0123456789)) %then                                                                           
                   %let _error = 1;                                                                                                     
             %end;                                                                                                                      
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Check against valid list. Note blank padding.                                                                                       
    ----------------------------------------------------------------------;                                                             
             %else %if (&_vl) %then %do;                                                                                                
                %if ^%index(%str( &_val ),%str( &_word )) %then                                                                         
                   %let _error = 2;                                                                                                     
             %end;                                                                                                                      
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Get next word from parameter value                                                                                                  
    -----------------------------------------------------------------------;                                                            
             %let _n = %eval(&_n + 1);                                                                                                  
             %let _word = %qscan(&&&_parm,&_n,%str( ));                                                                                 
          %end; %* for each word in parameter value;                                                                                    
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Check for multiple _words. Set error flag if not allowed.                                                                           
    ----------------------------------------------------------------------;                                                             
          %if (&_n ^= 2) & ^(&_words) %then %let _error = 3;                                                                            
                                                                                                                                        
       %end; %* valid not null ;                                                                                                        
                                                                                                                                        
    %end; %* parameter value not null ;                                                                                                 
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Error processing - Parameter value null                                                                                             
    Error 4: Value required.                                                                                                            
    ----------------------------------------------------------------------;                                                             
    %else %if (&_req) %then %let _error = 4;                                                                                            
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Write error messages                                                                                                                
    ----------------------------------------------------------------------;                                                             
    %if (&_error) %then %do;                                                                                                            
       %let parmerr = 1;                                                                                                                
       %put %str( );                                                                                                                    
       %put ERROR: Macro %upcase(&macro) user error.;                                                                                   
                                                                                                                                        
       %if (&_error = 1) %then %do;                                                                                                     
          %put ERROR: &&&_parm is not a valid value for the &_parm &_parm_mv..;                                                         
          %put ERROR: Only positive integers are allowed.;                                                                              
          %let _vl = 0;                                                                                                                 
       %end;                                                                                                                            
                                                                                                                                        
       %else %if (&_error = 2) %then                                                                                                    
          %put ERROR: &&&_parm is not a valid value for the &_parm &_parm_mv..;                                                         
                                                                                                                                        
       %else %if (&_error = 3) %then %do;                                                                                               
          %put ERROR: &&&_parm is not a valid value for the &_parm &_parm_mv..;                                                         
          %put ERROR: The &_parm &_parm_mv may not have multiple values.;                                                               
       %end;                                                                                                                            
                                                                                                                                        
       %else %if (&_error = 4) %then                                                                                                    
          %put ERROR: A value for the &_parm &_parm_mv is required.;                                                                    
                                                                                                                                        
       %else %if (&_error = 5) %then %do;                                                                                               
          %if (&_parm ^= ) %then                                                                                                        
          %put ERROR: &&&_parm is not a valid value for the &_parm &_parm_mv..;                                                         
          %put ERROR: &_msg..;                                                                                                          
       %end;                                                                                                                            
                                                                                                                                        
       %if (&_vl) %then                                                                                                                 
          %put ERROR: Allowable values are: &_val..;                                                                                    
                                                                                                                                        
       %if %length(&_msg) %then %let s_msg = &_msg;                                                                                     
       %else %let s_msg = Problem with %upcase(&macro) parameter values - see LOG for details.;                                         
                                                                                                                                        
    %end; %* errors ;                                                                                                                   
                                                                                                                                        
    %quit:                                                                                                                              
                                                                                                                                        
    %*---------------------------------------------------------------------                                                             
    Unquote the the parameter value, unless it contains an ampersand or                                                                 
    percent sign.                                                                                                                       
    ----------------------------------------------------------------------;                                                             
    %if ^%sysfunc(indexc(&&&_parm,%str(&%%))) %then                                                                                     
       %let &_parm = %unquote(&&&_parm);                                                                                                
                                                                                                                                        
    %mend parmv;                                                                                                                        
    ;;;;                                                                                                                                
    run;quit;                                                                                                                           
                                                                                                                                        
                                                                                                                                        
    *_                                                                                                                                  
    | | ___   ___  _ __                                                                                                                 
    | |/ _ \ / _ \| '_ \                                                                                                                
    | | (_) | (_) | |_) |                                                                                                               
    |_|\___/ \___/| .__/                                                                                                                
                  |_|                                                                                                                   
    ;                                                                                                                                   
                                                                                                                                        
    filename ft15f001 "&oto./loop.sas";                                                                                                 
    parmcards4;                                                                                                                         
    /*=====================================================================                                                             
    Program Name            : loop.sas                                                                                                  
    Purpose                 : A "wrapper" macro to execute code over a                                                                  
                              list of items                                                                                             
    SAS Version             : SAS 8.2                                                                                                   
    Input Data              : N/A                                                                                                       
    Output Data             : N/A                                                                                                       
    Macros Called           : parmv                                                                                                     
    Originally Written by   : Scott Bass                                                                                                
    Date                    : 24APR2006                                                                                                 
    Program Version #       : 1.0                                                                                                       
    =======================================================================                                                             
    Copyright (c) 2016 Scott Bass (sas_l_739@yahoo.com.au)                                                                              
    This code is licensed under the Unlicense license.                                                                                  
    For more information, please refer to http://unlicense.org/UNLICENSE.                                                               
    =======================================================================                                                             
    Modification History    :                                                                                                           
    Programmer              : Scott Bass                                                                                                
    Date                    : 17JUL2006                                                                                                 
    Change/reason           : Added DLM parameter                                                                                       
    Program Version #       : 1.1                                                                                                       
    Programmer              : Scott Bass                                                                                                
    Date                    : 24DEC2009                                                                                                 
    Change/reason           : Made changes as suggested by Ian Whitlock,                                                                
                              updated header with additional usage cases                                                                
    Program Version #       : 1.2                                                                                                       
    Programmer              : Scott Bass                                                                                                
    Date                    : 13MAY2011                                                                                                 
    Change/reason           : Fixed nested macro scoping errors by explicitly                                                           
                              declaring the iterator and word as local.                                                                 
    Program Version #       : 1.3                                                                                                       
    =====================================================================*/                                                             
                                                                                                                                        
    /*---------------------------------------------------------------------                                                             
    Usage:                                                                                                                              
    %macro code;                                                                                                                        
       %put &word;                                                                                                                      
    %mend;                                                                                                                              
    %loop(Hello World);                                                                                                                 
    =======================================================================                                                             
    %let str = Hello,World;                                                                                                             
    %loop(%bquote(&str),dlm=%bquote(,));                                                                                                
    =======================================================================                                                             
    %macro code();                                                                                                                      
       %put &word;                                                                                                                      
    %mend;                                                                                                                              
    %loop(Hello World,mname=code());                                                                                                    
    =======================================================================                                                             
    %macro mymacro;                                                                                                                     
       proc print data=&word;                                                                                                           
       run;                                                                                                                             
    %mend;                                                                                                                              
    proc datasets kill nowarn nolist;                                                                                                   
    quit;                                                                                                                               
    data one;x=1;run;                                                                                                                   
    data two;y=2;run;                                                                                                                   
    proc sql noprint;                                                                                                                   
       select memname into :list separated by '|'                                                                                       
          from dictionary.tables                                                                                                        
          where libname = "WORK" and memtype = "DATA"                                                                                   
       ;                                                                                                                                
    quit;                                                                                                                               
    %loop(&list,dlm=|,mname=mymacro);                                                                                                   
    =======================================================================                                                             
    Calling a macro with parameters:                                                                                                    
    %macro mymacro(parm=&word);                                                                                                         
       %put &parm;                                                                                                                      
    %mend;                                                                                                                              
    %loop(hello world,mname=mymacro);    * this causes a tokenization error ;                                                           
    %loop(hello world,mname=mymacro());  * no error ;                                                                                   
    %mymacro(parm=hi);                                                                                                                  
    Note that the parm is the literal text '&word' (without quotes of course).                                                          
    See additional details in Nested calls of %loop use case below.                                                                     
    =======================================================================                                                             
    Nested calls of %loop:                                                                                                              
    %macro outer;                                                                                                                       
       %put &sysmacroname &__iter__ &word;                                                                                              
       %loop(INNER_1 INNER_2 INNER_3,mname=inner)                                                                                       
    %mend;                                                                                                                              
    %let iter_global=;                                                                                                                  
    %macro inner;                                                                                                                       
       %local iter_local;                                                                                                               
       %global iter_global;                                                                                                             
       %if (&iter_local  eq ) %then %let iter_local=1;                                                                                  
       %if (&iter_global eq ) %then %let iter_global=1;                                                                                 
       %let iter_local=%eval(&iter_local + (&__iter__ * 5));  %* not retained across outer loops ;                                      
       %let iter_global=%eval((&iter_global*2)+(&__iter__));  %* retained across outer loops since it is global ;                       
       %put &sysmacroname &__iter__ &word iter_local=&iter_local iter_global=&iter_global;                                              
       %* let __iter__=999;  %* if uncommented, this would cause a logic error ;                                                        
    %mend;                                                                                                                              
    %loop(OUTER_A OUTER_B OUTER_C OUTER_D OUTER_E,mname=outer)                                                                          
    Do NOT reset __iter__ in any inner macro or it will affect the outer macro.                                                         
    You can *REFERENCE* &__iter__, but do not *CHANGE* its value.                                                                       
    If you need to make logic decisions, copy &__iter__ to a local inner variable.                                                      
    Also, do NOT use %yourmacro(parameter=&word) syntax in nested calls to                                                              
    %loop.  For example, this will not work:                                                                                            
    options mlogic;                                                                                                                     
    %macro level1(firstvar=&word);                                                                                                      
       %put firstvar in Level1:  &firstvar;                                                                                             
       %loop(Variable2, mname=level2())                                                                                                 
    %mend level1;                                                                                                                       
    %macro level2(secondvar=&word);                                                                                                     
       %put secondvar in level2: &secondvar;                                                                                            
       %put INCORRECT: firstvar in Level2: &firstvar;                                                                                   
    %mend level2;                                                                                                                       
    %loop(Variable1, mname=level1())                                                                                                    
    In level1, firstvar is actually assigned the literal text '&word'                                                                   
    (without quotes of course). Inside the level1 macro, this text then                                                                 
    resolves to the value of &word AT THAT TIME. Once the level2 macro                                                                  
    executes, it changes the value of &word, which then changes further                                                                 
    references to &firstvar.                                                                                                            
    Instead, assign firstvar and secondvar to the value of &word INSIDE                                                                 
    each macro, as described for __iter__ above.                                                                                        
    Closely review the mlogic output above for more details.                                                                            
    Instead, code this as follows:                                                                                                      
    options nomlogic;                                                                                                                   
    %macro level1;                                                                                                                      
       %local firstvar;                                                                                                                 
       %let firstvar=&word;                                                                                                             
       %put &sysmacroname: firstvar in Level1:  &firstvar;                                                                              
       %loop(Variable4 Variable5, mname=level2)                                                                                         
    %mend level1;                                                                                                                       
    %macro level2;                                                                                                                      
       %local secondvar;                                                                                                                
       %let secondvar=&word;                                                                                                            
       %put &sysmacroname: secondvar in level2: &secondvar;                                                                             
       %put &sysmacroname: CORRECT: firstvar in Level2: &firstvar;                                                                      
    %mend level2;                                                                                                                       
    %loop(Variable1 Variable2 Variable3, mname=level1)                                                                                  
    -----------------------------------------------------------------------                                                             
    Notes:                                                                                                                              
    The nested macro "%code" must be created at run time before calling                                                                 
    this macro.                                                                                                                         
    Use the macro variable "&word" within your %code macro for each token                                                               
    (word) in the input list.                                                                                                           
    If your macro has any parameters (named or keyword) (even an empty                                                                  
    list), then specify the macro name with empty parentheses for the mname                                                             
    parameter or tokenization errors will occur during macro execution. For                                                             
    example, %loop(hello world,mname=mymacro());.  See examples in the                                                                  
    Usage section above.                                                                                                                
    If your input list has embedded blanks, specify a different dlm value.                                                              
    Do not use the prefix __ (two underscores) for any macro variables in                                                               
    the child macro.  This prefix is reserved for any macro variables in                                                                
    this looping macro.                                                                                                                 
    To "carry forward" the value of the iterator across loops, assign it                                                                
    (&__iter__) to a global macro variable in an inner macro.                                                                           
    ---------------------------------------------------------------------*/                                                             
                                                                                                                                        
    %macro loop                                                                                                                         
    /*---------------------------------------------------------------------                                                             
    Invoke the nested macro "%code" over a list of space separated                                                                      
    list of items.                                                                                                                      
    ---------------------------------------------------------------------*/                                                             
    (__LIST__      /* Space or character separated list of items (REQ)   */                                                             
    ,DLM=%str( )   /* Delimiter character (REQ).  Default is a space.    */                                                             
    ,MNAME=code    /* Macro name (Optional).  Default is "%code"         */                                                             
    );                                                                                                                                  
                                                                                                                                        
    %local macro parmerr __iter__ word;                                                                                                 
    %let macro = &sysmacroname;                                                                                                         
                                                                                                                                        
    %* check input parameters ;                                                                                                         
    %parmv(MNAME,        _req=1,_words=0,_case=N)                                                                                       
                                                                                                                                        
    %if (&parmerr) %then %goto quit;                                                                                                    
                                                                                                                                        
    %* the iterator MUST be unique between macro invocations ;                                                                          
    %* if not, nested invocations of this macro cause looping problems ;                                                                
    %* unfortunately, SAS macro does not support truly private variable scoping ;                                                       
                                                                                                                                        
    %let __iter__ = 1;                                                                                                                  
    %let word = %qscan(%superq(__list__),&__iter__,%superq(dlm));                                                                       
    %do %while (%superq(word) ne %str());                                                                                               
      %let word=%unquote(&word);                                                                                                        
    %&mname  /* do not indent macro call */                                                                                             
       %let __iter__ = %eval(&__iter__+1);                                                                                              
       %let word = %qscan(%superq(__list__),&__iter__,%superq(dlm));                                                                    
    %end;                                                                                                                               
                                                                                                                                        
    %quit:                                                                                                                              
    %* if (&parmerr) %then %abort;                                                                                                      
                                                                                                                                        
    %mend loop;                                                                                                                         
    ;;;;                                                                                                                                
    run;quit;                                                                                                                           
                                                                                                                                        
    *               _ _     _                                                                                                           
     ___  ___ _ __ | (_)___| |_                                                                                                         
    / __|/ _ \ '_ \| | / __| __|                                                                                                        
    \__ \  __/ |_) | | \__ \ |_                                                                                                         
    |___/\___| .__/|_|_|___/\__|                                                                                                        
             |_|                                                                                                                        
    ;                                                                                                                                   
                                                                                                                                        
    filename ft15f001 "&oto./seplist.sas";                                                                                              
    parmcards4;                                                                                                                         
    /*====================================================================                                                              
    Program Name            : seplist.sas                                                                                               
    Purpose                 : Emit a list of words separated by a delimiter.                                                            
    SAS Version             : Unknown                                                                                                   
    Input Data              : N/A                                                                                                       
    Output Data             : N/A                                                                                                       
    Macros Called           : None                                                                                                      
    Originally Written by   : Richard Devenezia                                                                                         
    Date                    : 02SEP1999                                                                                                 
    Program Version #       : 1.0                                                                                                       
    ======================================================================                                                              
    Copyright (c) 2016 Scott Bass (sas_l_739@yahoo.com.au)                                                                              
    This code is licensed under the Unlicense license.                                                                                  
    For more information, please refer to http://unlicense.org/UNLICENSE.                                                               
    =======================================================================                                                             
    Modification History    :                                                                                                           
    Programmer              : Scott Bass                                                                                                
    Date                    : 24APR2006                                                                                                 
    Change/reason           : Copied from                                                                                               
                              http://www.devenezia.com/downloads/sas/macros/index.php?m=seplist                                         
    Program Version #       : 1.1                                                                                                       
    Programmer              : Scott Bass                                                                                                
    Date                    : 07OCT2011                                                                                                 
    Change/reason           : Added %unquote(&prefix) and %unquote(&suffix)                                                             
                              to allow for incremented prefix or suffix                                                                 
    Program Version #       : 1.2                                                                                                       
    ====================================================================*/                                                              
                                                                                                                                        
    /*---------------------------------------------------------------------                                                             
    Usage:                                                                                                                              
    %put %seplist(Hello World);                                                                                                         
    * Returns Hello,World ;                                                                                                             
    %put %seplist(Hello World,nest=QQ);                                                                                                 
    * Returns "Hello","World" ;                                                                                                         
    %put %seplist(Hello   ^   World   ,nest=Q,indlm=^,trim=N);                                                                          
    * Returns 'Hello   ','   World' ;                                                                                                   
    %put %seplist(Hello   ^   World   ,nest=Q,indlm=^,trim=Y);                                                                          
    * Returns 'Hello','World' ;                                                                                                         
    %put %seplist(Hello   ^   World   ,nest=QQ,indlm=^,dlm=~,trim=Y);                                                                   
    * Returns "Hello"~"World" ;                                                                                                         
    %put %seplist(A B C,prefix=PREFIX_,suffix=_suffix);                                                                                 
    * Returns PREFIX_A_suffix,PREFIX_B_suffix,PREFIX_C_suffix ;                                                                         
    %put %seplist(A B C,prefix=%str(%"PREFIX_),suffix=%str(_suffix%"));                                                                 
    * Returns "PREFIX_A_suffix","PREFIX_B_suffix","PREFIX_C_suffix" ;                                                                   
    %put %seplist(A B C,prefix=%nrstr(name&n=));                                                                                        
    * Returns name1=A,name2=B,name3=C ;                                                                                                 
    %put %seplist(A B C,prefix=%nrstr(name&n=),suffix=%nrstr(_&n));                                                                     
    * Returns name1=A_1,name2=B_2,name3=C_3 ;                                                                                           
    -----------------------------------------------------------------------                                                             
    Notes:                                                                                                                              
    The NEST parameter is just a shortcut to set specifix                                                                               
    PREFIX and SUFFIX settings.  So, if the NEST parameter is specified,                                                                
    it overrides the PREFIX and SUFFIX parameters.                                                                                      
    If you want both PREFIX/SUFFIX and NESTing, do not specify NEST,                                                                    
    and "manually" specify the PREFIX and SUFFIX parameters.                                                                            
    ---------------------------------------------------------------------*/                                                             
                                                                                                                                        
    %macro seplist                                                                                                                      
    /*---------------------------------------------------------------------                                                             
    Emit a list of words separated by a delimiter                                                                                       
    ---------------------------------------------------------------------*/                                                             
    (ITEMS                                                                                                                              
    ,INDLM=%str( )                                                                                                                      
    ,DLM=%str(,)                                                                                                                        
    ,PREFIX=                                                                                                                            
    ,NEST=                                                                                                                              
    ,SUFFIX=                                                                                                                            
    ,TRIM=Y                                                                                                                             
    );                                                                                                                                  
                                                                                                                                        
    /*---------------------------------------------------------------------                                                             
    Usage:                                                                                                                              
    -----------------------------------------------------------------------                                                             
    Notes:                                                                                                                              
    %* Richard A. DeVenezia - 990902;                                                                                                   
    %*                                                                                                                                  
    %* emit a list of words separated by a delimiter                                                                                    
    %*                                                                                                                                  
    %* items  - list of items, separated by indlm                                                                                       
    %* indlm  - string that delimits each item of items                                                                                 
    %*   dlm  - string that delimits list of items emitted                                                                              
    %* prefix - string to place before each item                                                                                        
    %* nest   - Q (single quote ''),                                                                                                    
    %*          QQ (double quotes ""),                                                                                                  
    %*          P (parenthesis ()),                                                                                                     
    %*          C (curly braces {}),                                                                                                    
    %*          B (brackets [])                                                                                                         
    %* suffix - string to place after each item                                                                                         
    %*                                                                                                                                  
    %* Note: nest is a convenience, and could be accomplished using                                                                     
    %*       prefix and suffix                                                                                                          
    %*;                                                                                                                                 
    ---------------------------------------------------------------------*/                                                             
                                                                                                                                        
    %local macro parmerr emit nest prefix suffix item n;                                                                                
    %let macro = &sysmacroname;                                                                                                         
                                                                                                                                        
    %* check input parameters ;                                                                                                         
    %parmv(NEST,         _req=0,_words=0,_case=U,_val=Q QQ P C B)                                                                       
    %parmv(TRIM,         _req=0,_words=0,_case=U,_val=0 1)                                                                              
                                                                                                                                        
    %if (&parmerr) %then %goto quit;                                                                                                    
                                                                                                                                        
    %let emit=;                                                                                                                         
                                                                                                                                        
    %let nest = %upcase (&nest);                                                                                                        
                                                                                                                                        
    %if (&nest = Q) %then %do;                                                                                                          
       %let prefix = &prefix.%str(%');                                                                                                  
       %let suffix = %str(%')&suffix;                                                                                                   
    %end;                                                                                                                               
    %else                                                                                                                               
    %if (&nest = QQ) %then %do;                                                                                                         
       %let prefix = &prefix.%str(%");                                                                                                  
       %let suffix = %str(%")&suffix;                                                                                                   
    %end;                                                                                                                               
    %else                                                                                                                               
    %if (&nest = P) %then %do;                                                                                                          
       %let prefix = &prefix.%str(%();                                                                                                  
       %let suffix = %str(%))&suffix;                                                                                                   
    %end;                                                                                                                               
    %else                                                                                                                               
    %if (&nest = C) %then %do;                                                                                                          
       %let prefix = &prefix.%str({);                                                                                                   
       %let suffix = %str(})&suffix;                                                                                                    
    %end;                                                                                                                               
    %else                                                                                                                               
    %if (&nest = B) %then %do;                                                                                                          
       %let prefix = &prefix.%str([);                                                                                                   
       %let suffix = %str(])&suffix;                                                                                                    
    %end;                                                                                                                               
                                                                                                                                        
    %let n = 1;                                                                                                                         
    %let item = %qscan (&items, &n, %quote(&indlm));                                                                                    
                                                                                                                                        
    %do %while (%superq(item) ne );                                                                                                     
       %if (&trim) %then %let item=%qleft(%qtrim(&item));                                                                               
                                                                                                                                        
       %if (&n = 1) %then %do;                                                                                                          
          %if (&nest eq ) %then                                                                                                         
             %let emit = %unquote(&prefix.)&item.%unquote(&suffix);                                                                     
          %else                                                                                                                         
             %let emit = &prefix.&item.&suffix;                                                                                         
       %end;                                                                                                                            
       %else %do;                                                                                                                       
          %if (&nest eq ) %then                                                                                                         
             %let emit = &emit.&dlm.%unquote(&prefix.)&item.%unquote(&suffix);                                                          
          %else                                                                                                                         
             %let emit = &emit.&dlm.&prefix.&item.&suffix;                                                                              
       %end;                                                                                                                            
                                                                                                                                        
       %let n = %eval (&n+1);                                                                                                           
       %let item = %qscan (&items, &n, %quote(&indlm));                                                                                 
    %end;                                                                                                                               
                                                                                                                                        
    %unquote(&emit)                                                                                                                     
                                                                                                                                        
    %quit:                                                                                                                              
    %* if (&parmerr) %then %abort;                                                                                                      
                                                                                                                                        
    %mend seplist;                                                                                                                      
    ;;;;                                                                                                                                
    run;quit;                                                                                                                           
                                                                                                                                        
                                                                                                                                        
