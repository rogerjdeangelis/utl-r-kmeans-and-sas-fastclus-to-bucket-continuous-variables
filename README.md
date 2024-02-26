# utl-r-kmeans-and-sas-fastclus-to-bucket-continuous-variables
R kmeans and sas fastclus to bucket continuous variables;
    %let pgm=utl-r-kmeans-and-sas-fastclus-to-bucket-continuous-variables;

    R kmeans and sas fastclus to bucket continuous variables;

        TWO SOLUTIONS

            1 sas fastclus
            2 r kmeans

    github
    http://tinyurl.com/2pf8p9cv
    https://github.com/rogerjdeangelis/utl-r-kmeans-and-sas-fastclus-to-bucket-continuous-variables

    Related Repos

    https://github.com/rogerjdeangelis/utl-R-kmeans-clustering--of-tweets
    https://github.com/rogerjdeangelis/utl-kmeans-cluster-analysis-based-on-only-one-variable-in-r
    https://github.com/rogerjdeangelis/utl-optimum-number-of-clusters-elbow-plot
    https://github.com/rogerjdeangelis/utl-simple-informative-cluster-analysis-using-sas-interface-to-R
    https://github.com/rogerjdeangelis/utl_grouping_monthly_checking_account_into_two_clusters_by_year
    https://github.com/rogerjdeangelis/utl_linked_and_unlinked_clusters_of_servers_that_have_one_or_more_linkages_in_common

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*          INPUT                                 PROCESS                                      OUTPUT                     */
    /*     (Tukey stem&leaf better)                                                                                           */
    /*                                         SAS FASTCLUS                                                                   */
    /*                                         ============================                                                   */
    /*    2222233333333334444444444555555      proc fastclus data=have                     2222233333333334444444444555555    */
    /*    ...............................            out=clust                             ...............................    */
    /*    5678901234567890123456789012345            maxclusters=3;                        5678901234567890123456789012345    */
    /*   -+++++++++++++++++++++++++++++++         var z;                                  -+++++++++++++++++++++++++++++++    */
    /*   |  Generated Normals           |      run;                                       |                              |    */
    /*   |mu sigma mean sigma mean sigma|                                                 |                              |    */
    /*   |-------- ---------- ----------|      options ls=64 ps=32;                       |                              |    */
    /*   | 3  0.24    3  0.24    3  0.24|      proc chart data=have;                      |                              |    */
    /* 7 +      |                  |    + 7     vbar z / space=0                        7 +      1                  3    + 7  */
    /*   |      |                  |    |         midpoints=2.5 to 5.5 by .1;             |      1                  3    |    */
    /* 6 +      |           |      |    + 6    run;quit;                                6 +      1           2      3    + 6  */
    /*   |      |           |      |    |                                                 |      1           2      3    |    */
    /* 5 +    | |          ||    | ||   + 5    proc means data=clust mean;              5 +    1 1          22    3 33   + 3  */
    /*   |    | |          ||    | ||   |       class cluster;                            |    1 1          22    3 33   |    */
    /* 4 +    ||| |     || ||    | |||  + 4     var z;                                  4 +    111 1     22 22    3 333  + 2  */
    /*   |    ||| |     || ||    | |||  |      run;quit;                                  |    111 1     22 22    3 333  |    */
    /* 3 +    ||||||  |||||||   || |||  + 3                                             3 +    111111  2222222   33 333  + 1  */
    /*   |    ||||||  |||||||   || |||  |      R KMEANS                                   |    111111  2222222   33 333  |    */
    /* 2 +  | ||||||| |||||||   ||||||| + 2    ========                                 2 +  1 1111111 2222222   3333333 + 2  */
    /*   |  | ||||||| |||||||  |||||||| |                                                 |  1 1111111 I222222  33333333 |    */
    /* 1 + |||||||||||||||||| ||||||||| + 1    have<-read_sas("d:/sd1/have.sas7bdat");  1 + 111111111111222222 333333333 + 1  */
    /*   | |||||||||||||||||||||||||||| |      fit <- kmeans(have$Z, 3);                  | 1111111111111222222333333333 |    */
    /*   -+++++++++++++++++++++++++++++++                                                 -+++++++++++++++++++++++++++++++    */
    /*    2222233333333334444444444555555      proc means                                  2222233333333334444444444555555    */
    /*    ...............................       data=want_r_long_names mean;               ...............................    */
    /*    5678901234567890123456789012345      class fit_cluster;                          5678901234567890123456789012345    */
    /*                                         var z;                                                                         */
    /*              Z Midpoint                 run;quit;                                        Z Midpoint                    */
    /*                                                                                                                        */
    /*       INPUT                                                                        SAS FIT                             */
    /*    Obs      Z                                                                                                          */
    /*                                                                                    CLUSTER    OBS            MEAN      */
    /*     1    3.06342                                                                   ------------------------------      */
    /*     2    3.25793                                                                         3     32       4.9138796      */
    /*     3    3.19630                                                                         2     33       3.8846832      */
    /*     4    2.86733                                                                         1     25       2.9941155      */
    /*     5    3.36963                                                                   ------------------------------      */
    /*                                                                                                                        */
    /*    26    5.12879                                                                   R Fit                               */
    /*    27    5.16439                                                                                                       */
    /*    28    4.83147                                                                   CLUSTER    OBS            MEAN      */
    /*    29    4.69578                                                                   ------------------------------      */
    /*    30    4.53991                                                                   3           30       4.9526612      */
    /*                                                                                    2           28       4.0368946      */
    /*                                                                                    1           32       3.0837093      */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    data sd1.have;
     call streaminit(12345);
     do x=1 to 30; z = RAND('NORM', 3, .24); output; end;
     do x=1 to 30; z = RAND('NORM', 4, .24); output; end;
     do x=1 to 30; z = RAND('NORM', 5, .24); output; end;
     stop;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*       INPUT                                                                                                            */
    /*    Obs      Z                                                                                                          */
    /*                                                                                                                        */
    /*     1    3.06342                                                                                                       */
    /*     2    3.25793                                                                                                       */
    /*     3    3.19630                                                                                                       */
    /*     4    2.86733                                                                                                       */
    /*     5    3.36963                                                                                                       */
    /*                                                                                                                        */
    /*    26    5.12879                                                                                                       */
    /*    27    5.16439                                                                                                       */
    /*    28    4.83147                                                                                                       */
    /*    29    4.69578                                                                                                       */
    /*    30    4.53991                                                                                                       */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

     /*                   __           _       _
    / |  ___  __ _ ___   / _| __ _ ___| |_ ___| |_   _ ___
    | | / __|/ _` / __| | |_ / _` / __| __/ __| | | | / __|
    | | \__ \ (_| \__ \ |  _| (_| \__ \ || (__| | |_| \__ \
    |_| |___/\__,_|___/ |_|  \__,_|___/\__\___|_|\__,_|___/

    */

    proc fastclus data=have
          out=clust
          maxclusters=3;
       var z;
    run;

    options ls=64 ps=32;
    proc chart data=have;
     vbar z / space=0
       midpoints=2.5 to 5.5 by .1;
    run;quit;

    proc means data=clust min mean max;
     class cluster;
     var z;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*                       Analysis Variable : Z                                                                            */
    /*                                                                                                                        */
    /*              N                                                                                                         */
    /*  Cluster   Obs        Minimum           Mean        Maximum                                                            */
    /*  ----------------------------------------------------------                                                            */
    /*        3    32      4.3276690      4.9138796      5.3208414                                                            */
    /*        2    33      3.2940622      3.8846832      4.3186713                                                            */
    /*        1    25      2.5962737      2.9941155      3.2579344                                                            */
    /*  ----------------------------------------------------------                                                            */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___           _
    |___ \   _ __  | | ___ __ ___   ___  __ _ _ __  ___
      __) | | `__| | |/ / `_ ` _ \ / _ \/ _` | `_ \/ __|
     / __/  | |    |   <| | | | | |  __/ (_| | | | \__ \
    |_____| |_|    |_|\_\_| |_| |_|\___|\__,_|_| |_|___/

    */


    %utlfkil(d:/xpt/want.xpt);

    %utl_rbegin;
    parmcards4;
    library(haven);
    library(SASxport);
    have<-read_sas("d:/sd1/have.sas7bdat");
    fit <- kmeans(have$Z, 3);
    want <- data.frame(have, fit$cluster);
    want;
    for (i in 1:ncol(want)) {
          label(want[,i])<-colnames(want)[i];
          print(label(want[,i])); };
    write.xport(want,file="d:/xpt/want.xpt");
    ;;;;
    %utl_rend;

    /*--- handles long variable names by using the label to rename the variables  ----*/

    proc datasets lib=work nolist mt=data mt=view nodetails ;delete want want_r_long_names; run;quit;

    libname xpt xport "d:/xpt/want.xpt";
    proc contents data=xpt._all_;
    run;quit;

    data want_r_long_names;
      %utl_rens(xpt.want) ;
      set want;
    run;quit;
    libname xpt clear;

    proc means
     data=want_r_long_names min mean max;
    class fit_cluster;
    var z;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*                     Analysis Variable : Z Z                                                                            */
    /*                                                                                                                        */
    /*  fit.         N                                                                                                        */
    /*  cluster    Obs         Minimum            Mean         Maximum                                                        */
    /*  --------------------------------------------------------------                                                        */
    /*  1           32       2.5962737       3.0837093       3.5364353                                                        */
    /*  2           28       3.6847424       4.0368946       4.3366437                                                        */
    /*  3           30       4.5399118       4.9526612       5.3208414                                                        */
    /*  --------------------------------------------------------------                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
