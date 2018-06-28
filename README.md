# utl_r_graphics_visualizing_assciation_amoung_many_variables
R graphics visualizing assciation amoung many variables.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    R graphics visualizing assciation amoung many variables

     WPS/Proc R

     These plots can be done in SAS

     Two Plots

         1. Grid of scatter plots, density and correlation values
         2. Grid with bubbles the size of the Pearson correlation

    Output graphics

    see for scatter, density and corr values
    https://tinyurl.com/ybohj228
    https://github.com/rogerjdeangelis/utl_r_graphics_visualizing_assciation_amoung_many_variables/blob/master/utl_r_graphics_visualizing_assciation_amoung_many_variables_scat

    see for bubble corr plot
    https://tinyurl.com/ycjwyyhy
    https://github.com/rogerjdeangelis/utl_r_graphics_visualizing_assciation_amoung_many_variables/blob/master/utl_r_graphics_visualizing_assciation_amoung_many_variables_bub.

    github repo
    https://tinyurl.com/yb8b2s9e
    https://github.com/rogerjdeangelis/utl_r_graphics_visualizing_assciation_amoung_many_variables

    StackOverflow R
    https://tinyurl.com/y7fflugb
    https://stackoverflow.com/questions/51083395/how-to-plot-correlation-graphs-with-r2-for-a-big-datamatrix

    Maurits Evers profile
    https://stackoverflow.com/users/6530970/maurits-evers

    Robs profile
    https://stackoverflow.com/users/5640555/rob



    INPUT
    =====

      1. Grid of scatter plots, density and correlation values

         SD1.HAVXPO total obs=10

            A1       A2       A3       A4

          0.276    0.683    1.245    0.850
          0.409    0.172    1.749    0.358
          0.112    0.377    2.430    0.498
          0.877    0.842    0.831    0.986
          1.034    0.304    0.616    1.001
          1.063    0.624    0.613    0.991
          1.336    0.629    0.411    1.051
          1.139    0.404    0.264    0.900
          1.381    0.583    0.784    1.119
          1.111    0.605    0.461    1.386

      2. Grid with bubbles the size of the Pearson correlation

         Correlation matrix

         SD1.HAVCOR total obs=4

          VARIABLE       A1          A2          A3          A4

             A1        1.00000     0.28380    -0.88043     0.76763
             A2        0.28380     1.00000    -0.38310     0.60423
             A3       -0.88043    -0.38310     1.00000    -0.81019
             A4        0.76763     0.60423    -0.81019     1.00000


    PROCESS (complete code)
    =======================


    1. Grid of scatter plots, density and correlation values

    %utlfkil(d:/pdf/utl_r_graphics_visualizing_assciation_amoung_many_variables_scat.pdf);
    %utl_submit_wps64('
    libname sd1 "d:/sd1";
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk  sas7bdat "%sysfunc(pathname(work))";
    libname hlp  sas7bdat "C:\Progra~1\SASHome\SASFoundation\9.4\core\sashelp";
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);
    library(haven);
    have<-read_sas("d:/sd1/havxpo.sas7bdat");
    head(have);
    library(GGally);
    library(tidyverse);
    have %>%
        ggpairs -> myplot;
    pdf(file="d:/pdf/utl_r_graphics_visualizing_assciation_amoung_many_variables_scat.pdf"
      ,width=7,height=5);
    myplot;
    dev.off();
    endsubmit;
    run;quit;
    ');


    2. Grid with bubbles the size of the Pearson correlation


    %utlfkil(d:/pdf/utl_r_graphics_visualizing_assciation_amoung_many_variables_bub.pdf);
    %utl_submit_wps64('
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);
    library(haven);
    library(corrplot);
    have<-as.data.frame(read_sas("d:/sd1/havcor.sas7bdat"));
    row.names(have) <- have$VARIABLE;
    have[1] <- NULL;
    pdf(file="d:/pdf/utl_r_graphics_visualizing_assciation_amoung_many_variables_bub.pdf"
      ,width=5,height=3);
    corrplot(as.matrix(have),method="circle");
    dev.off();
    endsubmit;
    run;quit;
    ');


    OUTPUT
    ======

    https://tinyurl.com/ybohj228
    https://tinyurl.com/ycjwyyhy

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
      input peptide$ sample1-sample10;
    cards4;
    a1 0.276 0.409 0.112 0.877 1.034 1.063 1.336 1.139 1.381 1.111
    a2 0.683 0.172 0.377 0.842 0.304 0.624 0.629 0.404 0.583 0.605
    a3 1.245 1.749 2.430 0.831 0.616 0.613 0.411 0.264 0.784 0.461
    a4 0.850 0.358 0.498 0.986 1.001 0.991 1.051 0.900 1.119 1.386
    ;;;;
    run;quit;
    options ls=171;

    proc transpose data=sd1.have out=sd1.havXpo(drop=_name_);
    var sample1-sample10;
    id peptide;
    run;quit;

    ods trace on;
    ods output pearsoncorr=sd1.havCor(keep=variable a1-a4);
    proc corr data=sd1.havXpo;
    var a1-a4;
    with a1-a4;
    run;quit;
    ods trace off;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    see process


