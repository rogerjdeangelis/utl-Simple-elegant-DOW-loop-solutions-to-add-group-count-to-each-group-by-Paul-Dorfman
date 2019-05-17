# utl-Simple-elegant-DOW-loop-solutions-to-add-group-count-to-each-group-by-Paul-Dorfman
Simple elegant DOW loop solutions to add group count to each group by Paul Dorfman

    Simple elegant DOW loop solutions to add group count to each group by Paul Dorfman

          Two very elegant solutions by

              Paul Dorfman <sashole@bellsouth.net>
              Thu, May 16, 9:44 PM (12 hours ago)

               1. DOW loop with sum
               2. Even simpler DOW without sum


    github
    https://tinyurl.com/y5qx5ppj
    https://github.com/rogerjdeangelis/utl-Simple-elegant-DOW-loop-solutions-to-add-group-count-to-each-group-by-Paul-Dorfman

    sas forum
    https://tinyurl.com/yx8mu3es
    https://communities.sas.com/t5/New-SAS-User/counting-observations-in-by-group-and-then-assigning-those/m-p/559474

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    data have;
    input keyvar $ ;
    cards4;
    a
    b
    b
    b
    c
    c
    c
    c
    ;;;;
    run;quit;

    WORK.HAVE total obs=8

    Obs    KEYVAR

     1       a
     2       b
     3       b
     4       b
     5       c
     6       c
     7       c
     8       c

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    Up to 40 obs WORK.WANT total obs=8

    Obs    KEYVAR    CNT     RULES

     1       a        1      add group count=1

     2       b        3      add group count=3
     3       b        3
     4       b        3

     5       c        4      add group count=4
     6       c        4
     7       c        4
     8       c        4

    *
     _ __  _ __ ___   ___ ___  ___ ___
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    ;

    **************************
    1. DOW loop with sum     *
    **************************

    Paul Dorfman <sashole@bellsouth.net>
    Thu, May 16, 9:44 PM (12 hours ago)

    Roger,

    One of the beauties of the DoW loop is that you don't have
    to retain since while the DoW is looping, program
    control never reaches the top of the DATA step.
    Nor do you have to reinitialize before each BY group;
    The same action the DoW avoids inside it causes CNT to be
    reset to missing before each BY group automatically:

    data want ;
      do until (last.keyvar) ;
        set have ;
        by keyvar ;
        cnt = sum (cnt, 1) ;
      end ;
      do _n_ = 1 to cnt ;
        output ;
      end ;
    run ;


    ***********************************
    2. Even simpler DOW without sum   *
    ***********************************

    In fact, even using the SUM function is superfluous
    in this case, as we can hand the counting task over to the DoW itself:

    data want ;
      do cnt = 1 by 1 until (last.keyvar) ;
        set have ;
        by keyvar ;
      end ;
      do _n_ = 1 to cnt ;
        output ;
      end ;
    run ;

    Best regards

    data want;

      do until (last.keyvar);
         set have;
         by keyvar;
         retain cnt 0;
         cnt+1;
      end;

      do _n_=1 to cnt;
        output;
      end;

      cnt=0;

    run;quit;

