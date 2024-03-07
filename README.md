# Bash Training on Codewars

## Bouncing Balls
[codewars addr](https://www.codewars.com/kata/5544c7a5cb454edb3c000047)

### Solution 1
```bash
#!/bin/bash
bouncingBall() {
    # your code
    # h     drop height
    # bounce percentage of h
    # window must be less than h 
    
    # check whther argruments is valid
    # If all three conditions above are fulfilled, return a positive integer, otherwise return -1.

    local h=$1;
    local bounce=$2;
    local window=$3;
    
  
    # Float parameter "h" in meters must be greater than 0  
    # Float parameter "bounce" must be greater than 0 and less than 1
    # Float parameter "window" must be less than h.
    declare -i check;
    check=$(echo "$h > 0" | bc);
    check+=$(echo "$bounce > 0 && $bounce < 1" | bc);
    check+=$(echo "$window < $h" | bc);
    
    
    if [ $check -ne 3 ]
    then
      echo -1
      return
    fi
    
    
    
    # calculate the times that ball comes to our view.
    # including failing and bouncing
    # failing h => h * bounce
    # bouncing
    # failing h => h * bounce
    
    declare -i cnt=-1
	
    # while echo "$h" > $window" | bc 
    # error: the command echo"$h > $window" | bc return value is always 0 unless generate a error. 
    # in condition expression, 0 is as true, not zero is as false. so it will fall into an
    # infinity loop.
    
    
    # correct it to while (( $(echo "$h" > "window" | bc) ))
    # if true, $() => 1, (( )) => 0, 0 as true in bash, continue to execute loop.
    # if false, $() => 0, (( )) => 1, 1 as false in bash, end to execute loop.
    while (( $(echo "$h > $window" | bc) ))
    do
      cnt+=2
      h=$(echo "scale=8;$h * $bounce" | bc)
    done
    
    echo $cnt
}
bouncingBall $1 $2 $3
```
Knowledge Points
* how to use bc command for comparing and performing arithmetic operations with floating-point numbers. 
* condition expresion
* arithmetic expresion
* how to use declare comamnd to set the attribute of a variable.

Pitfalls
* distinguish between return status and standard output.
* bc use standard input as its input, not parameters.
* pay attention to precision when arithmetic operations. such as `echo "scale=8"; $h * $bounce`
