# utl-given-the-I5th-day-in-mar-and-apr-concatelate-the-months-giving-marxapr-sql-sas-r-python
Given the 15th day in mar and apr concatenate the months giving mar&amp;apr r and python
    %let pgm=utl-given-the-I5th-day-in-mar-and-apr-concatelate-the-months-giving-marxapr-sql-sas-r-python;

    %stop_submission;

    Given the 15th day in mar and apr concatelate the months giving mar&apr r and python

    CONTENTS (SQL SOLUTIONS)
        1 sas sql no arrays
          clumsy and not dynamic
        2 sas sql arrays do_over
          clumsy and dynamic
        3 r sql
          elegant?
        4 python sql
          elegant?


    SOAPBOX ON
      ISO dates are much more useful than numeric date especially when using many sql databases?
      SQLITE group_concat function simplifies the squeries.
    SOAPBOX OFF


    github
    https://tinyurl.com/57tunawk
    https://github.com/rogerjdeangelis/utl-given-the-I5th-day-in-mar-and-apr-concatelate-the-months-giving-marxapr-sql-sas-r-python

    sas communities
    https://tinyurl.com/mpr8bxuk
    https://communities.sas.com/t5/SAS-Programming/Finding-where-value-in-a-column-are-repeated-and-putting-them/m-p/751871#M236760

    Related group_concat
    github
    https://tinyurl.com/2sbbxj7u
    https://github.com/rogerjdeangelis/utl-example-of-using-r-sqlite-sqldf-group-concat-function-to-concat-a-hierarchy-of-strings

    https://tinyurl.com/37kr8jcn
    https://github.com/rogerjdeangelis/utl-example-of-sqlite-group_concat-and-associated-sas-datastep-solution

    https://tinyurl.com/5jj9t8wu
    https://github.com/rogerjdeangelis/utl-roll-up-adverse-events-by-patient-and-date-using-sql-groupcat-r-python-and-excel


    /**************************************************************************************************************************/
    /* INPUT                                | PROCESS                                    |                                    */
    /* =====                                | =======                                    |                                    */
    /*   MON      DATEC                     | 1 SAS SQL NO ARRAYS                        |                                    */
    /*                                      | ===================                        |                                    */
    /*   MAR    2018-03-04                  |                                            |                                    */
    /*   MAR    2018-03-13                  | Data sorted by day of the month            |                D                   */
    /*   MAR    2018-03-18                  | for documentation purpose only.            |                A                   */
    /*   MAR    2018-03-25                  | Data does not have to be sorted            |                Y                   */
    /*   APR    2018-04-01                  | Concat groupings of day_of_month           |                _  P                */
    /*   APR    2018-04-08                  |                                            |                O  A                */
    /*   APR    2018-04-15                  | MON   DATEC    WANT                        |                F  R                */
    /*   APR    2018-04-22                  |                                            |                _  T  R             */
    /*   APR    2018-04-25                  | APR 2018-04-01                             |         D      M  I  E             */
    /*   MAY    2018-05-06                  | MAR 2018-03-04                             |         A      O  T  S             */
    /*   MAY    2018-05-13                  | MAY 2018-05-06                             |  M      T      N  I  U             */
    /*   MAY    2018-05-20                  | APR 2018-04-08                             |  O      E      T  O  L             */
    /*   MAY    2018-05-27                  |                                            |  N      C      H  N  T             */
    /*                                      | MAR 2018-03-13  MAR,MAY(same day)          |                                    */
    /* options validvarname=upcase;         | MAY 2018-05-13  remove                     |  MPR 2018-04-01 01 1 APR           */
    /* libname sd1 "d:/sd1";                |                                            |  MAR 2018-03-04 04 1 MAR           */
    /* data sd1.have;                       | APR 2018-04-15                             |  MAY 2018-05-06 06 1 MAY           */
    /* input mon $ datec $10.;              | MAR 2018-03-18                             |  MPR 2018-04-08 08 1 APR           */
    /* cards4;                              | MAY 2018-05-20                             |  AAR 2018-03-13 13 1 MAR,MAY       */
    /* MAR 2018-03-04                       | APR 2018-04-22                             |  APR 2018-04-15 15 1 APR           */
    /* MAR 2018-03-13                       |                                            |  AAR 2018-03-18 18 1 MAR           */
    /* MAR 2018-03-18                       | MAR 2018-03-25 MAR,APR(same day)           |  AAY 2018-05-20 20 1 MAY           */
    /* MAR 2018-03-25                       | APR 2018-04-25 remove                      |  APR 2018-04-22 22 1 APR           */
    /* APR 2018-04-01                       |                                            |  MAR 2018-03-25 25 1 MAR,APR       */
    /* APR 2018-04-08                       | MAY 2018-05-27                             |  MAY 2018-05-27 27 1 MAY           */
    /* APR 2018-04-15                       |                                            |                                    */
    /* APR 2018-04-22                       |                                            |                                    */
    /* APR 2018-04-25                       | proc sql;                                  |                                    */
    /* MAY 2018-05-06                       |   create                                   |                                    */
    /* MAY 2018-05-13                       |     table reps as                          |                                    */
    /* MAY 2018-05-20                       |   select                                   |                                    */
    /* MAY 2018-05-27                       |     *                                      |                                    */
    /* ;;;;                                 |    ,substr(datec,9,2) as day_of_month      |                                    */
    /* run;quit;                            |   from                                     |                                    */
    /*                                      |     sd1.have                               |                                    */
    /*                                      |   order                                    |                                    */
    /*                                      |    by substr(datec,9,2), datec             |                                    */
    /*                                      | ;                                          |                                    */
    /*                                      |   create                                   |                                    */
    /*                                      |     table want (                           |                                    */
    /*                                      |        where=(partition ne 2)              |                                    */
    /*                                      |        drop= months1 months2               |                                    */
    /*                                      |        )  as                               |                                    */
    /*                                      |   select                                   |                                    */
    /*                                      |     mon                                    |                                    */
    /*                                      |    ,datec                                  |                                    */
    /*                                      |    ,day_of_month                           |                                    */
    /*                                      |    ,partition                              |                                    */
    /*                                      |    ,max(case when partition=1              |                                    */
    /*                                      |       then mon else "" end) as months1     |                                    */
    /*                                      |    ,max(case when partition=2              |                                    */
    /*                                      |       then mon else "" end) as months2     |                                    */
    /*                                      |    ,catx(','                               |                                    */
    /*                                      |       ,calculated months1                  |                                    */
    /*                                      |       ,calculated months2) as result       |                                    */
    /*                                      |   from                                     |                                    */
    /*                                      |     %sqlpartitionx(reps,by=day_of_month)   |                                    */
    /*                                      |   group                                    |                                    */
    /*                                      |     by day_of_month                        |                                    */
    /*                                      | ;quit;                                     |                                    */
    /*                                      |                                            |                                    */
    /*                                      |                                            |                                    */
    /*                                      | proc print data=want heading=vertical;     |                                    */
    /*                                      | run;quit;                                  |                                    */
    /*                                      |                                            |                                    */
    /*                                      |---------------------------------------------------------------------------------*/
    /*                                      | 1 SAS SQL ARRAYS                           |                D                   */
    /*                                      | ================                           |                A                   */
    /*                                      |                                            |                Y                   */
    /*                                      | proc datasets lib=work nolist nodetails;   |                _  P                */
    /*                                      |  delete want;                              |                O  A                */
    /*                                      | run;quit;                                  |                F  R                */
    /*                                      |                                            |                _  T  R             */
    /*                                      | proc sql;                                  |         D      M  I  E             */
    /*                                      |   create                                   |         A      O  T  S             */
    /*                                      |     table reps as                          |  M      T      N  I  U             */
    /*                                      |   select                                   |  O      E      T  O  L             */
    /*                                      |     *                                      |  N      C      H  N  T             */
    /*                                      |    ,substr(datec,9,2) as day_of_month      |                                    */
    /*                                      |   from                                     | APR 2018-04-01 01 1 APR            */
    /*                                      |     sd1.have                               | MAR 2018-03-04 04 1 MAR            */
    /*                                      |   order                                    | MAY 2018-05-06 06 1 MAY            */
    /*                                      |     by substr(datec,9,2), datec            | APR 2018-04-08 08 1 APR            */
    /*                                      | ;                                          | MAR 2018-03-13 13 1 MAR,MAY        */
    /*                                      |   *--- reps cannot be a view d       ---;  | APR 2018-04-15 15 1 APR            */
    /*                                      |   *--- because of imbedded monotonic ---;  | MAR 2018-03-18 18 1 MAR            */
    /*                                      |   *--- and sas sql optimizer         ---;  | MAY 2018-05-20 20 1 MAY            */
    /*                                      |                                            | APR 2018-04-22 22 1 APR            */
    /*                                      |   select                                   | MAR 2018-03-25 25 1 MAR,APR        */
    /*                                      |     max(partition)                         | MAY 2018-05-27 27 1 MAY            */
    /*                                      |   into                                     |                                    */
    /*                                      |    :maxdim trimmed                         |                                    */
    /*                                      |   from                                     |                                    */
    /*                                      |     %sqlpartitionx(reps,by=day_of_month)   |                                    */
    /*                                      |   ;                                        |                                    */
    /*                                      | %array(_mx,values=1-&maxdim);              |                                    */
    /*                                      |                                            |                                    */
    /*                                      |    create                                  |                                    */
    /*                                      |      table want(where=(                    |                                    */
    /*                                      |        partition ne 2)                     |                                    */
    /*                                      |      drop=months1 months2                  |                                    */
    /*                                      |     ) as                                   |                                    */
    /*                                      |   select                                   |                                    */
    /*                                      |     mon                                    |                                    */
    /*                                      |    ,datec                                  |                                    */
    /*                                      |    ,day_of_month                           |                                    */
    /*                                      |    ,partition                              |                                    */
    /*                                      |    ,%do_over(_mx,phrase=%str(              |                                    */
    /*                                      |      max(case when partition=?             |                                    */
    /*                                      |       then mon else "" end) as months?)    |                                    */
    /*                                      |      ,between=comma)                       |                                    */
    /*                                      |    ,catx(','                               |                                    */
    /*                                      |    ,%do_over(_mx,phrase=%str(              |                                    */
    /*                                      |      calculated months?),between=comma))   |                                    */
    /*                                      |      as result                             |                                    */
    /*                                      |   from                                     |                                    */
    /*                                      |     %sqlpartitionx(reps,by=day_of_month)   |                                    */
    /*                                      |   group                                    |                                    */
    /*                                      |     by day_of_month                        |                                    */
    /*                                      | ;quit;                                     |                                    */
    /*                                      |                                            |                                    */
    /*                                      | proc print data=want heading=vertical;     |                                    */
    /*                                      | run;quit;                                  |                                    */
    /*                                      |                                            |                                    */
    /*                                      |---------------------------------------------------------------------------------*/
    /*                                      | 3 R SQL                                    |    DATEC       MTHS                */
    /*                                      | =======                                    |                                    */
    /*                                      |                                            |  2018-04-01    APR                 */
    /*                                      | proc datasets lib=sd1 nolist nodetails;    |  2018-03-04    MAR                 */
    /*                                      |  delete want;                              |  2018-05-06    MAY                 */
    /*                                      | run;quit;                                  |  2018-04-08    APR                 */
    /*                                      |                                            |  2018-03-13    MAR,MAY             */
    /*                                      | %utl_rbeginx;                              |  2018-04-15    APR                 */
    /*                                      | parmcards4;                                |  2018-03-18    MAR                 */
    /*                                      | library(haven)                             |  2018-05-20    MAY                 */
    /*                                      | library(sqldf)                             |  2018-04-22    APR                 */
    /*                                      | source("c:/oto/fn_tosas9x.R")              |  2018-03-25    MAR,APR             */
    /*                                      | options(sqldf.dll                          |  2018-05-27    MAY                 */
    /*                                      |   = "d:/dll/sqlean.dll")                   |                                    */
    /*                                      | have<-read_sas(                            |                                    */
    /*                                      |   "d:/sd1/have.sas7bdat")                  |                                    */
    /*                                      | print(have)                                |                                    */
    /*                                      | want<-sqldf("                              |                                    */
    /*                                      |  with                                      |                                    */
    /*                                      |     daynum as (                            |                                    */
    /*                                      |  select                                    |                                    */
    /*                                      |    mon                                     |                                    */
    /*                                      |   ,datec                                   |                                    */
    /*                                      |   ,time_get_month(datec) as mnth           |                                    */
    /*                                      |   ,substr(strftime('%d',datec),1,2)        |                                    */
    /*                                      |      as month_day                          |                                    */
    /*                                      |  from                                      |                                    */
    /*                                      |    have )                                  |                                    */
    /*                                      |  select                                    |                                    */
    /*                                      |    min(datec) as datec                     |                                    */
    /*                                      |   ,group_concat(mon) as mths               |                                    */
    /*                                      |  from                                      |                                    */
    /*                                      |    daynum                                  |                                    */
    /*                                      |  group                                     |                                    */
    /*                                      |    by month_day                            |                                    */
    /*                                      | ")                                         |                                    */
    /*                                      | want;                                      |                                    */
    /*                                      | fn_tosas9x(                                |                                    */
    /*                                      |       inp    = want                        |                                    */
    /*                                      |      ,outlib ="d:/sd1/"                    |                                    */
    /*                                      |      ,outdsn ="want"                       |                                    */
    /*                                      |      )                                     |                                    */
    /*                                      | ;;;;                                       |                                    */
    /*                                      | %utl_rendx;                                |                                    */
    /*                                      |                                            |                                    */
    /*                                      | proc print data=sd1.want;                  |                                    */
    /*                                      | run;quit;                                  |                                    */
    /*                                      |                                            |                                    */
    /*                                      |---------------------------------------------------------------------------------*/
    /*                                      | 4 PYTHON SQL                               |  PYTHON                            */
    /*                                      | ============                               |           datec     mths           */
    /*                                      |                                            |  0   2018-04-01      APR           */
    /*                                      | proc datasets lib=sd1                      |  1   2018-03-04      MAR           */
    /*                                      |   nolist nodetails;                        |  2   2018-05-06      MAY           */
    /*                                      |  delete pywant;                            |  3   2018-04-08      APR           */
    /*                                      | run;quit;                                  |  4   2018-03-13  MAR,MAY           */
    /*                                      |                                            |  5   2018-04-15      APR           */
    /*                                      | %utl_pybeginx;                             |  6   2018-03-18      MAR           */
    /*                                      | parmcards4;                                |  7   2018-05-20      MAY           */
    /*                                      | exec(open(  \                              |  8   2018-04-22      APR           */
    /*                                      |  'c:/oto/fn_pythonx.py').read());          |  9   2018-03-25  MAR,APR           */
    /*                                      | have,meta = ps.read_sas7bdat( \            |  10  2018-05-27      MAY           */
    /*                                      |  'd:/sd1/have.sas7bdat');                  |                                    */
    /*                                      | want=pdsql('''                             |                                    */
    /*                                      |  with                                      |  SAS                               */
    /*                                      |     daynum as (                            |    DATEC       MTHS                */
    /*                                      |  select                                    |                                    */
    /*                                      |    mon                                     |  2018-04-01    APR                 */
    /*                                      |   ,datec                                   |  2018-03-04    MAR                 */
    /*                                      |   ,time_get_month(datec) as mnth           |  2018-05-06    MAY                 */
    /*                                      |   ,substr(strftime('%d',datec),1,2)        |  2018-04-08    APR                 */
    /*                                      |      as month_day                          |  2018-03-13    MAR,MAY             */
    /*                                      |  from                                      |  2018-04-15    APR                 */
    /*                                      |    have )                                  |  2018-03-18    MAR                 */
    /*                                      |                                            |  2018-05-20    MAY                 */
    /*                                      |  select                                    |  2018-04-22    APR                 */
    /*                                      |    min(datec) as datec                     |  2018-03-25    MAR,APR             */
    /*                                      |   ,group_concat(mon) as mths               |  2018-05-27    MAY                 */
    /*                                      |  from                                      |                                    */
    /*                                      |    daynum                                  |                                    */
    /*                                      |  group                                     |                                    */
    /*                                      |    by month_day                            |                                    */
    /*                                      |    ''');                                   |                                    */
    /*                                      | print(want);                               |                                    */
    /*                                      | fn_tosas9x(want                            |                                    */
    /*                                      |   ,outlib='d:/sd1/'                        |                                    */
    /*                                      |   ,outdsn='pywant'                         |                                    */
    /*                                      |   ,timeest=3);                             |                                    */
    /*                                      | ;;;;                                       |                                    */
    /*                                      | %utl_pyendx;                               |                                    */
    /*                                      |                                            |                                    */
    /*                                      | proc print data=sd1.pywant;                |                                    */
    /*                                      | run;quit;                                  |                                    */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    input mon $ datec $10.;
    cards4;
    MAR 2018-03-04
    MAR 2018-03-13
    MAR 2018-03-18
    MAR 2018-03-25
    APR 2018-04-01
    APR 2018-04-08
    APR 2018-04-15
    APR 2018-04-22
    APR 2018-04-25
    MAY 2018-05-06
    MAY 2018-05-13
    MAY 2018-05-20
    MAY 2018-05-27
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*   MON      DATEC                                                                                                       */
    /*                                                                                                                        */
    /*   MAR    2018-03-04                                                                                                    */
    /*   MAR    2018-03-13                                                                                                    */
    /*   MAR    2018-03-18                                                                                                    */
    /*   MAR    2018-03-25                                                                                                    */
    /*   APR    2018-04-01                                                                                                    */
    /*   APR    2018-04-08                                                                                                    */
    /*   APR    2018-04-15                                                                                                    */
    /*   APR    2018-04-22                                                                                                    */
    /*   APR    2018-04-25                                                                                                    */
    /*   MAY    2018-05-06                                                                                                    */
    /*   MAY    2018-05-13                                                                                                    */
    /*   MAY    2018-05-20                                                                                                    */
    /*   MAY    2018-05-27                                                                                                    */
    /**************************************************************************************************************************/

    /*                             _
    / |  ___  __ _ ___   ___  __ _| |  _ __   ___    __ _ _ __ _ __ __ _ _   _ ___
    | | / __|/ _` / __| / __|/ _` | | | `_ \ / _ \  / _` | `__| `__/ _` | | | / __|
    | | \__ \ (_| \__ \ \__ \ (_| | | | | | | (_) || (_| | |  | | | (_| | |_| \__ \
    |_| |___/\__,_|___/ |___/\__, |_| |_| |_|\___/  \__,_|_|  |_|  \__,_|\__, |___/
                                |_|                                      |___/
    */

    proc datasets lib=work nolist nodetails;
     delete want;
    run;quit;

    proc sql;
      create
        table reps as
      select
        *
       ,substr(datec,9,2) as day_of_month
      from
        sd1.have
      order
       by substr(datec,9,2), datec
    ;
      create
        table want (
           where=(partition ne 2)
           drop= months1 months2
           )  as
      select
        mon
       ,datec
       ,day_of_month
       ,partition
       ,max(case when partition=1
          then mon else "" end) as months1
       ,max(case when partition=2
          then mon else "" end) as months2
       ,catx(','
          ,calculated months1
          ,calculated months2) as result
      from
        %sqlpartitionx(reps,by=day_of_month)
      group
        by day_of_month
    ;quit;

    proc print data=want;
    run;quit;

    /**************************************************************************************************************************/
    /*                       DAY_OF_                                                                                          */
    /*  MON      DATEC        MONTH     PARTITION    RESULT                                                                   */
    /*                                                                                                                        */
    /*  APR    2018-04-01      01           1        APR                                                                      */
    /*  MAR    2018-03-04      04           1        MAR                                                                      */
    /*  MAY    2018-05-06      06           1        MAY                                                                      */
    /*  APR    2018-04-08      08           1        APR                                                                      */
    /*  MAR    2018-03-13      13           1        MAR,MAY                                                                  */
    /*  APR    2018-04-15      15           1        APR                                                                      */
    /*  MAR    2018-03-18      18           1        MAR                                                                      */
    /*  MAY    2018-05-20      20           1        MAY                                                                      */
    /*  APR    2018-04-22      22           1        APR                                                                      */
    /*  MAR    2018-03-25      25           1        MAR,APR                                                                  */
    /*  MAY    2018-05-27      27           1        MAY                                                                      */
    /**************************************************************************************************************************/

    /*___                              _
    |___ \   ___  __ _ ___   ___  __ _| |   __ _ _ __ _ __ __ _ _   _ ___
      __) | / __|/ _` / __| / __|/ _` | |  / _` | `__| `__/ _` | | | / __|
     / __/  \__ \ (_| \__ \ \__ \ (_| | | | (_| | |  | | | (_| | |_| \__ \
    |_____| |___/\__,_|___/ |___/\__, |_|  \__,_|_|  |_|  \__,_|\__, |___/
                                    |_|                         |___/
    */

    proc datasets lib=work nolist nodetails;
     delete want;
    run;quit;

    proc sql;
      create
        table reps as
      select
        *
       ,substr(datec,9,2) as day_of_month
      from
        sd1.have
      order
        by substr(datec,9,2), datec
    ;
      *--- reps cannot be a view d       ---;
      *--- because of imbedded monotonic ---;
      *--- and sas sql optimizer         ---;

      select
        max(partition)
      into
       :maxdim trimmed
      from
        %sqlpartitionx(reps,by=day_of_month)
      ;
    %array(_mx,values=1-&maxdim);

       create
         table want(where=(
           partition ne 2)
         drop=months1 months2
        ) as
      select
        mon
       ,datec
       ,day_of_month
       ,partition
       ,%do_over(_mx,phrase=%str(
         max(case when partition=?
          then mon else "" end) as months?)
         ,between=comma)
       ,catx(','
       ,%do_over(_mx,phrase=%str(
           calculated months?),between=comma))
           as result
      from
        %sqlpartitionx(reps,by=day_of_month)
      group
        by day_of_month
    ;quit;

    proc print data=want ;
    run;quit;


    /**************************************************************************************************************************/
    /*|                      DAY_OF_                                                                                          */
    /*| MON      DATEC        MONTH     PARTITION    RESULT                                                                   */
    /*|                                                                                                                       */
    /*| APR    2018-04-01      01           1        APR                                                                      */
    /*| MAR    2018-03-04      04           1        MAR                                                                      */
    /*| MAY    2018-05-06      06           1        MAY                                                                      */
    /*| APR    2018-04-08      08           1        APR                                                                      */
    /*| MAR    2018-03-13      13           1        MAR,MAY                                                                  */
    /*| APR    2018-04-15      15           1        APR                                                                      */
    /*| MAR    2018-03-18      18           1        MAR                                                                      */
    /*| MAY    2018-05-20      20           1        MAY                                                                      */
    /*| APR    2018-04-22      22           1        APR                                                                      */
    /*| MAR    2018-03-25      25           1        MAR,APR                                                                  */
    /*| MAY    2018-05-27      27           1        MAY                                                                      */
    /**************************************************************************************************************************/

    /*____                    _
    |___ /   _ __   ___  __ _| |
      |_ \  | `__| / __|/ _` | |
     ___) | | |    \__ \ (_| | |
    |____/  |_|    |___/\__, |_|
                           |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete want;
    run;quit;

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
    options(sqldf.dll
      = "d:/dll/sqlean.dll")
    have<-read_sas(
      "d:/sd1/have.sas7bdat")
    print(have)
    want<-sqldf("
     with
        daynum as (
     select
       mon
      ,datec
      ,time_get_month(datec) as mnth
      ,substr(strftime('%d',datec),1,2)
         as month_day
     from
       have )
     select
       min(datec) as datec
      ,group_concat(mon) as mths
     from
       daynum
     group
       by month_day
    ")
    want;
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /* R                       |  SAS                                                                                         */
    /*         datec    mths   |    DATEC       MTHS                                                                          */
    /*                         |                                                                                              */
    /* 1  2018-04-01     APR   |  2018-04-01    APR                                                                           */
    /* 2  2018-03-04     MAR   |  2018-03-04    MAR                                                                           */
    /* 3  2018-05-06     MAY   |  2018-05-06    MAY                                                                           */
    /* 4  2018-04-08     APR   |  2018-04-08    APR                                                                           */
    /* 5  2018-03-13 MAR,MAY   |  2018-03-13    MAR,MAY                                                                       */
    /* 6  2018-04-15     APR   |  2018-04-15    APR                                                                           */
    /* 7  2018-03-18     MAR   |  2018-03-18    MAR                                                                           */
    /* 8  2018-05-20     MAY   |  2018-05-20    MAY                                                                           */
    /* 9  2018-04-22     APR   |  2018-04-22    APR                                                                           */
    /* 10 2018-03-25 MAR,APR   |  2018-03-25    MAR,APR                                                                       */
    /* 11 2018-05-27     MAY   |  2018-05-27    MAY                                                                           */
    /**************************************************************************************************************************/

    /*  _                 _   _                             _
    | || |    _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
    | || |_  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
    |__   _| | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
       |_|   | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
             |_|    |___/                                |_|
    */

    proc datasets lib=sd1
      nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    exec(open(  \
     'c:/oto/fn_pythonx.py').read());
    have,meta = ps.read_sas7bdat( \
     'd:/sd1/have.sas7bdat');
    want=pdsql('''
     with
        daynum as (
     select
       mon
      ,datec
      ,time_get_month(datec) as mnth
      ,substr(strftime('%d',datec),1,2)
         as month_day
     from
       have )

     select
       min(datec) as datec
      ,group_concat(mon) as mths
     from
       daynum
     group
       by month_day
       ''');
    print(want);
    fn_tosas9x(want
      ,outlib='d:/sd1/'
      ,outdsn='pywant'
      ,timeest=3);
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;


    /**************************************************************************************************************************/
    /* python                   |                                                                                             */
    /*          datec     mths  |    DATEC       MTHS                                                                         */
    /*                          |                                                                                             */
    /* 0   2018-04-01      APR  |  2018-04-01    APR                                                                          */
    /* 1   2018-03-04      MAR  |  2018-03-04    MAR                                                                          */
    /* 2   2018-05-06      MAY  |  2018-05-06    MAY                                                                          */
    /* 3   2018-04-08      APR  |  2018-04-08    APR                                                                          */
    /* 4   2018-03-13  MAR,MAY  |  2018-03-13    MAR,MAY                                                                      */
    /* 5   2018-04-15      APR  |  2018-04-15    APR                                                                          */
    /* 6   2018-03-18      MAR  |  2018-03-18    MAR                                                                          */
    /* 7   2018-05-20      MAY  |  2018-05-20    MAY                                                                          */
    /* 8   2018-04-22      APR  |  2018-04-22    APR                                                                          */
    /* 9   2018-03-25  MAR,APR  |  2018-03-25    MAR,APR                                                                      */
    /* 10  2018-05-27      MAY  |  2018-05-27    MAY                                                                          */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

