# utl-weighted-moving-sum-for-several-variables
Weighted moving sum for several variables 

    Weighted moving sum for several variables                                                                           
                                                                                                                        
    Without loss of generality, only 3 month sums are documented                                                        
                                                                                                                        
    There are several simple SAS solutions in my repos, but R is more comprehensive?                                    
                                                                                                                        
    github                                                                                                              
    https://tinyurl.com/yxas594y                                                                                        
    https://github.com/rogerjdeangelis/utl-weighted-moving-sum-for-several-variables                                    
                                                                                                                        
    SAS Forum                                                                                                           
    https://tinyurl.com/y3gwvktm                                                                                        
    https://communities.sas.com/t5/SAS-Programming/Moving-sum-by-12-months-moving-weight-by-3months/m-p/577649          
                                                                                                                        
    For More SAS and R solutions see                                                                                    
                                                                                                                        
    https://tinyurl.com/yyrbv568                                                                                        
    https://github.com/rogerjdeangelis?utf8=%E2%9C%93&tab=repositories&q=roll+in%3Aname&type=&language=                 
                                                                                                                        
    https://tinyurl.com/y6sstnk3                                                                                        
    https://github.com/rogerjdeangelis?utf8=%E2%9C%93&tab=repositories&q=moving+in%3Aname&type=&language=               
                                                                                                                        
    *_                   _                                                                                              
    (_)_ __  _ __  _   _| |_                                                                                            
    | | '_ \| '_ \| | | | __|                                                                                           
    | | | | | |_) | |_| | |_                                                                                            
    |_|_| |_| .__/ \__,_|\__|                                                                                           
            |_|                                                                                                         
    ;                                                                                                                   
                                                                                                                        
    * make some data;                                                                                                   
                                                                                                                        
    data have;                                                                                                          
      array wgtsa[12] wgta1-wgta12 (2 2 2 1 1 1 3 3 3 3 3 3);                                                           
      array wgtsb[12] wgtb1-wgtb12 (2 3 3 3 1 1 1 3 3 3 2 2);                                                           
      array wgtsc[12] wgtc1-wgtc12 (2 2 1 2 2 2 3 2 2 2 2 3);                                                           
      do year=2018 to 2019;                                                                                             
        do month=1 to 12;                                                                                               
          mth=cats(year,put(month,z2.));                                                                                
          sales=int(3*uniform(1234)+1);                                                                                 
          wgt1=wgtsa[month];                                                                                            
          wgt2=wgtsb[month];                                                                                            
          wgt3=wgtsc[month];                                                                                            
          output;                                                                                                       
        end;                                                                                                            
      end;                                                                                                              
      drop year month wgta: wgtb: wgtc: ;                                                                               
    run;quit;                                                                                                           
                                                                                                                        
    WORK.HAVE total obs=24                                                                                              
                                                                                                                        
       MTH      SALES    WGT1    WGT2    WGT3                                                                           
                                                                                                                        
      201801      1        2       2       2                                                                            
      201802      1        2       3       2                                                                            
      201803      2        2       3       1                                                                            
      201804      1        1       3       2                                                                            
      201805      1        1       1       2                                                                            
      201806      1        1       1       2                                                                            
      201807      1        3       1       3                                                                            
      201808      1        3       3       2                                                                            
      201809      2        3       3       2                                                                            
                                                                                                                        
    *           _                                                                                                       
     _ __ _   _| | ___  ___                                                                                             
    | '__| | | | |/ _ \/ __|                                                                                            
    | |  | |_| | |  __/\__ \                                                                                            
    |_|   \__,_|_|\___||___/                                                                                            
                                                                                                                        
    ;                                                                                                                   
                                                                                                                        
    RULES (Showing only SALES, WGT1 AND SALES1)                                                                         
                                                                                                                        
    WORK.WANT total obs=24                                                                                              
                                       |  RULES                                                                         
       MTH      SALES    WGT1   SALES1 |  =====                                                                         
                                       |                                                                                
      201801      1        2       .   |                                                                                
      201802      1        2       .   |                                                                                
      201803      2        2       8   |  = 1+2 + 1*2 + 2*2                                                             
                                       |                                                                                
      201804      1        1       7   |  = 1+2 + 2*2 + 1*1                                                             
                                       |                                                                                
      201805      1        1       6   |                                                                                
      201806      1        1       3   |                                                                                
      201807      1        3       5   |                                                                                
      201808      1        3       7   |                                                                                
      201809      2        3      12   |                                                                                
    *            _               _                                                                                      
      ___  _   _| |_ _ __  _   _| |_                                                                                    
     / _ \| | | | __| '_ \| | | | __|                                                                                   
    | (_) | |_| | |_| |_) | |_| | |_                                                                                    
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                   
                    |_|                                                                                                 
    ;                                                                                                                   
                                                                                                                        
     WRK.WANT total obs=24                                                                                              
                                                                                                                        
       MTH      SALES    WGT1    WGT2    WGT3    SALES1    SALES2    SALES3                                             
                                                                                                                        
      201801      1        2       2       2        .         .         .                                               
      201802      1        2       3       2        .         .         .                                               
      201803      2        2       3       1        8        11         6                                               
      201804      1        1       3       2        7        12         6                                               
      201805      1        1       1       2        6        10         6                                               
      201806      1        1       1       2        3         5         6                                               
      201807      1        3       1       3        5         3         7                                               
      201808      1        3       3       2        7         5         7                                               
      201809      2        3       3       2       12        10         9                                               
    ....                                                                                                                
    *          _       _   _                                                                                            
     ___  ___ | |_   _| |_(_) ___  _ __                                                                                 
    / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                               
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                               
                                                                                                                        
    ;                                                                                                                   
                                                                                                                        
    proc datasets lid=sd1.;                                                                                             
    delete want;                                                                                                        
    run;quit;                                                                                                           
                                                                                                                        
    %utlfkil(d:/xpt/want.xpt);                                                                                          
                                                                                                                        
    options validvarname=upcase;                                                                                        
    libname sd1 "d:/sd1";                                                                                               
    data sd1.havWgt;                                                                                                    
      set have;                                                                                                         
        sales1=sales*wgt1;                                                                                              
        sales2=sales*wgt2;                                                                                              
        sales3=sales*wgt3;                                                                                              
      keep month sales1 sales2 sales3;                                                                                  
    run;quit;                                                                                                           
                                                                                                                        
    %utl_submit_r64('                                                                                                   
    library(haven);                                                                                                     
    library(SASxport);                                                                                                  
    have<-read_sas("d:/sd1/havwgt.sas7bdat");                                                                           
    library(dplyr);                                                                                                     
    library(zoo);                                                                                                       
    want<-as.data.frame(rollsum(have, k=3, fill = NA, align = "right"));                                                
    write.xport(want,file="d:/xpt/want.xpt");                                                                           
    ');                                                                                                                 
                                                                                                                        
    libname xpt xport "d:/xpt/want.xpt";                                                                                
    data want;                                                                                                          
      merge  have xpt.want;                                                                                             
    run;quit;                                                                                                           
    libname xpt clear;                                                                                                  
                                                                                                                        
                                                                                                                        
