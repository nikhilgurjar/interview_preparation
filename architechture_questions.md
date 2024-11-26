### what are SCD and its types
   SCD -> slowly changing dimensions 
  they are used to cater things which changes from time to time
  SCD0 -> never changes when inserted like date of joining in employee table
  SCD1 -> address in customer table
        if you directly update address, you wont have historical data like where he was earlier
        another case if there is some analysis and report done on number of peples in a state, then if we change it directly and someone want to devise jan report again he cant as he will get latest data always
        so basically in SCD1 no history is tracked

  SCD2 
  history will be retained
  we can add 3 columns status, start date, end date
  end date should not be kept null as it can create problems in between and join conditions/merge conditions

  SCD3 used in salesforce condition
  lets say there are 5 peoples under you, they goes to different cities and towns nearby for parcel
  now lets say range of parcel gicing increases
  new column are created with prior_column_name
  generally not used

  scd5 = 1+3
  scd6 = 1+2+3
  https://www.luzmo.com/blog/slowly-changing-dimensions#:~:text=Type%204,in%20a%20different%20table%20altogether.
  
  
  
