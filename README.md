# utl-count-a-limited-number-of-sequential-zeroes
Count a limited number of sequential zeroes
    Count a limited number of sequential zeroes                                                                     
                                                                                                                    
    github                                                                                                          
    https://tinyurl.com/y6kdr6kt                                                                                    
    https://github.com/rogerjdeangelis/utl-count-a-limited-number-of-sequential-zeroes                              
                                                                                                                    
    SAS Forum                                                                                                       
    https://tinyurl.com/y52t5e7r                                                                                    
    https://communities.sas.com/t5/SAS-Programming/Starting-a-counter-when-value-goes-to-zero/td-p/580221/page/2    
                                                                                                                    
    novinosrin                                                                                                      
    https://communities.sas.com/t5/user/viewprofilepage/user-id/138205                                              
                                                                                                                    
    *_                   _                                                                                          
    (_)_ __  _ __  _   _| |_                                                                                        
    | | '_ \| '_ \| | | | __|                                                                                       
    | | | | | |_) | |_| | |_                                                                                        
    |_|_| |_| .__/ \__,_|\__|                                                                                       
            |_|                                                                                                     
    ;                                                                                                               
                                                                                                                    
    data limits;                                                                                                    
    input group$ limit;                                                                                             
    datalines;                                                                                                      
    x 2                                                                                                             
    y 3                                                                                                             
    ;                                                                                                               
    run;                                                                                                            
                                                                                                                    
    data have;                                                                                                      
    input group$ MW;                                                                                                
    datalines;                                                                                                      
    x 0                                                                                                             
    x 5                                                                                                             
    x 5                                                                                                             
    x 0                                                                                                             
    x 0                                                                                                             
    x 0                                                                                                             
    y 0                                                                                                             
    y 5                                                                                                             
    y 0                                                                                                             
    y 0                                                                                                             
    y 0                                                                                                             
    y 0                                                                                                             
    y 0                                                                                                             
    run;                                                                                                            
                                                                                                                    
                                                                                                                    
    WORK.LIMITS total obs=2                                                                                         
                                                                                                                    
       GROUP    LIMIT                                                                                               
                                                                                                                    
         x        2                                                                                                 
         y        3                                                                                                 
                                                                                                                    
    WORK.HAVE total obs=13                                                                                          
                                                                                                                    
                   | RULES                                                                                          
                   |                                                                                                
      GROUP    MW  | LIMIT  COUNTER   RULES                                                                         
                   |                                                                                                
        x       0  |   2       .      We have a zero but not another 0                                              
        x       5  |           .      Not 0                                                                         
        x       5  |           .      Not 0                                                                         
                                                                                                                    
        x       0  |           1      We have two sequential 0s                                                     
        x       0  |           2                                                                                    
        x       0  |           .      Over the limit so set to missing                                              
                                                                                                                    
        y       0  |   3       .      We have a zero but not 2 subsequent 0s                                        
        y       5  |           .                                                                                    
        y       0  |           1      We have a zero and 3 0s so start counter                                      
        y       0  |           2                                                                                    
        y       0  |           3                                                                                    
        y       0  |           .      Over the limit so set to missing                                              
        y       0  |           .      Over the limit so set to missing                                              
                                                                                                                    
    *            _               _                                                                                  
      ___  _   _| |_ _ __  _   _| |_                                                                                
     / _ \| | | | __| '_ \| | | | __|                                                                               
    | (_) | |_| | |_| |_) | |_| | |_                                                                                
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                               
                    |_|                                                                                             
    ;                                                                                                               
                                                                                                                    
     WORK.WANT total obs=13                                                                                         
                                                                                                                    
      GROUP    MW    LIMIT    COUNTER                                                                               
                                                                                                                    
        x       0      2         .                                                                                  
        x       5      2         .                                                                                  
        x       5      2         .                                                                                  
        x       0      2         1                                                                                  
        x       0      2         2                                                                                  
        x       0      2         .                                                                                  
        y       0      3         .                                                                                  
        y       5      3         .                                                                                  
        y       0      3         1                                                                                  
        y       0      3         2                                                                                  
        y       0      3         3                                                                                  
        y       0      3         .                                                                                  
        y       0      3         .                                                                                  
                                                                                                                    
    *          _       _   _                                                                                        
     ___  ___ | |_   _| |_(_) ___  _ __                                                                             
    / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                            
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                           
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                           
                                                                                                                    
    ;                                                                                                               
                                                                                                                    
    data want;                                                                                                      
                                                                                                                    
    do until(last.group);                                                                                           
     merge have limits;                                                                                             
     by group;                                                                                                      
     * very clever logic;                                                                                           
     if mw then _f=1;    * only when mw is not 0 set _f=1;                                                          
     if _f then _c=sum(_c,mw=0); * don't increment unless 0;                                                        
     counter=_c;                                                                                                    
     if mw or  _c > limit then counter=.; * non 0 or over limit set to missing;                                     
                                          * otherwise we have incremented 0s;                                       
     output;                                                                                                        
    end;                                                                                                            
                                                                                                                    
    drop _:;                                                                                                        
    run;                                                                                                            
                                                                                                                    
                                                                                                                    
                                                                                                                    
